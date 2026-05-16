# System Design: Collaborative Text Editor (e.g., Google Docs)

## 1. Requirement Gathering

### 1.1 Functional Requirements

- **Create & Retrieve Documents:** Users can create new documents, title them, and fetch existing ones instantly.
- **Collaborative Editing:** Multiple users can edit the same document simultaneously, seeing each other's changes in real-time.
- **Rich Text Formatting:** Support for full document styling (bold, italic, underline, fonts, sizes, alignments).
- **Live Cursors & Presence:** Users can see the real-time cursor positions and presence (who is online) of other collaborators.
- **Offline Access & Sync:** Users can edit documents offline, and the system automatically reconciles and syncs changes upon reconnection.
- **Access Control:** Fine-grained sharing permissions (Owner, Editor, Commenter, Viewer).

### 1.2 Non-Functional Requirements

- **Low Latency:** Real-time text sync must happen within milliseconds to ensure a fluid typing experience ($< 100\text{ ms}$ local echo, $< 500\text{ ms}$ peer replication).
- **Consistency:** The system must resolve conflicts gracefully. All users must eventually see the exact same document state (**Eventual Consistency**).
- **Scalability:** The system must scale to handle millions of active users and thousands of concurrent edits on a single hot document.
- **High Availability & Durability:** Edits must be auto-saved and durable. The service should withstand server failures without losing data.

---

## 2. Capacity Estimation

### Users and Documents

- **Daily Active Users (DAU):** 50 million
- **Peak Concurrent Users:** $\sim 1\text{ million}$
- **Total Documents Stored:** 2 billion

### Storage Estimation

- **Document Content:** An average document is roughly $100\text{ KB}$ (structured text, formatting, metadata).

$$\text{Total Doc Storage} = 2\text{ Billion} \times 100\text{ KB} = 200\text{ TB}$$

- **Version History Storage:** Assuming an average of 50 versions per document saved as deltas ($1\text{ KB}$ per delta):

$$\text{Version Storage} = 2\text{ Billion} \times 50 \times 1\text{ KB} = 100\text{ TB}$$

- **Total Data Footprint:** $\sim 300\text{ TB}$

### Network Bandwidth (Throughput)

- If 1 million concurrent users are typing at an average speed of 2 characters per second, the system must process $2\text{ million operations/sec}$.
- If each operation payload (character + metadata + formatting) is around $500\text{ bytes}$:

$$\text{Ingress Bandwidth} = 2,000,000 \times 500\text{ bytes} = 1\text{ GB/s}$$

---

## 3. High-Level Architecture

Google Docs requires two main pathways: an HTTP REST pathway for standard CRUD actions (metadata, sharing, creating docs) and a WebSocket pathway for real-time collaboration.

### Core Components

- **API Gateway:** Handles authentication, rate limiting, and routes standard HTTP requests to the appropriate microservices.
- **Document Service:** Manages HTTP requests for creating, updating metadata (titles, permissions), and retrieving documents.
- **Real-Time Collaboration Service (WebSocket Server):** Maintains persistent, bidirectional connections with active clients to pass edit operations back and forth instantly.
- **Concurrency/Conflict Resolution Engine:** A cluster of stateful background workers assigned to handle conflicting edits on active documents.
- **Presence Service:** Tracks who is currently viewing/editing a document and broadcasts cursor coordinates.

---

## 4. Database Design

We must separate relational metadata from volatile, high-throughput editing data.

### 4.1 User & Document Metadata (Relational DB / NewSQL)

Great for ACID compliance to handle sharing rights and user accounts. Can use PostgreSQL or Google Spanner.

#### `users` Table

| Column Name  | Type      | Description                    |
| ------------ | --------- | ------------------------------ |
| `user_id`    | UUID (PK) | Unique identifier for the user |
| `name`       | VARCHAR   | User's display name            |
| `email`      | VARCHAR   | User's email address           |
| `created_at` | TIMESTAMP | Timestamp of account creation  |

#### `documents` Table

| Column Name  | Type      | Description                        |
| ------------ | --------- | ---------------------------------- |
| `doc_id`     | UUID (PK) | Unique identifier for the document |
| `title`      | VARCHAR   | Title of the document              |
| `owner_id`   | UUID (FK) | References `users.user_id`         |
| `created_at` | TIMESTAMP | Timestamp of document creation     |
| `updated_at` | TIMESTAMP | Timestamp of last modification     |

#### `permissions` Table

| Column Name | Type                | Description                     |
| ----------- | ------------------- | ------------------------------- |
| `doc_id`    | UUID (Composite PK) | References `documents.doc_id`   |
| `user_id`   | UUID (Composite PK) | References `users.user_id`      |
| `role`      | ENUM                | `'VIEW'`, `'COMMENT'`, `'EDIT'` |

