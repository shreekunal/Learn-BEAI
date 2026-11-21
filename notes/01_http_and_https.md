# Topic 1: HTTP, HTTPS & QUIC (0 to 100 Notes)

---

## 0. Fundamental Physical Concepts

Before understanding web protocols, we must understand how computers communicate across physical wires and radio waves:

* **Client:** Any program or device asking for data or an action (e.g., your browser, mobile app, or a backend script).
* **Server:** A computer running 24/7 in a data center listening for incoming requests and serving answers.
* **IP Address (Internet Protocol Address):** Like the **street address of a building** (e.g., `142.250.190.46`). It directs data to the correct physical machine.
* **Port:** Like the **apartment or office number** inside that building (a number between `0` and `65535`). Tells the operating system which program gets the message.
  * **Port 80:** Default door for unencrypted HTTP.
  * **Port 443:** Default door for encrypted HTTPS.
* **Protocol:** An agreed-upon set of rules, formats, and grammar that two computers follow so they can understand each other.
* **RFC (Request for Comments):** Official technical specification documents created by global internet engineers (IETF). These define the standards of the web.
* **Packet:** Networks cannot transmit large files in one piece. Data is sliced into small envelopes called packets (~1,500 bytes each) that travel independently across cables and routers.

```text
+---------------------+                      +---------------------+
|   Client Machine    |                      |   Server Machine    |
| (Your laptop/phone) |                      |  (Data center host) |
|                     |                      |                     |
|  [ Browser / App ]  |                      | [ Web Server App ]  |
|          |          |                      |          ^          |
+----------|----------+                      +----------|----------+
           |                                            |
           | ===> Connects to IP (Building) ===========>|
           | ===> Reaches Port 80 or 443 (Door) =======>|
           |                                            |
           +====== Speaks agreed HTTP Protocol =========+
```

---

## 1. What is HTTP?

**HTTP** stands for **Hypertext Transfer Protocol**:
* **Hypertext:** Any digital data (text, JSON, images, audio, video).
* **Transfer:** Moving data across a network between computers.
* **Protocol:** The rulebook governing the conversation.

### Core Architectural Characteristic: Statelessness
* **State** means memory of past interactions.
* **Statelessness** means the server has **complete amnesia**. It treats every single request as completely independent.
* Request #2 knows nothing about Request #1 unless the client explicitly sends context (like an auth token or session ID) inside Request #2.

---

## 2. Anatomy of an HTTP Message (The Wire Format)

HTTP/1.1 is human-readable text ending with carriage return/line feed (`\r\n`).

### The HTTP Request
```text
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Content-Length: 33

{"name": "Kunal", "role": "Dev"}
```
1. **Start Line:** Method (`POST`), Path (`/users`), Protocol Version (`HTTP/1.1`).
2. **Headers:** Key-value metadata (`Host`, `Content-Type`, `Content-Length`).
3. **Empty Line (`\r\n`):** Universal boundary marking the end of headers.
4. **Body:** The data payload.

### The HTTP Response
```text
HTTP/1.1 201 Created
Content-Type: application/json
Content-Length: 43

{"id": 101, "name": "Kunal", "status": "active"}
```
1. **Status Line:** Protocol Version (`HTTP/1.1`), Status Code (`201`), Reason Phrase (`Created`).
2. **Headers:** Metadata regarding payload, caching, and server identity.
3. **Empty Line:** End of headers.
4. **Body:** Response data.

---

## 3. HTTP Methods & Semantic Rules

| Method | Safe? | Idempotent? | Description & Behavior |
| :--- | :---: | :---: | :--- |
| **GET** | **YES** | **YES** | Read-only. Fetches a resource. Has no side effects on server state. |
| **POST** | **NO** | **NO** | Submits data to create new records or run actions. Executing $N$ times creates $N$ records. |
| **PUT** | **NO** | **YES** | Completely replaces/overwrites the resource at target URI. Executing 1 or 100 times leaves the resource in the exact same state. |
| **PATCH** | **NO** | **NO\*** | Partial update. (*Can be designed idempotent, but not mandated by RFC). |
| **DELETE**| **NO** | **YES** | Removes the resource. Deleting once removes it; repeating leaves it absent. |

* **Safe:** Read-only operation. Does not modify server state.
* **Idempotent:** Calling the operation once has the exact same side-effect on server state as calling it multiple times ($f(f(x)) = f(x)$).

---

## 4. HTTP Status Codes

Status codes are 3-digit numbers grouped by class:

