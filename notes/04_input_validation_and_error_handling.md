# Topic 4: Input Validation & Error Handling (Architectural Notes)

---

## 0. The Golden Law of Backend Engineering

> **"Never Trust the Client."**

Frontend form validation (JavaScript/HTML) is purely for user experience (UX). Anyone can bypass the frontend using `curl`, Postman, or automated scripts. **Input Validation on the backend is your application's primary defensive perimeter.**

---

## 1. Core Concepts: Syntactic vs. Semantic Validation

```text
[ INCOMING HTTP REQUEST ]
          |
          v
+--------------------------------------------------------------------------+
| 1. CONTROLLER / INGRESS BOUNDARY (Fail-Fast Layer)                      |
|    - Syntactic Validation (Schema Validation)                            |
|      * Is the body valid JSON?                                           |
|      * Are required fields present?                                      |
|      * Are data types correct (e.g. integer vs string)?                  |
|    - If invalid ===> REJECT IMMEDIATELY with HTTP 400 / 422              |
+--------------------------------------------------------------------------+
          | (Passed)
          v
+--------------------------------------------------------------------------+
| 2. SERVICE / BUSINESS LOGIC LAYER                                        |
|    - Semantic Validation (Domain Rules)                                  |
|      * Does this account have enough balance?                            |
|      * Is this discount code valid for this product?                     |
|    - If invalid ===> REJECT with HTTP 409 Conflict / 422 Unprocessable   |
+--------------------------------------------------------------------------+
          | (Passed)
          v
+--------------------------------------------------------------------------+
| 3. DATABASE LAYER                                                        |
|    - Persist clean, trusted data into tables.                            |
+--------------------------------------------------------------------------+
```

| Type | What it checks | Where it executes | HTTP Status Code | Example |
| :--- | :--- | :--- | :--- | :--- |
| **Syntactic Validation** | Format, structure, and data types. | **Controller / Ingress** (Entry gate) | `400 Bad Request` or `422 Unprocessable` | "Email missing '@' sign"; "Quantity is negative". |
| **Semantic Validation** | Meaning, context, and business rules. | **Service Layer** (Domain logic) | `409 Conflict` or `422 Unprocessable` | "Email already registered in database"; "Insufficient funds". |

---

## 2. Declarative Schema Validation

Instead of writing dozens of manual `if-else` blocks, modern backends declare **Validation Schemas** (JSON Schema, Zod, Pydantic, Bean Validation).

* **Fail-Fast Principle:** Check the schema at the immediate boundary. If validation fails, reject the request immediately before executing business logic, allocating memory, or opening database connections.

---

## 3. Standard Error Responses (RFC 9457 "Problem Details")

Professional APIs use standardized JSON envelopes paired with accurate 4xx/5xx status codes:

```http
HTTP/1.1 422 Unprocessable Content
Content-Type: application/problem+json

{
  "type": "https://api.example.com/errors/validation-failed",
  "title": "Validation Failed",
  "status": 422,
  "detail": "The payload contained 2 invalid fields.",
  "instance": "/orders/req_9921",
  "invalid_params": [
    {
      "name": "quantity",
      "reason": "Must be greater than 0, received -5"
    },
    {
      "name": "countryCode",
      "reason": "Must be a 2-letter ISO code, received 'UNITED_STATES'"
    }
  ]
}
```

---

## 4. Security: Preventing Information Leakage (OWASP)

* **Never return raw database or stack trace errors to the client:** Revealing database connection strings, table names, or server file paths exposes critical vulnerabilities to attackers.
* **The Dual-Logging Rule:**
  1. **To the Client:** Return a sanitized, high-level message with an `error_id`:  
     `{"status": 500, "title": "Internal Server Error", "error_id": "err_9981"}`.
  2. **To the Server Logs:** Log the complete internal stack trace tagged with `err_9981` for developer debugging.

---

## 5. BTP Lens (Optional Reference)

* In **SAP CAP**, validation rules are declared directly in CDS models:
  ```cds
  entity Orders {
    key ID       : UUID;
        quantity : Integer @assert.range: [1, 100];
        email    : String  @mandatory;
  }
  ```
* CAP automatically enforces these constraints before persisting to SAP HANA, formatting errors for the **SAPUI5 Message Manager** to highlight invalid inputs on Fiori screens.