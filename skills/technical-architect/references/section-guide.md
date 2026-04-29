# Section Writing Guide

Detailed instructions for each section of the architecture document.

---

## 1. Executive Summary

One paragraph, 4–6 sentences. Must answer:
- What does the system do?
- Why does it exist (the problem it solves)?
- What are the 1–2 most important architectural choices?
- Who is the target audience of this document?

Do not mention specific technologies in the first sentence. Lead with the purpose.

**Good:** "A scalable REST API powering an e-commerce platform. The system handles product catalog, authentication, and order management, designed for horizontal scalability with clear domain separation."

**Bad:** "This is a NestJS application using TypeORM and Redis that does e-commerce stuff."

---

## 2. Goals & Constraints

Three subsections, always in this order:

**Functional Requirements** — bullet list of what the system must do, written as user-observable behaviours. Not implementation tasks.

**Non-Functional Requirements** — bullet list with concrete numbers where possible.
- Availability: e.g. 99.9% uptime SLA
- Latency: e.g. p95 < 200ms reads, p95 < 500ms writes
- Throughput: e.g. 1,000 RPS sustained
- Security/compliance standards if applicable

**Constraints** — bullet list of fixed decisions the team cannot change.
- Language/runtime mandates
- Existing infrastructure the system must integrate with
- Compliance requirements (GDPR, PCI-DSS, SOC2, HIPAA)
- Budget or team size constraints that affect architecture

If a number is unknown, write `[TBD]` rather than omitting the line.

---

## 3. System Overview

A single Mermaid diagram. No prose before the diagram except one sentence of orientation.
After the diagram, one short paragraph (2–3 sentences) describing the main data flow in plain English.

The diagram must show:
- Client entry points
- The application itself (with internal groupings if it has meaningful sub-components)
- Data stores
- External services

The diagram must NOT show:
- Individual functions or methods
- Internal class hierarchies
- Database schemas
- HTTP routes

---

## 4. Component Breakdown

One `###` subsection per major component. Each subsection covers:
1. **Responsibility** — what this component owns, in one sentence.
2. **Interface** — what it accepts as input and what it returns or publishes. For services: key endpoints or method signatures (not all of them — just the most important). For workers: what triggers them and what they produce.
3. **Why it exists as a separate unit** — only include this if the boundary is non-obvious.

For the application's internal modules, a table is acceptable:

| Module | Responsibility | Key Interface |
|--------|---------------|---------------|

Cross-cutting concerns (auth middleware, logging, error handling) get one paragraph, not individual subsections.

Do not describe how a component works internally. "The auth module issues JWTs" is fine. "The auth module calls bcrypt.compare then signs a payload with jsonwebtoken" is too much detail.

---

## 5. Data Architecture

Three subsections:

**Core Entities** — plain text schema block (not SQL DDL, not ORM code). Format:

```
entity_name
  field_name (type, constraints)
  field_name (type, constraints)
  foreign_key (FK → other_entity)
```

Include only entities that represent core domain concepts. Junction tables, audit tables, and config tables are optional.

**Storage Technology Choices** — one paragraph per storage technology actually used. Each paragraph must answer:
- What data lives here?
- Why this technology over the obvious alternative?

If there is no cache, no queue, no object storage — omit those paragraphs. Do not document what does not exist.

**Data Flow** — one paragraph describing the lifecycle of a typical write request from entry to persistence, including any async side effects. Write it as a sequence in prose: "Request arrives → X → Y → Z. Side effects are handled by..."

---

## 6. Key Architectural Decisions

A table summarizing significant technology choices and architectural decisions with brief rationale.

```md
| Decision | Choice | Rationale |
|----------|--------|-----------|
| Database | PostgreSQL | Relational integrity required; team familiarity |
| Auth strategy | JWT (RS256) | Stateless; supports multiple clients |
```

Every row must name a real decision made for this project. Do not add generic placeholder rows.

---

## 7. Security & Compliance

Cover these topics with one paragraph each (omit topics that are not applicable):

- **Authentication** — mechanism, token format, signing algorithm, key storage
- **Authorization** — model (RBAC, ABAC, ownership), where it is enforced (middleware vs. service layer)
- **Secrets management** — where secrets live, how they reach the application at runtime
- **Data protection** — encryption at rest, encryption in transit, any PII handling rules
- **Payment / sensitive data** — if applicable, how the system avoids storing sensitive data directly (e.g. delegating to Stripe, Adyen)
- **Compliance** — which standards apply and what the system does to meet them

This section describes the model, not the implementation. "JWT with RS256" is the right level. "Here is the Passport strategy code" is too much.

---

## 8. Observability

Four topics, each one paragraph:

**Logging** — format (structured JSON?), levels used in production, what context is always included (request ID, user ID, etc.), where logs are shipped.

**Metrics** — what is instrumented and how (library/agent), which metrics are most important (name them specifically), where dashboards live.

**Tracing** — whether distributed tracing is in place, what span boundaries look like, sampling rate.

**Alerting** — what triggers an alert (be specific: error rate threshold, latency threshold, queue depth), where alerts go.

End with a sentence naming the 1–2 most operationally important signals for this specific system. Do not write a generic list.

---

## 9. Error Handling Conventions

Only include this section if the team has an explicit convention. If unsure, ask the user before writing.

Structure:
1. One sentence stating the overall pattern (e.g. "Services return errors as values rather than throwing").
2. A short service code example in the project's language showing the pattern.
3. A short controller/handler code example showing how callers handle the returned error.
4. A bullet list of 3–5 rules, stated as imperatives.

Keep code examples minimal — 10–20 lines each. The goal is to show the pattern, not implement a full feature.

Do not invent a pattern. If the user has not defined one, skip this section or use `[TO BE DEFINED]`.

---

## 10. Testing Expectations

Only include this section if the team has explicit conventions. If unsure, ask the user before writing.

Structure:
1. One sentence stating what gets unit tested and what does not.
2. **What to test** — bullet list of what assertions are expected.
3. **Setup pattern** — one short code example (10–20 lines) showing the test file structure, mocking approach, and assertion style.
4. **File location** — where test files live relative to source files.
5. **Coverage** — the team's policy, stated plainly. If no hard number, say so and state the qualitative rule instead.

Do not write exhaustive test suites. Show the pattern, not every case.

---

## 11. Appendix: Project Structure

A directory tree of the source directory with inline `#` comments on non-obvious directories and files. Show 2–3 levels deep. Omit `node_modules`, build artifacts, and lock files.

```
src/
├── main.ts                   # Application entry point
├── app.module.ts             # Root module / bootstrap config
├── config/                   # Validated config schema
├── common/                   # Shared utilities, guards, interceptors
│   ├── guards/
│   └── filters/
├── modules/
│   ├── auth/
│   │   ├── auth.controller.ts
│   │   ├── auth.service.ts
│   │   └── dto/
│   └── ...
└── database/
    ├── entities/             # ORM entity definitions
    └── migrations/           # Versioned schema migrations
```

If the project uses a monorepo, show the workspace structure at the top level, then drill into the relevant package.
