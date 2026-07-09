# RadioGuessr — Complete Interview Knowledge Base

---

## 1. What Is This Project?

RadioGuessr is a full-stack web application that is a geography guessing game. A player listens to a live radio stream from somewhere in the world and then clicks on a 3D interactive globe to guess where that station is broadcasting from. The closer the guess, the higher the score. A game is 5 rounds, and the final score (max 25,000) is saved to a global leaderboard if the user is logged in.

The project started as a pure frontend application. I then designed and built a complete backend — a Node.js/Express API server with a Redis caching layer (via Upstash), PostgreSQL database, JWT authentication, persistent leaderboard, rate limiting, and station prefetching.

---

## 2. Full Tech Stack

### Frontend
| Technology | Version | Why Chosen |
|---|---|---|
| React | 19 | Component model fits the multi-phase game UI (start → loading → playing → result → final). Hooks allow clean local state per component. |
| Vite | 7 | Faster than Create React App. Native ESM, instant HMR, and a proxy feature that routes `/api/*` to the backend in dev without CORS issues. |
| Zustand | 5 | Simpler than Redux for this use case. No boilerplate, no providers, no reducers. The entire game state is one `create()` call. Chosen over Context API because Context re-renders the whole tree on every state change — Zustand is selector-based. |
| Framer Motion | 12 | Declarative animation library for React. Used for phase transitions (start screen fading in/out, result cards sliding). |
| Globe.gl | 2 | A Three.js wrapper specifically for 3D globe rendering. Handles WebGL, camera controls, lat/lng to 3D coordinates, arc animations. |
| Three.js | 0.169 | Globe.gl's underlying renderer. Pinned to 0.169 because Globe.gl has known compatibility issues with newer Three.js versions. |
| Tailwind CSS | 4 | Utility-first CSS. No context switching between CSS files. Tailwind v4 uses a Vite plugin so there's no separate config file. |
| Lucide React | 1.7 | Icon library. Tree-shakable — only imports the icons actually used. |
| HLS.js | 1.6 | Some radio stations stream in HLS (HTTP Live Streaming) format, which `<audio>` doesn't support natively. HLS.js handles the `.m3u8` playlist parsing and chunk loading. |
| html2canvas | 1.4 | Available in package.json for a future share-card feature. Takes a DOM screenshot and returns a canvas element. |
| Vercel Analytics | 2 | Lightweight page view and event tracking. Integrates as a React component. |

### Backend
| Technology | Why Chosen | Alternative Considered |
|---|---|---|
| Node.js | Same language as the frontend — no context switching. Great async I/O model for an API that makes multiple upstream HTTP calls. | Python/FastAPI — more overhead, separate language |
| Express | Minimal, unopinionated. Easy to add middleware (CORS, JSON parsing, rate limiting). Well-understood in interviews. | Fastify — faster but less familiar; Koa — too minimal |
| Redis via Upstash (@upstash/redis) | Shared, persistent cache that survives server restarts and scales across multiple instances. Upstash is Redis-over-HTTP — no native binary, works on Windows, free tier. | Plain JS Map — per-process, lost on restart, can't share across instances |
| PostgreSQL (via Supabase) | Relational model fits structured data (users, game sessions). ACID compliance matters for score integrity. Supabase provides free hosted Postgres with SSL and connection pooling. | MongoDB — no joins, overkill schema flexibility for this structured data |
| pg (node-postgres) | Direct Postgres driver. No ORM overhead. Forces you to write real SQL which is better for interviews. | Sequelize/Prisma — ORM abstraction hides SQL, harder to explain in interviews |
| bcryptjs | Industry-standard password hashing. Cost factor 10 means ~100ms per hash — fast enough for UX, slow enough to deter brute force. Pure JS (bcryptjs vs bcrypt) chosen for Windows compatibility. | argon2 — more modern but harder to install on Windows |
| jsonwebtoken | Signs and verifies JWTs. Stateless auth — no session table needed. | express-session + cookies — requires server-side session store |
| dotenv | Loads environment variables from `.env` into `process.env`. Keeps secrets out of code. | Hardcoded values — never acceptable |
| nodemon | Watches `server/` directory and restarts on file changes. Replaced `node --watch` which was too aggressive, restarting on bcrypt temp files. | node --watch — caused ECONNRESET during bcrypt hashing |
| concurrently | Runs `vite` and `nodemon` in a single terminal with `npm run dev:all`. | Two terminals — inconvenient |