* **1xx (Informational):** Request received, continuing process (e.g., `101 Switching Protocols`).
* **2xx (Success):** Request received, understood, and accepted.
  * `200 OK`: Standard success.
  * `201 Created`: Resource successfully created.
  * `204 No Content`: Action succeeded; no body returned.
* **3xx (Redirection):** Further action required to complete request.
  * `301 Moved Permanently`: Permanent redirect; update bookmarks.
  * `304 Not Modified`: Cached copy is fresh; reuse local data.
  * `307 / 308`: Temporary / Permanent redirect preserving the HTTP method.
* **4xx (Client Error):** The caller made an error.
  * `400 Bad Request`: Malformed syntax or invalid payload.
  * `401 Unauthorized`: Missing or invalid authentication credentials.
  * `403 Forbidden`: Authenticated, but user lacks permission.
  * `404 Not Found`: Resource URI does not exist.
  * `409 Conflict`: Request conflicts with current server state (e.g. duplicate email).
  * `429 Too Many Requests`: Client exceeded rate limits.
* **5xx (Server Error):** The server failed to fulfill a valid request.
  * `500 Internal Server Error`: Unhandled application crash.
  * `502 Bad Gateway`: Reverse proxy received an invalid response from upstream app.
  * `503 Service Unavailable`: Server is overloaded or down for maintenance.
  * `504 Gateway Timeout`: Upstream service took too long to answer proxy.

---

## 5. HTTPS & TLS Mechanics

HTTPS wraps HTTP inside **TLS (Transport Layer Security)**, ensuring the **CIA Triad**:
1. **Confidentiality:** Eavesdroppers cannot read data.
2. **Integrity:** Tampering is instantly detected.
3. **Authenticity:** Server identity is proven via verified certificates.

### Encryption Fundamentals:
* **Symmetric Encryption:** One shared key locks and unlocks data. Very fast, but key must be secretly shared.
* **Asymmetric Encryption:** A public key locks data; only the matching secret private key can unlock it. Used during setup.

### The TLS Handshake (Best of Both Worlds):
1. **Asymmetric phase:** Client and server use asymmetric math (Diffie-Hellman Key Exchange) to safely agree on a temporary shared symmetric key without transmitting the key over the wire.
2. **Certificate validation:** Client verifies the server's X.509 Certificate against trusted **Certificate Authorities (CAs)**.
3. **Symmetric phase:** All subsequent HTTP traffic is encrypted using high-speed symmetric encryption (e.g., AES-GCM).

---

## 6. HTTP Evolution: HTTP/1.1 vs HTTP/2 vs HTTP/3 (QUIC & UDP)

```text
+-------------------+---------------------+-----------------------------------+
| Version           | Core Feature        | Bottleneck / Limitation           |
+-------------------+---------------------+-----------------------------------+
| HTTP/1.1 (1997)   | Plain text messages | Application Head-of-Line Blocking |
|                   |                     | (Single file line per connection) |
|                   |                     |                                   |
| HTTP/2 (2015)     | Multiplexed frames  | TCP Transport Head-of-Line        |
|                   | over single TCP     | Blocking (1 lost packet stalls    |
|                   | connection          | all streams on the connection)    |
|                   |                     |                                   |
| HTTP/3 (2020+)    | QUIC over UDP       | None (Streams are independent;    |
|                   |                     | built-in encryption; 0-RTT)       |
+-------------------+---------------------+-----------------------------------+
```

### The Deep Dive on QUIC & UDP:
* **TCP vs UDP:** TCP guarantees in-order delivery by stalling the whole pipe if one packet drops. UDP is lightweight and fires packets without stalling.
* **Why QUIC was built on UDP:** Middleboxes (routers/firewalls) block new protocols due to internet ossification. UDP passes through everywhere.
* **True Stream Independence:** In HTTP/3, if packet drops on Stream 2 (Image), Stream 1 (CSS) and Stream 3 (JSON) continue flowing with zero delay.
* **Fast Handshakes:** Fuses transport and TLS 1.3 into 1 round trip (or 0-RTT on reconnect).
* **Connection Migration:** QUIC identifies connections using a 64-bit **Connection ID (CID)** rather than `IP:Port`. When switching from Wi-Fi to 5G, the connection survives without dropping or reconnecting.

---

## 7. BTP Lens (Optional Reference)

* **TLS Termination:** Cloud Foundry GoRouter / Approuter terminates incoming Port 443 HTTPS traffic and validates custom domain certificates (managed via Custom Domain Service).
* **Internal Routing:** Ingress proxies forward requests to backend containers (CAP Node.js / Java) across secure internal software networks.