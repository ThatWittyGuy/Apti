# 02 — Diagrams: URL Shortener Project
### All Architecture Diagrams — Renderable in GitHub, VS Code, mermaid.live

> **How to view:** Paste any diagram block into [mermaid.live](https://mermaid.live), or view this file on GitHub (renders automatically), or install the "Markdown Preview Mermaid Support" VS Code extension.

---

## Table of Contents

1. [System Architecture — Full Overview](#1-system-architecture--full-overview)
2. [High-Level Design (HLD)](#2-high-level-design-hld)
3. [Low-Level Design (LLD) — Backend Class Structure](#3-low-level-design-lld--backend-class-structure)
4. [ER Diagram — Database Schema](#4-er-diagram--database-schema)
5. [Request Lifecycle — POST /shorten (Full Sequence)](#5-request-lifecycle--post-shorten-full-sequence)
6. [Request Lifecycle — GET /r/{codeOrAlias} (Redirect)](#6-request-lifecycle--get-rcodeorialias-redirect)
7. [Request Lifecycle — GET /qr (QR Code Generation)](#7-request-lifecycle--get-qr-qr-code-generation)
8. [Request Lifecycle — GET /all (List URLs)](#8-request-lifecycle--get-all-list-urls)
9. [Frontend Component Hierarchy](#9-frontend-component-hierarchy)
10. [Frontend State Machine](#10-frontend-state-machine)
11. [Alias Validation Flow](#11-alias-validation-flow)
12. [Short Code Generation Algorithm](#12-short-code-generation-algorithm)
13. [TTL and Expiry Decision Tree](#13-ttl-and-expiry-decision-tree)
14. [Concurrency — Race Condition Handling](#14-concurrency--race-condition-handling)
15. [Deployment Diagram — Current Dev Setup](#15-deployment-diagram--current-dev-setup)
16. [Deployment Diagram — Ideal Production Setup](#16-deployment-diagram--ideal-production-setup)
17. [Package Dependency Graph](#17-package-dependency-graph)
18. [Data Lifecycle — A URL from Birth to Expiry](#18-data-lifecycle--a-url-from-birth-to-expiry)
19. [CORS Preflight Flow](#19-cors-preflight-flow)
20. [Handler Routing Map](#20-handler-routing-map)

---

## 1. System Architecture — Full Overview

Shows every component in the system and how they connect.

```mermaid
graph TB
    subgraph CLIENT["CLIENT — Browser"]
        UI["React App\n(App.jsx)\nlocalhost:5173"]
    end

    subgraph VITE["VITE DEV SERVER\nlocalhost:5173"]
        PROXY["Proxy Layer\nvite.config.js\n/shorten → :8080\n/r → :8080\n/all → :8080\n/qr → :8080"]
    end

    subgraph JAVA["JAVA BACKEND — localhost:8080"]
        MAIN["Main.java\nHttpServer\nThreadPool(4)"]

        subgraph HANDLERS["handlers/"]
            SH["ShortenHandler\nPOST /shorten"]
            RH["RedirectHandler\nGET /r/{code|alias}"]
            LH["ListHandler\nGET /all"]
            QH["QRHandler\nGET /qr?url=..."]
        end

        subgraph UTIL["util/"]
            VAL["Validator.java\nisValidUrl()\nvalidateAlias()"]
            JU["JsonUtil.java\nsendJson()\nsendImage()\nsendCors()\nextractJsonValue()"]
        end

        subgraph DB_LAYER["db/"]
            DBC["Database.java\ninit()\ngetConnection()\ncodeExists()"]
        end
    end

    subgraph STORAGE["STORAGE"]
        SQLITE[("SQLite\nurls.db\n\nTable: urls\ncode PK\nalias UNIQUE\noriginal_url\ncreated_at\nexpires_at\ndeleted")]
        ZXING["ZXing Library\ncore-3.5.2.jar\njavase-3.5.2.jar\nQR Generation"]
    end

    UI -->|"fetch('/shorten')\nfetch('/all')\nfetch('/qr?url=...')| PROXY
    PROXY -->|"forwards to :8080"| MAIN
    UI -->|"Browser navigates\nGET /r/code directly"| MAIN
    MAIN --> SH
    MAIN --> RH
    MAIN --> LH
    MAIN --> QH
    SH --> VAL
    SH --> JU
    SH --> DBC
    RH --> JU
    RH --> DBC
    LH --> JU
    LH --> DBC
    QH --> JU
    QH --> ZXING
    DBC -->|"JDBC\nDriverManager"| SQLITE
```

---

## 2. High-Level Design (HLD)

Shows the system from a 30,000-foot view — layers and responsibilities only, no code details.

```mermaid
graph LR
    subgraph PRESENTATION["PRESENTATION LAYER"]
        FE["React Frontend\nUser Interface\nForm / Table / Modal"]
    end

    subgraph API["API LAYER"]
        BE["Java HttpServer\nREST Endpoints\nRequest Routing\nCORS Handling"]
    end

    subgraph BUSINESS["BUSINESS LOGIC LAYER"]
        BL["Handlers + Utils\nValidation\nCode Generation\nTTL Calculation\nQR Generation"]
    end

    subgraph DATA["DATA LAYER"]
        DB["SQLite via JDBC\nPersistent Storage\nUniqueness Enforcement\nExpiry Filtering"]
    end

    FE <-->|"HTTP JSON\nover Vite Proxy"| BE
    BE --> BL
    BL <-->|"JDBC\nPreparedStatement"| DB

    style PRESENTATION fill:#dbeafe
    style API fill:#fef9c3
    style BUSINESS fill:#dcfce7
    style DATA fill:#fce7f3
```

---

## 3. Low-Level Design (LLD) — Backend Class Structure

Shows every class, its methods, fields, and relationships.

```mermaid
classDiagram
    class Main {
        +main(String[] args)
        -HttpServer server
        -ThreadPool executor(4)
        <<entry point>>
    }

    class Database {
        -String DB_URL
        +init() void
        +getConnection() Connection
        +codeExists(String code) boolean
    }

    class ShortenHandler {
        -long DEFAULT_TTL_SECONDS
        +handle(HttpExchange) void
        -generateCode() String
    }

    class RedirectHandler {
        +handle(HttpExchange) void
    }

    class ListHandler {
        +handle(HttpExchange) void
    }

    class QRHandler {
        +handle(HttpExchange) void
        -generateQR(String url, int size) byte[]
    }

    class Validator {
        -Set~String~ RESERVED_WORDS
        -Set~String~ BLOCKED_WORDS
        +isValidUrl(String url) boolean
        +validateAlias(String alias) String
    }

    class JsonUtil {
        +extractJsonValue(String json, String key) String
        +escapeJson(String s) String
        +sendJson(HttpExchange, int, String) void
        +sendImage(HttpExchange, int, byte[]) void
        +sendCors(HttpExchange) void
        -setCorsHeaders(HttpExchange) void
    }

    class HttpHandler {
        <<interface>>
        +handle(HttpExchange) void
    }

    Main --> Database : init()
    Main --> ShortenHandler : registers
    Main --> RedirectHandler : registers
    Main --> ListHandler : registers
    Main --> QRHandler : registers

    ShortenHandler ..|> HttpHandler
    RedirectHandler ..|> HttpHandler
    ListHandler ..|> HttpHandler
    QRHandler ..|> HttpHandler

    ShortenHandler --> Database : codeExists()\ngetConnection()
    ShortenHandler --> Validator : isValidUrl()\nvalidateAlias()
    ShortenHandler --> JsonUtil : sendJson()\nextractJsonValue()\nescapeJson()

    RedirectHandler --> Database : getConnection()
    RedirectHandler --> JsonUtil : sendJson()

    ListHandler --> Database : getConnection()
    ListHandler --> JsonUtil : sendJson()\nescapeJson()

    QRHandler --> JsonUtil : sendJson()\nsendImage()
```

---

## 4. ER Diagram — Database Schema

The single table schema with all constraints shown.

```mermaid
erDiagram
    URLS {
        TEXT code PK "6-char random Base62\ne.g. aB3xYz\nPRIMARY KEY = auto indexed"
        TEXT alias UK "Optional custom alias\ne.g. my-link\nUNIQUE + nullable\nSQL NULL allowed multiple times"
        TEXT original_url "Full destination URL\nNOT NULL\ne.g. https://reddit.com/..."
        INTEGER created_at "Unix epoch seconds\nNOT NULL\nSet at INSERT time"
        INTEGER expires_at "Unix epoch seconds\nNOT NULL\ncreated_at + (ttlDays * 86400)"
        INTEGER deleted "Soft delete flag\n0 = active\n1 = deleted\nDEFAULT 0"
    }
```

### Index Map

```mermaid
graph LR
    subgraph TABLE["urls table"]
        COL1["code\nTEXT PK"]
        COL2["alias\nTEXT UNIQUE"]
        COL3["original_url\nTEXT NOT NULL"]
        COL4["created_at\nINTEGER NOT NULL"]
        COL5["expires_at\nINTEGER NOT NULL"]
        COL6["deleted\nINTEGER DEFAULT 0"]
    end

    subgraph INDEXES["Indexes (auto + explicit)"]
        IDX1["sqlite_autoindex_urls_1\nauto-created by PRIMARY KEY\non: code\ntype: B-tree"]
        IDX2["sqlite_autoindex_urls_2\nauto-created by UNIQUE\non: alias\ntype: B-tree"]
        IDX3["idx_alias\nexplicitly created\non: alias\ntype: B-tree\n(reinforces UNIQUE index)"]
    end

    COL1 --> IDX1
    COL2 --> IDX2
    COL2 --> IDX3

    style IDX1 fill:#dcfce7
    style IDX2 fill:#fef9c3
    style IDX3 fill:#fce7f3
```

---

## 5. Request Lifecycle — POST /shorten (Full Sequence)

Every layer, every step, every decision point.

```mermaid
sequenceDiagram
    actor User
    participant React as React (App.jsx)<br/>:5173
    participant Vite as Vite Proxy<br/>:5173
    participant Main as Main.java<br/>HttpServer :8080
    participant SH as ShortenHandler
    participant Val as Validator
    participant JU as JsonUtil
    participant DB as Database.java
    participant SQLite as SQLite (urls.db)

    User->>React: Types URL + alias + selects TTL<br/>Clicks "Shorten"

    React->>React: handleShorten(e)<br/>e.preventDefault()<br/>setLoading(true)

    React->>Vite: POST /shorten<br/>Content-Type: application/json<br/>{"url":"https://...","alias":"my-link","ttlDays":"7"}

    Vite->>Main: Forwards to http://localhost:8080/shorten<br/>(transparent to browser)

    Main->>Main: ThreadPool picks up request<br/>Matches /shorten context
    Main->>SH: dispatch handle(exchange)

    SH->>SH: Check method = POST ✓

    SH->>SH: readAllBytes() → body string

    SH->>JU: extractJsonValue(body, "url")
    JU-->>SH: "https://reddit.com/..."

    SH->>JU: extractJsonValue(body, "alias")
    JU-->>SH: "my-link"

    SH->>JU: extractJsonValue(body, "ttlDays")
    JU-->>SH: "7"

    SH->>Val: isValidUrl("https://reddit.com/...")
    Val-->>SH: true ✓

    SH->>SH: Long.parseLong("7") = 7<br/>7 within 1–365 ✓<br/>ttlSeconds = 604800

    SH->>SH: alias = "my-link".trim().toLowerCase()<br/>= "my-link"

    SH->>Val: validateAlias("my-link")
    Val->>Val: length 7: 3–30 ✓
    Val->>Val: regex match ✓
    Val->>Val: not in RESERVED_WORDS ✓
    Val->>Val: not in BLOCKED_WORDS ✓
    Val-->>SH: null (valid)

    SH->>SH: generateCode() → "aB3xYz"

    SH->>DB: codeExists("aB3xYz")
    DB->>SQLite: SELECT 1 FROM urls WHERE code = 'aB3xYz'
    SQLite-->>DB: empty ResultSet
    DB-->>SH: false (code is free)

    SH->>SH: now = Instant.now().getEpochSecond()<br/>expiresAt = now + 604800

    SH->>DB: getConnection()
    DB-->>SH: Connection

    SH->>SQLite: INSERT INTO urls<br/>(code, alias, original_url, created_at, expires_at)<br/>VALUES ('aB3xYz','my-link','https://...', now, expiresAt)

    alt Alias already taken
        SQLite-->>SH: SQLException: UNIQUE constraint failed: urls.alias
        SH->>JU: sendJson(exchange, 409, "Alias already taken")
        JU-->>React: HTTP 409 JSON error
        React->>React: setError("Alias already taken")
        React-->>User: Shows error message in red
    else Success
        SQLite-->>SH: 1 row inserted

        SH->>SH: shortUrl = "http://localhost:8080/r/my-link"<br/>(alias takes priority over code)

        SH->>JU: sendJson(exchange, 200, responseJson)
        JU->>JU: Set Content-Type: application/json<br/>Set CORS headers<br/>sendResponseHeaders(200, len)<br/>write bytes

        JU-->>Vite: HTTP 200<br/>{"shortUrl":"...","code":"aB3xYz","alias":"my-link","expiresAt":...}
        Vite-->>React: Forwards response

        React->>React: setShortUrl(data.shortUrl)<br/>setHistory(prev => [newItem, ...prev])<br/>setInputUrl("")<br/>setAlias("")<br/>setLoading(false)

        React-->>User: Shows short URL with Copy + QR buttons<br/>History table updates with new row
    end
```

---

## 6. Request Lifecycle — GET /r/{codeOrAlias} (Redirect)

```mermaid
sequenceDiagram
    actor User
    participant Browser
    participant Main as Main.java<br/>HttpServer :8080
    participant RH as RedirectHandler
    participant DB as Database.java
    participant SQLite as SQLite (urls.db)
    participant Dest as Destination Website

    User->>Browser: Clicks or types<br/>http://localhost:8080/r/my-link

    Note over Browser,Main: Direct request to :8080<br/>Does NOT go through Vite proxy<br/>(Vite proxy only handles requests FROM React app)

    Browser->>Main: GET /r/my-link HTTP/1.1

    Main->>Main: Matches /r/ context prefix
    Main->>RH: dispatch handle(exchange)

    RH->>RH: path = "/r/my-link"<br/>codeOrAlias = "my-link"<br/>.toLowerCase().trim()

    RH->>DB: getConnection()
    DB-->>RH: Connection

    RH->>SQLite: SELECT original_url, expires_at, deleted<br/>FROM urls<br/>WHERE (LOWER(alias) = 'my-link' OR code = 'my-link')<br/>ORDER BY CASE WHEN LOWER(alias)='my-link' THEN 0 ELSE 1 END<br/>LIMIT 1

    alt Row not found
        SQLite-->>RH: empty ResultSet
        RH->>Browser: HTTP 404<br/>{"error":"Short URL not found"}
        Browser-->>User: Shows 404 error
    else Row found
        SQLite-->>RH: {original_url, expires_at, deleted}

        alt deleted = 1
            RH->>Browser: HTTP 404<br/>{"error":"Short URL not found"}
            Browser-->>User: Shows 404 error
        else now > expires_at
            RH->>Browser: HTTP 410<br/>{"error":"This short URL has expired"}
            Browser-->>User: Shows 410 Gone error
        else Valid and active
            RH->>Browser: HTTP 302<br/>Location: https://reddit.com/...<br/>Access-Control-Allow-Origin: *

            Note over Browser: Browser receives 302<br/>Reads Location header<br/>Automatically sends new GET request

            Browser->>Dest: GET https://reddit.com/...
            Dest-->>Browser: 200 OK (full webpage)
            Browser-->>User: Reddit page loads
        end
    end
```

---

## 7. Request Lifecycle — GET /qr (QR Code Generation)

```mermaid
sequenceDiagram
    actor User
    participant React as React (App.jsx)
    participant Vite as Vite Proxy
    participant QH as QRHandler
    participant ZX as ZXing Library
    participant JU as JsonUtil

    User->>React: Clicks "QR" button<br/>next to a short URL

    React->>React: setQrUrl("http://localhost:8080/r/my-link")<br/>Modal renders

    React->>React: Browser renders<br/><img src="/qr?url=http%3A%2F%2Flocalhost%3A8080%2Fr%2Fmy-link">

    Note over React: encodeURIComponent converts<br/>: → %3A, / → %2F<br/>so inner URL doesn't break outer URL

    React->>Vite: GET /qr?url=http%3A%2F%2Flocalhost%3A8080%2Fr%2Fmy-link

    Vite->>QH: Forwards to :8080

    QH->>QH: getQuery() → "url=http%3A%2F%2F..."<br/>split by "&"<br/>find param starting with "url="<br/>URLDecoder.decode() → "http://localhost:8080/r/my-link"

    QH->>ZX: new QRCodeWriter()<br/>hints = {ERROR_CORRECTION: M, MARGIN: 2}<br/>writer.encode(url, QR_CODE, 300, 300, hints)

    ZX->>ZX: Encode string to QR matrix<br/>Apply error correction (Level M = 15%)<br/>Build 300×300 BitMatrix<br/>(each cell = true/false = black/white)

    ZX-->>QH: BitMatrix (300×300 boolean grid)

    QH->>ZX: MatrixToImageWriter.writeToStream(matrix, "PNG", outputStream)

    ZX->>ZX: Convert BitMatrix to PNG pixels<br/>true cell → black pixel<br/>false cell → white pixel<br/>Write PNG file format bytes

    ZX-->>QH: byte[] (PNG image data)

    QH->>JU: sendImage(exchange, 200, pngBytes)
    JU->>JU: Content-Type: image/png<br/>CORS headers<br/>sendResponseHeaders(200, pngBytes.length)<br/>write bytes

    JU-->>React: HTTP 200<br/>Content-Type: image/png<br/>[PNG binary data]

    React->>React: Browser receives image/png<br/><img> tag renders QR code

    React-->>User: QR code visible in modal<br/>User can scan with phone
```

---

## 8. Request Lifecycle — GET /all (List URLs)

```mermaid
sequenceDiagram
    participant React as React (App.jsx)
    participant Vite as Vite Proxy
    participant LH as ListHandler
    participant DB as Database.java
    participant SQLite as SQLite (urls.db)

    Note over React: On component mount<br/>useEffect(()=>{...}, [])

    React->>Vite: GET /all

    Vite->>LH: Forwards to :8080

    LH->>LH: now = Instant.now().getEpochSecond()

    LH->>DB: getConnection()
    DB-->>LH: Connection

    LH->>SQLite: SELECT code, alias, original_url, expires_at<br/>FROM urls<br/>WHERE expires_at > {now} AND deleted = 0<br/>ORDER BY created_at DESC

    SQLite-->>LH: ResultSet (all active, non-expired, non-deleted rows)

    LH->>LH: Loop through ResultSet<br/>For each row:<br/>  shortUrl = alias ?? code<br/>  append JSON object to StringBuilder<br/>  alias null → JSON null<br/>  alias present → "alias-name"

    LH->>LH: JSON array complete<br/>[{code,alias,originalUrl,shortUrl,expiresAt},...]

    LH-->>React: HTTP 200<br/>Content-Type: application/json<br/>[{...},{...},...]

    React->>React: .then(r => r.json())<br/>.then(setHistory)<br/>History state updated

    React-->>React: Re-render: table shows all active URLs<br/>Newest first (ORDER BY created_at DESC)
```

---

## 9. Frontend Component Hierarchy

```mermaid
graph TD
    subgraph REACT["React App — App.jsx (single component)"]
        APP["App()\nState: inputUrl, alias, ttlDays\nshortUrl, error, loading\nhistory, copied, qrUrl\n\nEffects: fetch /all on mount\nHandlers: handleShorten, copyToClipboard\nHelpers: formatExpiry, qrSrc"]

        subgraph MODAL["QR Modal (conditional — renders when qrUrl != null)"]
            OVERLAY["div.modalOverlay\nonClick: setQrUrl(null)"]
            MODALBOX["div.modal\nonClick: stopPropagation"]
            QRTITLE["h3 — QR Code"]
            QRURL["p — shows the URL being encoded"]
            QRIMG["img src={/qr?url=encodeURIComponent(qrUrl)}\nFetches PNG from backend automatically"]
            MODALHINT["p — Click outside to close"]
        end

        subgraph CARD1["Card 1 — Shorten Form"]
            TITLE["h1 — URL Shortener"]
            FORM["form onSubmit={handleShorten}"]
            URLINPUT["input[text] — URL field\ncontrolled: value=inputUrl"]
            ALIASROW["div.aliasRow"]
            ALIASPFX["span — short.ly/"]
            ALIASINPUT["input[text] — alias field\ncontrolled: value=alias\nmaxLength=30"]
            HINT["p — validation hint text"]
            TTLROW["div.ttlRow"]
            TTLLABEL["label — Expires in"]
            TTLSELECT["select — 1/7/30/90/365 days\ncontrolled: value=ttlDays"]
            SUBMITBTN["button[submit]\ndisabled when loading=true\ntext: Shortening... / Shorten"]
            ERROR["p.error — conditional\nshows error message in red"]
            RESULTBOX["div.result — conditional\nshows when shortUrl != null"]
            SHORTLINK["a href={shortUrl}"]
            COPYBTN["button — Copy / Copied!\nonClick: copyToClipboard(shortUrl)"]
            QRBTN["button — QR\nonClick: setQrUrl(shortUrl)"]
        end

        subgraph CARD2["Card 2 — History Table (conditional — shows when history.length > 0)"]
            SUBTITLE["h2 — Active URLs"]
            TABLE["table"]
            THEAD["thead — Original URL / Short URL / Alias / Expires / QR"]
            TBODY["tbody — maps history array"]
            ROW["tr key={item.code} × N rows"]
            TD1["td — truncated original URL\ntitle={full url} for hover"]
            TD2["td — a href={item.shortUrl}"]
            TD3["td — item.alias or em-dash"]
            TD4["td — formatExpiry(item.expiresAt)"]
            TD5["td — QR button\nonClick: setQrUrl(item.shortUrl)"]
        end
    end

    APP --> MODAL
    APP --> CARD1
    APP --> CARD2
    MODAL --> OVERLAY --> MODALBOX
    MODALBOX --> QRTITLE
    MODALBOX --> QRURL
    MODALBOX --> QRIMG
    MODALBOX --> MODALHINT
    CARD1 --> TITLE
    CARD1 --> FORM
    FORM --> URLINPUT
    FORM --> ALIASROW
    ALIASROW --> ALIASPFX
    ALIASROW --> ALIASINPUT
    FORM --> HINT
    FORM --> TTLROW
    TTLROW --> TTLLABEL
    TTLROW --> TTLSELECT
    FORM --> SUBMITBTN
    CARD1 --> ERROR
    CARD1 --> RESULTBOX
    RESULTBOX --> SHORTLINK
    RESULTBOX --> COPYBTN
    RESULTBOX --> QRBTN
    CARD2 --> SUBTITLE
    CARD2 --> TABLE
    TABLE --> THEAD
    TABLE --> TBODY
    TBODY --> ROW
    ROW --> TD1
    ROW --> TD2
    ROW --> TD3
    ROW --> TD4
    ROW --> TD5
```

---

## 10. Frontend State Machine

All the states App.jsx can be in and what triggers transitions.

```mermaid
stateDiagram-v2
    [*] --> IDLE : Component mounts\nfetch /all → setHistory

    IDLE --> LOADING : User submits form\nsetLoading(true)\nsetError(null)\nsetShortUrl(null)

    LOADING --> SUCCESS : fetch POST /shorten\nreturns 200\nsetShortUrl(data.shortUrl)\nsetHistory([new,...prev])\nclear inputs

    LOADING --> ERROR : fetch returns 4xx/5xx\nOR network failure\nsetError(message)

    ERROR --> LOADING : User edits form\nand resubmits

    SUCCESS --> LOADING : User submits another URL\n(setShortUrl(null) clears result)

    SUCCESS --> COPIED : User clicks Copy button\nnavigator.clipboard.writeText()\nsetCopied(true)

    COPIED --> SUCCESS : After 2000ms\nsetCopied(false)

    SUCCESS --> QR_OPEN : User clicks QR button\nsetQrUrl(shortUrl)

    IDLE --> QR_OPEN : User clicks QR in history table\nsetQrUrl(item.shortUrl)

    QR_OPEN --> IDLE : User clicks overlay\nsetQrUrl(null)

    QR_OPEN --> SUCCESS : (if came from SUCCESS state)\nUser clicks overlay\nsetQrUrl(null)
```

---

## 11. Alias Validation Flow

Every check in `Validator.validateAlias()` and `ShortenHandler` in sequence.

```mermaid
flowchart TD
    START([User submits alias]) --> BLANK{alias blank\nor null?}

    BLANK -->|Yes| SKIP[Skip alias processing\nalias = null\nUse random code instead]
    SKIP --> CODEGEN([Generate random code])

    BLANK -->|No| LOWER[alias = aliasRaw.trim().toLowerCase]
    LOWER --> LEN{Length\n3–30 chars?}

    LEN -->|No| E1[400: Alias must be\nbetween 3 and 30 characters]
    LEN -->|Yes| REGEX{"Matches regex\n[a-z0-9][a-z0-9\\-]*[a-z0-9]?"}

    REGEX -->|No| E2[400: Only letters, numbers,\nhyphens allowed.\nNo leading/trailing hyphens]
    REGEX -->|Yes| RESERVED{In RESERVED_WORDS?\nall, r, shorten, api,\nadmin, login, qr, etc.}

    RESERVED -->|Yes| E3[400: Reserved word\ncannot be used as alias]
    RESERVED -->|No| BLOCKED{In BLOCKED_WORDS?\ngoogle, facebook,\namazon, apple, etc.}

    BLOCKED -->|Yes| E4[400: Protected name\ncannot be used as alias]
    BLOCKED -->|No| VALID[Alias is valid\nproceed to INSERT]

    VALID --> INSERT[INSERT INTO urls\ncode, alias, url, timestamps]

    INSERT --> UNIQUE{UNIQUE\nconstraint\npasses?}
    UNIQUE -->|Yes| OK[200: Success\nreturn shortUrl with alias]
    UNIQUE -->|No| E5[409: Alias already taken\nchoose a different one]

    style E1 fill:#fca5a5
    style E2 fill:#fca5a5
    style E3 fill:#fca5a5
    style E4 fill:#fca5a5
    style E5 fill:#fca5a5
    style OK fill:#86efac
    style SKIP fill:#fef08a
```

---

## 12. Short Code Generation Algorithm

```mermaid
flowchart TD
    START([ShortenHandler needs a code]) --> GEN

    subgraph GENLOOP["do-while loop"]
        GEN["generateCode():\nchars = a-z A-Z 0-9\nFor i = 0 to 5:\n  pick chars at Math.random() * 62\nReturn 6-char string\ne.g. 'aB3xYz'"]
        CHECK["Database.codeExists(code)\nSELECT 1 FROM urls\nWHERE code = ?"]
        EXISTS{Row found\nin DB?}
    end

    GEN --> CHECK
    CHECK --> EXISTS
    EXISTS -->|Yes - collision!\n~1 in 56 billion chance| GEN
    EXISTS -->|No - code is free| USE

    USE["Use this code\nfor INSERT"]

    subgraph MATH["Why 56 billion combinations"]
        CALC["Charset size: 62\n(26 lower + 26 upper + 10 digits)\n\nCode length: 6\n\nTotal: 62^6 = 56,800,235,584"]
    end

    USE --> INSERT["INSERT INTO urls\nwith this code"]

    style EXISTS fill:#fef9c3
    style CALC fill:#dbeafe
```

### Base62 Alternative (Interview Talking Point)

```mermaid
flowchart LR
    subgraph CURRENT["Current: Random Base62"]
        R1["Math.random() × 62\n× 6 times\n→ random 6-char code\n\nCollision: do-while check\nUniqueness: probabilistic"]
    end

    subgraph ALTERNATIVE["Production: ID + Base62 Encode"]
        A1["DB auto-increment ID\ne.g. ID = 1000"]
        A2["while id > 0:\n  sb.append(chars[id % 62])\n  id = id / 62\nreverse string\n→ 'G8'"]
        A3["No collision possible\nID is unique by definition\nNo DB check needed"]
        A1 --> A2 --> A3
    end

    CURRENT -->|"Upgrade path\nat scale"| ALTERNATIVE
```

---

## 13. TTL and Expiry Decision Tree

```mermaid
flowchart TD
    subgraph INSERT["At INSERT time — ShortenHandler"]
        I1["ttlDays provided?\nDefault = 7 if not"]
        I2["ttlSeconds = ttlDays × 24 × 60 × 60"]
        I3["now = Instant.now().getEpochSecond()"]
        I4["expires_at = now + ttlSeconds\n(stored as INTEGER in DB)"]
        I1 --> I2 --> I3 --> I4
    end

    subgraph REDIRECT["At REDIRECT time — RedirectHandler"]
        R1["SELECT expires_at FROM urls\nWHERE code/alias = ?"]
        R2{"Instant.now().getEpochSecond()\n> expires_at?"}
        R3["410 Gone\nThis short URL has expired"]
        R4["302 Redirect\nLocation: original_url"]
        R1 --> R2
        R2 -->|Yes - expired| R3
        R2 -->|No - still valid| R4
    end

    subgraph LIST["At LIST time — ListHandler"]
        L1["SELECT ... FROM urls\nWHERE expires_at > ?\nAND deleted = 0"]
        L2["Only active URLs\nreturned to frontend"]
        L1 --> L2
    end

    subgraph EXAMPLES["TTL Examples"]
        E1["1 day = 86,400 seconds"]
        E2["7 days = 604,800 seconds"]
        E3["30 days = 2,592,000 seconds"]
        E4["365 days = 31,536,000 seconds"]
    end

    I4 -.->|"stored in DB"| R1
    I4 -.->|"stored in DB"| L1

    style R3 fill:#fca5a5
    style R4 fill:#86efac
    style L2 fill:#86efac
```

---

## 14. Concurrency — Race Condition Handling

Shows why we skip the existence check and go straight to INSERT.

```mermaid
sequenceDiagram
    participant T1 as Thread 1 (User A)
    participant T2 as Thread 2 (User B)
    participant DB as SQLite (urls.db)

    Note over T1,T2: Both users request alias "promo" simultaneously

    rect rgb(255, 220, 220)
        Note over T1,T2,DB: ❌ WRONG approach — check then insert
        T1->>DB: SELECT 1 WHERE alias = 'promo' → empty
        T2->>DB: SELECT 1 WHERE alias = 'promo' → empty
        Note over T1,T2: Both see "alias is free"
        T1->>DB: INSERT alias='promo' → success
        T2->>DB: INSERT alias='promo' → success (DUPLICATE DATA!)
    end

    rect rgb(220, 255, 220)
        Note over T1,T2,DB: ✅ OUR approach — go straight to INSERT
        T1->>DB: INSERT alias='promo'
        T2->>DB: INSERT alias='promo'
        Note over DB: DB's internal locking ensures<br/>only one INSERT wins
        DB-->>T1: Success (1 row inserted)
        DB-->>T2: SQLException: UNIQUE constraint failed: urls.alias
        T2->>T2: catch SQLException<br/>check message contains "UNIQUE constraint failed: urls.alias"
        T2-->>T2: return HTTP 409 Conflict<br/>"Alias already taken"
    end
```

---

## 15. Deployment Diagram — Current Dev Setup

```mermaid
graph TB
    subgraph DEV["Developer Machine"]
        subgraph TERMINAL1["Terminal 1"]
            JAVAC["javac -cp classpath Main.java handlers/*.java util/*.java db/*.java"]
            JAVA["java -cp classpath Main\n→ HttpServer on :8080"]
            URLSDB[("urls.db\nbackend/src/urls.db")]
            JAVA --> URLSDB
        end

        subgraph TERMINAL2["Terminal 2"]
            NPM["npm run dev\n→ Vite Dev Server on :5173"]
        end

        subgraph BROWSER["Browser"]
            TAB["localhost:5173\nReact App"]
        end

        subgraph VSCODE["VS Code"]
            CODE["Source files\nbackend/src/*.java\nfrontend/src/*.jsx"]
        end
    end

    TAB -->|"fetch /shorten\nfetch /all\nfetch /qr"| NPM
    NPM -->|"Proxy: forwards\nAPI calls to :8080"| JAVA
    TAB -->|"Direct browser nav\nGET /r/{code}"| JAVA
    JAVA -->|"JDBC read/write"| URLSDB

    style DEV fill:#f0f9ff
    style TERMINAL1 fill:#dcfce7
    style TERMINAL2 fill:#fef9c3
    style BROWSER fill:#fce7f3
```

---

## 16. Deployment Diagram — Ideal Production Setup

```mermaid
graph TB
    subgraph INTERNET["Internet"]
        USER["Users worldwide"]
    end

    subgraph CDN["CDN (Cloudflare / AWS CloudFront)"]
        STATIC["Static Assets\nReact build output\nindex.html, JS, CSS"]
    end

    subgraph SERVER["Production Server (e.g. EC2 / Render / Railway)"]
        subgraph NGINX["Nginx — Port 80/443"]
            SSL["SSL Termination\nHTTPS (Let's Encrypt)"]
            PROXY["Reverse Proxy\nlocation /shorten → :8080\nlocation /r/ → :8080\nlocation /all → :8080\nlocation /qr → :8080\nlocation / → static files"]
        end

        subgraph JAVAPROC["Java Process — Port 8080"]
            MAIN["Main.java\nHttpServer\nThreadPool(10+)"]
            HANDLERS["All Handlers\nShortenHandler\nRedirectHandler\nListHandler\nQRHandler"]
            MAIN --> HANDLERS
        end

        subgraph DBSERVER["Database"]
            PG[("PostgreSQL\nor MySQL\n(replace SQLite\nfor multi-instance)")]
        end

        subgraph CACHE["Cache Layer"]
            REDIS[("Redis\ncode → original_url\nTTL matches URL expiry")]
        end
    end

    USER -->|"HTTPS"| CDN
    USER -->|"HTTPS short.yourdomain.com/r/..."| NGINX
    CDN --> NGINX
    SSL --> PROXY
    PROXY --> MAIN
    HANDLERS -->|"Cache hit:\nreturn immediately"| REDIS
    HANDLERS -->|"Cache miss:\nquery DB, then cache"| PG
    PG --> REDIS

    subgraph CHANGES["What changes from dev to prod"]
        C1["1. BASE_URL env var: http://localhost:8080 → https://short.yourdomain.com"]
        C2["2. DB: SQLite → PostgreSQL (change connection string + swap JAR)"]
        C3["3. Add Redis client for redirect caching"]
        C4["4. Nginx replaces Vite proxy"]
        C5["5. npm run build → serve /dist as static files"]
        C6["6. Java code: 0 changes to handlers/logic"]
    end

    style INTERNET fill:#e0f2fe
    style CDN fill:#fef9c3
    style NGINX fill:#dcfce7
    style JAVAPROC fill:#fce7f3
    style DBSERVER fill:#f3e8ff
    style CACHE fill:#fff7ed
```

---

## 17. Package Dependency Graph

Shows which Java package depends on which — and what the allowed dependencies are.

```mermaid
graph TD
    subgraph DEFAULT["Default Package"]
        MAIN["Main.java"]
    end

    subgraph DB_PKG["db package"]
        DATABASE["Database.java"]
    end

    subgraph HANDLERS_PKG["handlers package"]
        SHORTENHANDLER["ShortenHandler.java"]
        REDIRECTHANDLER["RedirectHandler.java"]
        LISTHANDLER["ListHandler.java"]
        QRHANDLER["QRHandler.java"]
    end

    subgraph UTIL_PKG["util package"]
        VALIDATOR["Validator.java"]
        JSONUTIL["JsonUtil.java"]
    end

    subgraph EXTERNAL["External JARs"]
        SQLITE_JDBC["sqlite-jdbc.jar\njava.sql.*"]
        ZXING_CORE["core-3.5.2.jar\ncom.google.zxing.*"]
        ZXING_SE["javase-3.5.2.jar\ncom.google.zxing.client.j2se.*"]
        JDK["JDK Built-in\ncom.sun.net.httpserver.*\njava.net.*\njava.time.*\njava.util.*"]
    end

    MAIN --> DB_PKG
    MAIN --> HANDLERS_PKG
    MAIN --> JDK

    SHORTENHANDLER --> DB_PKG
    SHORTENHANDLER --> UTIL_PKG
    SHORTENHANDLER --> JDK
    SHORTENHANDLER --> SQLITE_JDBC

    REDIRECTHANDLER --> DB_PKG
    REDIRECTHANDLER --> UTIL_PKG
    REDIRECTHANDLER --> JDK
    REDIRECTHANDLER --> SQLITE_JDBC

    LISTHANDLER --> DB_PKG
    LISTHANDLER --> UTIL_PKG
    LISTHANDLER --> JDK
    LISTHANDLER --> SQLITE_JDBC

    QRHANDLER --> UTIL_PKG
    QRHANDLER --> JDK
    QRHANDLER --> ZXING_CORE
    QRHANDLER --> ZXING_SE

    DATABASE --> SQLITE_JDBC
    DATABASE --> JDK

    JSONUTIL --> JDK

    VALIDATOR --> JDK

    style DEFAULT fill:#dbeafe
    style DB_PKG fill:#dcfce7
    style HANDLERS_PKG fill:#fce7f3
    style UTIL_PKG fill:#fef9c3
    style EXTERNAL fill:#f3e8ff
```

---

## 18. Data Lifecycle — A URL from Birth to Expiry

```mermaid
timeline
    title Life of a Short URL

    section Created
        T+0s    : User submits https://reddit.com/...
                : alias = "my-link", ttlDays = 7
                : code = "aB3xYz" generated
                : INSERT into urls.db
                : expires_at = now + 604800

    section Active Period (7 days)
        T+1min  : User shares short URL
                : http://localhost:8080/r/my-link
        T+1hr   : First redirect
                : RedirectHandler → 302
                : expires_at check passes
        T+2days : More redirects
                : Still active
                : /all returns this URL
        T+6days : User generates QR code
                : QRHandler creates PNG
                : User prints QR on poster

    section Expiry
        T+7days : expires_at timestamp reached
                : RedirectHandler: now > expires_at
                : Returns HTTP 410 Gone
                : /all no longer returns this URL
                : Row still in DB (soft expiry)

    section After Expiry
        T+8days : Old alias "my-link" still in DB
                : UNIQUE constraint still holds
                : Nobody can claim "my-link" again
                : Printed QR code now shows 410 error
```

---

## 19. CORS Preflight Flow

```mermaid
sequenceDiagram
    participant Browser
    participant Vite as Vite Proxy :5173
    participant Java as Java Backend :8080

    Note over Browser,Java: Step 1 — Browser sends CORS preflight (OPTIONS)
    Note over Browser: Browser sees fetch() to a different origin<br/>Must ask permission first

    Browser->>Vite: OPTIONS /shorten HTTP/1.1<br/>Origin: http://localhost:5173<br/>Access-Control-Request-Method: POST<br/>Access-Control-Request-Headers: Content-Type

    Vite->>Java: Forwards OPTIONS request

    Java->>Java: handler sees OPTIONS method<br/>calls JsonUtil.sendCors(exchange)

    Java->>Java: Set headers:<br/>Access-Control-Allow-Origin: *<br/>Access-Control-Allow-Methods: GET, POST, OPTIONS<br/>Access-Control-Allow-Headers: Content-Type<br/>sendResponseHeaders(200, -1)

    Java-->>Vite: HTTP 200 (with CORS headers)
    Vite-->>Browser: HTTP 200 (with CORS headers)

    Note over Browser: Preflight approved.<br/>Now sends actual request.

    Note over Browser,Java: Step 2 — Actual POST request
    Browser->>Vite: POST /shorten<br/>Content-Type: application/json<br/>{"url":"...","alias":"..."}

    Vite->>Java: Forwards POST

    Java->>Java: ShortenHandler processes request
    Java-->>Vite: HTTP 200 with CORS headers + JSON body
    Vite-->>Browser: Response

    Note over Browser: Browser checks Access-Control-Allow-Origin: *<br/>Allows JavaScript to read the response
```

---

## 20. Handler Routing Map

Shows exactly which URL patterns hit which handler and what HTTP methods are supported.

```mermaid
graph LR
    subgraph INCOMING["Incoming Requests to :8080"]
        R1["POST /shorten\nbody: {url, alias, ttlDays}"]
        R2["OPTIONS /shorten\n(CORS preflight)"]
        R3["GET /r/aB3xYz\n(random code)"]
        R4["GET /r/my-link\n(custom alias)"]
        R5["GET /all"]
        R6["OPTIONS /all"]
        R7["GET /qr?url=http://..."]
        R8["OPTIONS /qr"]
        R9["POST /r/...\nGET /shorten\nor any wrong method"]
    end

    subgraph ROUTING["Main.java — createContext()"]
        C1["context: /shorten"]
        C2["context: /r/"]
        C3["context: /all"]
        C4["context: /qr"]
    end

    subgraph HANDLERS["Handlers"]
        SH["ShortenHandler\n→ validate URL\n→ validate alias\n→ generate code\n→ INSERT\n→ 200 JSON"]
        RH["RedirectHandler\n→ lookup alias OR code\n→ check expiry\n→ 302 / 404 / 410"]
        LH["ListHandler\n→ SELECT non-expired\n→ 200 JSON array"]
        QH["QRHandler\n→ decode URL param\n→ ZXing generate PNG\n→ 200 image/png"]
        ERR["405 Method Not Allowed"]
        CORS["sendCors()\n200 + CORS headers"]
    end

    R1 --> C1 --> SH
    R2 --> C1 --> CORS
    R3 --> C2 --> RH
    R4 --> C2 --> RH
    R5 --> C3 --> LH
    R6 --> C3 --> CORS
    R7 --> C4 --> QH
    R8 --> C4 --> CORS
    R9 --> ERR

    style SH fill:#dcfce7
    style RH fill:#dbeafe
    style LH fill:#fef9c3
    style QH fill:#fce7f3
    style ERR fill:#fca5a5
    style CORS fill:#f3e8ff
```

---

> **Tip for interviews:** When asked to draw a diagram on a whiteboard, start with Diagram 2 (HLD) to show the big picture, then drill into whichever layer the interviewer is interested in. Have Diagram 5 (POST /shorten sequence) ready to walk through step by step — it covers the most ground in one diagram.
