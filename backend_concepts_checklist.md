# Backend + AI Concepts Checklist — Streamlined "BTP + AI Architect" Track

Track: **Option A — Streamlined for Enterprise SAP BTP & AI Architecture**  
Focuses on **Tier 1 (Core)** and **Tier 2 (Systems Edge)** concepts from first principles, while marking **Tier 3 (Managed/Cloud-provided)** infrastructure as optional to fast-track your progression to **AI Engineering**.

Concepts are taught stack-agnostic. The SAP BTP mapping lives in one place: **Appendix A (BTP Lens)** at the bottom.

---

## Legend
- `[ ]` **[CORE]** — Non-negotiable must-master topic. Deep first-principles dive.
- `[ ]` **[EDGE]** — Systems Architect knowledge. High-value conceptual mastery.
- `[-]` **[MANAGED]** — Provided by the cloud platform. Optional / reference only.

## Study-Order Note
Sections are a catalog, not a strict sequence. Prerequisites to respect:
- **Section 10** (threads, concurrency, async) before **Sections 3–4** (distributed systems, queues).
- **Section 4** (events, queues) before **Saga / distributed transactions** in Section 3.
- **Sections 1, 2, 7** before **Section 14** (RAG needs auth, DB and security basics).
- All of **Sections 1–11** before **Sections 13–16** (AI Engineering).

---

## 1. Backend Fundamentals
- [x] **[CORE]** HTTP and HTTPS (Methods, status codes, headers, HTTP/1.1 vs 2 vs 3, TLS 1.3, ports)
- [x] **[CORE]** Request–response lifecycle (Sockets, DNS, TCP, serialization, lifecycle)
- [x] **[CORE]** REST API design (Resources, verbs, idempotent methods, filtering, error format)
- [ ] **[CORE]** Input validation & error handling (Schema validation, fail-fast, consistent error responses, RFC 9457)
- [ ] **[CORE]** Middleware (Interception chain, onion pattern, pipeline architecture)
- [ ] **[CORE]** Authentication and authorization (AuthN vs AuthZ, identity propagation)
- [ ] **[CORE]** Sessions, cookies, and JWT (Token structures, signatures, claims, storage)
- [ ] **[CORE]** OAuth 2.0 (Grant types, Authorization Code + PKCE, Client Credentials)
- [ ] **[CORE]** OpenID Connect (ID token, discovery, userinfo; the AuthN layer on top of OAuth 2.0)
- [ ] **[CORE]** Rate limiting (Algorithms: Token bucket, Leaky bucket, Sliding window)
- [ ] **[EDGE]** SAML & SSO (Federation, IdP vs SP, assertions, trust relationships)
- [ ] **[EDGE]** API versioning & contracts (OpenAPI, backward compatibility, deprecation)
- [ ] **[EDGE]** gRPC & GraphQL (RPC and query-style APIs vs REST, when to choose which)
- [ ] **[EDGE]** WebSockets (Full-duplex persistent connections vs HTTP polling)

---

## 2. Databases & Persistence
- [ ] **[CORE]** SQL vs. NoSQL (Relational, Document, Key-Value, and Vector databases for AI)
- [ ] **[CORE]** Indexing and query optimization (B-Trees, execution plans, table scans)
- [ ] **[CORE]** ACID properties and transactions (Atomicity, Consistency, Isolation, Durability)
- [ ] **[CORE]** Isolation levels and deadlocks (Dirty reads, phantom reads, row vs table locks)
- [ ] **[CORE]** Optimistic vs. pessimistic locking (Version columns, `SELECT ... FOR UPDATE`, lost updates)
- [ ] **[CORE]** ORMs and the N+1 problem (ORM vs query builder vs raw SQL, lazy vs eager loading)
- [ ] **[CORE]** Connection pooling (Socket reuse, pool sizing, connection leak prevention)
- [ ] **[EDGE]** Normalization and denormalization (1NF to 3NF vs read-optimized design)
- [ ] **[EDGE]** Database migrations (Versioned DDL, schema evolution, rollback strategies)
- [-] **[MANAGED]** Database replication and sharding (Operated by the managed DB service; know the concepts via Section 3)
- [-] **[MANAGED]** Read replicas (Managed at cloud infrastructure layer)

