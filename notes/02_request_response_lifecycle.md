# Topic 2: Request–Response Lifecycle (Architectural Notes)

---

## 0. Overview & Scope

The **Request–Response Lifecycle** is the end-to-end journey data takes from when a client (web browser, mobile app, or external service) initiates an action, through the network, into the backend application pipeline, and back.

For backend and systems architecture, we focus on the **Application and Systems Architecture layers (L4–L7)**—where engineers make design decisions, optimize latency, prevent security breaches, and diagnose production failures.

```text
[ CLIENT ]                                             [ BACKEND SYSTEM ]
  Browser / App / Client Service                         API Gateway / Server / DB
     |                                                              |
     |--- 1. DNS: "Where is api.example.com?" --------------------->| (Resolves to IP)
     |                                                              |
     |--- 2. CONNECT: Open / Reuse TCP+TLS Connection ------------->| (Ready to talk)
     |                                                              |
     |--- 3. SEND: Serialized HTTP Request (e.g. JSON) ------------>|
     |                                                              |
     |                     +----------------------------------------+
     |                     | 4. APPLICATION PIPELINE:
     |                     |    a. Ingress / Gateway (Validates TLS, routes path)
     |                     |    b. Middleware        (Auth token check, logging)
     |                     |    c. Controller        (Validates & parses payload)
     |                     |    d. Service / Logic   (Business rules)
     |                     |    e. Database          (Queries / writes data)
     |                     +----------------------------------------+
     |                                                              |
     |<-- 5. RETURN: HTTP Response (Status Code + JSON Body) -------|
```

---

## 1. The 5 Core Phases (From First Principles)

### Phase 1: DNS Resolution (Finding the Machine)
* **What it does:** Translates human-readable domain names (`api.example.com`) into routable IP addresses (`93.184.216.34`).
* **Lookup Flow:** Browser/App Cache $\rightarrow$ OS Cache $\rightarrow$ Router Cache $\rightarrow$ Recursive ISP Resolver $\rightarrow$ Authoritative DNS Server.
* **Architectural Impact:**
  * **TTL (Time to Live):** Dictates how long clients cache DNS records. Short TTLs (e.g. 60s) allow rapid IP failover during outages/migrations; long TTLs (e.g. 1 day) reduce DNS lookup latency.

### Phase 2: Connection & Pooling (Sockets & Handshakes)
* **What it does:** Establishes a virtual communication endpoint (a **Socket**, defined by `Client IP:Port` and `Server IP:Port`) using the TCP 3-way handshake (`SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`), followed by the TLS 1.3 cryptographic handshake.
* **Architectural Impact:**
  * **Connection Latency:** Opening a brand-new connection takes 1 to 2 network round-trips (50–200ms depending on distance).
  * **Connection Pooling (`keep-alive`):** High-throughput backends keep established sockets open to reuse them across multiple requests, eliminating repeated handshake delays.

### Phase 3: Serialization (Memory $\leftrightarrow$ Wire Format)
* **What it does:** 
  * **Serialization (Marshalling):** Converts live, in-memory program objects (in RAM) into a flat, linear stream of bytes (typically UTF-8 JSON text or binary Protobuf) suitable for transmission over network wires.
  * **Deserialization (Unmarshalling):** Reconstructs incoming bytes back into active program objects at the receiver.
* **Architectural Impact:**
  * Payload size directly impacts network transfer time and CPU serialization overhead. Heavy APIs optimize using compression (`gzip`/`brotli`) or binary serialization (Protobuf/gRPC).

### Phase 4: Inside the Application Pipeline
When bytes reach the server, they pass through a structured architectural chain:
1. **Ingress / API Gateway (e.g., NGINX, Envoy):** Terminates TLS, validates domain routing, load balances across application instances.
2. **Middleware Chain:** Executes cross-cutting concerns before business logic (e.g., validating JWT auth tokens, rate limiting, distributed tracing headers).
3. **Controller / Handler:** Matches the HTTP method and path (`POST /orders`), extracts query parameters, and deserializes the request body.
4. **Service Layer (Domain Logic):** Applies business rules (e.g., checks inventory, computes tax, calculates discounts).
5. **Database / Persistence:** Communicates with the database over a managed connection pool to read/write records.

### Phase 5: Response & Error Boundaries
The server packages the outcome into an HTTP Response with appropriate **Status Codes**:
* **2xx:** Success (e.g., `201 Created` with resource ID).
* **4xx (Client Boundary):** Request failed validation, authentication, or resource not found.
* **5xx (Server Boundary):** Unhandled code exception (`500`) or upstream timeout (`504`).

---

## 2. Real-World Analogy

### Ordering at a Restaurant
* **DNS:** Looking up the restaurant's phone number in a directory.
* **Connection & Socket:** Dialing the phone, waiting for the line to connect, and hearing "Hello!".
* **Serialization:** Translating your internal craving for food into spoken words over the microphone.
* **Application Pipeline:**
  * *Hostess (Gateway):* Greets and directs you to a table.
  * *Waiter (Middleware & Controller):* Checks your age/ID, takes your order, and writes the ticket.
  * *Chef (Service Layer):* Prepares the meal according to recipe rules.
  * *Pantry (Database):* Where ingredients are stored and retrieved.
* **Response & Deserialization:** The dish is served to your table; you consume and enjoy it.

---

## 3. Key Takeaways for Backend & AI Engineers

1. **Keep-Alive & Connection Pools are Mandatory:** Never open a raw TCP/TLS connection for every single HTTP request in microservice or LLM API calls; reuse pooled sockets.
2. **Respect the Layers:** Keep Controllers thin (parsing/validation only), put business logic in Services, and isolate persistence in Repositories.
3. **Know Your Error Boundaries:** Fast diagnosis of whether an error originated at the Gateway (502/504), Application (500), or Client (400/401/403/422).

---

## 4. BTP Lens (Optional Reference)

* **Entry Point:** Public traffic hits Cloudflare / SAP Global Load Balancers, routing to the **Application Router** or Cloud Foundry **GoRouter**.
* **Internal Relay:** The router acts as the reverse proxy, checking session/JWT tokens with **XSUAA / IAS**, then forwarding the request over private software networks to your application runtime (e.g., Cloud Foundry or Kyma/K8s).
* **Persistence:** Application services (CAP Node.js/Java) query **SAP HANA Cloud** via a pooled database client (`hdb` / JDBC).