# cekingx-agent-skills

A collection of custom agent skills for cekingx application development workflows.

## Skills

### user-story

Generates user stories following a customized Agile methodology for backend, frontend, and smart contract development.

**Triggers:** "create user stories for", "break down this epic", "write user stories", "generate stories"

**Input:** BOD requirements, epics, feature requests, or problem statements

**Output:** Structured user stories with:
- "As a..., I want..., So that..." format
- Story point estimates (Fibonacci)
- User-focused acceptance criteria
- High-level implementation tasks (Smart Contract / Backend / Frontend)

**Key principle:** Stories capture *what* and *why* (product layer). Detailed technical specs belong in implementation tasks, not here.

---

### task-decomposer

Decomposes user stories into layer-specific implementation tasks with enough technical context to guide developers — without writing the full implementation.

**Triggers:** "decompose this user story", "break down into tasks", "create implementation tasks", "break this down by layer"

**Input:** A user story

**Output:** One task per technology layer (Smart Contract / Backend / Frontend), each containing:
- Technical scope: functions, endpoints, or components needed (named and described, not implemented)
- Full endpoint specifications for new Backend APIs (method, path, request/response, error codes)
- Implementation notes (dependencies, security, libraries)
- Concrete implementation checklist

**Key principle:** Tasks capture *how* to build it at a planning level. They guide developers without replacing their work.

---

### technical-architect

Generates a comprehensive technical architecture document (`docs/technical-architecture.md`) from project context.

**Triggers:** "document my architecture", "write an architecture doc", "create a system design document", "generate technical docs"

**Input:** Project name, language/framework, modules, data stores, external integrations

**Output:** A single `docs/technical-architecture.md` covering:
- Executive summary, goals & constraints
- System overview (Mermaid flowchart)
- Component breakdown, domain model, data architecture
- Key architectural decisions, security, observability
- Error handling and testing conventions
- Project structure appendix

**Key principle:** Decisions over descriptions — always explains *why* a technology was chosen, not just *what* it is.

---

## Structure

```
master/
└── agile-methodology.md                     # Shared methodology reference

skills/
├── user-story/
│   ├── SKILL.md                              # Skill definition and generation process
│   ├── assets/
│   │   ├── template.md                       # Output template
│   │   └── example-user-stories.md          # Reference examples
│   └── references/
│       └── agile-methodology.md             # Full methodology reference
│
├── task-decomposer/
│   ├── SKILL.md                              # Skill definition and decomposition process
│   ├── assets/
│   │   ├── task-decomposer-template.md       # Output template
│   │   ├── example-smart-contract-task.md   # Example: Smart Contract layer
│   │   ├── example-backend-task.md          # Example: Backend layer
│   │   └── example-frontend-task.md         # Example: Frontend layer
│   └── references/
│       └── agile-methodology.md             # Full methodology reference
│
└── technical-architect/
    ├── SKILL.md                              # Skill definition and generation process
    └── references/
        └── section-guide.md                 # Detailed writing guidance per section
```

## Usage

Skills are invoked automatically by Claude Code when your request matches the trigger conditions, or explicitly via `/user-story`, `/task-decomposer`, or `/technical-architect`.

## Workflow

```
BOD Requirement
    │
    ▼
user-story ──► User stories (what & why)
    │
    ▼
task-decomposer ──► Layer tasks with technical specs (how)
    │
    ▼
Development

technical-architect ──► docs/technical-architecture.md (system design)
```
