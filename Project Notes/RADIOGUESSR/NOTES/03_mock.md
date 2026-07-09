# RadioGuessr — Full Mock Interview (STAR Method)

> Complete technical interview from intro to deep-dive. Study every answer — interviewers at FAANG and fintech will probe any of these threads.

---

## OPENING — "Tell me about yourself"

"I'm a final-year B.Tech CSE student. For my showcase project I built RadioGuessr — a full-stack geography guessing game where you listen to live radio streams from around the world and guess their location on a 3D globe.

What started as a React frontend project became a proper backend engineering exercise. I designed and built a Node.js/Express API server that proxies a third-party radio API, added a Redis caching layer via Upstash, JWT authentication, a PostgreSQL leaderboard with proper schema design and indexing, a sliding window rate limiter I wrote from scratch, and station prefetching to eliminate loading delays.

I'm particularly interested in backend systems and distributed systems concepts — things like caching strategies, API design, and database query optimization. I'd love to talk through the architecture I built."

---

## SECTION 1: Project Overview Questions

---

### Q: "Walk me through your project at a high level."

"RadioGuessr has two main parts.

The frontend is a React app using Vite and Zustand for state management. It renders a 3D interactive globe using Globe.gl — a Three.js wrapper. A player listens to a live radio stream, clicks the globe to place their guess, and sees how far off they were. There are 5 rounds per game, max 25,000 points.

The backend is a Node.js Express server I built from scratch. It sits between the frontend and the Radio Browser API — a public directory of 30,000+ live radio stations. The server adds a Redis caching layer via Upstash, JWT-based auth, a PostgreSQL leaderboard, and a sliding window rate limiter.

The database is PostgreSQL on Supabase — two tables, `users` and `game_sessions`. The cache is Redis on Upstash — an HTTP-based Redis service that works without a local Redis installation. Auth is JWT. I wrote the rate limiter from scratch."

---

### Q: "Why did you build a backend? The game could have worked purely client-side."

**S:** The original project was purely frontend — React hitting the Radio Browser API directly from the browser.

**T:** This created three problems: if Radio Browser rate-limited us or went down, the game died; there was no way to persist scores or have a leaderboard; every user's browser was independently hammering the same upstream API with identical queries.

**A:** I designed an Express proxy server. The frontend calls my API, which checks a Redis cache before hitting Radio Browser. Cache hits are served in ~10ms instead of 500-2000ms. I added JWT auth, a PostgreSQL leaderboard, and rate limiting. The Redis cache is shared — if two players request German stations within the same 10 minutes, only the first one pays the Radio Browser latency.

**R:** Decoupled architecture. If Radio Browser changes their API, I update one file. The leaderboard gives players a reason to come back. And the caching means the app is resilient to Radio Browser being slow or temporarily unavailable.

---

## SECTION 2: Backend Architecture Deep-Dive

---

### Q: "Explain your caching strategy."

**S:** Radio Browser's station search API was being called on every single round start — sometimes multiple times due to retry logic. Each call took 500ms to 2 seconds.

**T:** I needed to reduce upstream API calls, improve latency, and make the app resilient to Radio Browser being slow.

**A:** I implemented a cache-aside pattern backed by Redis via Upstash. The cache key encodes all query parameters: `rg:cache:DE:votes:true:40:jazz`. On a request, I call `await cache.get(key)` — an async HTTP GET to Upstash. Cache hit: return the parsed station array in ~10ms. Cache miss: fetch from Radio Browser, then call `cache.set(key, data)` — which issues a Redis `SET key value EX 600` command. The `EX 600` tells Redis to auto-delete the key after 600 seconds (10 minutes). The write is fire-and-forget — I don't await it, so the station data is returned to the client without waiting for the Redis write to confirm.

**R:** The first user who triggers a particular query pays the Radio Browser latency. Every subsequent user within 10 minutes gets a cache hit — ~10ms from Redis. Terminal shows `[cache] HIT` vs `[cache] MISS` for every request, so cache behavior is observable in real time.

**Follow-up: "Why Redis over a JS Map?"**

