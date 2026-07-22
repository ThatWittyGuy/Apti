# 01 — Knowledge Base: URL Shortener Project
### Complete A-to-Z Reference for Understanding, Explaining, and Defending Every Decision

---

## Table of Contents

1. [What This Project Is](#1-what-this-project-is)
2. [Full Tech Stack](#2-full-tech-stack)
3. [Project Architecture](#3-project-architecture)
4. [Complete Data Flow](#4-complete-data-flow)
5. [Database — Deep Dive](#5-database--deep-dive)
6. [Backend — Deep Dive on Every Handler](#6-backend--deep-dive-on-every-handler)
7. [Utility Classes — Deep Dive](#7-utility-classes--deep-dive)
8. [Short Code Generation — Algorithm Deep Dive](#8-short-code-generation--algorithm-deep-dive)
9. [Custom Alias System — Deep Dive](#9-custom-alias-system--deep-dive)
10. [TTL and Expiry System — Deep Dive](#10-ttl-and-expiry-system--deep-dive)
11. [QR Code System — Deep Dive](#11-qr-code-system--deep-dive)
12. [HTTP Fundamentals Used in This Project](#12-http-fundamentals-used-in-this-project)
13. [CORS — What It Is and How We Handle It](#13-cors--what-it-is-and-how-we-handle-it)
14. [Concurrency and Thread Safety](#14-concurrency-and-thread-safety)
15. [Input Validation — Every Rule and Why](#15-input-validation--every-rule-and-why)
16. [Frontend Architecture](#16-frontend-architecture)
17. [Vite Proxy — What It Is and Why It Matters](#17-vite-proxy--what-it-is-and-why-it-matters)
18. [Every Design Decision with Reasoning](#18-every-design-decision-with-reasoning)
19. [Alternatives Considered and Rejected](#19-alternatives-considered-and-rejected)
20. [Known Limitations and Upgrade Path](#20-known-limitations-and-upgrade-path)

---

## 1. What This Project Is

A URL shortener is a service that takes a long URL (e.g. `https://example.com/very/long/path?with=query&params=true`) and maps it to a short code (e.g. `http://localhost:8080/r/aB3xYz`). When someone visits the short URL, the server looks up the original URL and issues an HTTP redirect, sending the browser to the original destination. The user never "sees" the server — they just get transparently forwarded.

This implementation is a full-stack URL shortening service built with React on the frontend and a plain Java HTTP server on the backend — no Spring Boot, no Express, no frameworks of any kind. The backend uses only what ships with the Java Development Kit plus two external JARs: one for SQLite database access (sqlite-jdbc) and one for QR code generation (ZXing). Every HTTP concept — CORS headers, status codes, request parsing, response writing — is handled manually in code. This makes the project valuable as a learning and portfolio piece: there is no framework hiding what is happening, so every line is explainable and defensible.

---

## 2. Full Tech Stack

| Technology | Version | Role | Why Chosen | Alternative Considered | Why Alternative Rejected |
|---|---|---|---|---|---|
| Java | 17 | Backend language | LTS release, widely used in enterprise, strong typing, `HttpServer` ships with JDK | Java 21 (newer LTS) | Java 17 is more universally installed; features used don't need 21 |
| `com.sun.net.httpserver.HttpServer` | JDK built-in | HTTP server | Zero dependencies, ships with JDK, forces manual HTTP understanding | Spring Boot, Jetty, Netty | Spring Boot hides too much; Jetty/Netty are external dependencies — defeating the purpose |
| SQLite | 3.x | Database | Zero setup, single file, full SQL support, ACID compliant | MySQL, PostgreSQL, H2 | MySQL/Postgres need a running server and credentials; H2 is in-memory only by default |
| sqlite-jdbc | 3.x | JDBC driver for SQLite | Only way to connect Java to SQLite; industry standard | No real alternative | N/A — this is the only SQLite JDBC driver |
| ZXing Core | 3.5.2 | QR code matrix generation | Google's open-source QR library, industry standard in Java | qrgen, nayuki | ZXing is the most widely used and documented; others are wrappers around it anyway |
| ZXing JavaSE | 3.5.2 | Converts QR matrix to PNG image | Companion to ZXing Core; handles image writing | Manual BufferedImage | JavaSE module handles this cleanly — no reason to reinvent |
| React | 18 | Frontend UI | Component model, useState/useEffect hooks make async data clean | Vue, plain HTML/JS | React is most common in job market; plain HTML has no state management |
| Vite | 5 | Frontend build tool + dev server | Fast HMR, built-in proxy config to forward API calls to backend | Create React App (CRA) | CRA is deprecated; Vite is the current standard |
| CSS-in-JS (inline styles) | N/A | Styling | No extra dependencies, self-contained component | Tailwind, CSS modules | Tailwind needs a build step config; CSS modules add file count; inline styles are portable |

---

## 3. Project Architecture

### 3.1 Folder Structure with Every File Explained

```
url-shortener/
│
├── backend/
│   ├── lib/                          # External JARs — manually managed, no build tool
│   │   ├── sqlite-jdbc.jar           # JDBC driver: lets Java talk to SQLite via SQL
│   │   ├── core-3.5.2.jar            # ZXing Core: QR code matrix algorithm
│   │   └── javase-3.5.2.jar          # ZXing JavaSE: converts QR matrix → PNG bytes
│   │
│   ├── .vscode/
│   │   └── settings.json             # Tells VS Code Java extension where the JARs are
│   │                                 # Without this, VS Code shows red import errors
│   │                                 # even though javac compiles fine from terminal
│   │
│   └── src/
│       ├── Main.java                 # Entry point. Creates HttpServer, registers all
│       │                             # context paths, sets thread pool, starts server.
│       │                             # Intentionally thin — only wires things together.
│       │
│       ├── db/
│       │   └── Database.java         # All database concerns in one place:
│       │                             # - DB_URL constant (the connection string)
│       │                             # - init(): creates table + index if not exists
│       │                             # - getConnection(): opens a new JDBC connection
│       │                             # - codeExists(): checks if a random code is taken
│       │
│       ├── handlers/
│       │   ├── ShortenHandler.java   # Handles POST /shorten
│       │   │                         # Parses body, validates URL + alias + TTL,
│       │   │                         # generates code, inserts into DB, returns JSON
│       │   │
│       │   ├── RedirectHandler.java  # Handles GET /r/{codeOrAlias}
│       │   │                         # Looks up by alias OR code, checks expiry,
│       │   │                         # issues 302 redirect or appropriate error
│       │   │
│       │   ├── ListHandler.java      # Handles GET /all
│       │   │                         # Returns JSON array of all active (non-expired,
│       │   │                         # non-deleted) URLs ordered newest first
│       │   │
│       │   └── QRHandler.java        # Handles GET /qr?url=...
│       │                             # Decodes URL query param, generates QR PNG,
│       │                             # returns raw image bytes
│       │
│       └── util/
│           ├── Validator.java        # Pure validation logic — no HTTP, no DB
│           │                         # isValidUrl(): checks http/https prefix
│           │                         # validateAlias(): format + reserved + blocked
│           │
│           └── JsonUtil.java         # HTTP response helpers and JSON parsing
│                                     # sendJson(): writes JSON response with headers
│                                     # sendImage(): writes PNG bytes with image headers
│                                     # sendCors(): handles OPTIONS preflight
│                                     # extractJsonValue(): parses a field from JSON string
│                                     # escapeJson(): escapes quotes/backslashes for JSON
│
└── frontend/
    ├── src/
    │   ├── App.jsx                   # The entire frontend UI in one component.
    │   │                             # State: inputUrl, alias, ttlDays, shortUrl,
    │   │                             # error, loading, history, copied, qrUrl
    │   │                             # On mount: fetches /all to populate history
    │   │                             # On submit: POSTs to /shorten, updates state
    │   │
    │   └── main.jsx                  # ReactDOM.createRoot — mounts App into #root div
    │
    ├── index.html                    # Shell HTML file. Has <div id="root"> where
    │                                 # React mounts. Vite injects the script tag.
    │
    ├── vite.config.js                # Two jobs:
    │                                 # 1. Registers @vitejs/plugin-react (JSX support)
    │                                 # 2. Proxy config: /shorten, /r, /all, /qr
    │                                 #    all forwarded to http://localhost:8080
    │
    └── package.json                  # Dependencies: react, react-dom
                                      # DevDependencies: vite, @vitejs/plugin-react
```

### 3.2 Layered Architecture View

```
┌─────────────────────────────────────────────┐
│              PRESENTATION LAYER              │
│         React (App.jsx) on :5173             │
│  Form → fetch() → display results           │
└──────────────────┬──────────────────────────┘
                   │ HTTP (proxied by Vite)
┌──────────────────▼──────────────────────────┐
│               ROUTING LAYER                  │
│        Main.java — HttpServer :8080          │
│  /shorten → ShortenHandler                  │
│  /r/      → RedirectHandler                 │
│  /all     → ListHandler                     │
│  /qr      → QRHandler                       │
└──────────────────┬──────────────────────────┘
                   │ Handler calls
┌──────────────────▼──────────────────────────┐
│              BUSINESS LOGIC LAYER            │
│  Handlers + Validator + JsonUtil             │
│  Validation, code generation, TTL calc,      │
│  QR generation, JSON building                │
└──────────────────┬──────────────────────────┘
                   │ JDBC
┌──────────────────▼──────────────────────────┐
│               DATA LAYER                     │
│       Database.java + SQLite (urls.db)       │
│  Table: urls — code, alias, url, TTL         │
└─────────────────────────────────────────────┘
```

---

## 4. Complete Data Flow

### 4.1 Shortening a URL (Happy Path)

```
1. USER types "https://reddit.com/r/..." into the input field in React
2. USER optionally types "my-link" as alias, selects "7 days" TTL
3. USER clicks "Shorten"

4. React's handleShorten() fires:
   - Sets loading = true (button shows "Shortening...")
   - Builds body object: { url, ttlDays, alias }
   - Calls fetch("/shorten", { method: "POST", body: JSON.stringify(body) })

5. Vite dev server intercepts /shorten
   - Sees it matches the proxy rule
   - Forwards the request to http://localhost:8080/shorten
   - (In production, Nginx would do this same job)

6. Java HttpServer receives the request on port 8080
   - Matches /shorten context → dispatches to ShortenHandler
   - A thread from the fixed thread pool (size 4) picks it up

7. ShortenHandler.handle() runs:

   7a. Check request method = POST (reject OPTIONS/GET/etc)

   7b. Read request body bytes → convert to String (UTF-8)
       body = "{"url":"https://reddit.com/...","alias":"my-link","ttlDays":"7"}"

   7c. extractJsonValue(body, "url") → "https://reddit.com/..."
       extractJsonValue(body, "alias") → "my-link"
       extractJsonValue(body, "ttlDays") → "7"

   7d. Validator.isValidUrl("https://reddit.com/...") → true (starts with https://)

   7e. Parse ttlDays: Long.parseLong("7") = 7, within 1-365 range
       ttlSeconds = 7 * 24 * 60 * 60 = 604800

   7f. alias = "my-link".trim().toLowerCase() = "my-link"
       Validator.validateAlias("my-link"):
         - Length 7: between 3-30 ✓
         - Regex [a-z0-9][a-z0-9\-]*[a-z0-9]: matches ✓
         - Not in RESERVED_WORDS ✓
         - Not in BLOCKED_WORDS ✓
         - Returns null (null = valid)

   7g. Generate random 6-char code: e.g. "aB3xYz"
       Database.codeExists("aB3xYz") → false (not in DB)
       (loop exits — first try succeeded)

   7h. Timestamps:
       now = Instant.now().getEpochSecond() = 1783356000 (example)
       expiresAt = 1783356000 + 604800 = 1783960800

   7i. Open JDBC connection to urls.db
       PreparedStatement:
         INSERT INTO urls (code, alias, original_url, created_at, expires_at)
         VALUES ('aB3xYz', 'my-link', 'https://reddit.com/...', 1783356000, 1783960800)
       executeUpdate() → success (alias "my-link" not taken, UNIQUE constraint passes)

   7j. Build response JSON:
       shortUrl = "http://localhost:8080/r/my-link"  (alias takes priority over code)
       response = {"shortUrl":"http://localhost:8080/r/my-link","code":"aB3xYz","alias":"my-link","expiresAt":1783960800}

   7k. JsonUtil.sendJson(exchange, 200, response)
       Sets Content-Type: application/json
       Sets CORS headers
       Writes 200 with body bytes

8. Response travels back through Vite proxy to React fetch()

9. React's handleShorten() continues:
   - res.ok = true (status 200)
   - data = { shortUrl, code, alias, expiresAt }
   - setShortUrl(data.shortUrl) → result box appears with the short URL
   - setHistory(prev => [newItem, ...prev]) → history table updates
   - setInputUrl(""), setAlias("") → form clears
   - setLoading(false) → button returns to "Shorten"

10. USER sees the short URL with Copy and QR buttons
```

### 4.2 Redirecting (visiting the short URL)

```
1. USER clicks http://localhost:8080/r/my-link in browser

2. Browser sends GET /r/my-link to Java server
   (Note: this goes directly to :8080, NOT through Vite)
   (Vite proxy only applies to requests FROM the React app)

3. HttpServer matches /r/ context → RedirectHandler

4. RedirectHandler.handle():
   4a. path = "/r/my-link"
       codeOrAlias = "my-link" (substring after "/r/", lowercased)

   4b. PreparedStatement:
       SELECT original_url, expires_at, deleted FROM urls
       WHERE (LOWER(alias) = 'my-link' OR code = 'my-link')
       ORDER BY CASE WHEN LOWER(alias) = 'my-link' THEN 0 ELSE 1 END
       LIMIT 1

       The ORDER BY clause: if a row matches by alias, it gets sort value 0
       (higher priority). If it matches by code, it gets sort value 1.
       LIMIT 1 takes only the top result.
       This means alias always wins over code if both somehow match.

   4c. ResultSet has a row:
       original_url = "https://reddit.com/..."
       expires_at = 1783960800
       deleted = 0

   4d. Check deleted: 0 → not deleted, continue
   4e. Check expiry: Instant.now().getEpochSecond() = 1783356100
       1783356100 < 1783960800 → not expired, continue

   4f. Set response headers:
       Location: https://reddit.com/...
       Access-Control-Allow-Origin: *
       Status: 302

   4g. sendResponseHeaders(302, -1)
       -1 means: no response body, close connection

5. Browser receives 302 with Location header
6. Browser automatically follows the redirect
7. Browser navigates to https://reddit.com/...
8. USER arrives at Reddit (the server is no longer involved)
```

### 4.3 QR Code Generation

```
1. USER clicks "QR" button next to a short URL in React
2. setQrUrl(item.shortUrl) → modal opens
3. Modal renders: <img src={`/qr?url=${encodeURIComponent(shortUrl)}`} />
4. Browser sends GET /qr?url=http%3A%2F%2Flocalhost%3A8080%2Fr%2Fmy-link
5. Vite proxy forwards to http://localhost:8080/qr?url=...
6. QRHandler:
   - Parses query string to extract url param
   - URL-decodes it: "http://localhost:8080/r/my-link"
   - Calls generateQR(url, 300):
     - Creates QRCodeWriter
     - Sets hints: ERROR_CORRECTION = M, MARGIN = 2
     - writer.encode(url, BarcodeFormat.QR_CODE, 300, 300, hints) → BitMatrix
     - MatrixToImageWriter.writeToStream(matrix, "PNG", outputStream)
     - Returns byte[]
   - JsonUtil.sendImage(exchange, 200, pngBytes)
     - Sets Content-Type: image/png
     - Writes bytes directly
7. Browser receives PNG bytes
8. <img> tag renders the QR code image
9. USER sees QR code in the modal, can scan with phone
```

---

## 5. Database — Deep Dive

### 5.1 Why SQLite

SQLite is a file-based relational database. Unlike MySQL or PostgreSQL, there is no separate database server process to start — the database lives entirely in a single file (`urls.db`) and the SQLite engine runs inside your Java process itself via the JDBC driver.

**Concrete reasons for choosing SQLite here:**
- Zero setup: no installation, no configuration, no credentials, no running daemon
- The JDBC interface is identical to MySQL/PostgreSQL — `PreparedStatement`, `ResultSet`, `Connection` — so switching databases later is a matter of changing the connection string and swapping one JAR
- SQLite is ACID compliant — it has real transactions, real constraints, and real rollback
- For the scale this project operates at (thousands of URLs, not millions), SQLite is not a bottleneck

**What ACID means in this context:**
- **Atomicity:** An INSERT either fully succeeds or fully fails — you never get a half-inserted row
- **Consistency:** The UNIQUE constraint on `alias` is always enforced — the DB never enters a state with two rows having the same alias
- **Isolation:** Concurrent reads don't interfere with writes (SQLite uses file-level locking)
- **Durability:** Once an INSERT returns success, the data is written to disk

### 5.2 Full Schema with Every Decision Explained

```sql
CREATE TABLE IF NOT EXISTS urls (
    code         TEXT PRIMARY KEY,
    alias        TEXT UNIQUE,
    original_url TEXT NOT NULL,
    created_at   INTEGER NOT NULL,
    expires_at   INTEGER NOT NULL,
    deleted      INTEGER NOT NULL DEFAULT 0
);

CREATE INDEX IF NOT EXISTS idx_alias ON urls(alias);
```

**Column-by-column breakdown:**

**`code TEXT PRIMARY KEY`**
- The auto-generated 6-character random alphanumeric string (e.g. `"aB3xYz"`)
- `TEXT` because it's a string — SQLite's TEXT maps to Java's String cleanly
- `PRIMARY KEY` does two things: enforces uniqueness AND automatically creates an index on `code`. This means lookups by code (`WHERE code = ?`) are O(log n) not O(n)
- Why not INTEGER PRIMARY KEY (auto-increment)? Because the code IS the meaningful identifier — it's what appears in the URL. An integer would need to be encoded separately.

**`alias TEXT UNIQUE`**
- Optional custom alias chosen by the user (e.g. `"my-link"`)
- `TEXT` — same reason as code
- `UNIQUE` — enforces that no two rows can have the same alias. This is the race condition protection: two concurrent INSERTs with the same alias will result in exactly one succeeding and one failing with a constraint violation. The application layer catches this exception and returns HTTP 409.
- **Crucially nullable:** `UNIQUE` in SQL allows multiple `NULL` values because `NULL` means "no value" — it is not considered a duplicate of another `NULL`. This means rows without an alias (alias = SQL NULL) do not conflict with each other. This is standard SQL behaviour, not SQLite-specific.
- No `NOT NULL` — intentionally omitted so alias can be absent

**`original_url TEXT NOT NULL`**
- The full destination URL submitted by the user
- `TEXT NOT NULL` — a URL is always required. An empty string would mean the redirect has nowhere to go.
- Not indexed — we never query by original URL (we query by code or alias)

**`created_at INTEGER NOT NULL`**
- Unix epoch timestamp in seconds at the time of insertion
- `INTEGER` not `TEXT` or `DATETIME` — integers are simpler to compare, have no timezone ambiguity, and no parsing needed. `Instant.now().getEpochSecond()` gives a long which maps directly to SQLite INTEGER.
- `NOT NULL` — always set at insert time

**`expires_at INTEGER NOT NULL`**
- Unix epoch timestamp in seconds when this URL should stop working
- `INTEGER` for same reasons as `created_at`
- Used in every redirect check: `Instant.now().getEpochSecond() > expires_at`
- Also used in the `/all` query: `WHERE expires_at > ?` to filter out expired rows
- `NOT NULL` — every URL has a TTL (we set a default of 7 days if none provided)

**`deleted INTEGER NOT NULL DEFAULT 0`**
- Soft delete flag: 0 = active, 1 = deleted
- `INTEGER` not `BOOLEAN` — SQLite has no native boolean type. By convention, 0 = false, 1 = true.
- `DEFAULT 0` — on insert, if this column is not specified, it gets 0 automatically. Our INSERT statement never explicitly sets this column.
- Why soft delete instead of hard delete? So that deleted aliases are never reusable. If we hard-deleted (DELETE FROM urls WHERE code = ?), the alias would disappear from the UNIQUE index and someone else could claim it immediately. With soft delete, the row stays, the alias stays in the UNIQUE index, and it can never be claimed again.

### 5.3 The Index

```sql
CREATE INDEX IF NOT EXISTS idx_alias ON urls(alias);
```

The `PRIMARY KEY` on `code` already creates an index on code. But `UNIQUE` on alias also implicitly creates an index. So why this explicit `CREATE INDEX` statement?

Explicit creation makes it clear and intentional. It also ensures the index exists with a named identifier (`idx_alias`) which makes it easier to inspect or drop. The `IF NOT EXISTS` means it's safe to run `initDB()` on every startup without error.

**What an index does:** Instead of scanning every row to find `alias = 'my-link'` (O(n) scan), SQLite uses a B-tree structure where it can find the row in O(log n). For a table with 100,000 rows, that's the difference between checking 100,000 rows vs roughly 17 comparisons.

### 5.4 Why Not Add More Columns

Intentionally lean schema. Things deliberately not added:
- `click_count` — would require an UPDATE on every redirect (write load on the hot path)
- `user_id` — no authentication in this project
- `updated_at` — aliases can't be updated, so this has no meaning
- `ip_address` — privacy concern, not needed for core functionality

---

## 6. Backend — Deep Dive on Every Handler

### 6.1 Main.java — The Bootstrap

```java
HttpServer server = HttpServer.create(new InetSocketAddress(8080), 0);
server.createContext("/shorten", new ShortenHandler());
server.createContext("/r/",      new RedirectHandler());
server.createContext("/all",     new ListHandler());
server.createContext("/qr",      new QRHandler());
server.setExecutor(Executors.newFixedThreadPool(4));
server.start();
```

**`HttpServer.create(new InetSocketAddress(8080), 0)`**
- Creates a TCP server socket bound to port 8080 on all network interfaces
- The `0` is the backlog — how many pending connections to queue before refusing. `0` uses the system default (usually 50).

**`server.createContext("/path", handler)`**
- Registers a handler for a URL path prefix
- `/r/` with the trailing slash means it matches `/r/anything` — the handler receives the full path and extracts the code from it
- Contexts are prefix-matched: `/shorten` matches exactly `/shorten`. `/r/` matches `/r/abc`, `/r/xyz`, etc.

**`Executors.newFixedThreadPool(4)`**
- By default, `HttpServer` is single-threaded — every request would be handled one at a time, serially
- Setting an executor tells the server to dispatch each incoming request to a thread from the pool
- Pool size 4: up to 4 requests can be handled simultaneously
- Why 4? Reasonable default for a dev machine. In production this would be tuned based on CPU cores and expected concurrency.
- Without this line: if one request takes 100ms (e.g. slow DB query), all other simultaneous requests wait 100ms before even starting

**`Class.forName("org.sqlite.JDBC")`**
- Explicitly loads the SQLite JDBC driver class into the JVM
- JDBC drivers register themselves with `DriverManager` when their class is loaded
- Modern JDBC (4.0+) supports auto-loading via `ServiceLoader`, but explicitly loading is more reliable across environments and JAR configurations
- Must happen before any `DriverManager.getConnection()` call

### 6.2 ShortenHandler — POST /shorten

This is the most complex handler. Here is every step with the reasoning:

**Step 1: Method check**
```java
if ("OPTIONS".equalsIgnoreCase(exchange.getRequestMethod())) {
    JsonUtil.sendCors(exchange); return;
}
if (!"POST".equalsIgnoreCase(exchange.getRequestMethod())) {
    JsonUtil.sendJson(exchange, 405, "{\"error\":\"Method not allowed\"}"); return;
}
```
OPTIONS is the CORS preflight (explained in section 13). Every other non-POST method gets 405 Method Not Allowed. Using `equalsIgnoreCase` defensively — HTTP spec says method names are case-sensitive and always uppercase, but being lenient costs nothing.

**Step 2: Read body**
```java
String body = new String(exchange.getRequestBody().readAllBytes(), StandardCharsets.UTF_8);
```
`readAllBytes()` reads the entire request body into a byte array. Convert to String using UTF-8 — always specify the charset explicitly, never rely on the platform default.

**Step 3: Parse JSON manually**
We use `extractJsonValue()` from `JsonUtil` — a simple String search that finds `"key"` then finds the value between the next pair of quotes. This is intentionally minimal. A real production system would use Jackson or Gson, but:
- Adding a JSON library is another JAR dependency
- For a body with 3 known fields, manual parsing is readable and has no failure modes we don't control
- It's easier to explain in an interview

**Step 4: URL validation**
```java
if (!Validator.isValidUrl(originalUrl)) { ... return 400 ... }
```
Validation happens server-side even though the frontend also validates. Reason: the API is publicly accessible — anything can POST to `/shorten`. Never trust client-side validation alone.

**Step 5: TTL parsing**
```java
long days = Long.parseLong(ttlDaysStr);
if (days < 1 || days > 365) { ... return 400 ... }
ttlSeconds = days * 24 * 60 * 60;
```
`Long.parseLong` throws `NumberFormatException` if the string isn't a number — caught explicitly. Range check: 1 day minimum (no sub-day expiry), 365 days maximum (no permanent URLs in this system). Default is 7 days if ttlDays not provided.

**Step 6: Alias processing**
```java
alias = aliasRaw.trim().toLowerCase();
String validationError = Validator.validateAlias(alias);
```
Always lowercase before validation. `trim()` removes leading/trailing whitespace. Then validate format, reserved words, blocked words. If validation returns a non-null string, return 400 with that specific error message.

**Step 7: Code generation with collision avoidance**
```java
do { code = generateCode(); } while (Database.codeExists(code));
```
Do-while guarantees at least one attempt. `codeExists` queries the DB. At current scale (~millions of possible short URLs), collision probability on first try is extremely low (1 in 56 billion). The loop is a safety net, not an expected path.

**Step 8: INSERT with constraint handling**
```java
try {
    ps.executeUpdate(); // INSERT
} catch (SQLException e) {
    if (e.getMessage().contains("UNIQUE constraint failed: urls.alias")) {
        return 409; // Alias taken
    }
    return 500; // Other DB error
}
```
We do NOT check alias availability before inserting. This is deliberate — see Section 14 (Concurrency). We let the DB enforce uniqueness and catch the error. The exception message from SQLite always contains "UNIQUE constraint failed: urls.alias" when the alias column causes the violation, which is how we distinguish alias collision from other DB errors.

**Step 9: Build short URL**
```java
String shortUrl = "http://localhost:8080/r/" + (alias != null ? alias : code);
```
If alias is present, the short URL uses the alias. Otherwise, it uses the code. Both work because `RedirectHandler` checks both columns.

### 6.3 RedirectHandler — GET /r/{codeOrAlias}

This is the hot path — called every time someone visits a short URL. It must be fast and correct.

**Path extraction:**
```java
String codeOrAlias = path.substring("/r/".length()).toLowerCase().trim();
```
`/r/My-Link` → `my-link`. Lowercasing here ensures case-insensitive matching even if someone manually types the URL with different case.

**The SQL query:**
```sql
SELECT original_url, expires_at, deleted FROM urls
WHERE (LOWER(alias) = ? OR code = ?)
ORDER BY CASE WHEN LOWER(alias) = ? THEN 0 ELSE 1 END
LIMIT 1
```

This query does three things at once:
1. Matches by alias OR by code — so the same handler works for both `/r/my-link` and `/r/aB3xYz`
2. Prioritises alias over code via the `CASE` expression in `ORDER BY` — if somehow the same string is both an alias for one row and a code for another, the alias match wins (sort value 0 < 1)
3. `LIMIT 1` ensures we get at most one result

The PreparedStatement has 3 parameters (all set to `codeOrAlias`) for the three `?` placeholders.

**Response decision tree:**
```
Row not found → 404 Not Found (never existed)
deleted = 1   → 404 Not Found (treats deleted as if it never existed)
now > expires_at → 410 Gone (existed but is now expired)
all checks pass → 302 redirect with Location header
```

The distinction between 404 and 410 is semantically important and shows HTTP knowledge.

**The 302 redirect:**
```java
exchange.getResponseHeaders().set("Location", originalUrl);
exchange.sendResponseHeaders(302, -1);
exchange.getResponseBody().close();
```
`-1` as content length tells `HttpServer` there is no body. Must close the response body stream or the connection hangs.

### 6.4 ListHandler — GET /all

Returns all active URLs as a JSON array for the frontend history table.

**The SQL:**
```sql
SELECT code, alias, original_url, expires_at FROM urls
WHERE expires_at > ? AND deleted = 0
ORDER BY created_at DESC
```

`expires_at > now` filters out expired rows. `deleted = 0` filters out soft-deleted rows. `ORDER BY created_at DESC` means newest first in the UI.

**JSON building:**
Manual `StringBuilder` construction. Each row becomes a JSON object. The `alias` field is handled specially:
```java
"\"alias\":" + (al != null ? "\"" + al + "\"" : "null")
```
If alias is SQL NULL, `rs.getString("alias")` returns Java null. We output JSON `null` (not the string `"null"`) so the frontend can do `item.alias || "—"`.

**Short URL construction:**
```java
String shortUrl = "http://localhost:8080/r/" + (al != null ? al : code);
```
Same logic as ShortenHandler — alias takes priority if present.

### 6.5 QRHandler — GET /qr?url=...

**Query string parsing:**
```java
String query = exchange.getRequestURI().getQuery(); // "url=http%3A%2F%2F..."
for (String param : query.split("&")) {
    if (param.startsWith("url=")) {
        url = URLDecoder.decode(param.substring(4), StandardCharsets.UTF_8);
    }
}
```
`getQuery()` returns everything after `?`. Split by `&` for multiple params (though we only have one). `URLDecoder.decode` converts `%3A` → `:`, `%2F` → `/`, etc. — undoing the URL encoding the frontend applied with `encodeURIComponent`.

**QR generation:**
```java
QRCodeWriter writer = new QRCodeWriter();
Map<EncodeHintType, Object> hints = Map.of(
    EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.M,
    EncodeHintType.MARGIN, 2
);
BitMatrix matrix = writer.encode(url, BarcodeFormat.QR_CODE, 300, 300, hints);
ByteArrayOutputStream out = new ByteArrayOutputStream();
MatrixToImageWriter.writeToStream(matrix, "PNG", out);
```

- `QRCodeWriter`: ZXing class that converts a string into a QR code matrix
- `BitMatrix`: a 2D grid of true/false values where true = black cell, false = white cell
- `ErrorCorrectionLevel.M`: Medium error correction — can recover if ~15% of the QR code is obscured. Choices are L (7%), M (15%), Q (25%), H (30%). M is a good balance between data density and robustness.
- `MARGIN = 2`: padding around the QR code in cells. Standard is 4, we use 2 to keep it compact at 300px
- `MatrixToImageWriter.writeToStream`: converts the BitMatrix to PNG bytes
- `300, 300`: output image size in pixels

**Why generate from the short URL, not the original:**
QR codes encode character data. Shorter strings = less data = smaller, less dense, easier-to-scan QR matrix. A 6-character code like `http://localhost:8080/r/aB3xYz` produces a much simpler QR than a 200-character Reddit URL. Also, the QR code "inherits" the TTL — scanning it after the URL expires correctly returns 410.

---

## 7. Utility Classes — Deep Dive

### 7.1 Validator.java

Pure functions with no side effects. Takes input, returns a result. No HTTP, no DB.

**`isValidUrl(String url)`**
```java
return url.startsWith("http://") || url.startsWith("https://");
```
Deliberately simple. Does not use a regex or try to parse the URL. Reasons:
- URL parsing with `new URL(string)` throws a checked exception — clutters the call site
- A regex sophisticated enough to validate all valid URLs is extremely complex
- `startsWith` catches the most common input errors (missing protocol, typos like `htps://`)
- If it starts with http:// or https://, it's valid enough to attempt storing

**`validateAlias(String alias)`**
Returns null if valid, returns an error message String if invalid. The caller checks `if (validationError != null)` to decide whether to send a 400.

```java
// Length check
if (alias.length() < 3 || alias.length() > 30) { ... }

// Character/format check
if (!alias.matches("[a-z0-9][a-z0-9\\-]*[a-z0-9]") && !alias.matches("[a-z0-9]")) { ... }
```

The regex `[a-z0-9][a-z0-9\-]*[a-z0-9]`:
- First char: letter or digit (no hyphen at start)
- Middle chars: letters, digits, or hyphens (zero or more)
- Last char: letter or digit (no hyphen at end)
- Combined with `|| !alias.matches("[a-z0-9]")`: single-character aliases (3 chars minimum enforced above, so this handles exactly-3-char or just general single char safety)

Why this format? Common convention. GitHub, Bitly, most slug-based systems use this pattern. Hyphens in the middle are readable (`my-link` better than `mylink`). Starting/ending with hyphens is ugly and confusing.

**RESERVED_WORDS includes `"qr"`**: Because `/qr` is a route in our server. If someone created alias "qr", the redirect handler would match it, but then the QR handler would also match `/qr?url=...`. The reserved word prevents this ambiguity.

### 7.2 JsonUtil.java

**`extractJsonValue(String json, String key)`**
Minimal JSON field extractor. Finds `"key"` in the string, then finds the value between the next pair of double quotes. Limitations:
- Only works for string values (not numbers or booleans)
- Doesn't handle escaped quotes inside values
- Would break on nested JSON

For `ttlDays` which is a number, the frontend sends it as a string (`"ttlDays": "7"`) so extractJsonValue works. This is a deliberate simplification.

**`escapeJson(String s)`**
```java
return s.replace("\\", "\\\\").replace("\"", "\\\"");
```
Order matters: replace backslashes first, then quotes. If you did quotes first, the newly added `\"` would then have its backslash double-escaped. Applied to URLs and error messages before embedding in JSON strings.

**`sendJson` vs `sendImage`**
Two separate methods because the `Content-Type` header differs:
- `sendJson`: `Content-Type: application/json`
- `sendImage`: `Content-Type: image/png`

The browser uses `Content-Type` to know how to interpret the response. An `<img>` tag making a request needs `image/png` or it won't render.

**`sendCors`**
```java
exchange.sendResponseHeaders(200, -1);
exchange.getResponseBody().close();
```
Uses 200 instead of 204 (No Content) to avoid the Java HttpServer warning: `sendResponseHeaders: rCode = 204: forcing contentLen = -1`. The warning was harmless but noisy. 200 with -1 content length is equally correct for OPTIONS responses.

---

## 8. Short Code Generation — Algorithm Deep Dive

### 8.1 The Character Set — Base62

```java
String chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
// 26 lowercase + 26 uppercase + 10 digits = 62 characters
```

"Base62" means: a number system with 62 possible digits (instead of 10 for decimal or 2 for binary). Using 62 characters means 6 characters gives 62^6 = 56,800,235,584 (~56 billion) possible codes.

Why not more characters? URL-safe characters are limited. Characters like `/`, `?`, `#`, `&`, `=`, `%` have special meaning in URLs. Spaces aren't allowed. Adding `_` or `-` would be fine but adds marginal value.

### 8.2 The Generation Loop

```java
private String generateCode() {
    String chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < 6; i++) {
        sb.append(chars.charAt((int) (Math.random() * chars.length())));
    }
    return sb.toString();
}
```

`Math.random()` returns a double in [0.0, 1.0). Multiplied by 62, gives [0.0, 62.0). Cast to int gives 0–61. Used as index into chars string.

### 8.3 Collision Probability

With n existing codes and a space of 56 billion:

- At 1,000 URLs: probability of collision ≈ 1,000 / 56,000,000,000 ≈ 0.0000018% per attempt
- At 1,000,000 URLs: ≈ 0.0018% per attempt
- Collision only becomes a real concern at hundreds of millions of URLs

The do-while loop handles it:
```java
do { code = generateCode(); } while (Database.codeExists(code));
```
In practice for this project, the loop runs exactly once almost every time.

### 8.4 Theoretical Weakness: `Math.random()`

`Math.random()` uses `java.util.Random` internally which is a pseudo-random number generator (PRNG) — it's deterministic given a seed. In a security-critical system (where codes must be unguessable), you'd use `SecureRandom`:

```java
// Production upgrade
SecureRandom random = new SecureRandom();
// replace Math.random() with:
chars.charAt(random.nextInt(chars.length()))
```

For this project, `Math.random()` is acceptable — we're not storing secrets, just URL mappings.

### 8.5 The Alternative: Base62 Encoding of Auto-Increment ID

The production approach used by services like Bitly:

```java
public static String encode(long id) {
    String chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    StringBuilder sb = new StringBuilder();
    while (id > 0) {
        sb.append(chars.charAt((int)(id % 62)));
        id /= 62;
    }
    return sb.reverse().toString();
}
```

- ID 1 → `"1"` (index 27 in chars → `"b"` actually, depends on chars ordering)
- Each DB row has an auto-increment integer ID
- No collision possible because each ID is unique by DB design
- No existence check needed
- Downside: sequential codes (ID 1001 → `"Pb"`, ID 1002 → `"Qb"`) — predictable

Fix for predictability: shuffle the `chars` string. The encoding is still unique but the output looks random.

---

## 9. Custom Alias System — Deep Dive

### 9.1 The Full Validation Pipeline

Every alias goes through this sequence before touching the database:

```
1. Null/blank check → skip alias processing entirely if blank
2. trim() → remove whitespace
3. toLowerCase() → normalise case
4. validateAlias():
   a. Length: 3–30 chars
   b. Format regex: letters/digits/hyphens, no leading/trailing hyphens
   c. Reserved words: "all", "r", "shorten", "api", "admin", etc. + "qr"
   d. Blocked words: brand names
5. INSERT with UNIQUE constraint as final safety net
```

### 9.2 Why the Order Matters

We validate BEFORE hitting the database. If we called the DB first, we'd waste a connection and query on obviously invalid input. The pipeline is cheapest-first: string operations (free) → DB query (costs a connection + disk read).

### 9.3 Why Aliases Are Always Lowercased

```java
alias = aliasRaw.trim().toLowerCase();
```

Done at input time, stored lowercase in DB, matched with `LOWER(alias)` in SQL. This means:
- `MY-LINK` and `my-link` and `My-Link` are all treated as `my-link`
- Stored as `my-link` in DB
- Queried as `LOWER(alias) = 'my-link'`
- No two people can claim the same alias with different capitalisations

Alternative: store as-is, match case-insensitively in SQL. Chosen against because: storing canonical lowercase is cleaner, and LOWER() on an indexed column prevents the index from being used efficiently in some DB engines (though SQLite handles it fine).

### 9.4 SQL NULL Behaviour for UNIQUE

This is a subtle but important SQL fact. In our schema:
```sql
alias TEXT UNIQUE
```

No `NOT NULL`. This allows SQL `NULL` in the alias column. SQL's `UNIQUE` constraint treats each `NULL` as a distinct value — `NULL` is not equal to `NULL` in SQL (because `NULL` means "unknown", and two unknowns are not necessarily the same unknown). Therefore:

- Row 1: alias = NULL ← allowed
- Row 2: alias = NULL ← also allowed (not a constraint violation)
- Row 3: alias = "my-link" ← allowed
- Row 4: alias = "my-link" ← CONSTRAINT VIOLATION

This is exactly what we want: rows without aliases don't conflict with each other, but rows with the same alias do.

### 9.5 Alias Reuse Policy

When a URL expires, its alias is NOT freed. The row remains in the DB with the alias still set. The UNIQUE constraint still holds. This means once `"summer-sale"` is claimed, it can never be claimed again — even after expiry.

Why? The alternative — freeing aliases after expiry — risks someone intentionally waiting for a popular alias to expire and then claiming it to capture traffic from old QR codes, printed materials, or cached links. Not reusing is safer.

---

## 10. TTL and Expiry System — Deep Dive

### 10.1 How TTL is Stored

TTL is stored as an absolute Unix timestamp (`expires_at`), not as a duration. At insert time:

```java
long now = Instant.now().getEpochSecond();       // e.g. 1783356000
long expiresAt = now + (ttlDays * 24 * 60 * 60); // e.g. 1783960800 (7 days later)
```

Why absolute timestamp, not duration?
- Duration would require arithmetic on every read: `created_at + duration > now`
- Absolute timestamp is a single comparison: `expires_at > now`
- Absolute timestamp is also more intuitive to inspect in the DB: you can convert `1783960800` to a date and immediately understand when it expires

### 10.2 Expiry Check Location

Expiry is checked in TWO places:

**In RedirectHandler (on every redirect):**
```java
if (Instant.now().getEpochSecond() > rs.getLong("expires_at")) {
    JsonUtil.sendJson(exchange, 410, "{\"error\":\"This short URL has expired\"}");
    return;
}
```

**In ListHandler (when listing URLs):**
```sql
WHERE expires_at > ? AND deleted = 0
```

The list only shows non-expired URLs, so the frontend history table stays clean. The redirect checks on every hit, so even if a URL is in someone's browser history or a cached page, visiting it after expiry gives the correct 410.

### 10.3 HTTP 410 Gone — Why Not 404

| Status | Meaning | When to Use |
|---|---|---|
| `404 Not Found` | Resource does not exist | Code was never created |
| `410 Gone` | Resource existed but is permanently gone | URL was created, has now expired |

Using 410 gives browsers and crawlers accurate information:
- Crawlers that receive 404 might retry later thinking it was a temporary error
- Crawlers that receive 410 remove the URL from their index permanently
- Users get a more specific error message

### 10.4 Expired Rows Are Never Deleted

There is no cleanup job that deletes expired rows. They stay in the DB. This is acceptable for a portfolio project. Implications:
- DB grows over time (but slowly for URL shortener usage)
- Expired aliases are never freed (intentional — see Section 9.5)
- The `WHERE expires_at > ?` filter in queries handles correctness

Upgrade path: a background thread (or scheduled job) that runs `DELETE FROM urls WHERE expires_at < ? AND deleted = 0` nightly.

---

## 11. QR Code System — Deep Dive

### 11.1 How QR Codes Work (Conceptually)

A QR code is a 2D matrix of black and white cells. Each cell is a bit (black = 1, white = 0). The pattern of bits encodes the data using the QR code specification (ISO 18004). Specific regions of the QR matrix have fixed meaning:
- Three corner squares: finder patterns (let the scanner know where the QR is and its orientation)
- Timing patterns: help determine cell size
- Data cells: the actual encoded content

Error correction: extra data is added so the QR can be decoded even if part is obscured (logo overlay, dirt, damage). Level M (our choice) handles up to 15% damage.

### 11.2 ZXing's Role

ZXing handles the entire QR encoding — we just provide a string and get back a `BitMatrix`. The `BitMatrix` is a 2D array of booleans (300×300 in our case). `MatrixToImageWriter.writeToStream` converts this to a PNG image where `true` cells become black pixels and `false` cells become white pixels.

We never need to understand the QR encoding algorithm itself — we just use the library. This is the right answer in an interview: "I used ZXing, which is Google's open-source QR library, the industry standard for Java. I configured error correction level M and a margin of 2 cells."

### 11.3 The Request/Response Cycle

The frontend renders QR codes as `<img>` tags with a `src` pointing to our backend:
```jsx
<img src={`/qr?url=${encodeURIComponent(shortUrl)}`} />
```

When the browser renders this `<img>` tag, it automatically sends a GET request to that URL. The backend returns raw PNG bytes with `Content-Type: image/png`. The browser renders the bytes as an image without any JavaScript needed. This is a simple and elegant pattern — the browser's native image loading does all the work.

`encodeURIComponent` converts special characters in the URL: `:` → `%3A`, `/` → `%2F`, etc. This is necessary because the URL is being passed as a query parameter inside another URL. Without encoding, the browser would misparse the inner URL's slashes as part of the outer URL's path.

### 11.4 Why QR is Stateless

The QR handler does not touch the database. It generates the QR image purely from the URL string passed to it. This means:
- It can be called with any URL, not just ones in our DB
- It's pure — same input always gives same output
- It's fast — no DB query on the hot path

A design choice: we could verify the URL exists in our DB before generating the QR. Decided against it — adds a DB query for no real benefit. The QR just encodes whatever string you give it.

---

## 12. HTTP Fundamentals Used in This Project

### 12.1 HTTP Request Structure

Every HTTP request has:
- **Method**: GET, POST, PUT, DELETE, OPTIONS, etc.
- **Path**: `/shorten`, `/r/my-link`
- **Headers**: `Content-Type: application/json`, `Origin: http://localhost:5173`
- **Body**: (for POST) the JSON payload

In our handlers, we access these via:
```java
exchange.getRequestMethod()   // "POST"
exchange.getRequestURI()      // URI object with .getPath(), .getQuery()
exchange.getRequestHeaders()  // Headers map
exchange.getRequestBody()     // InputStream for the body
```

### 12.2 HTTP Response Structure

Every HTTP response has:
- **Status code**: 200, 302, 400, 404, 409, 410, 500
- **Headers**: `Content-Type`, `Location`, `Access-Control-Allow-Origin`
- **Body**: JSON string, PNG bytes, or empty

```java
exchange.getResponseHeaders().set("Content-Type", "application/json");
exchange.sendResponseHeaders(200, bytes.length); // status + content-length
exchange.getResponseBody().write(bytes);
exchange.getResponseBody().close();
```

**Important:** `sendResponseHeaders` must be called before writing the body. The content length must match the actual bytes written, or the response will be truncated or the connection will hang.

### 12.3 Status Codes Used and Why

| Code | Name | Used For |
|---|---|---|
| 200 | OK | Successful shorten, successful list, successful QR |
| 302 | Found (Temporary Redirect) | Redirect to original URL |
| 400 | Bad Request | Invalid URL, bad alias format, bad TTL |
| 404 | Not Found | Code/alias doesn't exist in DB |
| 405 | Method Not Allowed | Wrong HTTP method used |
| 409 | Conflict | Alias already taken |
| 410 | Gone | URL existed but has expired |
| 500 | Internal Server Error | Database errors, unexpected exceptions |

### 12.4 The Redirect in Detail

HTTP redirect is a two-step dance:
1. Client requests short URL → server responds 302 with `Location` header
2. Client automatically sends a NEW request to the `Location` URL
3. Client loads the destination

The client handles step 2 automatically — browsers, `curl`, most HTTP clients follow redirects by default. The server is only involved in step 1.

**301 vs 302:**
- 301 Permanent: browser caches the redirect. Subsequent visits go directly to the destination without hitting our server.
- 302 Temporary: browser re-checks with our server on every visit.

We use 302 because we need to enforce TTL on every access. With 301, a browser that visited a URL before expiry would cache the redirect and continue using it even after expiry — we'd have no way to show the 410 error.

---

## 13. CORS — What It Is and How We Handle It

### 13.1 What CORS Is

CORS (Cross-Origin Resource Sharing) is a browser security mechanism. The "same-origin policy" says: JavaScript code on `http://localhost:5173` cannot make requests to `http://localhost:8080` (different port = different origin). Without permission, the browser blocks the response.

CORS is the permission system. The backend server tells the browser: "it's okay, I allow requests from other origins."

### 13.2 The Preflight Request

For POST requests with `Content-Type: application/json`, the browser sends a "preflight" OPTIONS request before the actual request:

```
OPTIONS /shorten HTTP/1.1
Origin: http://localhost:5173
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type
```

The browser is asking: "Can I send a POST request with a JSON body from localhost:5173?" Our server must respond affirmatively:

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, OPTIONS
Access-Control-Allow-Headers: Content-Type
```

Only then does the browser send the actual POST.

### 13.3 Our CORS Implementation

We set these headers on EVERY response (not just OPTIONS):
```java
exchange.getResponseHeaders().set("Access-Control-Allow-Origin", "*");
exchange.getResponseHeaders().set("Access-Control-Allow-Methods", "GET, POST, OPTIONS");
exchange.getResponseHeaders().set("Access-Control-Allow-Headers", "Content-Type");
```

`Access-Control-Allow-Origin: *` means any origin is allowed. In production, you'd restrict this to your specific frontend domain: `Access-Control-Allow-Origin: https://yourapp.com`.

CORS is enforced by browsers only — it doesn't protect against server-to-server requests or `curl`. It's a browser safety feature, not an authentication mechanism.

---

## 14. Concurrency and Thread Safety

### 14.1 The Thread Pool

```java
server.setExecutor(Executors.newFixedThreadPool(4));
```

Four threads handle requests concurrently. This means four requests can be in-flight simultaneously. Without this, requests would queue up and each would wait for the previous to complete.

### 14.2 The Race Condition — and Why the DB Solves It

**The naive (wrong) approach:**
```
Thread A: SELECT — alias "promo" doesn't exist
Thread B: SELECT — alias "promo" doesn't exist
Thread A: INSERT alias "promo" — success
Thread B: INSERT alias "promo" — success (duplicate! data corruption)
```

**Our approach — let the DB enforce it:**
```
Thread A: INSERT alias "promo" — success (UNIQUE constraint passes)
Thread B: INSERT alias "promo" — FAILS with "UNIQUE constraint failed: urls.alias"
Thread B: catches SQLException → returns 409 to client
```

The database handles concurrent writes correctly at the storage level using file-level locking (SQLite) or row-level locking (MySQL/PostgreSQL). We never need application-level locking (synchronized blocks, mutexes) for this operation. The rule: when the database can enforce a constraint, always prefer that over application-level checks.

### 14.3 Connection-Per-Request Pattern

```java
private static Connection getConnection() throws SQLException {
    return DriverManager.getConnection(DB_URL);
}
```

Every handler call opens a new JDBC connection and closes it (via try-with-resources). This is simple but not optimal — opening a connection has overhead (file open, driver initialisation).

**Production upgrade:** a connection pool (HikariCP is the standard). Pre-opens N connections, hands them out on request, returns them to the pool when done. Eliminates per-request connection overhead.

For this project, connection-per-request is acceptable — SQLite connections are cheap and the request volume is low.

### 14.4 HashMap No Longer Used

The original version used a static `HashMap<String, String>` to store URL mappings. This had a concurrency bug: `HashMap` is not thread-safe. Concurrent reads and writes to a HashMap can cause infinite loops or data corruption in Java. The fix would have been `ConcurrentHashMap`. But we replaced the HashMap entirely with SQLite, which handles concurrency correctly. So this is no longer an issue — but worth knowing for the "how did you handle thread safety" question.

---

## 15. Input Validation — Every Rule and Why

### 15.1 URL Validation

**Rule:** Must start with `http://` or `https://`
**Why:** Prevents empty strings, prevents storing relative paths, ensures the redirect `Location` header has a valid absolute URL. HTTP redirects require an absolute URL in the `Location` header.

**What's not validated:**
- Whether the URL actually resolves (would require an HTTP request to the target — too slow, too complex)
- Whether the domain exists
- Whether the path is valid

These are acceptable omissions for a portfolio project. Production systems like Bitly do check if the URL is reachable.

### 15.2 Alias Validation — Every Rule

| Rule | Implementation | Reason |
|---|---|---|
| 3 character minimum | `alias.length() < 3` | Very short aliases (1-2 chars) would exhaust the short namespace quickly and look odd |
| 30 character maximum | `alias.length() > 30` | Aliases should be memorable and short — 30 is generous |
| Letters, digits, hyphens only | Regex `[a-z0-9][a-z0-9\-]*[a-z0-9]` | Special characters in URL paths cause encoding/parsing issues |
| No leading hyphen | First char must be `[a-z0-9]` | Looks wrong, convention against it |
| No trailing hyphen | Last char must be `[a-z0-9]` | Looks wrong, convention against it |
| Reserved words blocked | Set lookup | Prevents breaking app routes |
| Brand names blocked | Set lookup | Prevents impersonation / misleading links |
| Always lowercased | `.toLowerCase()` before validation | Case-insensitive matching, canonical storage |

### 15.3 TTL Validation

| Rule | Reason |
|---|---|
| Must be parseable as a long integer | `Long.parseLong` with NumberFormatException catch |
| Minimum 1 day | Sub-day expiry not supported by the UI (no hour/minute options) |
| Maximum 365 days | No permanent URLs — forces periodic renewal, keeps DB from growing indefinitely |
| Default 7 days if not provided | Sensible default if frontend omits the field |

### 15.4 Validation is on Both Frontend and Backend

Frontend validates because it gives instant feedback to the user without a round trip.
Backend validates because the API is publicly accessible — anyone with `curl` can call `/shorten`. Never trust client-side validation alone. The backend is the last line of defence.

---

## 16. Frontend Architecture

### 16.1 State Management

All state lives in a single `App.jsx` component using React hooks:

| State variable | Type | Purpose |
|---|---|---|
| `inputUrl` | string | Controlled input for the URL field |
| `alias` | string | Controlled input for the alias field |
| `ttlDays` | number | Controlled select for TTL |
| `shortUrl` | string or null | The result short URL to display |
| `error` | string or null | Error message to display below form |
| `loading` | boolean | Disables submit button during request |
| `history` | array | All active URLs (from /all + new ones added) |
| `copied` | boolean | Briefly true after copy-to-clipboard |
| `qrUrl` | string or null | URL to show QR for — null = modal closed |

### 16.2 Data Fetching Pattern

**On mount (useEffect with empty deps array):**
```jsx
useEffect(() => {
    fetch("/all")
        .then(r => r.json())
        .then(setHistory)
        .catch(() => {});
}, []);
```

Loads existing URLs so the history table is populated even on page refresh. The empty `catch` swallows errors silently — if the backend isn't running, the history just stays empty rather than showing an error.

**On form submit:**
```jsx
async function handleShorten(e) {
    e.preventDefault(); // Prevents page reload (default form behaviour)
    // ... fetch POST ...
}
```

`e.preventDefault()` is critical — without it, the browser would reload the page on form submit.

### 16.3 Optimistic UI Update

After a successful shorten:
```jsx
setHistory(prev => [newItem, ...prev]);
```

We add the new URL to the history immediately without re-fetching `/all`. This is the correct pattern — we already have the data from the server's response, so there's no need for an extra round trip.

### 16.4 QR Modal Pattern

```jsx
{qrUrl && (
    <div style={styles.modalOverlay} onClick={() => setQrUrl(null)}>
        <div style={styles.modal} onClick={e => e.stopPropagation()}>
            <img src={`/qr?url=${encodeURIComponent(qrUrl)}`} />
        </div>
    </div>
)}
```

`qrUrl` is null → modal not rendered. `qrUrl` is set → modal renders. Click on overlay → `setQrUrl(null)` closes modal. `e.stopPropagation()` on the inner div prevents clicks inside the modal from bubbling up to the overlay and closing it.

The `<img src>` with the QR URL causes the browser to automatically fetch the QR image from the backend. No JavaScript needed for the image fetch — it's just a standard img tag.

---

## 17. Vite Proxy — What It Is and Why It Matters

### 17.1 The Problem Without Proxy

Without the proxy:
- React runs on `http://localhost:5173`
- Backend runs on `http://localhost:8080`
- Frontend code would need `fetch("http://localhost:8080/shorten", ...)`
- This hardcodes the backend URL into the frontend

Why hardcoding is bad:
- In production, the backend would be at a different URL
- You'd need different code for dev vs prod
- CORS issues become more complex

### 17.2 How Vite Proxy Works

```js
server: {
    proxy: {
        "/shorten": "http://localhost:8080",
        "/r":       "http://localhost:8080",
        "/all":     "http://localhost:8080",
        "/qr":      "http://localhost:8080",
    }
}
```

When the Vite dev server receives a request to `/shorten`, it forwards (proxies) it to `http://localhost:8080/shorten`. From the browser's perspective, everything is on `http://localhost:5173`. No CORS issue because the browser doesn't know the request is being forwarded.

Frontend code becomes:
```javascript
fetch("/shorten", ...) // works in dev (Vite proxies it)
fetch("/shorten", ...) // works in prod (Nginx proxies it)
```

Same code, different proxy configuration. This is the correct pattern.

### 17.3 The Production Equivalent

In production, an Nginx server would do the same job:
```nginx
location /shorten { proxy_pass http://localhost:8080; }
location /r/      { proxy_pass http://localhost:8080; }
location /all     { proxy_pass http://localhost:8080; }
location /qr      { proxy_pass http://localhost:8080; }
location /        { root /var/www/url-shortener/dist; }
```

The Vite-built static files go in `/dist`, served by Nginx. API calls are proxied to the Java backend. Same architecture, just Vite replaced by Nginx.

---

## 18. Every Design Decision with Reasoning

| Decision | What We Did | Why |
|---|---|---|
| No framework | Raw `com.sun.net.httpserver` | Forces understanding of HTTP fundamentals; every line is explainable |
| SQLite not MySQL | Single file, zero setup | No infrastructure; JDBC interface is identical for both |
| UNIQUE constraint for alias | DB-enforced, not app-enforced | Handles race conditions correctly without application locking |
| Soft delete (`deleted` flag) | Row stays in DB | Aliases are never reused — prevents alias hijacking |
| 302 not 301 redirect | Temporary redirect | Allows TTL enforcement on every visit |
| 410 not 404 for expired | 410 Gone | Semantically correct; tells crawlers the resource existed but is gone |
| Unix timestamps not SQL dates | `INTEGER` column | No timezone ambiguity, simple integer comparison, no parsing |
| Random Base62 not hash | Random 6-char string | Simple, non-guessable, no deterministic collision issues |
| No JSON library | Manual `extractJsonValue` | Zero extra dependencies; only 3 fields to parse |
| Alias always lowercased | `.toLowerCase()` at input | Canonical storage; prevents case-confusion duplicates |
| Aliases can never be reused | Soft delete + UNIQUE | Prevents alias hijacking after expiry |
| QR from short URL | Not from original URL | Shorter string = simpler QR; QR inherits TTL |
| Connection per request | New connection each time | Simple; acceptable at low scale |
| Thread pool size 4 | `newFixedThreadPool(4)` | Handles concurrent requests; tunable |
| Default TTL 7 days | Constant in ShortenHandler | Sensible default; all URLs have an expiry |
| Vite proxy | `/shorten` not `http://localhost:8080/shorten` | Frontend code doesn't hardcode backend URL |
| Manual CORS headers | Set on every response | Required for browser security model |
| `Class.forName` for JDBC | Explicit driver loading | More reliable than auto-loading across environments |
| Packages (`db/`, `handlers/`, `util/`) | Split by responsibility | Single Responsibility Principle; easy to navigate |
| `qr` in RESERVED_WORDS | Prevents alias "qr" | Would conflict with the `/qr` route |

---

## 19. Alternatives Considered and Rejected

### Backend Framework

**Spring Boot:**
- Pro: industry standard, tons of features, built-in dependency injection, auto-configuration
- Con: hides HTTP details behind annotations; harder to explain "what actually happens"; slow startup; overkill for a portfolio project
- Rejected: defeats the purpose of showing HTTP fundamentals

**Jetty / Netty:**
- Pro: proper production HTTP servers, much better than `com.sun.net.httpserver`
- Con: external dependencies; still more framework than we want
- Rejected: would need build tool (Maven/Gradle) just to manage the JAR

### Database

**MySQL / PostgreSQL:**
- Pro: production-ready, scales horizontally, rich feature set
- Con: requires running a separate server process, credentials, setup
- Rejected: too much setup friction for a portfolio project; JDBC code would be identical

**H2 (in-memory Java DB):**
- Pro: pure Java, zero setup, embedded
- Con: in-memory only by default (data lost on restart); file mode is less well-known
- Rejected: SQLite is more universally recognised; SQLite file mode is simpler to configure

**MongoDB / NoSQL:**
- Pro: flexible schema, JSON-native
- Con: loses SQL semantics (no UNIQUE constraint); harder to reason about; no real benefit for this data model
- Rejected: URL mappings are a perfect relational data model; no reason for NoSQL

### Build Tool

**Maven / Gradle:**
- Pro: dependency management, reproducible builds, IDE integration
- Con: adds complexity; requires learning a build tool on top of everything else; we only have 3 JARs
- Rejected: manual JAR management is simpler for 3 dependencies; shows you understand the classpath

### Short Code Generation

**MD5/SHA256 hash of URL:**
- Pro: deterministic — same URL always gives same code
- Con: hexadecimal output only (0-9 and a-f) = 16^6 = 16 million combinations vs 62^6 = 56 billion; truncation causes collisions; MD5 is cryptographically broken
- Rejected: fewer combinations, more collision-prone, sounds bad in interview

**UUID:**
- Pro: guaranteed unique, no DB check needed
- Con: 36 characters is too long for a short URL; ugly
- Rejected: defeats the purpose of a URL shortener

**Auto-increment ID + Base62:**
- Pro: zero collision, no check needed, scales to billions
- Con: sequential codes are predictable (enumerable)
- Considered valid alternative: mentioned as upgrade path, not implemented to keep code simple

### JSON Handling

**Jackson / Gson:**
- Pro: proper JSON parsing, handles all edge cases, type-safe
- Con: another JAR dependency; for 3 fields, heavyweight
- Rejected: manual parsing is simpler and fully explainable for this scope

---

## 20. Known Limitations and Upgrade Path

### Current Limitations

| Limitation | Impact | Upgrade |
|---|---|---|
| `Math.random()` not `SecureRandom` | Codes theoretically predictable | Replace with `SecureRandom.nextInt()` |
| Connection per request (no pool) | Connection overhead on every request | Add HikariCP connection pool |
| No expired row cleanup | DB grows indefinitely | Background thread running nightly DELETE |
| Single server, single DB file | Can't scale horizontally | Postgres + connection string change |
| No HTTPS | Traffic not encrypted | Put Nginx with SSL in front |
| `localhost:8080` hardcoded in URLs | Won't work on real domain | Make base URL configurable via env var |
| No rate limiting | Single IP can spam `/shorten` | Token bucket per IP in a request filter |
| Manual JSON building | Fragile if values contain quotes | We use `escapeJson()` as a fix, but Jackson is safer |
| No request logging | Can't debug production issues | Add a logging handler or interceptor |
| QR only works locally | QR encodes localhost URL, not scannable externally | Use real domain, make base URL configurable |

### The QR Localhost Issue

QR codes encode `http://localhost:8080/r/my-link`. If you scan this on your phone, your phone will try to connect to `localhost` — which means your phone's own localhost, not your computer's. It won't work. This is fundamentally a deployment issue: once the app is deployed with a real domain, QR codes work perfectly. Mentioning this proactively in an interview shows awareness.

### Upgrade Sequence (in priority order)

1. **Make base URL configurable:** Read from environment variable: `System.getenv("BASE_URL")`, default to `http://localhost:8080`. Affects URL construction in ShortenHandler and ListHandler. Fixes QR codes in production.
2. **Add HikariCP connection pool:** Replace `DriverManager.getConnection()` with pool. Significant performance improvement.
3. **Switch to PostgreSQL:** Change `DB_URL` constant, swap JAR. Everything else (SQL, JDBC calls) stays the same.
4. **Add Redis cache for redirects:** Cache `code → original_url` mapping. Redirects become memory lookups.
5. **Add `SecureRandom`:** Single line change in `generateCode()`.
6. **Add rate limiting:** Track requests per IP per minute in a `ConcurrentHashMap<String, AtomicInteger>`. Reset every minute via `ScheduledExecutorService`.
7. **Add HTTPS:** Deploy behind Nginx with Let's Encrypt certificate. Java code unchanged.
8. **Add a cleanup job:** `ScheduledExecutorService` running `DELETE FROM urls WHERE expires_at < ?` once per day.

---

*This document covers the full depth of the project as built. For system design at scale, horizontal scaling strategies, and production architecture — see 02-system-design.md.*