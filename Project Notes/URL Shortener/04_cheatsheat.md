# 04 — Cheat Sheet: URL Shortener Project
### Read This the Night Before. Read It Again the Morning Of.

> **How to use:** This is not for learning — it is for recall. If anything here surprises you, go back to the relevant section in `01-knowledge-base.md` or `03-mock-interview.md` and re-read it. Everything here should feel familiar, not new.

---

## THE OPENING — Say This Out Loud Right Now

> *"I built a full-stack URL shortener — React frontend, plain Java HTTP server on the backend with no frameworks, and SQLite for persistent storage. I deliberately avoided Spring Boot and used only the JDK's built-in HttpServer, which forced me to handle HTTP, CORS, concurrency, and database access manually. The features are: URL shortening with random Base62 codes, optional custom aliases with full validation, TTL-based expiry, QR code generation, and persistent storage that survives restarts. It's intentionally simple enough that I can explain every single line — but the system design surface area is deep."*

Practice until this comes out naturally without reading it.

---

## SECTION 1 — Project in 30 Seconds

| What | Detail |
|---|---|
| **What it does** | Long URL in → short code out → redirect on visit |
| **Frontend** | React 18 + Vite, single component `App.jsx` |
| **Backend** | Java 17, `com.sun.net.httpserver.HttpServer`, port 8080 |
| **Database** | SQLite via JDBC, file `urls.db`, one table: `urls` |
| **QR** | Google ZXing library, generates PNG from short URL |
| **No frameworks** | No Spring Boot, no Express, no Servlet |
| **Proxy** | Vite forwards `/shorten`, `/r`, `/all`, `/qr` to `:8080` |

---

## SECTION 2 — The 4 Endpoints (Know Cold)

| Method | Endpoint | Handler | Returns | Key Logic |
|---|---|---|---|---|
| `POST` | `/shorten` | `ShortenHandler` | `200` JSON with shortUrl | Validate → generate code → INSERT |
| `GET` | `/r/{code or alias}` | `RedirectHandler` | `302` redirect | Lookup → check expiry → Location header |
| `GET` | `/all` | `ListHandler` | `200` JSON array | `WHERE expires_at > now AND deleted = 0` |
| `GET` | `/qr?url=...` | `QRHandler` | `200` image/png | URLDecode param → ZXing → PNG bytes |

---

## SECTION 3 — Database Schema (One Line Each)

```sql
CREATE TABLE urls (
    code         TEXT PRIMARY KEY,    -- 6-char Base62, auto-indexed, never null
    alias        TEXT UNIQUE,         -- optional, nullable, UNIQUE allows multiple NULLs
    original_url TEXT NOT NULL,       -- the destination URL
    created_at   INTEGER NOT NULL,    -- Unix epoch seconds at insert time
    expires_at   INTEGER NOT NULL,    -- created_at + (ttlDays × 86400)
    deleted      INTEGER DEFAULT 0    -- soft delete: 0=active, 1=deleted
);
CREATE INDEX idx_alias ON urls(alias); -- B-tree, O(log n) alias lookups
```

**3 things to know about this schema:**
1. `alias TEXT UNIQUE` — nullable UNIQUE because SQL NULL ≠ NULL (each NULL is distinct)
2. Timestamps as INTEGER — no timezone issues, single comparison for expiry check
3. `deleted` not hard delete — keeps alias permanently claimed after expiry

---

## SECTION 4 — The 8 Design Decisions (One Line Each)

Memorise these. Every "why did you..." question maps to one of these.

| Decision | One-Line Reason |
|---|---|
| No Spring Boot | Forces manual HTTP understanding; every line is explainable |
| SQLite over MySQL | Zero setup; identical JDBC interface — change one line to switch |
| UNIQUE constraint for aliases | DB enforces uniqueness atomically; handles race conditions correctly |
| Soft delete (deleted flag) | Aliases are never reusable after expiry — prevents hijacking |
| 302 not 301 redirect | 302 hits server every time; enforces TTL on every visit |
| 410 not 404 for expired | 410 = "existed but gone"; 404 = "never existed" — semantically correct |
| Unix INTEGER timestamps | Simple integer comparison; no timezone ambiguity; no parsing |
| QR from short URL, not original | Shorter string = simpler QR matrix; QR inherits TTL behaviour |

---

## SECTION 5 — The Race Condition Answer

This will almost certainly be asked. Say it confidently.

> *"For alias creation, the naive approach is check-then-insert: SELECT to see if alias exists, then INSERT if it doesn't. This has a race condition — two concurrent threads can both pass the SELECT check before either does the INSERT, resulting in a duplicate. My fix: skip the SELECT entirely. Go straight to INSERT. The UNIQUE constraint on the alias column means only one INSERT can succeed. The other gets a SQLException with 'UNIQUE constraint failed: urls.alias' in the message. I catch that and return 409 Conflict. The database handles concurrent writes atomically — it's more reliable than any application-level check."*