### 4.2 Document Content & Operations (NoSQL / Key-Value)

Document structures are highly hierarchical. We store snapshots in a NoSQL Document Store (like MongoDB or DynamoDB) or an Object Store (S3 / Google Cloud Storage) for cold documents.

#### `operations_log` (Cassandra or DynamoDB)

Optimized for high-frequency writes to track every single keystroke/delta sequentially.

- **Partition Key:** `doc_id`
- **Sort Key:** `sequence_number`

| Column Name       | Type   | Description                                           |
| ----------------- | ------ | ----------------------------------------------------- |
| `doc_id`          | UUID   | Partition Key                                         |
| `sequence_number` | BIGINT | Ordered index of the operation                        |
| `user_id`         | UUID   | User who performed the edit                           |
| `operation_type`  | ENUM   | `INSERT`, `DELETE`, `FORMAT`                          |
| `payload`         | JSON   | Contains position, characters, and styling attributes |

---

## 5. API Design

### 5.1 Document Management (REST)

- `POST /api/v1/documents` $\rightarrow$ Creates a new document.
- `GET /api/v1/documents/{doc_id}` $\rightarrow$ Fetches document metadata and the latest text snapshot.
- `PATCH /api/v1/documents/{doc_id}` $\rightarrow$ Updates document metadata (e.g., changing the title).

### 5.2 Real-Time Collaboration (WebSocket Events)

Once connected via `ws://[collaboration.googledocs.com/connect](https://collaboration.googledocs.com/connect)`, communication shifts to event frames:

#### Client to Server (Sending an Edit)

```json
{
  "event": "edit_op",
  "doc_id": "d123",
  "user_id": "u456",
  "seq_num": 104,
  "op": {
    "type": "insert",
    "pos": 12,
    "val": "a",
    "attributes": { "bold": true }
  }
}
```

#### Server to Client (Broadcasting an Edit)

```json
{
  "event": "broadcast_op",
  "doc_id": "d123",
  "seq_num": 105,
  "user_id": "u456",
  "op": {
    "type": "insert",
    "pos": 12,
    "val": "a"
  }
}
```

---

## 6. Design Deep Dive: Concurrency & Conflict Resolution

The heart of Google Docs is how it handles two users typing at the exact same location at the exact same time.

### 6.1 Operational Transformation (OT)

Google Docs historically relies on **Operational Transformation (OT)**. OT is a centralized approach where a single server dictates the definitive timeline.

#### How OT Works

Imagine a document containing the text: `"cat"`.

- **User A** wants to insert `"h"` at index 0 to make it `"chat"`. Operation: $O_1 = \text{Insert}(0, \text{"h"})$.
- **User B** wants to insert `"s"` at index 3 to make it `"cats"`. Operation: $O_2 = \text{Insert}(3, \text{"s"})$.

If both send their operations simultaneously, the server receives them. If the server executes $O_1$ first, the document becomes `"chat"`.

If User B's original operation $O_2$ ($\text{Insert}(3, \text{"s"})$) is applied blindly to `"chat"`, the `"s"` lands in the wrong spot because the word's length shifted!

OT solves this by transforming the operations:

- The server transforms $O_2$ relative to $O_1$: $O_2' = \text{Insert}(4, \text{"s"})$.
- The text correctly resolves to `"chats"` for everyone.

> 💡 **Pros:** True single-source-of-truth timeline; highly memory-efficient for document representation.
> ⚠️ **Cons:** Highly stateful server architecture. The server must keep track of what version history every connected client is currently looking at to calculate transformations properly.

### 6.2 Conflict-Free Replicated Data Types (CRDT)

An alternative modern approach (used by Figma and newer collaborative tools) is **CRDT**. Instead of transforming indexes based on a central timeline, CRDT assigns a globally unique, mathematically ordered identifier to _every single character_.

- Characters are treated as items in a linked list.
- If a character is added between index 1 (ID: `1.0`) and index 2 (ID: `2.0`), it receives an allocated ID like `1.5`.
- Even if operations arrive out of order, or completely offline, they can be merged deterministically on any machine without a centralized server restructuring them.

> 💡 **Pros:** Excellent for peer-to-peer syncing and handling complex offline edits easily.
> ⚠️ **Cons:** High storage/memory overhead due to the metadata tracking unique IDs for every character.

### 6.3 Alternative: Conflict-Free Replicated Data Types (CRDTs)

If the interviewer pushes for a decentralized or serverless-friendly model, discuss **CRDTs** (e.g., _Yjs_ or _Automerge_).

Instead of absolute indices ($0, 1, 2, \dots$), CRDTs give every character a globally unique identifier composed of a `(siteId, counter)` and a position identifier (like a fraction or fractional index).