I actually started with a plain JS Map with a manually tracked `expiresAt` field. It worked for a single server process, but had three hard limits. First, it's per-process — if you run two Express instances behind a load balancer, each has its own Map, so the same query gets fetched from Radio Browser twice. Redis is external — all instances share one cache. Second, it's lost on server restart — every code change with nodemon wiped the cache. Redis is persistent — cache survives restarts. Third, a Map has no memory bound — it grows until the process runs out of RAM. Redis has configurable max memory with eviction policies.

**Follow-up: "Why Upstash over a local Redis server?"**

Standard Redis requires running `redis-server` as a separate process with native binaries. On Windows this adds installation complexity and potential native dependency issues — I'd already seen that with `bcrypt`. Upstash is Redis-over-HTTP. The `@upstash/redis` client sends standard Redis commands as HTTPS requests to their servers. Zero local install, works on Windows, free tier covers 100k requests/day. The trade-off is a slight latency increase — a local Redis responds in under 1ms, Upstash responds in ~10-20ms over the network. For a 10-minute cached value, that extra 15ms is completely irrelevant.

---

### Q: "How does Redis TTL work? How is it different from what you had before?"

**A:** With the old Map, I stored `{ value, expiresAt: Date.now() + ttlMs }`. On every `get()` call, I checked `Date.now() > entry.expiresAt`. If stale, I deleted the entry and returned null. This is called lazy eviction — the stale entry sits in memory until someone accesses it. A key could be expired but still consuming memory for hours if nobody reads it.

Redis TTL is first-class. The command `SET key value EX 600` atomically stores the value and registers a 600-second timer. Redis handles eviction with two mechanisms: lazy deletion (check expiry on access, same as the Map approach) and active expiry sweeps (Redis periodically scans a random sample of keys with TTLs and deletes expired ones proactively). The result: expired keys are guaranteed to be evicted within a bounded time, not just "eventually when accessed". You never read a stale value from Redis — if the key is expired, it's gone, and `GET` returns null.

---

### Q: "What is fire-and-forget in the context of your cache write?"

**A:** After fetching station data from Radio Browser, I write it to Redis like this:

```js
const data = await fetch(...).then(r => r.json());
cache.set(cacheKey, data); // no await
return data;               // return immediately
```

I don't `await` the `cache.set()`. This means the function returns the data to the caller before the Redis write completes. If the Redis write fails, `cache.set()` catches the error internally and logs a warning — but the caller already has the data, so the user is unaffected.

This is correct because Redis is not the source of truth — Radio Browser is. If a write fails, the only consequence is a cache miss next time. Waiting for the write to confirm would add ~15ms of Upstash network latency to every cache-miss response for zero benefit to the user. This pattern is called a write-behind or non-blocking cache write.

---

### Q: "Explain your rate limiting implementation."

**S:** API endpoints needed protection from abuse — login is a brute-force target, and the station route hits an external API with its own rate limits.

**T:** Different routes need different limits, and they must be independent — a burst of station requests shouldn't consume the login budget.

**A:** I wrote a `createRateLimiter(windowMs, max, label)` factory function. Each call returns an Express middleware with its own `Map` of IP → timestamp array. On each request: drop timestamps older than the window, check count, reject with 429 if over limit.

The algorithm is sliding window, not fixed window. Fixed window allows bursts at boundaries — 60 requests at 0:59, 60 more at 1:00, 120 in 2 seconds. Sliding window looks at the last N seconds from *now*, preventing this.

Limits: login 5/15min, register 3/hour, station 60/min.

**R:** Each surface independently protected. The factory pattern means a station burst can't affect the login budget — different Maps, completely isolated.

**Follow-up: "Why not use an npm package like express-rate-limit?"**

I could have — `express-rate-limit` is well-tested. But writing it from scratch means I can explain every line. In a fintech interview, being able to say "I implemented a sliding window using a sorted timestamp array, O(1) amortized because the array is bounded by max" is more valuable than "I installed a package." The implementation is ~30 lines and straightforward.

---

### Q: "How does your session management work? You have two different types of sessions."

**A:** Right — they're completely different systems.

