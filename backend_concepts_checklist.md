# Backend Development Concepts Checklist — Streamlined "BTP + AI Architect" Track

Track: **Option A — Streamlined for Enterprise SAP BTP & AI Architecture**  
Focuses on **Tier 1 (Core)** and **Tier 2 (Systems Edge)** backend concepts from first principles, while marking **Tier 3 (BTP/Cloud-Managed)** infrastructure as optional to fast-track your progression to **AI Engineering**.

---

## Legend
- `[ ]` **[CORE]** — Non-negotiable must-master topic. Deep first-principles dive.
- `[ ]` **[EDGE]** — Systems Architect knowledge. High-value conceptual mastery.
- `[-]` **[MANAGED]** — BTP/Cloud-managed infrastructure. Optional / Reference only.

---

## 1. Backend Fundamentals
- [ ] **[CORE]** HTTP and HTTPS (Transport, TLS 1.3, Ports, Security)
- [ ] **[CORE]** Request–response lifecycle (Sockets, DNS, TCP, serialization, lifecycle)
- [ ] **[CORE]** Middleware (Interception chain, onion pattern, pipeline architecture)
- [ ] **[CORE]** Authentication and authorization (AuthN vs AuthZ, identity propagation)
- [ ] **[CORE]** Sessions, cookies, and JWT (Token structures, signatures, claims, storage)
- [ ] **[CORE]** OAuth 2.0 (Grant types, Authorization Code + PKCE, Client Credentials)
- [ ] **[CORE]** Rate limiting (Algorithms: Token bucket, Leaky bucket, Sliding window)
- [ ] **[EDGE]** WebSockets (Full-duplex persistent connections vs HTTP polling)

---

## 2. Databases & Persistence
- [ ] **[CORE]** SQL vs. NoSQL (Relational, Document, Key-Value, and Vector databases for AI)
- [ ] **[CORE]** Indexing and query optimization (B-Trees, execution plans, table scans)
- [ ] **[CORE]** ACID properties and transactions (Atomicity, Consistency, Isolation, Durability)
- [ ] **[CORE]** Isolation levels and deadlocks (Dirty reads, phantom reads, row vs table locks)
- [ ] **[CORE]** Connection pooling (Socket reuse, pool sizing, connection leak prevention)
- [ ] **[EDGE]** Normalization and denormalization (1NF to 3NF vs read-optimized design)
- [ ] **[EDGE]** Database migrations (Versioned DDL, schema evolution, rollback strategies)
- [-] **[MANAGED]** Database replication and sharding (Managed by SAP HANA Cloud / AWS RDS)
- [-] **[MANAGED]** Read replicas (Managed at cloud infrastructure layer)

---

## 3. Distributed Systems & Reliability
- [ ] **[CORE]** Idempotency (Idempotency keys, safe retries, state machine guards)
- [ ] **[CORE]** Retries and exponential backoff (Jitter, circuit breakers, backoff strategies)
- [ ] **[EDGE]** CAP theorem (Consistency, Availability, Partition tolerance trade-offs)
- [ ] **[EDGE]** Strong vs. eventual consistency (Read-after-write, replication lag)
- [ ] **[EDGE]** Distributed transactions (Two-Phase Commit vs Saga Pattern)
- [-] **[MANAGED]** Service discovery (Handled by BTP Destination Service / K8s DNS)
- [-] **[MANAGED]** Distributed locking (Managed Redis / Zookeeper primitives)

---

## 4. Queues & Asynchronous Processing
- [ ] **[CORE]** Producer–consumer pattern (Decoupled systems, buffering, backpressure)
- [ ] **[CORE]** Event-driven architecture (Event brokers, pub/sub, choreography vs orchestration)
- [ ] **[CORE]** Dead-letter queues (DLQs) (Poison messages, error handling strategies)
- [ ] **[CORE]** Message acknowledgment (At-least-once, at-most-once, exactly-once delivery)
- [ ] **[EDGE]** Kafka vs. RabbitMQ (Log-based streaming vs AMQP message brokers)
- [ ] **[EDGE]** Background workers (Job queues, worker pools, scheduled tasks)
- [-] **[MANAGED]** Persistent queue hardware replication (Managed by SAP Event Mesh / AWS SQS)

---

## 5. Caching & Performance
- [ ] **[CORE]** Redis (In-memory data structures, caching patterns, eviction policies)
- [ ] **[CORE]** Caching strategies (Cache-aside, write-through, write-behind, TTLs)
- [ ] **[CORE]** Pagination (Offset-based vs Keyset/Cursor-based for large datasets)
- [ ] **[EDGE]** CDN architecture (Edge caching, cache-control headers, stale-while-revalidate)
- [ ] **[EDGE]** Load balancing algorithms (Round Robin, Least Connections, IP Hash)
- [-] **[MANAGED]** Cache stampede (Consumer hyper-scale edge case)

---