---

## SECTION 6 — The Scaling Answer

When asked "how would you scale to 100k / 1M users":

**Step 1 — Cache redirects with Redis**
```
GET /r/code → check Redis first (~0.2ms hit) → miss: query DB, cache result
Redis TTL = URL's expires_at - now (auto-expires with the URL)
```

**Step 2 — Replace SQLite with PostgreSQL**
```
Change one line: DB_URL = "jdbc:postgresql://host/dbname"
All PreparedStatement / ResultSet code stays identical
```

**Step 3 — Load balancer + multiple Java instances**
```
Nginx → [Java :8080] [Java :8081] [Java :8082]
All stateless — state lives in PostgreSQL + Redis
```

**Step 4 — Code generation at scale**
```
Current: random Base62 + collision check (fine up to ~100M URLs)
At scale: auto-increment DB ID → Base62 encode → zero collision, no check needed
```

---

## SECTION 7 — Short Code Algorithm

```
chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
         └─ 26 lower ──────────────────┘└─ 26 upper ──────────────────┘└─ 10 ─┘
                                    = 62 characters

For i = 0 to 5:
    code += chars[Math.random() × 62]

Total combinations: 62^6 = 56,800,235,584 (~56 billion)

do { code = generateCode(); } while (Database.codeExists(code));
```

**Why Base62 not Base64?** Base64 adds `+` and `/` which are URL-unsafe characters.
**Why not MD5?** MD5 hex is base-16 → only 16^6 = 16 million combinations. Much worse.
**Production upgrade:** DB auto-increment ID → Base62 encode. Zero collision, no check needed.

---

## SECTION 8 — TTL / Expiry Flow

```
INSERT time:
  expiresAt = Instant.now().getEpochSecond() + (ttlDays × 86400)
  stored as INTEGER in DB

Redirect time (every single visit):
  if (Instant.now().getEpochSecond() > rs.getLong("expires_at"))
      return 410 Gone

List time (/all endpoint):
  WHERE expires_at > {now} AND deleted = 0
  → only active URLs returned to frontend
```

**TTL options:** 1, 7, 30, 90, 365 days. Default: 7 days.
**Row after expiry:** stays in DB forever (soft expiry). Alias permanently claimed.

---

## SECTION 9 — Alias Validation Pipeline

Every alias goes through this in order. Know the sequence:

```
1. Null/blank? → skip, use random code
2. trim().toLowerCase() → "My-Link" becomes "my-link"
3. Length 3–30? → else 400
4. Regex [a-z0-9][a-z0-9\-]*[a-z0-9]? → else 400
   (letters, digits, hyphens only; no leading/trailing hyphens)
5. In RESERVED_WORDS? (all, r, shorten, api, admin, login, qr...) → else 400
6. In BLOCKED_WORDS? (google, facebook, amazon...) → else 400
7. INSERT → UNIQUE constraint → else 409 "alias already taken"
```

**Key insight:** Step 7 is the race condition guard. Steps 1-6 are fast string ops. DB is the final authority.

---

## SECTION 10 — CORS in One Paragraph

> *"CORS — Cross-Origin Resource Sharing — is a browser security mechanism. Our React app on port 5173 and our Java server on port 8080 are different origins. Without permission, browsers block responses from different origins. For POST requests with JSON, the browser first sends a preflight OPTIONS request asking for permission. Our server responds with `Access-Control-Allow-Origin: *`, `Allow-Methods: GET, POST, OPTIONS`, `Allow-Headers: Content-Type`. The browser proceeds with the actual request. I handle OPTIONS in every handler. Important: CORS is browser-only — curl and server-to-server calls don't enforce it. It's a browser protection, not an authentication mechanism."*

---

## SECTION 11 — HTTP Status Codes Used (Know All 8)

| Code | Name | When |
|---|---|---|
| `200` | OK | Successful shorten / list / QR |
| `302` | Found (Temp Redirect) | Redirect to original URL |
| `400` | Bad Request | Invalid URL, bad alias format, bad TTL |
| `404` | Not Found | Code/alias never existed |
| `405` | Method Not Allowed | Wrong HTTP method |
| `409` | Conflict | Alias already taken |
| `410` | Gone | URL existed but has expired |
| `500` | Internal Server Error | DB error / unexpected exception |

**301 vs 302:** 301 = permanent, browsers cache it (bypasses expiry check). 302 = temporary, every visit hits server (enforces TTL). We use 302.
**404 vs 410:** 404 = never existed. 410 = existed but gone. We use 410 for expired.

---

## SECTION 12 — Java Concepts (Quick Answers)

