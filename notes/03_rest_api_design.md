# Topic 3: REST API Design (Architectural Notes)

---

## 0. Overview & Scope

**REST (Representational State Transfer)** is an architectural style designed by Roy Fielding (RFC 7231 / RFC 9110) that standardizes how web and backend systems structure endpoints, exchange resources, and handle state changes using the native features of HTTP.

---

## 1. Core Principles of REST

```text
+--------------------------------------------------------------------------+
|                     REST ARCHITECTURE CORE PRINCIPLES                    |
|                                                                          |
|  1. NOUNS, NOT VERBS:  Use plural nouns for URLs (/orders, not /getOrder)|
|  2. VERBS ARE ACTIONS: Let HTTP methods dictate CRUD operations          |
|  3. NESTED HIERARCHY:  Represent parent-child relationships cleanly      |
|  4. QUERY PARAMETERS:  Use ? for filtering, sorting, and pagination      |
|  5. PREDICTABLE ERRORS:Standard error envelopes (RFC 9457)               |
+--------------------------------------------------------------------------+
```

### 1.1 Resources as Nouns, Not Verbs
* **Rule:** URLs must point to **resources (things)**, never actions.
* **Conventions:** Always use plural nouns for collections (`/products`, `/orders`, `/users`).
* **Identifiers:** Append the unique ID to target an individual item (`/products/42`).

```text
Anti-Pattern (Avoid):                   RESTful Standard:
---------------------------------       ---------------------------------
POST /createUser                        POST   /users
GET  /fetchOrder?id=99                  GET    /orders/99
POST /updateUserAddress                 PATCH  /users/42
POST /deleteItem                        DELETE /items/15
```

### 1.2 Resource Hierarchies (Sub-resources)
* Use nested routes to express parent-child ownership:
  * `GET /users/42/orders`: Fetch all orders belonging to user 42.
  * `POST /users/42/orders`: Create a new order for user 42.
* **The 2-Level Limit Rule:** Avoid nesting deeper than two levels (e.g., avoid `/companies/1/depts/2/teams/3/members/4`). If a resource requires deep targeting, promote it to a top-level route with a filter (`/members/4` or `/members?team_id=3`).

---

## 2. HTTP Verbs & CRUD Mapping

| HTTP Verb | CRUD Action | Target Example | Success Code | Idempotent? |
| :--- | :--- | :--- | :--- | :---: |
| **`GET`** | **R**ead | `/orders/5` | `200 OK` | **YES** |
| **`POST`** | **C**reate | `/orders` | `201 Created` | **NO** |
| **`PUT`** | **U**pdate (Full Replace)| `/orders/5` | `200 OK` | **YES** |
| **`PATCH`**| **U**pdate (Partial / Delta) | `/orders/5` | `200 OK` | **NO\*** |
| **`DELETE`**| **D**elete | `/orders/5` | `204 No Content` | **YES** |

---

## 3. Deep Dive: Why is PATCH Marked as Idempotent "NO*"?

An operation is **idempotent** if executing it 1 time leaves the system in the exact same state as executing it 100 times consecutively ($f(f(x)) = f(x)$).

Under **RFC 5789** (the RFC that introduced `PATCH`), PATCH is officially classified as **non-idempotent by default**, although it *can* be written to behave idempotently depending on what instruction is inside the payload:

### Case 1: When PATCH is Idempotent (Setting Absolute Values)
```http
PATCH /users/42 HTTP/1.1
Content-Type: application/json

{"email": "kunal@example.com"}
```
* Execute 1 time: Email becomes `kunal@example.com`.
* Execute 10 times: Email remains `kunal@example.com`. 
* *Result: Idempotent.*

### Case 2: When PATCH is NOT Idempotent (Mutations & Delta Operations)
Because PATCH is designed to apply partial "patches" or instruction sets, developers often use it for incremental changes or array additions:

1. **Incrementing Counters:**
   ```http
   PATCH /accounts/42/balance HTTP/1.1
   {"increment_by": 50}
   ```
   * Execute 1 time: Balance increases by 50.
   * Execute 3 times: Balance increases by 150! The server state changed on every call.

2. **Appending to Collections / Lists (JSON Patch RFC 6902):**
   ```json
   [
     { "op": "add", "path": "/tags/-", "value": "vip_customer" }
   ]
   ```
   * Execute 1 time: Tag `"vip_customer"` is appended once.
   * Execute 2 times: Tag `"vip_customer"` is appended a second time: `["vip_customer", "vip_customer"]`.

**Conclusion:** Because the HTTP standard allows PATCH payloads to express relative changes or append actions, the protocol **cannot guarantee** idempotency. Hence: **`NO*`**.

---

## 4. Filtering, Sorting, and Pagination (Query Parameters)

Query parameters (after the `?`) modify how resources are queried without altering endpoint structure:

* **Filtering:** Narrow results (`GET /orders?status=shipped&country=DE`).
* **Sorting:** Order results (`GET /orders?sort=-created_at` — minus prefix indicates descending).
* **Pagination:** Control response volume to prevent memory spikes:
  * *Offset-based:* `GET /orders?limit=25&offset=50` (Page 3).
  * *Cursor-based (High Scale):* `GET /orders?limit=25&starting_after=ord_8891`.

---

## 5. Standard Error Handling (RFC 9457)

Professional APIs never return `200 OK` with an error message in the body. They use RFC 9457 **Problem Details for HTTP APIs** with appropriate 4xx/5xx status codes:

```http
HTTP/1.1 422 Unprocessable Content
Content-Type: application/problem+json

{
  "type": "https://api.example.com/errors/insufficient-funds",
  "title": "Insufficient Account Balance",
  "status": 422,
  "detail": "Account 1092 requires $150.00 for this transfer, but has $22.50.",
  "instance": "/transfers/tx_991823"
}
```

---

## 6. BTP Lens (Optional Reference)

* In SAP BTP, enterprise REST APIs are predominantly exposed as **OData v4** services.
* OData standardizes REST query semantics into predefined keywords:
  * `$filter`: `/Orders?$filter=Status eq 'Open'`
  * `$orderby`: `/Orders?$orderby=CreatedAt desc`
  * `$select`: `/Orders?$select=ID,GrossAmount`
  * `$expand`: `/Orders?$expand=Items` (joins child sub-resources).
* SAP CAP (Cloud Application Programming Model) automatically generates compliant REST/OData handlers from CDS entity definitions.