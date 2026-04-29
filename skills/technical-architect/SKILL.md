---
name: technical-architect
description: Generate comprehensive but concise technical architecture documents as markdown files. Use this skill whenever the user asks for a technical architecture document, system design doc, architecture overview, or any structured technical documentation describing how a system is built. Also trigger when the user says things like "document my architecture", "write an architecture doc for my project", "create a system design document", or "generate technical docs for my app". The output is always a markdown file (architecture.md). This skill is framework-agnostic and works for any backend, frontend, or full-stack project.
---

# Technical Architecture Document Skill

Produces a single `architecture.md` file.
Output is always framework-agnostic markdown. Code examples in the doc use the project's actual language/framework.

---

## Step 1 — Gather context

Before writing anything, collect the following from the user or infer from the conversation:

| Item | Ask if missing |
|------|---------------|
| Project name and one-line description | Always ask |
| Primary language and framework | Always ask |
| Main modules / domains | Ask if not obvious |
| Data stores (DB, cache, queue, etc.) | Ask if not obvious |
| External integrations (payment, email, auth provider, etc.) | Ask if not obvious |
| Team size and target audience of the doc | Optional, use defaults if absent |
| Any sections to skip | Ask — do not assume |
| Coding conventions (error handling, testing rules) | Ask — these make the doc most useful for a coding agent |

Do not invent specifics. If something is unknown, write a placeholder: `[TO BE DEFINED]`.

---

## Step 2 — Document structure

Produce the sections below **in order**, skipping any the user has explicitly excluded.
Use prose paragraphs inside sections — avoid bullet-point-heavy writing except where a list is the natural format (e.g., requirement lists, rule lists).

See `references/section-guide.md` for detailed instructions on each section.

### Sections

1. **Executive Summary** — one paragraph, non-technical, what/why/key choices
2. **Goals & Constraints** — functional requirements, non-functional requirements, hard constraints
3. **System Overview** — one Mermaid `flowchart TD` diagram showing major components and data flow
4. **Component Breakdown** — one subsection per major component; responsibility + interface, no internal implementation detail
5. **Domain Model** — core business entities, their relationships, and key invariants; one Mermaid `erDiagram`, no storage details
6. **Data Architecture** — core entities (plain text schema), storage technology choices with rationale, data flow narrative
7. **Key Architectural Decisions** — table of significant technology choices with rationale summaries
8. **Security & Compliance** *(skip if user says not relevant)* — auth/authz model, secrets, transport, compliance notes
9. **Observability** *(skip if user says not relevant)* — logging, metrics, tracing, alerting; state *what* is monitored, not just that it is
10. **Error Handling Conventions** *(include if user has a convention; ask if unsure)* — the pattern services use, with short code examples in the project's language
11. **Testing Expectations** *(include if user has a convention; ask if unsure)* — what gets unit tested, what does not, setup pattern with short example
12. **Appendix: Project Structure** — directory tree of `src/` with inline comments

---

## Step 3 — Mermaid diagram rules

- Always use `flowchart TD`
- Group related nodes in `subgraph` blocks (e.g., Clients, Application, Data)
- Use cylinder syntax `[(Name)]` for databases and caches
- Label edges with the protocol or data type where meaningful: `-->|HTTPS|`
- Keep it high-level — 6–12 nodes maximum; no method names or internal functions

Example skeleton:
```
flowchart TD
    subgraph Clients
        WEB["Web App"]
        MOB["Mobile App"]
    end

    subgraph YourApp ["Your Application"]
        MW["Middleware\nAuth · Logging"]
        MOD_A["Module A]
        MOD_B["Module B"]
    end

    DB[(Primary DB)]
    CACHE[(Cache)]
    EXT[External Service]

    WEB & MOB -->|HTTPS| MW
    MW --> MOD_A & MOD_B
    MOD_A & MOD_B --> DB
    MOD_B --> CACHE
    MOD_A --> EXT
```

---

## Step 4 — Output files

Always produce this file:

```
docs/technical-architecture.md          ← main document
```

Present the file with `present_files` at the end so the user can download it.

---

## Style rules

- **Decisions over descriptions.** Always explain *why* a technology was chosen, not just *what* it is.
- **No implementation detail** in the architecture doc. Code belongs in code; the doc describes shape, not internals.
- **Diagrams over prose** for system topology. One good Mermaid diagram replaces three paragraphs.
- **Placeholder over invention.** If a detail is unknown, write `[TO BE DEFINED]` rather than guessing.
- **Living document footer.** End `technical-architecture.md` with: *"This document should be reviewed and updated on any major architectural change."*
- **Prose in sections, not bullets.** Write paragraphs inside sections. Use bullet lists only for requirement lists and rule lists.
- **No framework-specific jargon** in section headings — headings like "Component Breakdown" and "Data Architecture" apply to any stack.
- Code examples in Error Handling and Testing sections should use the project's **actual** language and framework.

---

## Reference files

- `references/section-guide.md` — detailed writing guidance for each section