---

## 3. Project Architecture — Layer by Layer

### 3.1 Frontend Layers

```
src/
├── main.jsx              # React root, mounts <App />
├── App.jsx               # Globe canvas + phase-based overlay rendering
├── api.js                # All HTTP calls (station, auth, leaderboard)
├── audio.js              # Audio playback (HLS.js + native <audio>) abstraction
├── score.js              # Haversine distance formula → score calculation
├── analytics.js          # Vercel event tracking wrapper
├── index.css             # Tailwind base + global styles
├── data/
│   └── constants.js      # Country→station count, country→region, valid tags
├── store/
│   ├── useGameStore.js   # All game state and actions (Zustand)
│   └── useAuthStore.js   # Auth token + username (Zustand + localStorage)
├── components/
│   ├── phases/           # Full-screen overlays per game phase
│   │   ├── StartScreen.jsx
│   │   ├── PlayingScreen.jsx
│   │   ├── ResultScreen.jsx
│   │   └── FinalScreen.jsx
│   ├── overlays/
│   │   ├── AuthModal.jsx
│   │   ├── Leaderboard.jsx
│   │   ├── ScoreboardTooltip.jsx
│   │   └── VolumeControl.jsx
│   └── ui/               # Reusable small components
│       ├── AnimatedCard.jsx
│       ├── GithubIcon.jsx
│       └── DiscordIcon.jsx
└── Globe.jsx             # Globe.gl wrapper, handles 3D rendering + click events
```

### 3.2 Backend Layers

```
server/
├── index.js       # Express app: middleware, CORS, rate limiters, routes, session store
├── cache.js       # Redis-backed TTL cache via Upstash (GET/SET with EX TTL)
├── radioApi.js    # Radio Browser API proxy: fetchBatch(), pickStation()
├── db.js          # PostgreSQL connection pool (pg.Pool) + query helper
├── auth.js        # Express Router: POST /register, POST /login, GET /me
└── leaderboard.js # Express Router: POST / (save score), GET / (top 20)
```

---

## 4. Data Flow — A Single Game Round

### Step 1: User Opens the App
- React mounts. `App.jsx` renders the Globe canvas underneath all overlays.
- `useAuthStore` reads `rg_auth_token` from localStorage. If found, user is considered logged in.
- `StartScreen` renders on top of the globe.

