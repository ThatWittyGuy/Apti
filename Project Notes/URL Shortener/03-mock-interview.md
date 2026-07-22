# 03 — Mock Interview: URL Shortener Project
### Complete Technical Interview Simulation — STAR Format — Campus Placements

> **How to use this document:** Read the question, cover the answer, try answering out loud, then compare. Do this daily for 3 days before an interview. The goal is not memorisation — it is internalisation. You should be able to answer in your own words, not recite this verbatim.

---

## Table of Contents

1. [Opening Questions](#1-opening-questions)
2. [Project Overview Questions](#2-project-overview-questions)
3. [Backend Architecture Deep-Dive](#3-backend-architecture-deep-dive)
4. [Database Questions](#4-database-questions)
5. [Algorithm and Logic Questions](#5-algorithm-and-logic-questions)
6. [Concurrency and Thread Safety](#6-concurrency-and-thread-safety)
7. [Frontend Questions](#7-frontend-questions)
8. [System Design — Scaling Questions](#8-system-design--scaling-questions)
9. [Security Questions](#9-security-questions)
10. [Java Language Questions](#10-java-language-questions)
11. [Behavioral / STAR Questions](#11-behavioral--star-questions)
12. [Closing — Questions to Ask the Interviewer](#12-closing--questions-to-ask-the-interviewer)
13. [Quick Reference — Numbers to Memorise](#13-quick-reference--numbers-to-memorise)

---

## 1. Opening Questions

---

### Q: "Tell me about yourself."

**Answer:**

"I'm a final-year computer science student at [your college]. I enjoy backend development — specifically understanding how systems work at a fundamental level rather than relying on frameworks to abstract everything away.

For my most recent project, I built a URL shortener from scratch — a React frontend talking to a plain Java HTTP server with SQLite as the database. I deliberately avoided Spring Boot and used only the JDK's built-in HTTP server, which forced me to handle things like CORS, request routing, response headers, and concurrency manually. That hands-on experience gave me a much clearer understanding of how HTTP actually works under the hood.

Before that I had worked on more complex distributed systems projects, but I found that going back to fundamentals and being able to explain every single line of code is more valuable than having a flashy project I can't fully defend. I'm looking for a role where I can grow as a backend engineer, write clean maintainable code, and gradually take on more complex systems."

---

### Q: "Walk me through your resume."

**Answer:**

"Sure. My strongest project is the URL shortener I mentioned, which I'd love to discuss in detail — it covers HTTP, databases, concurrency, and system design in a compact package.

In terms of coursework, I've done data structures and algorithms, operating systems, and database management systems, which gave me the foundation for the decisions I made in this project — things like why I used a B-tree index, why I chose an integer for timestamps, and how to think about concurrent access.

I'm comfortable with Java as a backend language, and I've worked with React on the frontend. I'm familiar with SQL — both the query language and the engine-level concepts like indexes and constraints."

---

## 2. Project Overview Questions

---

### Q: "Tell me about this URL shortener project. What does it do?"

**Answer:**

"It's a full-stack URL shortening service. A user pastes a long URL, optionally sets a custom alias and an expiry period, and gets back a short URL. When someone visits the short URL, the server looks up the original URL in a database and issues an HTTP redirect.

The stack is React on the frontend, a plain Java HTTP server on the backend — no Spring Boot, using only `com.sun.net.httpserver` which ships with the JDK — and SQLite for persistent storage via JDBC. I also added QR code generation using Google's ZXing library.

The key features beyond basic shortening are: custom aliases with validation, TTL-based expiry where URLs stop working after a chosen period, and QR codes generated from the short URL so they stay compact and also inherit the TTL behaviour."

---

### Q: "Why did you build this instead of something more complex?"

**Answer:**

"Intentional decision. I had previously built a collaborative CRDT-based text editor, which is a genuinely complex distributed systems project. But I found that in interviews, every question about it led to follow-up questions I wasn't fully prepared for — CRDT merge algorithms, vector clocks, consistency models. I couldn't own it completely.

With a URL shortener, every single line of code is mine and I can explain every decision. But the surface area for system design questions is enormous — hashing, databases, caching, scaling, redirects, concurrency, TTL. It's a project that looks simple but rewards deep thinking. That's exactly what I wanted."

---

### Q: "What are the core features of your project?"

**Answer:**

"Five core features:

First, basic URL shortening — a long URL goes in, a 6-character Base62 code comes out, stored in SQLite.

Second, custom aliases — users can choose their own short code like 'my-link' instead of a random one. This required full validation: format checking, reserved word blocking, brand name blocking, and race condition handling via a database UNIQUE constraint.

Third, TTL-based expiry — every URL has an expiry timestamp stored as a Unix integer. On every redirect, the server checks whether the current time has passed the expiry. Expired URLs return HTTP 410 Gone, not 404.

Fourth, QR code generation — clicking a QR button generates a PNG from the short URL using Google's ZXing library and displays it in a modal. Importantly, the QR encodes the short URL, not the original, so it stays compact and inherits the TTL.

Fifth, persistent storage — everything is in SQLite, so data survives server restarts. Before switching to SQLite I had a HashMap which lost all data on restart."

---

### Q: "What was the most technically interesting part to build?"

**Answer:**

"The alias system, specifically the race condition handling. My first instinct was to check if an alias exists before inserting:

```
SELECT 1 WHERE alias = 'promo' → if empty, INSERT
```

But this has a race condition — two concurrent requests could both see the alias as free and both attempt the insert, resulting in duplicate data or one silently overwriting the other.

The correct solution is to skip the existence check entirely and go straight to INSERT, relying on the UNIQUE constraint on the alias column. If two threads race, exactly one INSERT wins. The other gets a `SQLException` with 'UNIQUE constraint failed: urls.alias' in the message. I catch that specific exception and return HTTP 409 Conflict.

The database's storage engine handles concurrent writes correctly at the file level. Trying to replicate that logic in the application layer with locks or checks is both slower and less reliable. The database is the single source of truth — always."

---

### Q: "What would you do differently if you rebuilt this?"

**Answer:**

"Three things.

First, I would make the base URL configurable via an environment variable from day one. Right now `localhost:8080` is hardcoded in the URL construction — fine for development, but it breaks QR codes when the app is deployed because QR codes encode a localhost URL that phones can't reach.

Second, I would add a connection pool from the start. Right now I open a new JDBC connection for every request. Connections have overhead — file open, driver initialisation. HikariCP is the standard Java connection pool and would eliminate that overhead with minimal code change.

Third, I would use `SecureRandom` instead of `Math.random()` for code generation. `Math.random()` uses a pseudo-random number generator which is theoretically predictable given the seed. For a URL shortener it's not a real security issue, but using `SecureRandom` is a better habit and a one-line change."

---

## 3. Backend Architecture Deep-Dive

---

### Q: "Why didn't you use Spring Boot?"

**Answer:**

"Two reasons — one practical, one educational.

Practically: Spring Boot is fantastic for production systems, but it hides an enormous amount of what's actually happening. Annotations like `@RestController`, `@GetMapping`, `@CrossOrigin` abstract away the HTTP layer. If I used them and you asked me what actually happens when a request comes in, I'd have to say 'Spring handles it.' That's not a great interview answer.

Educationally: by using `com.sun.net.httpserver` — which is in the JDK — I had to write every piece manually. CORS headers, request method checking, reading the body bytes, writing the response, setting the content-length. Every piece is visible and explainable. I understand HTTP because I had to handle it, not because a framework hid it from me.

In a real production job, I would absolutely use Spring Boot — it's battle-tested and significantly more capable. But for learning and demonstrating fundamentals, raw HttpServer was the right choice."

---

### Q: "Walk me through what happens when the server starts."

**Answer:**

"In `Main.java`'s `main` method, five things happen in sequence.

First, `Class.forName('org.sqlite.JDBC')` — this explicitly loads the SQLite JDBC driver class into the JVM. JDBC 4.0+ supports auto-loading via ServiceLoader, but explicit loading is more reliable across different classpath configurations. It must happen before any database connection is attempted.

Second, `Database.init()` — opens a connection and runs `CREATE TABLE IF NOT EXISTS urls (...)` and `CREATE INDEX IF NOT EXISTS idx_alias ON urls(alias)`. The `IF NOT EXISTS` clauses mean this is safe to call on every startup whether the database file exists or not.

Third, `HttpServer.create(new InetSocketAddress(8080), 0)` — creates a TCP server socket bound to port 8080. The `0` is the connection backlog, which defaults to the system setting.

Fourth, `server.createContext()` calls — registers four handlers: `ShortenHandler` for `/shorten`, `RedirectHandler` for `/r/`, `ListHandler` for `/all`, and `QRHandler` for `/qr`. These are prefix-matched.

Fifth, `server.setExecutor(Executors.newFixedThreadPool(4))` — without this, the server is single-threaded. Setting a thread pool means up to 4 requests can be processed concurrently. Then `server.start()` and the server blocks, waiting for connections."

---

### Q: "How does your server handle multiple requests at the same time?"

**Answer:**

"Java's `HttpServer` by default uses the calling thread — so without configuration, it would handle one request at a time, blocking all others while processing each one.

I set a fixed thread pool of size 4 using `Executors.newFixedThreadPool(4)`. When a request arrives, the HttpServer picks a thread from the pool and dispatches the handler to it. Up to 4 requests run simultaneously.

If a 5th request arrives while all 4 threads are busy, it waits in the server's accept queue — controlled by the backlog parameter in `InetSocketAddress`.

For this project, 4 threads is fine. In production you'd tune the pool size to match CPU cores and expected concurrency, or switch to a non-blocking I/O framework like Netty where threads aren't tied up during I/O waits."

---

### Q: "How do you parse JSON without a library?"

**Answer:**

"I wrote a method called `extractJsonValue` that does simple string searching. It finds the key in quotes, finds the colon after it, then extracts the value between the next pair of double quotes.

```java
String search = '"' + key + '"';
int keyIdx = json.indexOf(search);
int colon = json.indexOf(":", keyIdx);
int start = json.indexOf('"', colon + 1);
int end = json.indexOf('"', start + 1);
return json.substring(start + 1, end);
```

It has limitations — it only works for string values, it would break on nested JSON, and it doesn't handle escaped quotes inside values. For this project those limitations are acceptable because I control the request format: three known fields, all string values.

In production I would use Jackson or Gson. A JSON library handles all edge cases — special characters, nested objects, type mapping — and is a single dependency. The reason I didn't here is to keep the dependency count minimal and show I understand the underlying structure."

---

### Q: "How does redirection work in HTTP?"

**Answer:**

"HTTP redirect is a two-step process entirely orchestrated by the client — the server just provides instructions.

Step one: the client requests the short URL. Our server responds with status code 302 and a `Location` header containing the destination URL.

Step two: the client — browser, curl, whatever — sees the 302 and automatically sends a new GET request to the URL in the `Location` header. The server is no longer involved after that.

We use 302 Temporary Redirect specifically. The alternative is 301 Permanent, which browsers cache — once a browser has seen a 301, it redirects locally without hitting our server again. That sounds efficient, but it breaks TTL: a cached 301 would continue redirecting even after the URL has expired, bypassing our expiry check. 302 means every visit hits our server, which is what we need to enforce expiry."

---

### Q: "What's the difference between 404 and 410 and why does it matter?"

**Answer:**

"Both mean the resource isn't available, but they communicate different things.

404 Not Found means the resource has never existed at this URL, or the server won't confirm whether it does or doesn't.

410 Gone means the resource did exist at this URL but has been permanently removed.

For our URL shortener: if someone visits a code that was never created, we return 404. If they visit a code that existed but has expired, we return 410.

Why does this matter? Two reasons. For web crawlers and search engines: a 404 might be a temporary error they retry later. A 410 signals 'this is gone for good, remove it from your index.' For users and developers: a 410 gives a clearer, more specific message — you had this link before, it's just expired now, not that the link was wrong.

In the code, the redirect handler checks the result set and returns 410 when `Instant.now().getEpochSecond() > rs.getLong('expires_at')`."

---

### Q: "How do you handle CORS? What even is CORS?"

**Answer:**

"CORS — Cross-Origin Resource Sharing — is a browser security mechanism. Browsers enforce the 'same-origin policy': JavaScript on one origin cannot read responses from a different origin. Origin means scheme + host + port, so `localhost:5173` (Vite) and `localhost:8080` (Java) are different origins even on the same machine.

When our React app calls `fetch('/shorten')`, the browser checks if the server allows it. For POST requests with JSON content type, the browser first sends a preflight OPTIONS request asking: 'Can I send a POST from this origin with this content type?' Our server responds with `Access-Control-Allow-Origin: *`, `Access-Control-Allow-Methods: GET, POST, OPTIONS`, `Access-Control-Allow-Headers: Content-Type`. The browser then proceeds with the actual request.

I handle OPTIONS in every handler by calling `JsonUtil.sendCors()` which sends a 200 with those headers. I also set the CORS headers on every non-OPTIONS response because the browser checks them on the actual response too.

`Access-Control-Allow-Origin: *` means any origin is allowed. In production you'd restrict to your specific frontend domain.

Important: CORS is browser-only. curl, Postman, server-to-server requests — they don't enforce CORS. It's a browser protection, not a security mechanism."

---

### Q: "What would you add if you had more time — rate limiting?"

**Answer:**

"Yes, rate limiting would be a natural next addition. The current implementation has no protection against a single IP spamming POST /shorten and filling the database.

The standard approach is a token bucket or sliding window counter per IP.

For this project without adding infrastructure, I'd implement a simple in-memory sliding window using Java's `ConcurrentHashMap<String, Deque<Long>>` — the key is the client IP from the request, the value is a deque of request timestamps. On each request: remove timestamps older than 60 seconds, check if the count exceeds the limit (say 10 requests per minute), if exceeded return 429 Too Many Requests, otherwise add the current timestamp and proceed.

The limitation: this state is in-memory, so it resets on server restart and doesn't work across multiple server instances. The production upgrade is Redis — a shared, atomic counter accessible by all server instances. Redis's `INCR` command with `EXPIRE` is the standard pattern: `INCR ip:192.168.1.1` and `EXPIRE ip:192.168.1.1 60`."

---

### Q: "Does your project have authentication? Should it?"

**Answer:**

"No authentication in the current version — it's an open service. Anyone can shorten URLs and anyone who has the short URL can visit it.

Should it have authentication? Depends on the use case.

For a public shortener like bit.ly's free tier — no. Open access is the point. But without auth, there's no concept of 'my links' — you can't manage or delete links you created.

For a private or team shortener — yes. You'd add user accounts, each URL is owned by a user, and only the owner can see/manage their links. Implementation: JWT tokens in the Authorization header, a `users` table in the DB, and a `user_id` foreign key on the `urls` table.

For this portfolio project I intentionally skipped auth to keep the scope focused. I'd mention this proactively in an interview: 'Authentication is intentionally absent — adding it would require user management, token handling, and password hashing, which would have shifted the project's focus away from the core URL shortening mechanics.'"

---

## 4. Database Questions

---

### Q: "Why SQLite? What are its limitations?"

**Answer:**

"SQLite for three reasons: zero setup, file-based, and the JDBC interface is identical to every other SQL database.

Zero setup means no server process to start, no credentials to configure, no port to open. The database is a single file that Java creates automatically on first run.

File-based means the database is portable — copy `urls.db` and you have the entire database.

Identical JDBC interface means switching to PostgreSQL later requires changing exactly one line: the connection string from `jdbc:sqlite:urls.db` to `jdbc:postgresql://host/dbname`. Every PreparedStatement, ResultSet, and SQL query stays the same.

Limitations: SQLite uses file-level locking. Only one write operation can happen at a time — concurrent writes queue up. For our project with a pool of 4 threads, this means writes serialise at the DB level. Fine for low volume, not for high concurrency. Also, SQLite doesn't support network access — it's a local file. Can't share it across multiple server instances, which means no horizontal scaling without switching databases."

---

### Q: "Explain your database schema and every column's purpose."

**Answer:**

"One table: `urls`. Six columns.

`code TEXT PRIMARY KEY` — the randomly generated 6-character Base62 code. TEXT because it's a string. PRIMARY KEY enforces uniqueness and automatically creates a B-tree index, so lookups by code are O(log n).

`alias TEXT UNIQUE` — the optional custom alias. UNIQUE enforces that no two rows share an alias. Crucially, it's nullable — no NOT NULL — because SQL's UNIQUE constraint allows multiple NULL values. NULL means 'no alias', not 'duplicate alias'. This is standard SQL behaviour.

`original_url TEXT NOT NULL` — the destination URL. NOT NULL because a redirect without a destination is meaningless.

`created_at INTEGER NOT NULL` — Unix epoch seconds when the URL was created. INTEGER because Unix timestamps are integers, comparisons are fast, and there are no timezone issues.

`expires_at INTEGER NOT NULL` — Unix epoch seconds when the URL expires. Set at insert time as `created_at + (ttlDays * 86400)`. Checked on every redirect with `Instant.now().getEpochSecond() > expires_at`.

`deleted INTEGER NOT NULL DEFAULT 0` — soft delete flag. 0 = active, 1 = deleted. SQLite has no boolean type; by convention 0 is false, 1 is true. DEFAULT 0 means it's automatically 0 on every INSERT unless specified. We never hard-delete rows because keeping them means expired aliases are never reusable."

---

### Q: "Why do you store timestamps as integers instead of SQL datetime?"

**Answer:**

"Three reasons.

First, no timezone ambiguity. SQL datetime strings can be stored in different timezones depending on the client, the server locale, or the application. Unix timestamps are always UTC seconds since epoch — globally unambiguous.

Second, simple comparison. Checking expiry is one integer comparison: `Instant.now().getEpochSecond() > expires_at`. With datetime strings you'd need to parse, format, or use SQL datetime functions — more complex, more error-prone.

Third, `Instant.now().getEpochSecond()` in Java returns a long, which maps directly to SQLite's INTEGER type with no conversion. Clean mapping between Java and SQL types."

---

### Q: "What is a UNIQUE constraint and how does it differ from a PRIMARY KEY?"

**Answer:**

"Both enforce uniqueness — no two rows can have the same value in that column. The differences are:

PRIMARY KEY additionally implies NOT NULL. A PRIMARY KEY column can never be null. UNIQUE allows null — and as I mentioned, allows multiple nulls because null is not considered equal to null in SQL.

A table can only have one PRIMARY KEY. It can have multiple UNIQUE constraints on different columns.

In our schema, `code` is the PRIMARY KEY — it's the main identifier, never null, always present. `alias` is UNIQUE — it's optional (nullable) but must be unique when present.

Both create B-tree indexes automatically, so both support fast O(log n) lookups."

---

### Q: "What is a B-tree index and why did you create one on alias?"

**Answer:**

"A B-tree index is a sorted tree data structure that the database maintains alongside the table. When you query `WHERE alias = 'my-link'`, without an index the database scans every row — O(n). With a B-tree index, it finds the row in O(log n) — for 100,000 rows that's about 17 comparisons vs 100,000.

The UNIQUE constraint on alias automatically creates an index. I also wrote `CREATE INDEX IF NOT EXISTS idx_alias ON urls(alias)` explicitly — this is somewhat redundant with the UNIQUE index, but making it explicit with a named identifier (`idx_alias`) makes the intent clear and makes it easy to inspect or drop.

The `IF NOT EXISTS` means this is safe to call every time `Database.init()` runs — on a fresh database it creates the index, on an existing database it's a no-op."

---

### Q: "What is a PreparedStatement and why use it instead of string concatenation?"

**Answer:**

"PreparedStatement is a parameterised SQL query. Instead of building a SQL string like:

```java
String sql = "SELECT * FROM urls WHERE alias = '" + userInput + "'";
```

You write:
```java
PreparedStatement ps = conn.prepareStatement("SELECT * FROM urls WHERE alias = ?");
ps.setString(1, userInput);
```

Two reasons to prefer PreparedStatement.

First: SQL injection prevention. If `userInput` is `'; DROP TABLE urls; --`, string concatenation executes that as SQL. PreparedStatement sends the user input as a parameter — it's never interpreted as SQL syntax. The database driver handles escaping.

Second: performance. The database can compile and cache the query plan for the parameterised query. Subsequent calls with different parameter values reuse the compiled plan.

In every handler we use PreparedStatement. No raw string concatenation in SQL queries."

---

### Q: "What happens to expired URLs in the database? Are they ever deleted?"

**Answer:**

"They stay in the database forever in the current implementation. They're not hard deleted.

Why? Two reasons.

One: alias permanence. If we deleted expired rows, their aliases would be freed. Someone could then claim a previously popular alias — say 'summer-sale' — after the original expires. Old QR codes, printed materials, or bookmarks pointing to that alias would now redirect to someone else's URL. Soft expiry (keeping the row) prevents this.

Two: simplicity. Filtering expired rows with `WHERE expires_at > ?` is a single SQL clause. No background jobs, no scheduled tasks.

The downside: the database grows over time, even though most rows are 'dead'. The production upgrade is a background cleanup job — a `ScheduledExecutorService` that runs daily and runs `DELETE FROM urls WHERE expires_at < {timestamp_30_days_ago} AND deleted = 0`. The 30-day grace period gives even expired rows time to be audited before permanent deletion."

---

## 5. Algorithm and Logic Questions

---

### Q: "Explain your short code generation algorithm."

**Answer:**

"The algorithm is random Base62 selection.

I have a string of 62 characters: 26 lowercase letters, 26 uppercase letters, 10 digits. For each of 6 positions, I pick a random character from this string using `Math.random() * 62`, cast to int as an index.

That gives 62^6 = approximately 56 billion possible codes.

Then I check if the generated code already exists in the database with a SELECT query. If it does — collision — I regenerate. This is a do-while loop so it always runs at least once. At current scale, the probability of collision on the first attempt is 1 in 56 billion, so the loop runs exactly once in practice.

The collision is handled defensively at two levels: the do-while loop in application code, and the PRIMARY KEY constraint in the database as a final safety net.

The production alternative is Base62 encoding of a database auto-increment ID — zero collision possible, no check needed, because the ID is already unique by database design. The tradeoff is sequential codes are somewhat predictable, which is fixable by shuffling the character set."

---

### Q: "What is Base62? Why not Base64?"

**Answer:**

"Base62 uses 62 characters: a-z, A-Z, 0-9. It's a way to represent a number or a random string using only URL-safe characters.

Base64 uses 64 characters — it adds `+` and `/` (or `-` and `_` in URL-safe Base64). The problem: `+` and `/` have special meaning in URLs. They'd need to be percent-encoded, making the short URL look like `http://short.ly/r/aB%2FxYz` — ugly and defeats the purpose of shortening.

Base62 uses only alphanumeric characters, which are always safe in URL paths without encoding. That's the entire reason for choosing 62 over 64."

---

### Q: "How does TTL work in your system?"

**Answer:**

"TTL — Time To Live — determines how long a short URL remains valid.

At creation time, the user selects a TTL in days (1, 7, 30, 90, or 365). In the handler:

```java
long ttlSeconds = ttlDays * 24 * 60 * 60;
long expiresAt = Instant.now().getEpochSecond() + ttlSeconds;
```

`expiresAt` is stored in the database as a Unix integer.

At redirect time, every single redirect checks:
```java
if (Instant.now().getEpochSecond() > rs.getLong("expires_at")) {
    return 410 Gone;
}
```

This check is on the hot path — every redirect goes through it. It's a single integer comparison, so performance impact is negligible.

At list time, the `/all` query filters: `WHERE expires_at > {now}` — so the frontend only shows active URLs.

The row is never deleted — the URL becomes inaccessible when `now > expiresAt`, but the data stays in the database so the alias is permanently claimed."

---

### Q: "How do you validate a custom alias? Walk me through every rule."

**Answer:**

"Six validation rules, applied in order in `Validator.validateAlias()`:

One — length: 3 to 30 characters. Too short (1-2 chars) would exhaust the short namespace quickly. Too long defeats the point of a short alias.

Two — character format via regex `[a-z0-9][a-z0-9\\-]*[a-z0-9]`: letters, digits, and hyphens only. First and last character must be alphanumeric — no leading or trailing hyphens. Special characters like underscores, dots, or spaces would cause URL encoding issues.

Three — reserved words: a set containing our own route names — 'all', 'r', 'shorten', 'api', 'admin', 'login', 'qr', and others. If someone created alias 'all', visiting `/r/all` would conflict with our `/all` endpoint behaviour.

Four — blocked words: brand names like 'google', 'facebook', 'amazon'. Prevents impersonation and misleading links.

Five — case normalisation: before any of these checks, the alias is lowercased. So 'MyLink' and 'mylink' and 'MYLINK' are all treated as 'mylink'. Stored lowercase, matched lowercase.

Six — database uniqueness: the INSERT either succeeds or throws `SQLException` with 'UNIQUE constraint failed: urls.alias'. Application-level checks 1-5 are fast string operations. The database is the final authority on uniqueness, and it handles race conditions correctly."

---

## 6. Concurrency and Thread Safety

---

### Q: "What is a race condition? Does your project have one? How did you fix it?"

**Answer:**

"A race condition is when two or more threads access shared state simultaneously and the outcome depends on the timing of their execution — leading to incorrect or unpredictable results.

My project had a potential race condition in alias creation. The naive approach would be:

```
Thread A: SELECT — alias 'promo' doesn't exist
Thread B: SELECT — alias 'promo' doesn't exist
Thread A: INSERT alias 'promo' — success
Thread B: INSERT alias 'promo' — also 'success' (duplicate!)
```

Both threads saw the alias as free because both did the SELECT before either did the INSERT.

My fix: skip the SELECT check entirely. Go straight to INSERT. The UNIQUE constraint on the alias column means only one INSERT can succeed. The other gets a `SQLException`. I catch that exception, check that the message contains 'UNIQUE constraint failed: urls.alias', and return HTTP 409.

The database's storage engine serialises concurrent writes at the file level (SQLite uses file-level locking). Two threads can't interleave their INSERT operations. The constraint check and the INSERT are atomic from the database's perspective.

The rule: when the database can enforce a constraint, always prefer that over application-level checks. The database is a single shared source of truth. Application-level checks are inherently racy."

---

### Q: "Is HashMap thread-safe? Did you use it?"

**Answer:**

"No, `HashMap` is not thread-safe. Concurrent reads and writes to a HashMap can cause infinite loops or data corruption in Java — the internal linked-list structure can become circular during a resize operation under concurrent modification.

My original implementation used a static `HashMap<String, String>` to store URL mappings before I added SQLite. This was a bug — with a thread pool of 4, concurrent requests could corrupt the map.

The fix at the time would have been `ConcurrentHashMap`, which uses segment-level locking to allow safe concurrent access.

But I replaced the HashMap entirely with SQLite, which makes this a non-issue. SQLite handles concurrent access correctly. The lesson: don't replicate what the database already does reliably."

---

### Q: "What is a thread pool and why does pool size matter?"

**Answer:**

"A thread pool is a pre-created set of threads that sit idle waiting for work. When a task arrives, instead of creating a new thread (expensive — allocates stack memory, registers with the OS), a thread from the pool picks up the task. When done, the thread returns to the pool.

`Executors.newFixedThreadPool(4)` creates a pool that always has exactly 4 threads — no more, no fewer.

Why 4? Rules of thumb:

For CPU-bound tasks (heavy computation), pool size ≈ number of CPU cores. Adding more threads than cores just causes context-switching overhead.

For I/O-bound tasks (database queries, network calls — like ours), threads spend most of their time waiting. You can have more threads than cores — maybe 2x or 4x. While thread 1 is waiting for SQLite, thread 2 can do computation.

For this project, 4 is a reasonable default for a development machine. A production system would tune this based on load testing — or switch to a non-blocking async framework where threads aren't tied up during waits."

---

## 7. Frontend Questions

---

### Q: "Why React? Why not plain HTML and JavaScript?"

**Answer:**

"Plain HTML with JavaScript would work for this project, but React provides two things that make development cleaner.

First, declarative state management. In plain JS, when a user shortens a URL I'd have to manually find the table DOM element, create a row, append cells, update the short URL display. In React, I update `history` state and `shortUrl` state, and the UI re-renders automatically to reflect the new state. The code describes what the UI looks like, not how to manipulate the DOM to get there.

Second, controlled inputs. React's controlled components — `value={inputUrl} onChange={(e) => setInputUrl(e.target.value)}` — mean the component state is always in sync with what's in the input field. This makes form handling predictable.

For a project this size, plain JS would have been fine. React is a pragmatic choice because it's what most frontend roles use and it's what interviewers expect to see."

---

### Q: "Explain how the QR code modal works."

**Answer:**

"The modal is controlled by a single piece of state: `qrUrl`. When `qrUrl` is null, the modal doesn't render. When it has a value, the modal renders.

The QR image itself is just a standard HTML `<img>` tag:
```jsx
<img src={`/qr?url=${encodeURIComponent(qrUrl)}`} />
```

When the browser renders this tag, it automatically sends a GET request to `/qr?url=...`. The backend returns PNG bytes with `Content-Type: image/png`. The browser renders those bytes as an image. No JavaScript needed for the image fetch — it's native browser behaviour.

`encodeURIComponent` converts special characters: `:` becomes `%3A`, `/` becomes `%2F`. This is necessary because the URL is being passed as a query parameter inside another URL. Without encoding, the browser would misparse the slashes.

Closing: clicking the overlay sets `qrUrl(null)`. `e.stopPropagation()` on the inner modal div prevents clicks inside the modal from bubbling to the overlay and triggering the close."

---

### Q: "What is the Vite proxy and why does it exist?"

**Answer:**

"Vite proxy is a configuration in `vite.config.js` that tells Vite's development server to forward certain requests to another server.

```js
proxy: {
    "/shorten": "http://localhost:8080",
    "/r":       "http://localhost:8080",
    "/all":     "http://localhost:8080",
    "/qr":      "http://localhost:8080"
}
```

Why it exists: our React app runs on `localhost:5173` and the Java backend runs on `localhost:8080`. These are different origins. If the frontend called `http://localhost:8080/shorten` directly, the browser would see a cross-origin request, triggering CORS handling.

With the proxy, the frontend calls `/shorten` — same origin. Vite internally forwards it to `:8080`. The browser never sees the cross-origin request, so no CORS preflight. Also, frontend code never has a hardcoded backend URL, which means in production the same code works when a real reverse proxy handles the forwarding.

In production, Nginx does this same job: requests to `/shorten` get proxied to the Java process, requests to `/` get served the static built React files. Development Vite proxy and production Nginx are the same concept."

---

### Q: "How does `useEffect` work in your project?"

**Answer:**

"`useEffect` is React's mechanism for side effects — things outside the normal render cycle like data fetching, subscriptions, or timers.

In my project:
```jsx
useEffect(() => {
    fetch("/all")
        .then(r => r.json())
        .then(setHistory)
        .catch(() => {});
}, []);
```

The empty array `[]` as the second argument means this effect runs once — after the component mounts for the first time. It fetches all active URLs from the backend and sets the history state, which populates the table.

Without `useEffect`, I couldn't do this fetch during render — React's render function must be pure and synchronous. `useEffect` schedules the side effect to run after the DOM has been committed.

The empty catch is intentional: if the backend isn't running when the page loads, the history table just stays empty rather than showing a fetch error. Silent failure is acceptable here."

---

## 8. System Design — Scaling Questions

---

### Q: "How would you scale this to 100,000 users?"

**Answer:**

"I'd scale in layers, addressing the bottleneck at each stage.

**First bottleneck: the database.**
SQLite uses file-level locking — only one write at a time. At 100k users with frequent shortening and redirecting, this becomes a problem. Switch to PostgreSQL. The JDBC code doesn't change — only the connection string and JAR. PostgreSQL supports row-level locking, connection pooling, and horizontal read replicas.

**Second bottleneck: redirect performance.**
Redirects are the hot path — they happen far more often than URL creation (maybe 100:1 ratio). Every redirect hits the database. Add Redis as a cache in front of the database:

```
GET /r/my-link
  → Check Redis: key "my-link"
    → Hit: return cached original_url, redirect immediately (~1ms)
    → Miss: query PostgreSQL, cache result in Redis with TTL, redirect
```

After the first visit, every subsequent redirect for that code hits Redis only — sub-millisecond.

**Third bottleneck: single server.**
One Java process is a single point of failure and a CPU ceiling. Put a load balancer (Nginx, AWS ALB) in front of 3-5 Java instances. Each instance is stateless — no local state, all state in PostgreSQL and Redis. Any instance handles any request.

**Estimated capacity after these changes:**
- Redis can handle ~100,000 requests/second per instance
- PostgreSQL can handle ~10,000 writes/second
- 5 Java instances with 20 threads each handles 100 concurrent requests

That's well within 100k users."

---

### Q: "How would you add click analytics to this project?"

**Answer:**

"Click analytics means tracking how many times each short URL was visited, and potentially when and from where.

The naive approach: add a `click_count INTEGER DEFAULT 0` column and run `UPDATE urls SET click_count = click_count + 1 WHERE code = ?` on every redirect. Problem: this is a write on the hot path. Every redirect now does a SELECT and an UPDATE. At high traffic, this creates write contention — all threads competing to update the same row.

Better approach: write-behind asynchronous logging.

On redirect, push a click event to a queue — an in-memory `BlockingQueue` or a Redis list. A background thread drains the queue every second and does a bulk UPDATE: `UPDATE urls SET click_count = click_count + N WHERE code = ?`. N writes become 1 write per second per URL.

At scale: log click events to Apache Kafka. Analytics consumers process the stream — counts per URL, clicks per hour, geographic distribution. This decouples click tracking from the redirect latency completely. The redirect handler just fires an event and returns immediately."

---

### Q: "How would you design the short code generation to work across multiple servers?"

**Answer:**

"With a single server, my current approach — random code + database UNIQUE constraint — works fine. With multiple servers, it still works because the database is the shared authority. Two servers generating the same code both try to INSERT; one succeeds, the other fails and retries. The UNIQUE constraint on `code` handles this.

But at very high write rates, retry overhead accumulates. The production solution is base62 encoding of a distributed auto-increment counter.

One approach: use PostgreSQL's `SERIAL` or `BIGSERIAL` — a global auto-increment. Each INSERT gets the next integer. Encode that integer in Base62. No collision possible, no retry needed.

Another approach for extremely high scale: range-based ID allocation. A central coordinator pre-allocates ID ranges to each server — server 1 gets IDs 1-1000, server 2 gets 1001-2000. Each server generates codes locally from its range, no coordination needed per request. When a server exhausts its range, it requests the next batch. This is how systems like Flickr and Twitter generate IDs at scale."

---

### Q: "How would you add a caching layer for redirects?"

**Answer:**

"The redirect path is: receive GET /r/code → query SQLite → check expiry → send 302.

With Redis caching:

```
receive GET /r/code
  → Redis GET "url:code"
    → Cache hit: have original_url and it's valid
        → send 302 immediately (~0.2ms total)
    → Cache miss:
        → query SQLite
        → check expiry
        → if valid: Redis SET "url:code" original_url EX {seconds_until_expiry}
        → send 302 (~5ms total)
```

The Redis TTL matches the URL's `expires_at` — `EX {expiresAt - now}` in seconds. When the URL expires, the Redis key also expires automatically. No stale cache entries, no manual invalidation needed.

In Java, I'd use the Jedis or Lettuce client library. One `jedis.get(key)` call before the database query. Cache miss goes to SQLite and populates the cache. After the first visit to any short URL, all subsequent visits for that URL's lifetime are served from Redis.

Cache hit rate for a typical URL shortener is very high — popular links are visited repeatedly. 95%+ of redirects would be cache hits after warmup."

---

### Q: "What happens if your server crashes? What data is lost?"

**Answer:**

"Currently: no data is lost on a server crash. SQLite is file-based and ACID-compliant. Every successful INSERT has already been written to the `urls.db` file on disk — SQLite uses write-ahead logging (WAL) to ensure durability. When the server restarts, `Database.init()` reopens the existing file and all data is intact.

What IS lost on crash: the four threads in the pool that were actively processing requests. Those in-flight requests fail — the client gets a connection error. The user would need to retry.

What is NOT lost: any request that had already received a 200 response. The URL is in the database.

Edge case: a request was mid-INSERT when the crash happened. SQLite's atomicity means the INSERT either fully committed before the crash or it didn't happen at all. No partial writes.

If I scaled to multiple servers and PostgreSQL, the same guarantees apply — PostgreSQL's ACID properties ensure durability. The only new concern is replication lag if using read replicas, but that's a read consistency issue, not a data loss issue."

---

## 9. Security Questions

---

### Q: "How did you ensure security in this project?"

**Answer (STAR):**

**Situation:** A URL shortener has several obvious attack surfaces — SQL injection, spam/abuse of the shortening endpoint, malicious URL storage, and alias hijacking.

**Task:** Without adding authentication or rate limiting (which would scope-creep the project), I wanted to address the most critical vulnerabilities.

**Action:** Four specific security measures.

One — SQL injection prevention: every database query uses `PreparedStatement` with parameterised placeholders. User input is never concatenated into SQL strings. `ps.setString(1, userInput)` sends the value as a parameter, never as executable SQL.

Two — Input validation server-side: even though the frontend validates, the backend also validates every input. URL must start with http:// or https://. Alias must match the regex. This prevents malformed data reaching the database regardless of what the client sends.

Three — Alias hijacking prevention: soft delete means expired aliases are never reusable. A deleted or expired URL's alias stays in the database permanently, so it can't be claimed to capture old traffic.

Four — CORS restriction: production would restrict `Access-Control-Allow-Origin` to the specific frontend domain, preventing other websites from making API calls to our backend on behalf of users.

**Result:** The core attack surfaces are addressed without over-engineering for a portfolio project scope.

---

### Q: "What is SQL injection and how did you prevent it?"

**Answer:**

"SQL injection is when user input is interpreted as SQL code instead of data.

Classic example without prevention:
```java
String sql = "SELECT * FROM urls WHERE alias = '" + userInput + "'";
```

If `userInput` is `'; DROP TABLE urls; --`, the resulting SQL is:
```sql
SELECT * FROM urls WHERE alias = ''; DROP TABLE urls; --'
```

This executes two SQL statements — the second deletes the entire table.

Prevention: PreparedStatement with parameterised queries.
```java
PreparedStatement ps = conn.prepareStatement("SELECT * FROM urls WHERE alias = ?");
ps.setString(1, userInput);
```

The JDBC driver sends the query and the parameter separately to the database. The database treats the parameter purely as a string value, never as SQL syntax. Even if `userInput` contains SQL keywords or quotes, they're just characters.

Every query in my project uses PreparedStatement. No raw string concatenation in any SQL."

---

### Q: "What about malicious URLs? Could someone shorten a phishing link?"

**Answer:**

"Yes — the current implementation does no URL content validation. Anyone can shorten `http://definitely-not-phishing.com`. This is a real concern for production URL shorteners and why services like bit.ly do destination scanning.

The options:

One — URL reputation lookup: before storing, check the URL against a threat intelligence API like Google Safe Browsing. If flagged as malicious, reject with 400. Adds latency to the shorten endpoint but protects users.

Two — Click warning page: instead of a direct 302 redirect, show an intermediate page — 'You are about to visit {domain}. Continue?' This gives users a chance to see the destination before being redirected. Trade-off: breaks the seamless redirect experience.

Three — Domain allowlist or blocklist: simple set of known-bad domains, checked on every shorten request. Fast, no external API, but only catches known-bad domains.

For this portfolio project I intentionally skipped URL scanning — it would require an external API key and network call in the critical path. I'd mention this proactively: 'In production I would integrate Google Safe Browsing API to validate URLs before storing them.'"

---

## 10. Java Language Questions

---

### Q: "What is JDBC? How does it work?"

**Answer:**

"JDBC — Java Database Connectivity — is a standard Java API for connecting to relational databases. It's in the `java.sql` package, part of the JDK.

The key abstraction is the `Driver` pattern: each database (SQLite, MySQL, PostgreSQL) provides a driver JAR that implements the JDBC interfaces. Your Java code calls standard JDBC methods, and the driver translates them to the database's native protocol.

The flow in my project:
```
Class.forName("org.sqlite.JDBC")        → loads the driver class
DriverManager.getConnection(DB_URL)     → opens a connection (creates/opens urls.db)
conn.prepareStatement("SELECT ...")     → compiles the SQL query
ps.setString(1, value)                  → binds the parameter
ps.executeQuery()                       → sends to SQLite, returns ResultSet
rs.next()                               → moves cursor to next row
rs.getString("column")                  → reads value from current row
conn.close()                            → releases the connection
```

The crucial thing: my code only imports `java.sql.*`. If I change the driver JAR and connection string from SQLite to PostgreSQL, nothing else in my code changes. That's the power of the JDBC abstraction."

---

### Q: "Explain try-with-resources. Why did you use it?"

**Answer:**

"Try-with-resources is a Java feature (since Java 7) for automatic resource management. Any class that implements `AutoCloseable` can be used in a try-with-resources block:

```java
try (Connection conn = Database.getConnection();
     PreparedStatement ps = conn.prepareStatement("...")) {
    // use conn and ps
} // conn and ps automatically closed here, even if exception is thrown
```

`Connection` and `PreparedStatement` both implement `AutoCloseable`. The JVM guarantees they're closed when the try block exits — whether by normal completion or by exception.

Without this, you'd need:
```java
Connection conn = null;
try {
    conn = getConnection();
    // use conn
} finally {
    if (conn != null) conn.close();
}
```

Verbose and error-prone — easy to forget the null check or the close.

In my project, every database operation uses try-with-resources. Resource leaks (unclosed connections) are a common source of production bugs — connection pools exhaust, 'too many open files' errors appear. Try-with-resources eliminates the category entirely."

---

### Q: "What is `Instant.now().getEpochSecond()` and why use it?"

**Answer:**

"`Instant` is Java's representation of a point in time on the UTC timeline. `Instant.now()` returns the current moment. `getEpochSecond()` returns the number of seconds since January 1, 1970, 00:00:00 UTC — the Unix epoch.

I use it for two purposes: setting `created_at` and `expires_at` at insert time, and comparing against `expires_at` on every redirect.

Why this over `System.currentTimeMillis()`? `Instant` is in `java.time`, the modern Java time API introduced in Java 8. It's cleaner and has clearer semantics. `getEpochSecond()` returns seconds (which is what I store in SQLite as INTEGER), whereas `currentTimeMillis()` returns milliseconds — I'd need to divide by 1000, which is easy to forget.

Why not `new Date()` or `Calendar`? The old Java date/time API is notorious for being confusing, mutable, and having timezone gotchas. `java.time` is the modern replacement and what you'd use in any new Java code."

---

### Q: "What's the difference between `String` and `StringBuilder`?"

**Answer:**

"String in Java is immutable — every concatenation creates a new String object. In a loop building JSON:

```java
String json = "[";
for each row:
    json = json + "{...}"; // creates a new String object every iteration
json = json + "]";
```

With 1000 rows, this creates 1000 intermediate String objects and copies the growing string each time — O(n²) in total work.

StringBuilder is mutable — `append()` adds to the internal buffer without creating a new object:

```java
StringBuilder json = new StringBuilder("[");
for each row:
    json.append("{...}"); // modifies existing buffer
json.append("]");
String result = json.toString(); // one final String creation
```

This is O(n) total work.

In my `ListHandler`, I use StringBuilder for building the JSON array response, because the number of URLs could be large and repeated String concatenation would be wasteful."

---

### Q: "What does `equalsIgnoreCase` do and when would you use it?"

**Answer:**

"`equalsIgnoreCase` compares two strings without considering case — `'POST'.equalsIgnoreCase('post')` returns true.

I use it for HTTP method checking:
```java
if ('OPTIONS'.equalsIgnoreCase(exchange.getRequestMethod()))
```

The HTTP specification says method names are always uppercase. But being lenient costs nothing, and defensive programming means my handler works even if some HTTP client sends lowercase methods.

I also use it (through `toLowerCase()`) for alias comparison. Aliases are lowercased at input time and stored lowercase, so direct `equals()` comparison in SQL is fine. But for the SQL query I still use `LOWER(alias) = ?` to handle any legacy data that might not be normalised."

---

## 11. Behavioral / STAR Questions

---

### Q: "Tell me about a challenge you faced building this project and how you solved it."

**Answer (STAR):**

**Situation:** When I moved from the in-memory HashMap to SQLite, I ran into a `java.sql.SQLException: No suitable driver found for jdbc:sqlite:urls.db` error even though the sqlite-jdbc JAR was on the classpath.

**Task:** I needed to understand why the JDBC driver wasn't loading and fix it without changing the project structure significantly.

**Action:** I researched how JDBC driver loading works. JDBC 4.0 introduced `ServiceLoader` auto-discovery, where the JVM automatically loads JDBC drivers found on the classpath. But this depends on the JAR having a `META-INF/services/java.sql.Driver` file, and some environments or classpath configurations don't trigger auto-loading reliably. The explicit fix is `Class.forName("org.sqlite.JDBC")` — this directly loads the driver class, which registers it with `DriverManager` via a static initializer. I added this as the very first line in `main()`.

**Result:** Server started cleanly, `Database.init()` ran successfully, `urls.db` was created. I also documented this in comments so anyone running the project understands why that line is there — it's not magic, it's a deliberate fix for a known JDBC bootstrapping issue.

---

### Q: "Describe a decision you made that you later reconsidered."

**Answer (STAR):**

**Situation:** My original implementation stored all URL mappings in a static `HashMap<String, String>` in `Main.java`. This worked for basic functionality.

**Task:** I realised this had two critical problems: data was lost every time the server restarted, and `HashMap` is not thread-safe — with my 4-thread pool, concurrent access could corrupt the map.

**Action:** I initially considered replacing HashMap with `ConcurrentHashMap`, which would fix the thread-safety issue. But I reconsidered — persistent storage was a bigger gap than thread safety. Adding SQLite addressed both: JDBC handles concurrent access correctly (SQLite file-level locking), and data survives restarts. I also took the opportunity to split the code into proper packages — `Database.java`, separate handler classes — which I should have done from the start.

**Result:** The refactor took a full session but resulted in a much cleaner codebase. The lesson: don't patch a fundamentally flawed design choice — replace it cleanly. `ConcurrentHashMap` would have been a band-aid; SQLite was the right solution.

---

### Q: "How did you test your project?"

**Answer (STAR):**

**Situation:** This is a portfolio project without a formal test suite. I needed confidence that the features worked correctly — especially the edge cases around aliases, TTL, and race conditions.

**Task:** Verify all features and edge cases work without setting up JUnit (which would have added build tool complexity).

**Action:** I tested manually using a checklist:
- Shorten a URL with no alias → random code generated, redirect works
- Shorten with alias 'my-link' → alias appears in short URL
- Try alias 'my-link' again → 409 conflict returned
- Try alias 'admin' → reserved word error
- Try alias 'google' → blocked name error
- Try alias '-bad' → format error
- Wait (or manually update `expires_at` in SQLite) → 410 on redirect
- Delete `urls.db` and restart → clean state, server starts fine
- Check `/all` endpoint → only active URLs returned
- Open browser DevTools → verify 302 redirect, correct Location header
- QR button → modal opens, PNG renders, closes on overlay click

For race condition testing: I used `curl` in two simultaneous terminal windows posting the same alias.

**Result:** All cases behaved as expected. In hindsight I would add a JUnit test class with at least unit tests for `Validator.validateAlias()` and integration tests for the database layer. Pure unit tests don't require a build tool — just compile and run with the test framework JAR on the classpath.

---

### Q: "What was the most important design decision you made?"

**Answer (STAR):**

**Situation:** When implementing the custom alias feature, I needed to decide how to handle the case where two users simultaneously request the same alias.

**Task:** Design a collision detection strategy that is both correct under concurrency and simple to implement.

**Action:** My first instinct was an existence check: SELECT the alias, and if it doesn't exist, INSERT. But I realised this is a classic check-then-act race condition — two threads could both pass the SELECT check before either does the INSERT. I could add a Java `synchronized` block, but that serialises ALL shortening requests, not just ones with aliases.

The better solution: skip the SELECT. Go straight to INSERT. Let the database's UNIQUE constraint do the enforcement. The database handles concurrent INSERTs atomically — only one can win. The loser gets a `SQLException`. Catch it, check the message, return 409.

**Result:** Correct behaviour under all concurrency scenarios, with no application-level locking. Interestingly, this is the same pattern used in production systems — the database is more reliable than application-level checks, and it's always the single source of truth. This became one of my favourite talking points because it demonstrates concurrency thinking, database knowledge, and knowing when NOT to add complexity.

---

### Q: "What would you do differently if you started over?"

**Answer:**

"Three things, all of which I now know from building this:

First, make the base URL configurable from day one. I have `localhost:8080` hardcoded in the URL construction. When someone tries to demo this on a real server with a domain, it breaks immediately. One `System.getenv('BASE_URL')` with a fallback to localhost would have solved this with two lines.

Second, start with proper packages from the beginning. I originally had everything in one `Main.java`. Refactoring into `handlers/`, `util/`, and `db/` packages took real effort later — much easier to design it that way upfront.

Third, add a connection pool from the start. HikariCP is a single dependency and five lines of configuration. Opening a new JDBC connection per request is fine for a portfolio project but it's a habit I'd rather not build."

---

## 12. Closing — Questions to Ask the Interviewer

> Ask 2-3 of these. Asking good questions signals genuine interest and seniority. Never ask about salary or benefits in a technical round.

---

**About the team and tech:**

- "What does the backend stack look like at your company, and how much of it is Java?"
- "How do teams here handle database schema migrations in production?"
- "What does the onboarding process look like for a new engineer — how quickly are people expected to start contributing?"

**About engineering culture:**

- "How does code review work here? Is it PR-based, pair programming, or something else?"
- "How are technical decisions made — does the team propose and discuss, or is there a top-down architecture process?"
- "What does a typical sprint or week look like for a new backend engineer?"

**About growth:**

- "What's the thing that a strong new graduate hire does in the first 6 months that distinguishes them from an average one?"
- "Are engineers here expected to stay within a specific stack, or is there opportunity to work across different parts of the system?"

**Specific to this interview:**

- "Is there anything in what I described about this project that you'd have approached differently? I'd genuinely like to know."

> That last one is excellent. It shows confidence, openness to feedback, and turns the interview into a conversation. Interviewers remember candidates who ask it.

---

## 13. Quick Reference — Numbers to Memorise

Study this table. Interviewers often ask "how many X can your system handle" or "what's the complexity of Y."

### Project-Specific Numbers

| Fact | Number | Context |
|---|---|---|
| Short code length | 6 characters | Base62, 6 positions |
| Code combinations | ~56 billion | 62^6 = 56,800,235,584 |
| Charset size | 62 characters | 26 lower + 26 upper + 10 digits |
| Default TTL | 7 days | 604,800 seconds |
| TTL range | 1–365 days | Min 1 day, max 1 year |
| Alias length range | 3–30 characters | Validated in Validator.java |
| Thread pool size | 4 threads | `newFixedThreadPool(4)` in Main.java |
| QR image size | 300 × 300 pixels | Configured in QRHandler |
| QR error correction | Level M = 15% | Can recover if 15% of QR is obscured |
| DB columns | 6 | code, alias, original_url, created_at, expires_at, deleted |
| DB indexes | 2 (effective) | Primary key on code, UNIQUE on alias |
| Reserved words | ~18 | all, r, shorten, api, admin, login, qr, etc. |
| Blocked words | ~9 | google, facebook, amazon, apple, etc. |
| HTTP status codes used | 8 | 200, 302, 400, 404, 405, 409, 410, 500 |
| API endpoints | 4 | /shorten, /r/, /all, /qr |

### General Numbers Worth Knowing

| Fact | Number | Why It Matters |
|---|---|---|
| Redis latency | ~0.1–0.2ms | Cache hit response time |
| SQLite read latency | ~1–5ms | Local file, fast |
| PostgreSQL query latency | ~1–10ms | Network + disk |
| HTTP request overhead | ~1–5ms | TCP handshake + headers |
| B-tree search (100k rows) | ~17 comparisons | log₂(100,000) ≈ 17 |
| B-tree search (1M rows) | ~20 comparisons | log₂(1,000,000) ≈ 20 |
| Java thread stack size | ~512KB–1MB | Each thread allocates this |
| Unix epoch (approx 2024) | ~1,700,000,000 | Fits in a 32-bit integer until 2038 |
| Unix epoch (approx 2026) | ~1,780,000,000 | Fits in a 32-bit integer until 2038 |
| Base62 vs Base64 | 62 vs 64 chars | Base64 adds + and / which are URL-unsafe |
| MD5 output (hex) | 16 chars per byte | Truncated to 6 = 16^6 = 16M combinations |
| 62^6 vs 16^6 | 56B vs 16M | Why Base62 is far better than MD5 hex |

### Algorithm Complexities

| Operation | Complexity | Notes |
|---|---|---|
| Random code generation | O(1) | 6 iterations, constant |
| Collision check (codeExists) | O(log n) | B-tree index on code |
| Alias validation (string ops) | O(k) | k = alias length, effectively O(1) |
| Reserved word check | O(1) | HashSet lookup |
| INSERT with UNIQUE check | O(log n) | B-tree index traversal |
| Redirect lookup (alias OR code) | O(log n) | Index on both |
| List all active URLs | O(m) | m = number of active URLs |
| Base62 encode (ID approach) | O(log₆₂ n) | ~O(1) for 64-bit IDs |
| QR generation | O(k²) | k = QR matrix size (300×300 = fixed) |
| JSON building (list) | O(m) | m = number of rows |

### HTTP Status Codes (Know These Cold)

| Code | Name | When We Use It |
|---|---|---|
| 200 | OK | Successful shorten, list, QR |
| 302 | Found (Temporary Redirect) | Redirect to original URL |
| 400 | Bad Request | Invalid URL, bad alias, bad TTL |
| 404 | Not Found | Code never existed in DB |
| 405 | Method Not Allowed | Wrong HTTP method |
| 409 | Conflict | Alias already taken |
| 410 | Gone | URL existed but expired |
| 429 | Too Many Requests | Rate limiting (not yet implemented) |
| 500 | Internal Server Error | DB error, unexpected exception |

---

> **Final reminder:** The best interview answers connect the specific thing you built to the general concept being asked about. "In my project I did X. The reason is Y. If I were scaling this, I would do Z." That format — specific → general → scale — works for almost every technical question.