**PreparedStatement vs String concat:**
> Parameterised query. User input is never interpreted as SQL. Prevents SQL injection. Also faster — DB caches the query plan.

**try-with-resources:**
> Automatically closes `Connection` and `PreparedStatement` when block exits, even on exception. Prevents connection leaks.

**`Class.forName("org.sqlite.JDBC")`:**
> Explicitly loads the SQLite JDBC driver. Registers it with DriverManager via static initialiser. Without it: "No suitable driver found" error.

**`Instant.now().getEpochSecond()`:**
> Modern Java time API. Returns Unix seconds since epoch. No timezone issues. Maps directly to SQLite INTEGER.

**StringBuilder vs String:**
> String is immutable — concatenation in a loop creates N intermediate objects, O(n²). StringBuilder is mutable — O(n). Used in ListHandler for building JSON array.

**`newFixedThreadPool(4)`:**
> Without executor, HttpServer is single-threaded. Thread pool allows 4 concurrent requests. Each incoming request gets a thread from the pool.

---

## SECTION 13 — File Structure (One Line Each)

```
Main.java              → starts server, registers 4 handlers, sets thread pool
db/Database.java       → init(), getConnection(), codeExists()
handlers/
  ShortenHandler.java  → POST /shorten: validate, generate, INSERT
  RedirectHandler.java → GET /r/{x}: lookup, check expiry, 302 or 410
  ListHandler.java     → GET /all: active URLs as JSON array
  QRHandler.java       → GET /qr?url=...: ZXing PNG generation
util/
  Validator.java       → isValidUrl(), validateAlias() — pure logic, no HTTP/DB
  JsonUtil.java        → sendJson(), sendImage(), sendCors(), extractJsonValue()
App.jsx                → entire frontend: form, history table, QR modal
vite.config.js         → proxy /shorten /r /all /qr to localhost:8080
```

---

## SECTION 14 — Numbers Table (Memorise These)

### Project Numbers

| What | Number |
|---|---|
| Code length | 6 characters |
| Total code combinations | ~56 billion (62^6) |
| Charset size | 62 (26+26+10) |
| Default TTL | 7 days = 604,800 seconds |
| TTL range | 1–365 days |
| Alias length | 3–30 characters |
| Thread pool | 4 threads |
| QR image size | 300 × 300 px |
| QR error correction | Level M = 15% damage tolerance |
| DB table count | 1 (urls) |
| DB column count | 6 |
| Endpoints | 4 (/shorten, /r/, /all, /qr) |
| Handler classes | 4 |
| Utility classes | 2 |

### General Numbers

| What | Number |
|---|---|
| Redis latency (cache hit) | ~0.1–0.2ms |
| SQLite read latency | ~1–5ms |
| B-tree lookup (100k rows) | ~17 comparisons |
| B-tree lookup (1M rows) | ~20 comparisons |
| Base62 vs MD5 hex | 56B vs 16M combinations |
| 302 vs 301 | Temporary (check server) vs Permanent (browser caches) |
| Unix epoch ~2026 | ~1,780,000,000 |

### Complexities

| Operation | Complexity |
|---|---|
| Code generation | O(1) — 6 chars, constant |
| Collision check / alias lookup | O(log n) — B-tree index |
| Reserved word check | O(1) — HashSet |
| INSERT with UNIQUE | O(log n) — B-tree traversal |
| List active URLs | O(m) — m = active URL count |
| JSON building (list) | O(m) |
| QR generation | O(1) — fixed 300×300 size |

---

## SECTION 15 — What You'd Do Differently (3 Honest Answers)

Have these ready — they're almost always asked.

1. **Configurable base URL** — `localhost:8080` is hardcoded. Should be `System.getenv("BASE_URL")` with localhost fallback. Breaks QR in production otherwise.

2. **Connection pool from day one** — HikariCP, 5 lines of config. Opening a new JDBC connection per request has overhead. Fine for dev, bad habit.

3. **`SecureRandom` instead of `Math.random()`** — `Math.random()` is a PRNG, theoretically predictable. `SecureRandom` is one line change, proper for any system generating tokens/codes.

---

## SECTION 16 — What's NOT in This Project (and Why)

Be ready to explain intentional omissions. Own them — don't apologise.

| Missing Feature | Why Intentionally Skipped | What You'd Add |
|---|---|---|
| Authentication | Would shift focus away from core shortening mechanics | JWT tokens, `users` table, `user_id` FK on urls |
| Rate limiting | Adds infra complexity (Redis or in-memory state) | Token bucket per IP using ConcurrentHashMap or Redis INCR |
| URL content scanning | Needs external API call in critical path | Google Safe Browsing API check before INSERT |
| HTTPS | Needs SSL cert; deploy concern not code concern | Nginx with Let's Encrypt in front |
| Click analytics | Write on every redirect = hot path contention | Async queue + background thread, or Kafka at scale |
| Alias updates | Adds broken-link chain complexity | Deliberate choice: not supported |
| Expired row cleanup | Not needed for correctness | ScheduledExecutorService running nightly DELETE |
| Build tool (Maven/Gradle) | Only 3 JARs; manual classpath is clearer | Maven when dependency count grows |
| Unit tests | Kept scope minimal | JUnit for Validator, integration tests for Database |