Game session: when the browser first loads, `api.js` generates a UUID with `crypto.randomUUID()` and stores it in localStorage as `rg_session_id`. Every station request includes this as a query param. The server keeps a `Map` of `sessionId → Set<stationUUID>`. This prevents station repetition within a game — even though station batches are cached and shared across all users, each individual player's seen-UUID set is tracked separately. Sessions expire after 2 hours via a `setInterval` purge.

Auth token: a JWT. Stateless. Stored in localStorage as `rg_auth_token`. Sent in the `Authorization: Bearer` header on protected routes. The server verifies the signature — no database lookup. 7-day expiry.

The two are independent. You play without being logged in. The game session exists regardless. The JWT exists regardless of whether you're playing.

**Follow-up: "Why is the game session still in a Map and not Redis?"**

It's the next upgrade on my list. Currently if the server restarts, all active game sessions are lost — players mid-game might get station repeats. With Redis: `rg:session:abc123` → JSON array of seen UUIDs. The interface — `getOrCreateSession(id)` — doesn't change. It's a one-function swap. I documented this explicitly because it's a known limitation and I want to show I understand the tradeoff.

---

### Q: "Walk me through your database schema design decisions."

**A:** Two tables: `users` and `game_sessions`.

UUID primary keys — non-sequential, harder to enumerate, compatible with distributed ID generation.

`users` has `UNIQUE` constraints on both `email` and `username` — enforced at the database level. Postgres throws error code `23505` on violation, which I catch and return as a 409.

`game_sessions` stores one row per game, not one per round. The leaderboard query only needs `total_score` — `ORDER BY total_score DESC LIMIT 20`. Storing per-round would require `GROUP BY user_id + SUM(score)`, which is more expensive and harder to index. The tradeoff is I can't easily query per-round performance — acceptable for now.

The index: `CREATE INDEX idx_game_sessions_score ON game_sessions(total_score DESC)`. Without it, the leaderboard query is a full table scan O(N). With a B-tree index on `total_score DESC`, Postgres walks the index from the top, takes 20 entries, does 20 heap lookups for the full rows — O(log N + 20). The query cost is essentially constant regardless of table size.

`ON DELETE CASCADE` on the foreign key — deleting a user auto-deletes their game sessions. No orphaned rows.

`TIMESTAMPTZ` not `TIMESTAMP` — timezone-aware, always UTC. Avoids daylight saving issues.

---

### Q: "How does your JWT authentication work?"

**A:** Registration: client sends username, email, password. Server validates, hashes password with bcryptjs at cost factor 10 (~100ms), inserts to DB. On unique violation (error 23505), returns 409. On success, signs `{ sub: userId, username }` with a 7-day JWT.

Login: client sends email and password. Critical detail — I always call `bcryptjs.compare()` even if the user doesn't exist, using a dummy hash. This prevents a timing attack: if I returned immediately on "user not found", that response would be measurably faster than a failed bcrypt comparison. An attacker timing the response could enumerate valid email addresses. Always running bcrypt makes both cases take ~100ms.

Protected routes use `requireAuth` middleware — extracts the Bearer token, calls `jwt.verify()` which re-computes the HMAC-SHA256 signature and checks expiry. Attaches `{ userId, username }` to `req.user`. No database lookup needed for auth.

Main limitation: JWTs can't be revoked before expiry. If compromised, the user stays logged in for up to 7 days. Fix: refresh token pattern — short-lived access tokens (15min) plus a long-lived refresh token in an httpOnly cookie.

---

### Q: "What is prefetching and why did you implement it?"

**S:** Between rounds, the app hit a 'loading' phase where it waited for a station fetch. With cold Redis cache, this was 500ms to 2 seconds.

**T:** Players spend 5-15 seconds on the result screen reading their score. That idle time was being wasted.

**A:** I added `prefetchNextStation()` to the Zustand store. It fires immediately after `submitGuess()` — the moment phase becomes 'result'. It fetches the next station in the background and stores it in `prefetchedStation` state. When the player clicks Next, `startRound()` checks for a prefetched station first — consumes it instantly, skips the network call.