### Step 2: User Clicks Play
- `startRound(globeRef)` is called in `useGameStore`.
- Phase changes to `'loading'`.
- `fetchStation()` in `api.js` sends `GET /api/station?sessionId=<uuid>` to the Express server.
- Express server calls `pickStation(null, seenUUIDs)` in `radioApi.js`.
- `pickStation` selects a random country from the weighted pool, picks sort params, calls `fetchBatch()`.
- `fetchBatch` calls `cache.get(key)` — an async HTTP GET to Upstash Redis.
  - **Cache HIT:** Returns the JSON-parsed station array from Redis instantly (~5-20ms).
  - **Cache MISS:** Fetches from Radio Browser API (~500-2000ms), then calls `cache.set()` which does `SET key value EX 600` in Redis (fire-and-forget, doesn't block the response).
- Server validates the station (HTTPS URL, valid coordinates, not seen this session), returns a clean station object.
- Frontend receives `{ name, url, country, countrycode, state, lat, lng, language }`.
- Audio starts via `audio.js`. If the URL is `.m3u8`, HLS.js handles it; otherwise native `<audio>`.
- Phase changes to `'playing'`. Globe enters guessing mode.

### Step 3: User Submits a Guess
- User clicks the globe. `Globe.jsx` fires `onGlobeClick`, setting `guess: { lat, lng }` in the store.
- User clicks the Submit button. `submitGuess(globeRef)` runs.
- `calcScore(guess.lat, guess.lng, station.lat, station.lng)` uses the Haversine formula to calculate km distance.
- Score formula: `Math.round(5000 * Math.exp(-10 * km / 20015))`. Perfect guess = 5000, 10,000km away ≈ 7 points.
- Phase changes to `'result'`. Globe reveals a line between guess and actual station location.
- `prefetchNextStation()` fires in the background — fetches the next station silently while the user reads their score.

### Step 4: User Saves Score
- On FinalScreen, user clicks "Save Score".
- If not logged in → `AuthModal` appears.
- After login, `saveScore(totalScore, countries)` sends `POST /api/leaderboard` with `Authorization: Bearer <token>`.
- Server verifies JWT → extracts `userId` → inserts into `game_sessions` table.

---

## 5. Database Schema (Deep Dive)

### 5.1 The SQL

```sql
CREATE TABLE users (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  username   TEXT UNIQUE NOT NULL,
  email      TEXT UNIQUE NOT NULL,
  password   TEXT NOT NULL,          -- bcrypt hash, never plaintext
  created_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE game_sessions (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  total_score INTEGER NOT NULL,
  countries   TEXT[] NOT NULL,       -- array of ISO 3166-1 alpha-2 codes
  played_at   TIMESTAMPTZ DEFAULT now()
);

CREATE INDEX idx_game_sessions_score ON game_sessions(total_score DESC);
```

### 5.2 Design Decisions and Why

**UUID vs auto-increment ID:** UUIDs are generated client-independently, are non-sequential (harder to enumerate), and work across distributed systems. `gen_random_uuid()` is a PostgreSQL built-in.

**One row per game, not per round:** The leaderboard only queries `total_score`. Storing one row per game (with a `countries TEXT[]` array for the 5 countries) is simpler and the query is O(log N) via the index. If we stored one row per round, the leaderboard query would need a `GROUP BY + SUM` which is more expensive.

**`TEXT[]` for countries:** PostgreSQL native array type. Five country codes like `['DE', 'BR', 'JP', 'IN', 'US']` stored natively.

**`ON DELETE CASCADE`:** If a user is deleted, all their game sessions are automatically deleted. Prevents orphaned rows.

**`TIMESTAMPTZ` not `TIMESTAMP`:** Stores timezone-aware timestamps. Always UTC in Postgres. Avoids daylight saving bugs.

**The index:** `CREATE INDEX idx_game_sessions_score ON game_sessions(total_score DESC)`. The leaderboard query is `ORDER BY total_score DESC LIMIT 20`. Without the index, this is a full table scan O(N). With a B-tree index on `total_score DESC`, it's O(log N + 20).

### 5.3 The Leaderboard Query Explained

```sql
SELECT u.username, gs.total_score, gs.countries, gs.played_at
FROM game_sessions gs
JOIN users u ON u.id = gs.user_id
ORDER BY gs.total_score DESC
LIMIT 20;
```

Postgres reads the index from the top, takes 20 game session rows, then does 20 random lookups into the users table to fetch usernames. Total cost: ~20 heap fetches after the index walk.

---

## 6. Redis Caching — Deep Dive

### 6.1 What Redis Is

Redis (Remote Dictionary Server) is an in-memory data store — a separate server process that stores key-value pairs entirely in RAM, making reads and writes sub-millisecond. Unlike a database that writes to disk on every operation, Redis keeps everything in memory and optionally syncs to disk in the background.

### 6.2 Why Redis Over a JS Map

The previous approach used a plain JavaScript `Map` with a manually managed `expiresAt` field. It worked but had three hard limits:

**Per-process:** The Map lives inside the Node.js process. If you run two Express instances behind a load balancer, each has its own Map. A cache hit on instance 1 is a miss on instance 2 — doubling upstream traffic. Redis is external — all instances share one cache.

**Lost on restart:** Every time the server restarts (code change, crash, deploy), the Map is wiped. All cache entries are gone. Redis is persistent — cache survives restarts. In development with nodemon, this means you stop seeing redundant Radio Browser calls every time you save a file.

**No memory bound:** A Map grows unboundedly. Redis has configurable max memory and built-in eviction policies (LRU, LFU, volatile-ttl) — when memory is full, Redis automatically evicts entries based on the policy you configure.

### 6.3 Why Upstash Specifically

Standard Redis requires installing and running a `redis-server` binary. Upstash is Redis-as-a-service over HTTP. The `@upstash/redis` client translates Redis commands into HTTPS requests to Upstash's servers:

```
redis.get('rg:cache:DE:votes:true:40:jazz')
→ GET https://xxxx.upstash.io/get/rg:cache:DE:votes:true:40:jazz
```

No local install. No native bindings. Works on Windows. Free tier covers 100,000 requests/day — well within this project's needs.

### 6.4 How TTL Works in Redis vs the Old Map

**Old Map approach:**
```js
// Write: manually track expiry time
store.set(key, { value, expiresAt: Date.now() + ttlMs });

// Read: manually check clock
if (Date.now() > entry.expiresAt) { store.delete(key); return null; }
```
This is called lazy eviction — stale entries are only deleted when someone tries to read them. A key can sit in memory past its TTL until someone accesses it.

**Redis approach:**
```js
// Write: Redis handles TTL natively
await redis.set(PREFIX + key, JSON.stringify(value), { ex: 600 });

// Read: Redis auto-deletes expired keys — we never see stale data
const raw = await redis.get(PREFIX + key); // null if expired
```
`EX 600` tells Redis to delete this key after 600 seconds. Redis does this with two mechanisms: lazy deletion (check on access) AND active expiry sweeps (Redis periodically scans for expired keys and deletes them). The result: expired keys are guaranteed gone, they don't linger and consume memory.

### 6.5 Key Design and Namespacing

All cache keys are prefixed with `rg:cache:`:
```
rg:cache:DE:votes:true:40:jazz
rg:cache:BR:random:false:0:none
rg:cache:US:clicktrend:true:200:pop
```

The `rg:cache:` prefix is a namespace. If you later add Redis for sessions or rate limiting, their keys (`rg:session:abc123`, `rg:ratelimit:1.2.3.4`) don't collide with cache keys. You can also clear just the cache with `KEYS rg:cache:*` + `DEL` without touching sessions.

The rest of the key encodes the exact Radio Browser query parameters: `country:order:reverse:offset:tag`. Two requests that differ by even one parameter get different cache entries.

### 6.6 Serialization

Redis stores strings only. Station data is an array of objects. On write: `JSON.stringify(data)`. On read: `JSON.parse(raw)`. The `@upstash/redis` client sometimes auto-parses JSON, so `cache.js` guards against double-parsing:
```js
return typeof raw === 'string' ? JSON.parse(raw) : raw;
```

### 6.7 Fire-and-Forget Writes

After fetching from Radio Browser, the cache write is not awaited:
```js
cache.set(cacheKey, data); // no await — fire and forget
return data;               // return to caller immediately
```
This is intentional. The user gets their station data immediately without waiting for the Redis write to confirm. If the Redis write fails, `cache.set()` catches and logs the error internally — the user experience is unaffected (they just get a cache miss next time). This pattern is called a non-blocking write or write-behind — correct for a cache where the source of truth is always the upstream API.

### 6.8 Graceful Degradation

Both `cache.get()` and `cache.set()` catch all Redis errors internally and log a warning instead of throwing:
```js
export async function get(key) {
  try {
    const raw = await redis.get(PREFIX + key);
    ...
  } catch (err) {
    console.warn('[cache] Redis GET failed:', err.message);
    return null; // treat as cache miss — degrade gracefully
  }
}
```
If Upstash goes down, every request is treated as a cache miss and goes directly to Radio Browser. The game keeps working — just slower. This is the correct failure mode for a cache: it should never be in the critical path for correctness.

### 6.9 The /health Endpoint

`cache.size()` now does `redis.keys('rg:cache:*')` to count live entries. This is async — the health endpoint is updated to `async`:
```js
app.get('/api/health', async (req, res) => {
  const redisCacheEntries = await cacheSize();
  res.json({ ..., cacheEntries: redisCacheEntries, cacheBackend: 'redis' });
});
```
In production, `KEYS` is O(N) and should not be called in hot paths. It's fine here because `/health` is only called by monitoring systems, not by players.

---

## 7. Authentication — Deep Dive

### 7.1 Registration Flow
1. Client sends `POST /api/auth/register` with `{ username, email, password }`.
2. Server validates: username is 3-20 chars alphanumeric+underscore, password is 6+ chars.
3. `bcryptjs.hash(password, 10)` — bcrypt generates a random salt, hashes password+salt, returns a 60-char string. The salt is embedded in the hash.
4. `INSERT INTO users` — if username or email already exists, Postgres throws error code `23505` (unique violation). Server catches this and returns 409 Conflict.
5. JWT is signed with `jwt.sign({ sub: userId, username }, JWT_SECRET, { expiresIn: '7d' })`.
6. Token + username returned to client. Client stores token in localStorage.

### 7.2 Login Flow
1. Client sends `POST /api/auth/login` with `{ email, password }`.
2. Server fetches user by email.
3. **Timing attack prevention:** Even if the user doesn't exist, `bcryptjs.compare()` is still called with a dummy hash. This ensures the response time is the same whether the email exists or not.
4. If password matches, JWT is returned.

### 7.3 JWT Structure
```
Header:  { alg: "HS256", typ: "JWT" }
Payload: { sub: "<userId>", username: "<username>", iat: <timestamp>, exp: <timestamp> }
Signature: HMAC-SHA256(base64(header) + "." + base64(payload), JWT_SECRET)
```
The token is stateless — no database lookup needed to verify it.

### 7.4 JWT vs Sessions
| | JWT (what we use) | Server-side Sessions |
|---|---|---|
| Storage | Client (localStorage) | Server (DB/Redis) |
| Scalability | Stateless — works across multiple server instances | Requires shared session store |
| Revocation | Can't invalidate until expiry | Can delete session immediately |
| Payload | Username in token — no DB lookup for auth | Needs DB lookup per request |

---

## 8. Rate Limiting — Deep Dive

### 8.1 The Factory Pattern

```js
function createRateLimiter(windowMs, max, label) {
  const store = new Map(); // ip -> [timestamp, ...]
  return function rateLimit(req, res, next) { ... }
}

const stationLimiter  = createRateLimiter(60_000,     60, 'station');
const loginLimiter    = createRateLimiter(900_000,      5, 'login');
const registerLimiter = createRateLimiter(3_600_000,    3, 'register');
```

Each limiter has its own independent `Map`. A burst of station requests cannot affect the login budget.

### 8.2 Sliding Window Algorithm

**Fixed window problem:** A user can make 60 requests at 0:59 and 60 at 1:00 — 120 in 2 seconds. Sliding window looks at the last N seconds from *now*, preventing this.

```js
while (timestamps.length && timestamps[0] < windowStart) timestamps.shift();
if (timestamps.length >= max) return 429;
timestamps.push(now);
```

**Complexity:** O(n) where n is requests in the window — bounded by `max`, effectively O(1).

| Route | Limit | Window |
|---|---|---|
| `POST /api/auth/login` | 5 attempts | 15 min |
| `POST /api/auth/register` | 3 accounts | 1 hour |
| `GET /api/station` | 60 requests | 1 min |
| `GET /api/auth/me` | unlimited | — |

---

## 9. Station Prefetching — Deep Dive

When `submitGuess()` runs (transitioning to `'result'`), it immediately fires `prefetchNextStation()` in the background. The user spends 5-15 seconds on the result screen. The prefetch completes in 1-2 seconds. When they click Next, `startRound()` finds `prefetchedStation` in state and uses it instantly.

**With Redis cache:** Prefetch hits are even faster than before. If the next station's country batch is already in Redis from a previous round, the prefetch returns in ~20ms (Redis GET latency) rather than 500-2000ms (Radio Browser API). The two systems compound each other.

**Fallback safety:** If prefetch fails, `prefetchedStation` is set to `null`. `startRound()` falls back to a normal fetch. No error surfaces to the user.

---

## 10. Session Management (Game Sessions vs Auth Sessions)

Two completely different systems:

**Game session (in-memory, server-side):** A `Map` keyed by a `sessionId` UUID the browser generates. Stores a `Set<stationUUID>` of stations already seen. Prevents station repetition within a game. Expires after 2 hours. Still in-memory — moving to Redis is the next upgrade step (would allow sessions to survive server restarts and work across multiple instances).

**Auth token (JWT, client-side):** Stored in `localStorage`. Sent in the `Authorization: Bearer` header. 7-day expiry. Stateless — server doesn't store it.

These two are independent. You can play without being logged in. You can be logged in without an active game session.

---

## 11. Score Calculation

```js
function calcScore(guessLat, guessLng, actualLat, actualLng) {
  const R = 6371;
  const dLat = toRad(actualLat - guessLat);
  const dLng = toRad(actualLng - guessLng);
  const a = Math.sin(dLat/2)**2
          + Math.cos(toRad(guessLat)) * Math.cos(toRad(actualLat)) * Math.sin(dLng/2)**2;
  const km = Math.round(R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a)));
  return { km, score: Math.round(5000 * Math.exp(-10 * km / 20015)) };
}
```

**Haversine formula:** Great-circle distance on a sphere. 20,015 km is half Earth's circumference (maximum possible distance).

**Scoring:** `5000 * e^(-10 * km / 20015)` — exponential decay. 0km = 5000pts, 1000km ≈ 2838pts, 5000km ≈ 293pts, 10000km ≈ 7pts.

---

## 12. Audio System

`audio.js` abstracts two backends:
1. **Native `<audio>`:** For standard MP3/AAC streams.
2. **HLS.js:** For `.m3u8` playlist streams (HLS). Chrome and Firefox don't natively support HLS. HLS.js fetches the playlist, downloads chunks, feeds them to the MediaSource API.

---

## 13. Globe Implementation

`Globe.jsx` wraps Globe.gl (which wraps Three.js):
- **GeoJSON:** Country polygon data rendered on the globe surface.
- **Themes:** 4 visual themes by swapping the texture URL.
- **Arcs:** Rendered from guess point to actual station location after submission.
- **Click handler:** `onGlobeClick({ lat, lng })` updates `guess` in the game store.
- **Country highlighting:** TopoJSON client for polygon math on hover.

---

## 14. Alternatives Considered and Rejected

| Decision | What Was Considered | Why Rejected |
|---|---|---|
| Redux instead of Zustand | Redux Toolkit | Too much boilerplate for a single-store app. |
| CSS Modules instead of Tailwind | CSS Modules, styled-components | Context switching between JS and CSS files. |
| MongoDB instead of PostgreSQL | MongoDB Atlas | No joins — relational model is cleaner for leaderboard. |
| ORM (Prisma) instead of raw pg | Prisma, Sequelize | ORM hides SQL — hard to explain schema decisions in interviews. |
| JS Map cache instead of Redis | Plain Map with expiresAt | Per-process, lost on restart, can't share across instances. Redis was implemented. |
| Standard Redis instead of Upstash | ioredis + local redis-server | Requires binary install, native dependencies — problematic on Windows. Upstash is HTTP-based, zero install. |
| Server-side sessions instead of JWT | express-session | Requires a session store. JWT is stateless and scalable. |
| WebSockets for real-time leaderboard | Socket.io | No real-time need — leaderboard is fetched on demand. |

---

## 15. What I Would Do Next (Upgrade Path)

1. **Server-side score validation** — Currently the client sends `totalScore` and the server trusts it. Server-side validation would store each round's station UUID, then re-calculate the score on submission. This closes the cheating vector.

2. **Move game sessions to Redis** — The current session `Map` is in-memory, lost on restart. Moving to Redis (`rg:session:<id>` → JSON array of seen UUIDs) would make sessions persistent and shareable across instances. The `getOrCreateSession()` interface doesn't change — only the backing store.

3. **Redis-backed rate limiting** — The current rate limiter is per-process `Map`. With multiple instances, a user could bypass limits by hitting different servers. Redis sorted sets with `ZADD`/`ZCOUNT` implement a true distributed sliding window.

4. **Daily challenge with deterministic seeding** — Server generates today's stations using a date-based seed. All players see the same stations. Score validation becomes trivial.

5. **Refresh tokens** — Current 7-day JWT can't be revoked. Short-lived access tokens (15min) + long-lived refresh token in httpOnly cookie would allow logout-everywhere.

6. **Cache pre-warming** — On server startup, pre-fetch the top 20 most-played countries into Redis so the first players of the day never see a cache miss.