## 6. System Design & Clean Architecture
- [ ] **[CORE]** SOLID principles (Single Responsibility, Open-Closed, Liskov, Interface, Dependency Inversion)
- [ ] **[CORE]** Clean architecture (Separation of concerns: Controller -> Service -> Repository)
- [ ] **[CORE]** Dependency injection (IoC containers, testability, loose coupling)
- [ ] **[CORE]** API gateway & Reverse proxies (Ingress, path routing, TLS termination)
- [ ] **[EDGE]** Monolith vs. microservices (Trade-offs, operational complexity, boundaries)
- [ ] **[EDGE]** Modular monolith (Domain boundaries within a single deployable unit)
- [ ] **[EDGE]** Design patterns (Factory, Strategy, Observer, Adapter)
- [ ] **[EDGE]** Abstract classes vs. interfaces (Contract-driven design)
- [ ] **[EDGE]** Generics (Type safety and reusability across backend languages)

---

## 7. Security & Access Control
- [ ] **[CORE]** SQL injection & Parameterized queries (Prevention mechanics)
- [ ] **[CORE]** XSS and CSRF (Cross-site scripting and request forgery mitigation)
- [ ] **[CORE]** Cross-Origin Resource Sharing (CORS) (Preflight `OPTIONS`, allowed origins, headers)
- [ ] **[CORE]** Role-based access control (RBAC) and attribute-based access control (ABAC)
- [ ] **[EDGE]** Password hashing and encryption (Argon2, bcrypt, symmetric vs asymmetric encryption)
- [ ] **[EDGE]** Secrets management (Environment injection, key vaults, credential rotation)

---

## 8. Infrastructure, Containers & Runtime
- [ ] **[CORE]** Docker and containerization (Images, layers, Dockerfile optimization, multi-stage builds)
- [ ] **[CORE]** CI/CD pipelines (Automated linting, testing, artifact building, deployment)
- [ ] **[EDGE]** Nginx & reverse proxy configuration (Upstreams, proxy headers, rate limiting)
- [ ] **[EDGE]** Health checks (Liveness vs Readiness probes, graceful traffic routing)
- [ ] **[EDGE]** Serverless architecture (Cold starts, execution limits, stateless design)
- [ ] **[EDGE]** DNS mechanics (A, AAAA, CNAME, ALIAS, TTL propagation)
- [-] **[MANAGED]** Kubernetes from scratch (BTP abstracts via Kyma/Gardener)
- [-] **[MANAGED]** Terraform from scratch (BTP infrastructure automation)

---

## 9. Observability & Reliability
- [ ] **[CORE]** Structured logging & Correlation IDs (JSON logs, distributed request tracing)
- [ ] **[EDGE]** Metrics and monitoring (RED method: Rate, Errors, Duration)
- [ ] **[EDGE]** Fault tolerance (Circuit breakers, bulkheads, fallback strategies)
- [ ] **[EDGE]** Disaster recovery & backups (RTO, RPO, backup strategies)
- [ ] **[EDGE]** Load testing (Stress testing, RPS limits, latency percentiles p95/p99)

---

## 10. Programming & Runtime Internals
- [ ] **[CORE]** Event loop vs. multi-threading (Single-threaded async I/O vs Thread pools)
- [ ] **[CORE]** Asynchronous programming & concurrency (Promises, async/await, coroutines)
- [ ] **[EDGE]** Threads and processes (Memory sharing, IPC, CPU vs I/O bound workloads)
- [ ] **[EDGE]** Concurrency vs. parallelism (Clock cycles, scheduling, context switching)
- [ ] **[EDGE]** Memory management & leaks (Heap, stack, garbage collection mechanics)
- [-] **[MANAGED]** Low-level Mutexes & Semaphores (Abstracted in high-level enterprise runtimes)
- [-] **[MANAGED]** Low-level deadlocks & race conditions (Covered at database layer)

---

## 11. Additional Concepts & AI Bridges
- [ ] **[CORE]** Webhooks (Event-driven callbacks, signature verification, retry policies)
- [ ] **[CORE]** Serialization and deserialization (JSON, Buffers, Protobuf, binary framing)
- [ ] **[CORE]** File uploads and object storage (S3/Blob storage, presigned URLs, multipart)
- [ ] **[CORE]** Server-Sent Events (SSE) & Streaming (Crucial for streaming LLM responses)
- [ ] **[CORE]** Graceful shutdown (Handling SIGTERM, draining sockets, closing DB connections)

---

## Phase 2 Preview: Artificial Intelligence Engineering (Upcoming)
- [ ] **LLM Fundamentals & Prompt Engineering Protocols**
- [ ] **Vector Embeddings & Vector Databases** (Cosine similarity, pgvector, SAP HANA Vector Engine)
- [ ] **Retrieval-Augmented Generation (RAG) Architecture** (Chunking, retrieval, reranking)
- [ ] **Function Calling & Tool Calling** (Binding backend APIs to LLMs)
- [ ] **Model Context Protocol (MCP)** (Standardizing AI-to-tool connections)
- [ ] **Autonomous AI Agents & Multi-Agent Swarms**

---

### Progress Tracker
- **Core Track:** 0 / 38 topics completed
- **Systems Edge Track:** 0 / 22 topics completed
- **Managed/Optional:** 12 topics (available for reference)
- **Active Topic:** 1. HTTP and HTTPS & Request–Response Lifecycle