The interaction with Redis is interesting: if the next station's country batch is already warm in Redis, the prefetch returns in ~20ms (Redis GET latency) rather than 500-2000ms. The two systems compound each other.

Fallback: if prefetch fails, `prefetchedStation` is null. `startRound()` falls back to a normal fetch. Zero impact on the user.

**R:** Common case: loading phase is nearly instantaneous. The `station_load_success` analytics event includes `was_prefetched: true/false` so I can measure the actual hit rate.

---

## SECTION 3: Frontend Architecture Questions

---

### Q: "Why Zustand over Redux or Context API?"

**A:** Three reasons.

Redux needs actions, action creators, reducers, and a provider. For one store with a few dozen fields, that's excessive boilerplate. Zustand is one `create()` call.

Context API re-renders every consumer when the value changes. Zustand is selector-based — `useGameStore(state => state.phase)` only re-renders when `phase` changes. For a game with frequent state updates (audio loading, score changes, phase transitions), this matters.

Zustand works outside React components. `prefetchNextStation()` is called from within a Zustand action — it calls `get()` to read state and `fetchStation()` which is a plain async function. Redux would need dispatch and thunks for the same thing.

---

### Q: "How does the audio system handle different stream formats?"

**A:** Radio stations use two main formats: direct streams (MP3, AAC) and HLS (HTTP Live Streaming, `.m3u8` playlists).

Native `<audio>` handles direct streams. But Chrome and Firefox don't natively support HLS — only Safari does. HLS.js intercepts `.m3u8` URLs, fetches the playlist (a list of audio chunk URLs), downloads chunks sequentially, and feeds them to the MediaSource API that `<audio>` can play.

`audio.js` checks: if the URL contains `.m3u8` and `Hls.isSupported()`, use HLS.js. Otherwise native audio. Stream errors trigger the retry callback, which calls `fetchAndPlayReroll()` to try a different station.

---

### Q: "How does the score formula work?"

**A:** Haversine formula for great-circle distance — accounts for Earth's curvature. Euclidean distance would be wrong because longitude degrees represent different physical distances at different latitudes (111km at equator, ~55km at 60°N).

Scoring: `5000 × e^(-10 × km / 20015)`. Exponential decay. 20,015km is half Earth's circumference — the maximum possible distance. At 0km: 5000pts. At 1000km: ~2838pts. At 5000km: ~293pts. At 10000km: ~7pts.

The exponential rewards precision — the difference between 50km and 500km matters more than between 5000km and 6000km. If you know the country, you can get most of the points even without knowing the exact city.

---

## SECTION 4: System Design Questions

---

### Q: "How would you scale this to 100,000 concurrent users?"

**A:** Current architecture handles one server. Scaling requires addressing several bottlenecks:

**Frontend:** Static assets to a CDN (CloudFront, Cloudflare). Removes ~90% of traffic from the server.

**Cache — already Redis:** The cache is already on Upstash Redis, which is external and shared. Adding a second Express instance immediately benefits from the existing cache — no re-architecture needed. This is the main advantage of having moved from Map to Redis.

**Backend — horizontal scaling:** Add a load balancer (nginx, AWS ALB) in front of multiple Express instances. The station route is already effectively stateless for caching purposes. The remaining in-process state is: game sessions (Map) and rate limiters (Map). Game sessions move to Redis — `rg:session:<id>` → JSON. Rate limiters move to Redis sorted sets with atomic operations.

**Database:** Supabase free tier has 25 connections. At scale, tune `pg.Pool` max based on the DB plan's connection limit. Add read replicas for the leaderboard query — it's read-heavy and doesn't need to hit the primary.

**Radio Browser dependency:** At 100k users, pre-warm the Redis cache on startup for the top 50 countries. Increase TTL to 30 minutes — station lists don't change that fast.

---

### Q: "How would you add real-time multiplayer?"

**A:** Two players, same station simultaneously. Scores revealed only after both submit.

Architecture: WebSockets via Socket.io. Player A creates a room → server generates a room code. Player B joins → server matches them.