---

## 3. Distributed Systems & Reliability
- [ ] **[CORE]** Timeouts and deadlines (Connect vs read timeouts, deadline propagation, why no-timeout is a bug)
- [ ] **[CORE]** Idempotency (Idempotency keys, safe retries, state machine guards)
- [ ] **[CORE]** Retries and exponential backoff (Jitter, retry budgets, which errors are retryable)
- [ ] **[EDGE]** CAP theorem (Consistency, Availability, Partition tolerance trade-offs)
- [ ] **[EDGE]** Strong vs. eventual consistency (Read-after-write, replication lag)
- [ ] **[EDGE]** Distributed transactions (Two-Phase Commit vs Saga Pattern; study after Section 4)
- [-] **[MANAGED]** Service discovery (Handled by platform DNS / orchestrator; know what problem it solves)
- [-] **[MANAGED]** Distributed locking (Use managed primitives; understand leases and fencing-token pitfalls)

---

## 4. Queues & Asynchronous Processing
- [ ] **[CORE]** Producer–consumer pattern (Decoupled systems, buffering, backpressure)
- [ ] **[CORE]** Event-driven architecture (Event brokers, pub/sub, choreography vs orchestration)
- [ ] **[CORE]** Message acknowledgment (At-least-once, at-most-once, exactly-once delivery)
- [ ] **[CORE]** Dead-letter queues (DLQs) (Poison messages, retry policies, error handling strategies)
- [ ] **[EDGE]** Transactional outbox (Solving the DB-write + publish dual-write problem)
- [ ] **[EDGE]** Event schemas & contracts (CloudEvents, schema evolution, consumer compatibility)
- [ ] **[EDGE]** Kafka vs. RabbitMQ (Log-based streaming vs AMQP message brokers)
- [ ] **[EDGE]** Background workers (Job queues, worker pools, scheduled tasks)
- [-] **[MANAGED]** Durable queue storage and replication (Operated by the managed messaging service)

---

## 5. Caching & Performance
- [ ] **[CORE]** Redis (In-memory data structures, caching patterns, eviction policies)
- [ ] **[CORE]** Caching strategies (Cache-aside, write-through, write-behind, TTLs, invalidation)
- [ ] **[CORE]** Pagination (Offset-based vs Keyset/Cursor-based for large datasets)
- [ ] **[EDGE]** Cache stampede (Thundering herd, request coalescing, jittered TTLs; an app-level concern, not a platform one)
- [ ] **[EDGE]** CDN architecture (Edge caching, cache-control headers, stale-while-revalidate)
- [ ] **[EDGE]** Load balancing algorithms (Round Robin, Least Connections, IP Hash)

---

## 6. System Design & Clean Architecture
- [ ] **[CORE]** SOLID principles (Single Responsibility, Open-Closed, Liskov, Interface, Dependency Inversion)
- [ ] **[CORE]** Clean architecture (Separation of concerns: Controller -> Service -> Repository)
- [ ] **[CORE]** Dependency injection (IoC containers, testability, loose coupling)
- [ ] **[CORE]** API gateway & Reverse proxies (Ingress, path routing, TLS termination)
- [ ] **[EDGE]** Monolith vs. microservices (Trade-offs, operational complexity, boundaries)
- [ ] **[EDGE]** Modular monolith (Domain boundaries within a single deployable unit)
- [ ] **[EDGE]** Domain-driven design basics (Bounded contexts, aggregates, ubiquitous language)
- [ ] **[EDGE]** CQRS & event sourcing (Separating read/write models, event log as source of truth)
- [ ] **[EDGE]** Multi-tenancy (Shared schema vs schema-per-tenant vs DB-per-tenant, tenant context, noisy neighbors)
- [ ] **[EDGE]** Design patterns (Factory, Strategy, Observer, Adapter)
- [ ] **[EDGE]** Abstract classes vs. interfaces (Contract-driven design)
- [ ] **[EDGE]** Generics (Type safety and reusability across backend languages)

