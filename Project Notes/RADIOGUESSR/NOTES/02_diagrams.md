# RadioGuessr — Architecture Diagrams

> Render in any Mermaid-compatible viewer: GitHub, VS Code (Markdown Preview Mermaid Support extension), or mermaid.live

---

## 1. System Architecture Overview

```mermaid
graph TB
    subgraph Browser["Browser (React + Vite)"]
        UI["Phase UI Components\n(Start/Playing/Result/Final)"]
        GS["useGameStore\n(Zustand)"]
        AS["useAuthStore\n(Zustand + localStorage)"]
        GLOBE["Globe.jsx\n(Globe.gl + Three.js)"]
        AUDIO["audio.js\n(HLS.js + native audio)"]
        APIJS["api.js\n(fetch wrapper)"]
    end

    subgraph Express["Express Server (Node.js :3001)"]
        MW["Middleware\n(CORS, JSON, Rate Limiter)"]
        SESS["Game Session Store\n(in-memory Map)"]
        AR["Auth Router\n/api/auth/*"]
        LR["Leaderboard Router\n/api/leaderboard"]
        SR["Station Route\n/api/station"]
        RADIO["radioApi.js\n(pickStation, fetchBatch)"]
    end

    subgraph Cache["Redis Cache (Upstash)"]
        RC["Redis\nKey: rg:cache:country:order:...\nTTL: 600s (EX)"]
    end

    subgraph External["External Services"]
        RB["Radio Browser API\nde1.api.radio-browser.info"]
        PG["PostgreSQL\n(Supabase)"]
        STREAM["Live Radio Streams\n(MP3 / HLS)"]
    end

    UI --> GS
    UI --> AS
    GS --> APIJS
    AS --> APIJS
    GLOBE --> GS
    AUDIO --> STREAM

    APIJS -->|"GET /api/station"| SR
    APIJS -->|"POST /api/auth/*"| AR
    APIJS -->|"GET/POST /api/leaderboard"| LR

    SR --> MW
    AR --> MW
    LR --> MW

    MW --> SESS
    SR --> RADIO
    RADIO -->|"await cache.get(key)\nHTTP GET to Upstash"| RC
    RC -->|"HIT: return JSON array"| RADIO
    RC -->|"MISS: null"| RADIO
    RADIO -->|"MISS: fetch from source"| RB
    RB -->|"station list"| RADIO
    RADIO -->|"cache.set(key,data)\nSET key val EX 600\nfire-and-forget"| RC

    AR --> PG
    LR --> PG
```

---

## 2. Game State Machine (Phase Transitions)

```mermaid
stateDiagram-v2
    [*] --> start : App loads

    start --> loading : User clicks Play\nstartRound()

    loading --> playing : Station fetched\n+ audio started

    loading --> start : All 5 retries failed\n(error shown)

    playing --> loading : Reroll same country\nrerollCurrentStation()

    playing --> result : User submits guess\nsubmitGuess()

    result --> loading : User clicks Next\nstartRound()

    result --> final : Round 5 complete\nstartRound() detects round >= 5

    final --> start : User clicks Play Again\nresetGame()

    note right of result
        prefetchNextStation()
        fires here in background.
        May hit Redis cache —
        sub-20ms if warm.
    end note
```

---

## 3. ER Diagram (Database Schema)

```mermaid
erDiagram
    USERS {
        uuid id PK
        text username UK
        text email UK
        text password
        timestamptz created_at
    }

    GAME_SESSIONS {
        uuid id PK
        uuid user_id FK
        integer total_score
        text[] countries
        timestamptz played_at
    }

    USERS ||--o{ GAME_SESSIONS : "has many"
```

---

## 4. Request Lifecycle — Station Fetch (with Redis)

```mermaid
sequenceDiagram
    participant Browser
    participant Vite as Vite Proxy
    participant Express
    participant RateLimit as Rate Limiter
    participant Session as Session Store (Map)
    participant Redis as Redis (Upstash)
    participant RadioBrowser as Radio Browser API

    Browser->>Vite: GET /api/station?sessionId=abc
    Vite->>Express: Proxy → :3001/api/station?sessionId=abc

    Express->>RateLimit: Check IP (60 req/min sliding window)
    alt Rate limit exceeded
        RateLimit-->>Browser: 429 Too Many Requests + retryAfterMs
    end

    RateLimit->>Session: getOrCreateSession("abc")
    Session-->>Express: { seenUUIDs: Set }

    Note over Express,Redis: cache.get() = async HTTP GET to Upstash
    Express->>Redis: GET rg:cache:DE:votes:true:40:jazz
    alt Cache HIT (key exists, TTL not expired)
        Redis-->>Express: JSON string → parse → station array (~5-20ms)
    else Cache MISS (key absent or TTL expired)
        Redis-->>Express: null
        Express->>RadioBrowser: GET /json/stations/search?countrycode=DE&...
        RadioBrowser-->>Express: station list JSON (~500-2000ms)
        Note over Express,Redis: cache.set() = fire-and-forget SET EX 600
        Express--)Redis: SET rg:cache:DE:votes:true:40:jazz <json> EX 600
    end

    Express->>Express: Shuffle list\nSkip seenUUIDs\nValidate HTTPS + coords
    Express->>Session: seenUUIDs.add(stationUUID)
    Express-->>Browser: { name, url, lat, lng, countrycode, ... }
```