Room state: `{ roomId, players: [A, B], station, submissions: {A: null, B: null} }`. This lives in Redis — not in-process memory — so any server instance can handle any player in the room.

The station must be the same for both — server broadcasts it to both clients via `io.to(roomId).emit('station', station)` simultaneously. Clients don't request independently.

Edge case: one player submits, other disconnects. Set a 60-second timeout after first submission — if second player hasn't submitted, auto-submit with their last known guess position or null.

---

### Q: "Design a daily challenge."

**A:** Every day, all players play the same 5 stations. Scores compared on a shared daily leaderboard.

**Station generation:** Server computes today's stations using the current date as a seed: `seed = "2024-01-15"` → deterministic RNG → 5 country + station index selections. Idempotent — same result called 100 times on the same day.

**Why server-side:** Client-side seeding is cheatable. Anyone can reverse the seed and look up stations before playing. Server controls the seed and never exposes it.

**Score validation:** Server knows the correct stations. Client submits `[{ roundIndex, guessLat, guessLng }]`. Server re-calculates Haversine distance and score for each round. Client can't inflate scores.

**Cache the daily stations in Redis:** `rg:daily:2024-01-15` → JSON array of 5 stations. TTL = seconds until midnight. First request computes and caches; all subsequent requests are cache hits.

**Schema addition:** `daily_submissions` table — `(user_id, date, total_score)` with `UNIQUE(user_id, date)` preventing double submission.

---

## SECTION 5: Behavioral / STAR Questions

---

### Q: "Tell me about a technical challenge you faced."

**S:** After adding the Express backend, the server was crashing mid-request when users tried to register, producing an `ECONNRESET` error on the frontend.

**T:** The error was hard to debug because the server restarted before I could read the full stack trace. The frontend received `Unexpected end of JSON input` because the response was cut off mid-stream.

**A:** I narrowed it down by adding logs before and after each operation in the register route. The crash happened during `bcrypt.hash()`. Then I realized: `node --watch` was restarting the server whenever bcrypt touched internal temp files during the 100ms hashing operation. The server died mid-response.

Two fixes: first, switched from `node --watch` to `nodemon --watch server` — only watches my `server/` directory, ignores bcrypt's temp files. Second, switched from `bcrypt` (native C++ bindings, can have Windows compatibility issues) to `bcryptjs` (pure JavaScript, identical API, zero native dependencies).

**R:** Registration and login worked reliably. Lesson: development tooling can introduce non-obvious bugs that look like application logic failures.

---

### Q: "Tell me about a design decision you'd change in hindsight."

**S:** I originally built the cache using a plain JavaScript Map with a manually tracked `expiresAt` field.

**T:** It worked for a single process, but I knew from the start it had scaling limitations — per-process, lost on restart, no memory bound.

**A:** I moved to Redis (Upstash) later in the project. Looking back, I should have started with Redis. The interface — `cache.get(key)`, `cache.set(key, value)` — was always abstracted behind `cache.js`, so the swap was a one-file change. But during development, I was seeing redundant Radio Browser calls every time nodemon restarted. With Redis, cache would have survived those restarts from day one.

The lesson: when you know a component will eventually need to be an external service (cache, sessions, rate limiting), design the interface abstractly from the start. I did that — the `cache.js` module had a clean interface. But I could have started with the right backing store immediately.

**R:** The migration was clean because of the abstraction. The project now has Redis in production. The game session store is next — same pattern.

---

### Q: "How did you ensure your backend was secure?"

**A:** Several layers:

Password security: bcryptjs, cost factor 10. Hash embeds the salt — no separate salt column.

Timing attack prevention: Login always runs `bcryptjs.compare()` even if the email doesn't exist — dummy hash prevents response time leaking whether an email is registered.

SQL injection: exclusively parameterized queries via `pg` — `query('... WHERE email = $1', [email])`. User input never enters the SQL string.

Rate limiting: login 5/15min, register 3/hr. Stops brute-force and spam.

JWT: signed with a secret from environment variables, never hardcoded. `requireAuth` middleware verifies signature on every protected route.

Environment variables: `.env` in `.gitignore`. Never committed.