- **Example Position Allocation:** If character `'A'` has index `[1]` and character `'B'` has index `[2]`, an inserted character `'X'` between them will be assigned position `[1.5]`. If another user inserts `'Y'` concurrently, it might get position `[1.7]`.
- Because `[1.5] < [1.7]` deterministically everywhere, all nodes sort the characters identically without consulting a central server.

---

### Trade-off Matrix: OT vs. CRDT

| Metric               | Operational Transformation (OT)                                                        | Conflict-Free Replicated Data Types (CRDT)                                              |
| -------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Server State**     | **Heavy & Stateful.** Server must actively compute transforms and track client states. | **Lightweight/Stateless.** Server acts merely as an operation router/relay.             |
| **Storage Overhead** | **Low.** Only character data and a linear log of mutations are stored.                 | **High.** Every single character ever typed carries structural metadata and unique IDs. |
| **Network Payload**  | **Small.** Highly optimized index-based changes.                                       | **Larger.** Contains positional identifiers and site histories.                         |
| **Complexity**       | Centralized algorithmic complexity (handling edge-case transform loops).               | Distributed complexity (garbage-collecting deleted item metadata/tombstones).           |

---

## 7. Scaling and Optimization

### 7.1 Session Server Sharding

Because OT requires stateful servers to track active documents, we use **Sticky Sessions** or consistent hashing at the API Gateway layer. All users editing `doc_id: d123` are routed to the exact same **Collaboration Server Node**.

- This node keeps the document's unsaved operations in memory (**Redis/Memory Cache**) for ultra-fast processing.

### 7.2 Snapshots and Log Compaction

To avoid making a new user download millions of raw character deltas when opening a long-standing document:

- The system creates a static **Snapshot** of the document every 100–200 operations.
- When a user opens the document, they load the closest snapshot and pull only the few trailing deltas that occurred _after_ that snapshot's sequence number.

### 7.3 Offline Sync Handling

While offline, the client stores all local operations in an internal queue (**IndexedDB** in the browser). When the network reconnects:

1. The client sends its queued operations along with the base sequence number it last saw.
2. The server uses OT to catch up the client’s changes against everything that happened while it was gone, or pushes back a clean catch-up log.

---

## 8. WebSockets Explained: Use & Implementation

A fundamental requirement of real-time collaborative applications is instant, low-latency communication. Standard HTTP polling introduces unacceptable network overhead and rendering delays. This system solves this by adopting a **WebSocket** architecture.

### 8.1 Why WebSockets over HTTP?

- **Bidirectional Communication:** Unlike standard HTTP where only the client can initiate requests, WebSockets maintain a persistent TCP connection where both client and server can push messages raw at any moment.
- **Low Network Overhead:** Traditional HTTP requests carry heavy header payloads ($500\text{ bytes} - 2\text{ KB}$). A WebSocket frame has a framing overhead of only $2 - 10\text{ bytes}$ per packet after the handshake, significantly reducing ingress network utilization during rapid updates.
- **Real-Time Push Mechanics:** When User A types a character, the server must instantly force that delta down to User B. WebSockets allow the server to "fan-out" this mutation instantly without forcing User B to poll continuously.

### 8.2 Architectural Lifecycle & Flow

1. **Protocol Handshake:** The client initiates a standard HTTP/1.1 request containing specific upgrade headers:

```http
GET /api/v1/collaboration/connect HTTP/1.1
Host: collaboration.googledocs.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13

```

2. **Connection Upgrade:** The API Gateway validates the token, selects an available real-time server, and returns an `HTTP/1.1 101 Switching Protocols` status code. The raw TCP socket connection remains open, transforming into a custom binary/text streaming frame socket.
3. **Keep-Alives (Heartbeats):** To keep load balancers and firewalls from severing idle connections, both client and server exchange tiny `PING` and `PONG` control frames periodically (e.g., every 30 seconds).

### 8.3 Implementation Pattern within the System