---

## 5. Redis Cache — Hit vs Miss Flow

```mermaid
flowchart TD
    A["fetchBatch() called\ncountry=DE, order=votes, offset=40, tag=jazz"] 
    --> B["Build cache key\nrg:cache:DE:votes:true:40:jazz"]
    --> C["await cache.get(key)\nHTTP GET to Upstash Redis"]

    C -->|"HIT\nRedis returns JSON string\n~5-20ms"| D["JSON.parse(raw)\nReturn station array"]
    C -->|"MISS\nRedis returns null\n(key absent or TTL expired)"| E["fetch() Radio Browser API\n~500-2000ms"]

    E --> F["cache.set(key, data)\nredis.set(key, JSON.stringify(data), EX 600)\nFire-and-forget — no await"]
    F --> D

    D --> G["Shuffle array\n(different callers get different\nstations from same cached batch)"]
    G --> H{"Station valid?\nHTTPS url\nfinite lat/lng\nnot in seenUUIDs"}

    H -->|"Valid"| I["seenUUIDs.add(uuid)\nReturn station to client"]
    H -->|"Invalid — try next"| G
    H -->|"List exhausted"| J["Retry with new\ncountry + params"]
    J -->|"After 5 attempts"| K["502 error to client"]
```

---

## 6. Redis Key Lifecycle

```mermaid
sequenceDiagram
    participant App as Express Server
    participant Redis as Upstash Redis

    Note over App,Redis: First request for DE:votes:true:40:jazz

    App->>Redis: GET rg:cache:DE:votes:true:40:jazz
    Redis-->>App: null (key does not exist)

    App->>App: Fetch from Radio Browser (~1000ms)

    App--)Redis: SET rg:cache:DE:votes:true:40:jazz <20 stations JSON> EX 600
    Note over Redis: Key created. Internal TTL timer starts.\nRedis will auto-delete at T+600s.

    Note over App,Redis: Any request within next 10 minutes

    App->>Redis: GET rg:cache:DE:votes:true:40:jazz
    Redis-->>App: <20 stations JSON> (~10ms)
    Note over App: Parse JSON. Shuffle. Pick station. Done.

    Note over App,Redis: After 600 seconds

    Redis->>Redis: TTL expired → auto-delete key
    Note over Redis: Key is gone. Next request is a MISS again.
```

---

## 7. Auth Flow — Register and Login

```mermaid
sequenceDiagram
    participant Browser
    participant Express
    participant RateLimit as Rate Limiter\n(3/hr register, 5/15min login)
    participant DB as PostgreSQL (Supabase)

    Note over Browser,DB: REGISTER

    Browser->>Express: POST /api/auth/register\n{ username, email, password }
    Express->>RateLimit: Check IP (3/hour)
    RateLimit-->>Express: OK

    Express->>Express: Validate inputs (regex, length)
    Express->>Express: bcryptjs.hash(password, 10) ~100ms

    Express->>DB: INSERT INTO users (username, email, hashed_password)
    alt Unique violation (error code 23505)
        DB-->>Express: Email or username taken
        Express-->>Browser: 409 Conflict
    end
    DB-->>Express: { id, username }

    Express->>Express: jwt.sign({ sub: id, username }, secret, 7d)
    Express-->>Browser: 201 { token, username }
    Browser->>Browser: localStorage.setItem("rg_auth_token", token)

    Note over Browser,DB: LOGIN

    Browser->>Express: POST /api/auth/login { email, password }
    Express->>RateLimit: Check IP (5/15min)

    Express->>DB: SELECT id, username, password WHERE email = $1
    DB-->>Express: User row or null

    Note over Express: Always call bcryptjs.compare()\neven if user not found\n→ timing attack prevention
    Express->>Express: bcryptjs.compare(password, hash)

    alt Wrong password or no user
        Express-->>Browser: 401 Unauthorized
    end

    Express->>Express: jwt.sign(...)
    Express-->>Browser: 200 { token, username }
```

---

## 8. Rate Limiter — Sliding Window