---

## 7. Security & Access Control
- [ ] **[CORE]** OWASP Top 10 & secure coding (Broken access control, SSRF, mass assignment, security misconfiguration)
- [ ] **[CORE]** SQL injection & Parameterized queries (Prevention mechanics)
- [ ] **[CORE]** XSS and CSRF (Cross-site scripting and request forgery mitigation)
- [ ] **[CORE]** Cross-Origin Resource Sharing (CORS) (Preflight `OPTIONS`, allowed origins, headers)
- [ ] **[CORE]** Role-based access control (RBAC) and attribute-based access control (ABAC)
- [ ] **[EDGE]** Password hashing and encryption (Argon2, bcrypt, symmetric vs asymmetric encryption)
- [ ] **[EDGE]** Secrets management (Environment injection, key vaults, credential rotation)
- [ ] **[EDGE]** TLS in depth & mTLS (Certificate chains, CAs, client certificates, service-to-service trust)
- [ ] **[EDGE]** Dependency & supply-chain security (SCA scanning, lockfiles, SBOM, pinned images)

---

## 8. Infrastructure, Containers & Runtime
- [ ] **[CORE]** Docker and containerization (Images, layers, Dockerfile optimization, multi-stage builds)
- [ ] **[CORE]** CI/CD pipelines (Automated linting, testing, artifact building, deployment)
- [ ] **[CORE]** Twelve-factor app (Config in env, stateless processes, disposability, logs as streams, backing services)
- [ ] **[EDGE]** Deployment strategies (Rolling, blue-green, canary, feature flags, rollback)
- [ ] **[EDGE]** Kubernetes core concepts (Pods, Deployments, Services, Ingress, ConfigMaps/Secrets; usage, not cluster building)
- [ ] **[EDGE]** Nginx & reverse proxy configuration (Upstreams, proxy headers, rate limiting)
- [ ] **[EDGE]** Health checks (Liveness vs Readiness probes, graceful traffic routing)
- [ ] **[EDGE]** Serverless architecture (Cold starts, execution limits, stateless design)
- [ ] **[EDGE]** DNS mechanics (A, AAAA, CNAME, ALIAS, TTL propagation)
- [-] **[MANAGED]** Kubernetes cluster operations from scratch (Control plane, node management; handled by the platform)
- [-] **[MANAGED]** Terraform / infrastructure-as-code from scratch (Platform provisioning automation)

---

## 9. Observability & Reliability
- [ ] **[CORE]** Structured logging & Correlation IDs (JSON logs, request IDs across services)
- [ ] **[EDGE]** Distributed tracing & OpenTelemetry (Spans, trace context propagation; logs vs metrics vs traces)
- [ ] **[EDGE]** Metrics and monitoring (RED method: Rate, Errors, Duration)
- [ ] **[EDGE]** SLI / SLO / SLA & alerting (Error budgets, actionable alerts)
- [ ] **[EDGE]** Fault tolerance (Circuit breakers, bulkheads, fallback strategies)
- [ ] **[EDGE]** Disaster recovery & backups (RTO, RPO, backup strategies)
- [ ] **[EDGE]** Load testing (Stress testing, RPS limits, latency percentiles p95/p99)

---

