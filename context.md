# Learn BeAi — Learning Context & Tracker

## 1. Learner Profile & Overview
- **Learner:** Kunal
- **Role:** Associate SAP BTP Consultant
- **Project Goal:** Master Backend Architecture & Engineering from First Principles ("Learn BeAi: Backend + AI"). Track: **Option A — Streamlined for Enterprise SAP BTP & AI Architecture**. Complete core backend engineering first, followed by Artificial Intelligence engineering.

## 2. Learning Ground Rules
1. **Persistent Context:** Maintain `context.md` at the project root to preserve learning state, active topic, discussion notes, and roadmap across chats.
2. **Three-Tier Explanation Format (intuition first, depth last):**
   - **1. Easy Definition:** Plain English, zero fluff, intuitive explanation of what it does and why it exists.
   - **2. Real-World Analogy:** Relatable everyday analogy with clear mapping to backend components.
   - **3. Technical:** Universal, stack- and language-agnostic backend engineering. Architecture, protocol/networking mechanics (OSI layers, TCP/IP, RFC specs), data flow, serialization, security, and systems design.
3. **Interactive Discussion Loop:**
   - Deep dive into one topic at a time.
   - Kunal asks counter-questions, tests edge cases, and clarifies doubts.
   - Only when satisfied, the topic is marked complete in `backend_concepts_checklist.md` and recorded in `context.md`.
4. **Terminal-Friendly Diagrams:** Terminal width is strictly constrained (137 cols max). Avoid Mermaid code blocks (which often exceed 137 cols in the CLI renderer); use clean ASCII / text box diagrams (`text` blocks) kept strictly under 80-100 columns so they render cleanly inline.
5. **Stack & Language Agnostic, with an Optional BTP Lens:** Teach all concepts as universal backend/AI principles (applicable to Go, Rust, Java, Node.js, Python, C#, etc.). The three tiers above never anchor to SAP/BTP-specific frameworks (CAP, Approuter, XSUAA). Because Kunal is an SAP BTP consultant, a short optional **"BTP Lens"** footer (2–4 lines, mapped from Appendix A of the checklist) may follow a topic once the three tiers are done. Product names change often, so verify against current SAP docs.
6. **One Topic = One Checklist Item:** Each numbered topic maps to exactly one line in the checklist, so completing a topic ticks exactly one box. A lesson may touch a neighbouring topic, but only the active topic gets marked.
7. **Counts Stay Honest:** After any checklist edit, recount tier markers with `grep` and update the Progress Tracker here and in the checklist. Dependent topics must not be tagged lower than their prerequisites (see the Study-Order Note in the checklist).

---

## 3. Current Learning State
- **Active Track:** Option A — Streamlined for Enterprise SAP BTP & AI Architecture
- **Active Section:** 1. Backend Fundamentals
- **Active Topic:** 1. HTTP and HTTPS
- **Progress:** 0 / 71 Core Topics Completed (0 / 129 Core + Edge Topics; 10 Managed are reference-only)
- **Scope:** Phase 1 = Backend (Sections 1–12). Phase 2 = AI Engineering (Sections 13–16). Checklist reviewed and expanded on 2026-11-20.

---

## 4. Topic Completion Log & Notes Archive
*(Topics, resolved counter-questions, and core takeaways are recorded here once satisfied)*

| # | Section | Topic | Status | Completed Date | Key Takeaway / Resolution |
|---|---------|-------|--------|----------------|---------------------------|
| 1 | 1. Backend Fundamentals | HTTP and HTTPS | 🔄 In Progress | - | Lesson delivered (also previewed the full request lifecycle). Awaiting counter-questions. |
| 2 | 1. Backend Fundamentals | Request–response lifecycle | ⏳ Queued | - | Overview already seen in topic 1; deep dive on DNS, TCP, sockets, server-side processing still to do. |

---

## 5. Upcoming Topics Queue (Section 1: Backend Fundamentals)
*(Mirrors Section 1 of the checklist, line for line.)*
1. HTTP and HTTPS (Active)
2. Request–response lifecycle
3. REST API design
4. Input validation & error handling
5. Middleware
6. Authentication and authorization
7. Sessions, cookies, and JWT
8. OAuth 2.0
9. OpenID Connect
10. Rate limiting
11. SAML & SSO (Systems Edge)
12. API versioning & contracts (Systems Edge)
13. gRPC & GraphQL (Systems Edge)
14. WebSockets (Systems Edge)