```mermaid
sequenceDiagram
    participant Req as Request
    participant Limiter as Rate Limiter (login: 5/15min)
    participant Store as IP Timestamp Store (Map)

    Req->>Limiter: POST /api/auth/login from 1.2.3.4

    Limiter->>Store: timestamps["1.2.3.4"]
    Store-->>Limiter: [t1, t2, t3, t4, t5]

    Note over Limiter: windowStart = now - 15min\nDrop timestamps older than windowStart
    Note over Limiter: All 5 within window → count=5 >= max=5 → BLOCK

    Limiter-->>Req: 429 Too Many Requests\n{ retryAfterMs: 847000 }

    Note over Req,Store: 15 minutes later...

    Req->>Limiter: POST /api/auth/login from 1.2.3.4
    Limiter->>Store: timestamps["1.2.3.4"]
    Limiter->>Limiter: Drop all 5 (all expired)
    Note over Limiter: count=0 < max=5 → ALLOW
    Limiter->>Store: Push now
    Limiter-->>Req: next() → route handler
```

---

## 9. Station Prefetching — Timeline

```mermaid
gantt
    title Station Load Timeline (With vs Without Prefetch + Redis)
    dateFormat X
    axisFormat %ss

    section Without Prefetch (cold cache)
    User reads result screen       :a1, 0, 10
    Click Next - loading phase     :a2, 10, 11
    Redis MISS - fetch Radio Browser :a3, 11, 13
    Audio starts                   :a4, 13, 14

    section With Prefetch (cold cache)
    User reads result screen       :b1, 0, 10
    Prefetch fires - Redis MISS    :b2, 0, 2
    Prefetch ready in state        :milestone, b3, 2, 0
    Click Next - consume prefetch  :b4, 10, 11
    Audio starts                   :b5, 11, 12

    section With Prefetch (warm Redis cache)
    User reads result screen       :c1, 0, 10
    Prefetch fires - Redis HIT     :c2, 0, 1
    Prefetch ready sub-20ms        :milestone, c3, 1, 0
    Click Next - consume prefetch  :c4, 10, 11
    Audio starts instantly         :c5, 11, 11
```

---

## 10. Frontend Component Hierarchy

```mermaid
graph TD
    main["main.jsx\n(React root)"]
    App["App.jsx\n(Globe canvas + phase routing)"]

    Start["StartScreen.jsx\n(phase = start)"]
    Playing["PlayingScreen.jsx\n(phase = playing)"]
    Result["ResultScreen.jsx\n(phase = result)"]
    Final["FinalScreen.jsx\n(phase = final)"]

    Globe["Globe.jsx\n(Three.js / Globe.gl)"]
    Volume["VolumeControl.jsx"]
    Tooltip["ScoreboardTooltip.jsx"]
    Auth["AuthModal.jsx"]
    LB["Leaderboard.jsx"]

    main --> App
    App --> Globe
    App --> Start
    App --> Playing
    App --> Result
    App --> Final

    Start --> Auth
    Start --> LB
    Final --> Auth
    Playing --> Volume
    Playing --> Tooltip
```

---

## 11. Deployment — Current vs Production Target

```mermaid
graph LR
    subgraph Current["Current (Dev / Single Server)"]
        V["Vite :5173\n(Frontend)"]
        E["Express :3001\n(Backend)"]
        V -->|"Proxy /api/*"| E
        E -->|"HTTP GET/SET EX 600"| UP["Upstash Redis\n(shared, persistent)"]
        E --> SB["Supabase\n(Postgres)"]
        E --> RB["Radio Browser\n(External API)"]
    end

    subgraph Prod["Production Target (Horizontal Scale)"]
        CDN["CDN\n(Static frontend)"]
        LB2["Load Balancer"]
        E1["Express Instance 1"]
        E2["Express Instance 2"]
        REDIS["Upstash Redis\n(cache shared across\nboth instances)"]
        PG2["PostgreSQL\n(Managed DB)"]
        LB2 --> E1
        LB2 --> E2
        E1 -->|"Cache HIT/MISS\nshared state"| REDIS
        E2 -->|"Cache HIT/MISS\nshared state"| REDIS
        E1 --> PG2
        E2 --> PG2
        CDN --> LB2
    end
```

---

## 12. Score Calculation Visualised

```mermaid
xychart-beta
    title "Score vs Distance (km)"
    x-axis "Distance (km)" [0, 1000, 2000, 3000, 4000, 5000, 7000, 10000]
    y-axis "Score" 0 --> 5000
    line [5000, 2838, 1608, 912, 517, 293, 94, 7]
```

Formula: `score = 5000 × e^(−10 × km / 20015)`

| Distance | Score |
|---|---|
| 0 km (perfect) | 5,000 |
| 100 km | 4,754 |
| 500 km | 3,802 |
| 1,000 km | 2,888 |
| 2,000 km | 1,665 |
| 5,000 km | 293 |
| 10,000 km | 7 |
| 20,015 km (antipode) | ~0 |