## 10. Programming & Runtime Internals
- [ ] **[CORE]** Threads and processes (Memory sharing, IPC, CPU vs I/O bound workloads)
- [ ] **[CORE]** Concurrency vs. parallelism (Scheduling, context switching, multi-core)
- [ ] **[CORE]** Event loop vs. multi-threading (Single-threaded async I/O vs Thread pools)
- [ ] **[CORE]** Asynchronous programming & concurrency (Promises, async/await, coroutines)
- [ ] **[EDGE]** Race conditions & shared-state bugs (Lost updates, check-then-act, TOCTOU; occurs in app code, not only DBs)
- [ ] **[EDGE]** Memory management & leaks (Heap, stack, garbage collection mechanics)
- [-] **[MANAGED]** Low-level mutexes & semaphores (Abstracted in high-level runtimes; know when they're needed)

---

## 11. Additional Concepts & AI Bridges
- [ ] **[CORE]** Webhooks (Event-driven callbacks, signature verification, retry policies)
- [ ] **[CORE]** Serialization and deserialization (JSON, Buffers, Protobuf, binary framing)
- [ ] **[CORE]** File uploads and object storage (S3/Blob storage, presigned URLs, multipart)
- [ ] **[CORE]** Server-Sent Events (SSE) & Streaming (Crucial for streaming LLM responses)
- [ ] **[CORE]** Graceful shutdown (Handling SIGTERM, draining sockets, closing DB connections)

---

## 12. Testing & Quality
- [ ] **[CORE]** Test pyramid (Unit, integration, end-to-end; what each catches)
- [ ] **[CORE]** Test doubles & mocking (Stubs, fakes, mocks; DI as the enabler)
- [ ] **[EDGE]** Contract testing (Consumer-driven contracts, API compatibility checks)
- [ ] **[EDGE]** Testing async & failure paths (Retries, timeouts, idempotency, poison messages)

---

# Phase 2: Artificial Intelligence Engineering

*Start after Sections 1–12. Each AI topic is taught with the same three-tier format and the same stack-agnostic rule.*

## 13. LLM Fundamentals & Prompting
- [ ] **[CORE]** How LLMs work (Tokens, tokenization, context window, next-token prediction, transformer at a high level)
- [ ] **[CORE]** LLM API mechanics (Chat roles, messages, streaming, token usage, pricing, rate limits, error handling)
- [ ] **[CORE]** Sampling parameters (Temperature, top-p, max tokens, stop sequences; why outputs aren't deterministic)
- [ ] **[CORE]** Prompt engineering protocols (System vs user prompts, few-shot, chain-of-thought, prompt templates)
- [ ] **[CORE]** Structured outputs (JSON schema–constrained generation, validation, repair/retry)
- [ ] **[CORE]** Hallucinations & grounding (Why they occur, mitigation via context, citations, verification)
- [ ] **[EDGE]** Model selection (Capability vs latency vs cost, small vs large, open vs hosted, context length)
- [ ] **[EDGE]** Multimodal inputs (Images, PDFs, audio; document understanding)
- [ ] **[EDGE]** Prompting vs RAG vs fine-tuning (Decision framework, LoRA at a conceptual level)
- [-] **[MANAGED]** Pretraining, GPU serving, quantization (Provider/platform concern)

## 14. Embeddings, Vector Search & RAG
- [ ] **[CORE]** Vector embeddings (Semantic vectors, dimensions, similarity: cosine, dot product, L2)
- [ ] **[CORE]** Vector databases & ANN indexes (HNSW, IVF, recall vs speed; pgvector, HANA Vector Engine)
- [ ] **[CORE]** RAG ingestion pipeline (Parsing, chunking strategies, metadata, re-indexing)
- [ ] **[CORE]** RAG retrieval & generation (Top-k, reranking, context assembly, citations)
- [ ] **[CORE]** Access control in RAG (Document-level permissions, tenant isolation; ties to RBAC/ABAC and multi-tenancy)
- [ ] **[CORE]** RAG evaluation (Retrieval recall/precision, faithfulness, answer relevance)
- [ ] **[EDGE]** Hybrid search (BM25 + vector, metadata filtering, reciprocal rank fusion)
- [ ] **[EDGE]** Advanced RAG (Query rewriting, HyDE, multi-hop, GraphRAG, agentic RAG)
- [ ] **[EDGE]** Structured/enterprise data grounding (Text-to-SQL, semantic layers, querying business data safely)
- [-] **[MANAGED]** Vector index scaling and sharding (Operated by the managed DB service)

## 15. Tool Use, MCP & Agents
- [ ] **[CORE]** Function calling & tool calling (JSON-schema tool definitions, the tool-use loop, binding backend APIs to LLMs)
- [ ] **[CORE]** Model Context Protocol (MCP) (Servers, clients, tools/resources/prompts, standardized tool connections)
- [ ] **[CORE]** Agent loop (ReAct, plan-act-observe, stop conditions, step and cost limits)
- [ ] **[CORE]** Agent memory & context engineering (Short vs long-term memory, context window management, summarization, prompt caching)
- [ ] **[EDGE]** Multi-agent patterns (Orchestrator-worker, handoffs, swarms, when NOT to use multiple agents)
- [ ] **[EDGE]** Agent interoperability (A2A and agent-to-agent protocols, tool registries)
- [ ] **[EDGE]** Human-in-the-loop & durable execution (Approvals, long-running agents, resumable workflows; ties to Saga and queues)
- [ ] **[EDGE]** Agent frameworks (Orchestration frameworks vs hand-rolled loops, trade-offs)

## 16. LLM Production Engineering, Security & Governance
- [ ] **[CORE]** LLM evaluation (Golden datasets, LLM-as-judge, regression evals, offline vs online)
- [ ] **[CORE]** AI security (Prompt injection direct/indirect, data exfiltration, excessive agency, OWASP Top 10 for LLM apps)
- [ ] **[CORE]** Guardrails & output validation (Content filters, schema checks, PII redaction, allow-listed tools)
- [ ] **[CORE]** Reliability of LLM calls (Timeouts, retries, fallbacks across models/providers, idempotent tool calls)
- [ ] **[EDGE]** LLM observability (Tracing prompts and tool calls, token/cost/latency metrics, prompt versioning)
- [ ] **[EDGE]** Cost & latency optimization (Prompt/response caching, batching, model routing, streaming UX)
- [ ] **[EDGE]** Data privacy & compliance (Data residency, retention, no-training guarantees, GDPR, audit trails)
- [ ] **[EDGE]** AI governance in the enterprise (Approval flows, model/version control, responsible-AI policy, explainability)

---

## Appendix A: BTP Lens (Reference only, no checkboxes)

Where each concept lands on SAP BTP. Product names are from memory and change often; **verify against current SAP documentation** before relying on them. Used as a short "BTP Lens" footer after a topic when asked, never as the way a concept is taught.

| Concept area | BTP-side counterpart |
|---|---|
| AuthN/AuthZ, OAuth, OIDC, SAML, SSO | Identity Authentication (IAS), XSUAA, trust configuration |
| Identity propagation / outbound connectivity | Destination service, principal propagation, Cloud Connector (on-prem) |
| API gateway, routing, TLS termination | Application Router, API Management |
| Multi-tenancy | Subscription management / SaaS provisioning, tenant-aware services |
| Relational, vector persistence | SAP HANA Cloud (incl. Vector Engine), PostgreSQL on BTP |
| Caching | Redis on BTP |
| Events / queues | Event Mesh / Advanced Event Mesh (Integration Suite) |
| Runtimes (containers, 12-factor, K8s) | Cloud Foundry, Kyma |
| Secrets / credentials | Service bindings, Credential Store |
| Logging / monitoring | Cloud Logging and related observability services |
| CI/CD | Continuous Integration and Delivery service |
| LLM access, orchestration, ML ops | AI Core, AI Launchpad, Generative AI Hub |
| Enterprise copilots / agents | Joule, Joule Studio |
| LLM in app code (CAP) | CAP LLM plugin, SAP Build |
| Document extraction | Document Information Extraction |
| Business-data grounding | Business Data Cloud, Datasphere |

---

### Progress Tracker
- **Core Track:** 3 / 71 topics completed
- **Systems Edge Track:** 0 / 58 topics completed
- **Managed/Optional:** 10 topics (reference)
- **Active Topic:** 4. Input validation & error handling (next: 5. Middleware)
- *Counts verified with `grep` on the tier markers. Recount after any edit.*
