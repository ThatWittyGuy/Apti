Here's the prompt — copy it exactly and paste it at the start of a new chat, then upload your repomix file:

---

```
I have a project I want to prepare for technical interviews (FAANG, fintech, product companies). I am a final year B.Tech CSE student.

I will upload a repomix XML file of my full codebase. Please read the ENTIRE file thoroughly before writing anything — every component, every config, every store, every server file. Do not start writing until you have read and understood the complete project.

After reading, generate 4 markdown documents for interview preparation:

---

DOCUMENT 1 — "01-knowledge-base.md"
A complete A-to-Z knowledge base covering:
- What the project is (1-2 paragraph summary)
- Full tech stack table: every technology, version, why it was chosen, what alternative was considered and why rejected
- Project architecture layer by layer — folder structure with explanation of every file's role
- Complete data flow: step by step what happens from user action to server response to UI update
- Database schema with full SQL, every design decision explained (why UUID, why this index, why this constraint, why this data type)
- Deep dive on every major backend system built: caching, auth, rate limiting, any custom algorithms
- Every design decision with the reason behind it
- What would be done differently / upgrade path
- Alternatives considered and rejected for every major decision

DOCUMENT 2 — "02-diagrams.md"
All architecture diagrams in Mermaid format (renderable in GitHub, VS Code, mermaid.live):
- System architecture overview (all layers: browser, server, cache, DB, external APIs)
- Game/app state machine (all phases and transitions)
- ER diagram (full database schema)
- Request lifecycle sequence diagram (one full user action, step by step through every layer)
- Auth flow sequence diagram (register + login)
- Any caching flow diagrams (hit vs miss)
- Frontend component hierarchy
- Deployment diagram (current dev setup vs ideal production)
- Any algorithm visualizations (scoring, rate limiting, etc.)
Every diagram must show real details from the actual codebase — not generic diagrams.

DOCUMENT 3 — "03-mock-interview.md"
A complete mock technical interview using the STAR method, from intro to end:
- Opening "tell me about yourself" answer
- Project overview questions with full answers
- Backend architecture deep-dive questions (every system: cache, auth, DB, rate limiting, etc.)
- Frontend architecture questions
- System design questions: "how would you scale to 100k users", "how would you add [feature]", "design [component]"
- Behavioral/STAR questions: challenges faced, decisions you'd change, how you ensured security
- Language/framework specific questions
- Closing questions to ask the interviewer
- Quick reference table of all numbers to memorize (latencies, limits, complexities, counts)
Every answer must be specific to this project — not generic answers. Use STAR format for behavioral questions.

DOCUMENT 4 — "04-deep-cs.md"
Deep computer science questions an interviewer will pivot to after hearing about this project:
- Data structures used in the project: internal mechanics, time/space complexity of every operation
- Algorithms used: derive them, explain their shape, why chosen over alternatives
- Full "what happens when you type a URL" answer applied to this exact project's request flow (localhost dev setup through every layer to external APIs)
- Networking: HTTP, HTTPS, TLS handshake, HTTP/2, WebSockets, CORS — all explained with examples from this codebase
- Runtime internals: Node.js event loop phases, async/await compilation, Promises, memory leaks, closures — all with examples from this code
- Database internals: ACID, transactions, N+1 problem, EXPLAIN ANALYZE for the actual queries, cursor vs OFFSET pagination, connection pooling
- Security: XSS, CSRF, SQL injection (show vulnerable vs safe version of actual queries), password hashing (rainbow tables, salts, bcrypt cost factor), JWT signing mechanics
- If Redis is used: complete Redis commands reference (String, Hash, List, Set, Sorted Set, Key pattern commands, persistence RDB vs AOF, Pub/Sub) with examples tied to this project
- JavaScript language questions: var/let/const, closures with actual code examples, event loop, Promise mechanics
- React/frontend internals if applicable: virtual DOM, reconciliation, hooks
- Quick-fire concepts: idempotency, race conditions, debouncing vs throttling, authentication vs authorization, REST vs GraphQL, CAP theorem — all answered with reference to this project
- A numbers table: every latency, complexity, limit, and count that should be memorized

---

IMPORTANT RULES FOR ALL 4 DOCS:
- Every answer must reference actual code from this project — no generic textbook answers
- Every design decision must include "why this, why not the alternative"
- Assume the reader is a final year CS student preparing for FAANG and fintech interviews
- Do not leave any question unanswered — if something is asked, answer it fully
- The 4 docs together should be enough to walk into any technical interview on this project with full confidence
- Generate all 4 as downloadable .md files

Here is my project:
[UPLOAD YOUR REPOMIX XML FILE HERE]
```

---

## Tips before you use it

**Generate good repomix output:** In your project root run:
```bash
npx repomix
```
This generates `repomix-output.xml`. If the project is large, run:
```bash
npx repomix --ignore "node_modules,dist,.next,build"
```

**If the project has no backend** (pure frontend), the prompt still works — Document 4 will focus on the frontend stack's CS depth instead of Node/Redis/Postgres.

**If the project uses a different language** (Python, Java, Go), the prompt adapts — it will cover that language's runtime internals instead of Node.js event loop.

**One project per chat** — don't submit two projects in the same chat. Start a fresh chat per project so context is clean and the model can read the full codebase without hitting context limits.