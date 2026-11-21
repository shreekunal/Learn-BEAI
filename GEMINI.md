# Workspace Guidelines & Rules

## Git Command Policy
- All git operations and version control management must be performed exclusively by the user.
- Do NOT run git commands in background or synchronous tool execution unless explicitly commanded by the user for that specific invocation.

## Terminal & Diagram Formatting Policy
- Terminal width is strictly constrained (max 137 columns; design for < 100 columns).
- DO NOT use Mermaid diagrams (````mermaid````) if they risk exceeding width; Mermaid CLI renderers easily generate >137 column layouts even for simple graphs.
- Use clean ASCII / Unicode text-art diagrams (in ````text```` blocks) kept strictly under 80-100 columns so all visuals render reliably inline without warnings or clipping.

## Universal & Stack-Agnostic Instruction Policy
- All backend engineering concepts must be taught from pure computer science and systems architecture first principles.
- Explanations must be stack- and language-agnostic (applicable across Go, Rust, Java, Node.js, Python, C#, etc.).
- Never bias or anchor explanations to SAP-specific tools, frameworks, or cloud products (e.g., CAP, Approuter, XSUAA, BTP) unless specifically and explicitly requested by the user.

## Zero-Prerequisite / 0-to-100 Foundation Rule
- Teach from absolute zero. Assume no prior backend or networking prerequisites.
- Never drop an acronym, technical buzzword, or abstract term (e.g., stateless, idempotent, TCP, TLS, RFC, DNS, socket, packet, byte, port, serialization, cipher) without immediately explaining what it means in plain English, why it exists, and the physical or architectural problem it solves.
- Build concepts strictly step-by-step: fundamental problem -> physical/logical mechanism -> formal specification -> real-world system behavior.