---

## SECTION 17 — One-Line Answers for Rapid-Fire Questions

| Question | One-Line Answer |
|---|---|
| Why no Spring Boot? | Forces manual HTTP handling; every line is explainable |
| Why SQLite? | Zero setup; identical JDBC to MySQL/Postgres |
| Why 302 not 301? | 301 is cached by browser; bypasses TTL check |
| Why 410 not 404? | 410 = existed and is gone; 404 = never existed |
| Why soft delete? | Keeps alias permanently claimed; prevents hijacking |
| How do you handle race conditions? | DB UNIQUE constraint; skip SELECT, straight to INSERT |
| What happens if code collides? | do-while retries; probability 1 in 56 billion per attempt |
| Why Base62 not Base64? | Base64's + and / are URL-unsafe characters |
| Why store timestamps as integers? | No timezone issues; single integer comparison for expiry |
| What is CORS? | Browser security that prevents cross-origin JS requests without server permission |
| What is SQL injection? | User input interpreted as SQL; prevented by PreparedStatement |
| Is HashMap thread-safe? | No; I replaced it with SQLite which handles concurrency correctly |
| Why QR from short URL? | Shorter string = simpler QR; QR inherits TTL |
| Why is alias nullable UNIQUE? | SQL UNIQUE allows multiple NULLs; NULL means no alias, not duplicate |
| What is a B-tree index? | Sorted tree structure; turns O(n) scan into O(log n) lookup |
| How does redirect work? | Server returns 302 + Location header; browser follows automatically |
| What is try-with-resources? | Auto-closes JDBC resources; prevents connection leaks |
| What is PreparedStatement? | Parameterised SQL; prevents injection; caches query plan |

---

## SECTION 18 — 5 Questions to Ask the Interviewer

Pick 2. Ask them after they say "do you have any questions for us?"

1. *"What does the backend stack look like here, and how much greenfield work vs legacy maintenance is typical?"*
2. *"How does code review work — is it PR-based, and how thorough does it tend to be?"*
3. *"What does the first 3 months look like for a new engineer joining the team?"*
4. *"Is there anything about how I described this project that you'd have approached differently? I'd genuinely value that feedback."* ← Best one. Use this.
5. *"What separates a strong new-grad hire from an average one in your experience?"*

---

## SECTION 19 — Day-Of Checklist

### The night before:
- [ ] Read this entire cheat sheet once slowly
- [ ] Say the opening paragraph (Section 1) out loud 3 times
- [ ] Say the race condition answer (Section 5) out loud
- [ ] Say the scaling answer (Section 6) out loud
- [ ] Re-read any section that felt unfamiliar
- [ ] Commit and push latest code to GitHub — check the README renders properly

### Morning of:
- [ ] Read Sections 1–8 only (quick refresh, 10 minutes)
- [ ] Start the backend: `java -cp ".;../lib/..." Main`
- [ ] Start the frontend: `npm run dev`
- [ ] Shorten one URL, use a custom alias, try a reserved word (get the 400), open the QR code
- [ ] Visit the short URL — confirm the redirect works
- [ ] Check `localhost:5173` looks clean
- [ ] Close it. You're ready.

### In the interview room:
- [ ] Speak slowly. You know this better than you think.
- [ ] When asked something you're not sure about — say "let me think through that" — pause — then answer
- [ ] Connect every answer to your project: *"In my project I did X, because Y, and if I scaled it I would Z"*
- [ ] If they go somewhere deep you don't know — say "I haven't implemented that, but I know the approach would be..." and describe it conceptually
- [ ] Ask one of the Section 18 questions at the end

---

## SECTION 20 — The One Paragraph That Covers Everything

Read this out loud. This is your anchor. When nerves hit, come back to this mental model.

> *"A short URL is a key-value lookup. The key is the code or alias, the value is the original URL. Every system design decision flows from this: I need to store the mapping (SQLite), I need to look it up fast (B-tree index), I need to handle multiple people trying to claim the same key simultaneously (UNIQUE constraint), I need keys to expire (TTL integer comparison on every lookup), and I need to generate keys that are short, URL-safe, and hard to guess (Base62, 6 characters, 56 billion combinations). Everything else — CORS, thread pool, QR codes, Vite proxy — is infrastructure supporting this core lookup. That's the entire project."*

---

*Good luck. You know this project. Trust that.*