CORS: origin allowlist — only `localhost:5173` in dev and the production domain. No wildcard.

Redis keys prefixed with `rg:cache:` — namespace isolation in case Redis is shared with other features.

---

### Q: "What would you do differently for a production fintech company?"

**A:** Several things.

httpOnly cookies instead of localStorage for the JWT. localStorage is accessible to any JS on the page — XSS steals the token. httpOnly cookies are inaccessible to JS, automatically sent by the browser. Pair with a refresh token pattern: short-lived access token (15min) + long-lived refresh token in httpOnly cookie.

Proper secrets management. AWS Secrets Manager or HashiCorp Vault instead of `.env`. Secrets rotate, are audited, never appear in logs.

Server-side score validation. The client currently sends `totalScore` and the server trusts it. In fintech: store station UUID and correct coordinates when the round starts, re-calculate on submission. Never trust the client.

Redis-backed rate limiting. Current rate limiter is per-process Map — bypassed by hitting different instances. Redis sorted sets with `ZADD`/`ZCOUNT` implement a true distributed sliding window.

Audit logging. Every auth event — login, failed login, logout, password change — to an immutable log with timestamp, IP, user agent. Required for PCI-DSS, SOC 2.

Content Security Policy headers to prevent XSS script injection.

---

## SECTION 6: Language and Framework Questions

---

### Q: "Why Node.js for the backend?"

**A:** Three reasons: same language as the frontend (no context switching), excellent async I/O model (the backend makes many concurrent HTTP calls to Radio Browser and Upstash — Node's event loop handles this without thread-per-request overhead), and the npm ecosystem.

One limitation: bcrypt hashing blocks Node's event loop for ~100ms. In Go, that runs in a separate goroutine without blocking other requests. For this project, fine. At 10,000 req/s with bcrypt in the hot path, I'd look at Go or move bcrypt to a worker thread.

---

### Q: "Explain the Haversine formula."

**A:** Euclidean distance assumes a flat plane. Lat/lng are angles — at the equator, 1° longitude is ~111km; at 60°N, it's ~55km. Euclidean would treat them the same.

Haversine calculates the angle between two points on a sphere using `hav(θ) = sin²(θ/2)`, applied to the difference in latitudes and longitudes adjusted by the cosines of the latitudes (to account for longitude convergence at the poles). Multiply by Earth's radius (6,371km) to get kilometers.

Accurate to ~0.5% for Earth's actual oblate spheroid shape. More than sufficient for a game.

---

## CLOSING QUESTIONS — Ask These

1. "What does your caching strategy look like in production — Redis, Memcached, or something else? And how do you handle cache invalidation for user-facing data?"

2. "When you're reviewing backend code, what patterns raise the most concern — is it more around correctness, performance, or security?"

3. "What's the biggest technical challenge the team is actively working on that someone joining would contribute to early?"

4. "How do you handle database schema migrations in production? Zero-downtime deploys with schema changes is something I've been thinking about."

---

## QUICK REFERENCE — Numbers to Remember

| Fact | Value |
|---|---|
| Max score per round | 5,000 |
| Max total score | 25,000 |
| Earth's circumference (half) | 20,015 km |
| Cache TTL (Redis EX) | 600 seconds (10 minutes) |
| Cache backend | Redis via Upstash (HTTP) |
| Cache key prefix | `rg:cache:` |
| Cache write strategy | Fire-and-forget (no await) |
| Redis GET latency (Upstash) | ~10-20ms |
| Station batch size | 20 per fetch |
| Retry attempts (server) | 5 |
| Retry attempts (client) | 3 |
| Login rate limit | 5 per 15 minutes |
| Register rate limit | 3 per hour |
| Station rate limit | 60 per minute |
| JWT expiry | 7 days |
| Session expiry (game) | 2 hours |
| Session cleanup interval | 30 minutes |
| bcrypt cost factor | 10 (~100ms per hash) |
| Connection pool max | 10 |
| Leaderboard top N | 20 |
| Countries in pool | ~160 |
| Total Radio Browser stations | ~30,000+ |
| Upstash free tier | 100,000 requests/day |
