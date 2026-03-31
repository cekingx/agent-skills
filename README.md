# cekingx-agent-skills

A collection of custom agent skills for Claude Code, tailored for smart contract and blockchain development workflows.

## Skills

### user-story

Generates Taiga user stories following a customized Agile methodology for smart contract and backend development.

**Triggers:** "create user stories for", "break down this epic", "write user stories", "generate stories"

**Input:** BOD requirements, epics, feature requests, or problem statements

**Output:** Structured user stories with:
- "As a..., I want..., So that..." format
- Story point estimates (Fibonacci)
- User-focused acceptance criteria
- High-level implementation tasks (Smart Contract / Backend / Frontend)

**Key principle:** Stories capture *what* and *why* (product layer). Detailed technical specs belong in GitLab issues, not here.

## Structure

```
skills/
└── user-story/
    ├── SKILL.md                          # Skill definition and generation process
    ├── assets/
    │   ├── template.md                   # Output template
    │   └── example-user-stories.md       # Reference examples
    └── references/
        └── agile-methodology.md          # Full methodology reference
```

## Usage

Skills are invoked automatically by Claude Code when your request matches the trigger conditions, or explicitly via `/user-story`.