- **WebSocket Gateway Tier:** A fleet of lightweight, reverse-proxy workers (e.g., built using Netty, Node.js, or Go's Epoll framework) are optimized to hold millions of concurrent open TCP connections.
- **Publish/Subscribe Distribution Layer:** When a client pushes a text update frame over its socket, the gateway extracts the `doc_id` and publishes the event to an internal cluster bus (e.g., **Redis Pub/Sub** or **Kafka**).
- **Session Assignment:** The server reading that specific document topic picks up the frame, passes it to the Operation Transformation (OT) engine, and writes the output back to the Redis pub/sub cluster, which prompts the gateway instances to blast the payload out to all matching connections.

---

## 9. Scaling Architecture

Moving from a single-machine paradigm to a globally scalable structure capable of supporting $50\text{ million}$ DAU requires systematic architectural splitting.

### 9.1 Two-Tier Layer Splitting

The infrastructure decouples completely into a **Stateless Tier** and a **Stateful Tier**:

- **Stateless Gateway Tier:** Manages login, permission checking, home screen loading, and document metadata indexing. These applications can easily scale up or down horizontally via an Elastic Load Balancer (ELB) since they carry no session context.
- **Stateful Collaboration Tier:** Every active document session must reside on a specific, bounded cluster instance because Operational Transformation depends on deterministic ordering.

### 9.2 Hot-Document Handling (Read vs. Write Splitting)

When a document goes viral—such as a live press release or a high-traffic company presentation—thousands of users connect to read it while only a select few type.

To prevent the master collaboration node from running out of network socket capacity:

- **Writers Pathway:** Users with `EDIT` clearance connect directly to the active **Primary Stateful Collaboration Server** managing the document's OT processing timeline.
- **Readers Pathway:** Users with `VIEW` clearance are transparently dynamically routed away to a downstream tier of read-only **Edge Fan-out Servers**.
- These edge instances maintain a local memory mirror of the document text and subscribe to a master message stream from the primary server. They handle the heavy lifting of updating thousands of passive client WebSockets without straining the core mutation engine.

---

## 10. Failure Handling & Recovery

Distributed nodes fail regularly. A robust collaborative engine must maintain transactional durability and continuous editing availability during hardware or network drops.

### 10.1 Active Memory Lease Management

To prevent split-brain timelines where two server nodes independently process transformations for the same `doc_id`, strict consensus management is required:

- Distributed coordination engines (such as **etcd** or a highly available **Redis Cluster**) manage single-owner document leases.
- When a user opens a document session, the selected server registers an exclusive lease lock: `lock:doc_id:d123 -> server_ip_1`. This lease is configured with a short, defensive Time-to-Live (TTL) of 10 seconds.
- The hosting node runs a continuous background thread to refresh this heartbeat lock every 3 seconds.

### 10.2 Crash Recovery Workflow

If the stateful collaboration node hosting a hot document crashes unexpectedly:

1. **Lease Expiration:** Within 10 seconds, its heartbeat ceases and the coordination lock expires.
2. **Client Reconnection Trigger:** The clients lose their raw TCP connections and immediately invoke an exponential backoff retry pattern to reconnect to the load balancer.
3. **Session Re-assignment:** The API Gateway discovers the original lock is gone, selects a healthy node (**Server 2**), and forwards the incoming client requests there.
4. **State Reconstruction:** Server 2 reads the latest immutable document snapshot from the Object Store (e.g., S3) and replays any trailing operations logged in the NoSQL database after that snapshot sequence number. The memory state matches the pre-crash state perfectly, and editing resumes seamlessly within seconds.

---

## 11. Challenges & Learnings

Building real-time collaborative text processors exposes unique engineering trade-offs that are highly valued in system design interviews.

- **The State Synchronization Bottleneck:** Statefulness cannot be entirely stripped out of a real-time system. Attempting to make OT completely stateless by querying a database on every character stroke creates immense database strain and invalidates latency constraints. Stateful memory caching must be embraced at the application layer.
- **UI/UX Pacing (Throttling and Debouncing):** Sending an individual WebSocket packet for every keystroke introduces unnecessary processing overhead. Client-side engines should batch operations over small temporal horizons ($50 - 100\text{ ms}$) or wait until a natural pause in typing occurs to bundle changes into single transaction packages, balancing latency with server efficiency.
- **The Operational Drift Paradox:** Due to network drops, packet reordering, or client-side calculation bugs, a user's local text state can occasionally drift from the server's master text model. The architecture must support a silent validation step: the server occasionally appends a checksum (e.g., an MD5 hash of the complete text string) to broadcast events. If the client computes a different local hash, it quietly re-syncs by downloading the authoritative state delta.

---

## 12. Trade-offs & Final Thoughts

### 12.1 Centralized (OT) vs. Decentralized (CRDT) Core Architecture

The structural selection of the conflict resolution model establishes the boundaries of the system:

- **Operational Transformation (OT)** is highly optimized for performance and minimizes both storage footprints and payload sizes. It acts as the industry standard for traditional enterprise software applications where absolute control over authorization and versioning history is required.
- **CRDT** offers superior behavior for edge computing, offline-first peer-to-peer applications, or systems requiring zero-trust central storage architectures. However, it trades off system resource consumption, requiring aggressive memory optimization to clean up old tombstones from deleted text.

### 12.2 Database Selection Strategy

Using a hybrid database model balances structural correctness with raw performance constraints:

- **NewSQL / Relational DBs** ensure that user metadata, documents titles, and folder ownership trees maintain strict relational integrity with high transactional guarantees.
- **Wide-Column NoSQL DBs** are optimized for the scale of sequential, low-latency append operations needed to log millions of active edits across billions of documents. No relational database could comfortably survive the write-throughput of an entire global user base typing simultaneously without immense sharding complexity.

```

```
