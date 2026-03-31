---
name: user-story
description: Generate user stories following a customized Agile methodology for smart contract and backend development. Use this skill when the user provides a problem statement, BOD requirement, feature request, or epic and needs it structured into user stories with acceptance criteria and high-level implementation tasks. Triggers include requests like "create user stories for", "break down this epic", "write user stories", or "generate stories".
---

# User Story Generation

Generate comprehensive user stories following a customized Agile methodology adapted for smart contract and backend development.

## When to Use This Skill

Use when user provides:
- BOD requirements or ideas
- Epics that need to be broken down into user stories
- Feature requests needing structure
- Requests to create or update user stories

## Core Methodology

Read the full methodology in [references/agile-methodology.md](references/agile-methodology.md) before generating user stories.

**Key principles:**
- **Epics**: Large features from BOD with deadlines
- **User Stories**: Smallest functionality delivering user value
- **Format**: "As a [user type], I want [goal] so that [benefit]"
- **Estimation**: Use story points (relative sizing), not time-based estimates
- **Tasks**: High-level implementation categories only (Smart Contract, Backend, Frontend)
- **Critical**: contains ONLY epics, user stories, and high-level tasks. Detailed technical specs belong in GitLab issues.

## Generation Process

### 1. Understand the Requirement

- Clarify the epic's objective and deadline
- Identify the problem being solved or opportunity captured
- Determine the scope and constraints
- Understand which technology layers are involved (smart contract, backend, frontend)

### 2. Break Down into User Stories

Ask: **"What value does the user get?"**

- Write from user perspective
- Each story must deliver tangible value
- Use the format: "As a [user type], I want [goal] so that [benefit]"
- Estimate using story points (1, 2, 3, 5, 8, 13)
- Keep stories independent when possible

**Test**: If a user story is complete, the user should be able to do something they couldn't do before.

### 3. Define Acceptance Criteria

For each user story:
- Write testable conditions from **user perspective**
- Focus on what the user can do, not technical implementation
- Use checkboxes for tracking
- Keep criteria clear and verifiable

**Good acceptance criteria:**
- [ ] User can stake tokens through the UI
- [ ] User sees confirmation message after staking
- [ ] User's staked balance updates immediately

**Bad acceptance criteria (too technical):**
- [ ] Smart contract staking function implemented
- [ ] API endpoint returns 200 status code
- [ ] Database migration completed

### 4. Add High-Level Implementation Tasks

For each user story, create tasks by technology layer (only if applicable):
- **Smart Contract Implementation** - If smart contract work needed
- **Backend Implementation** - If backend/API work needed
- **Frontend Implementation** - If UI work needed

**Important**: These are just category labels, not detailed work items. Each task will link to multiple detailed GitLab issues.

### 5. Estimate Story Points

Use relative sizing (Fibonacci sequence):
- **1 point**: Trivial change, well understood
- **2 points**: Small, straightforward work
- **3 points**: Medium complexity
- **5 points**: Larger effort, some unknowns
- **8 points**: Significant work, consider splitting
- **13+ points**: Too large, must split into smaller stories

### 6. Prioritize

Assign priority based on:
- **High**: Critical for epic success, blocking other work
- **Medium**: Important but not blocking
- **Low**: Nice to have, can be deferred

### 7. Use the Template

Copy structure from [assets/template.md](assets/template.md) and fill it with generated content.

## Output Format

Always output as a markdown file following the template structure:
1. Epic information (name, description, business value, deadline)
2. User stories with full details
3. Each user story includes:
   - User story format (As a..., I want..., So that...)
   - Priority and story point estimate
   - Acceptance criteria (user-focused)
   - High-level implementation tasks (Smart Contract/Backend/Frontend)
   - Placeholders for GitLab issue links

## Smart Contract & Blockchain Context

For blockchain/smart contract features, common user story patterns:
- Token operations (transfer, approve, stake, unstake)
- Viewing balances and states
- Claiming rewards or benefits
- Managing permissions
- Interacting with other protocols

Remember: Focus on **what the user can do**, not how it's implemented technically.

## Quality Checklist

Before finalizing:
- [ ] Every user story delivers clear user value
- [ ] User stories follow "As a..., I want..., So that..." format
- [ ] Acceptance criteria are from user perspective (not technical implementation)
- [ ] Story points are estimated using relative sizing
- [ ] High-level tasks are implementation categories only (Smart Contract/Backend/Frontend)
- [ ] No detailed technical specifications in Taiga (those belong in GitLab)
- [ ] Epic deadline is noted
- [ ] Business value is clearly articulated
- [ ] Priorities are assigned

## Example User Stories

**Good:**
- "As a token holder, I want to stake my tokens so that I can earn rewards"
  - 5 story points
  - Acceptance criteria: User can stake, sees confirmation, balance updates
  - Tasks: Smart Contract Implementation, Frontend Implementation

**Bad (these are too technical):**
- "Implement staking function" (no user perspective)
- "Create staking API endpoint" (technical task, not user value)
- "Deploy contract to testnet" (technical task)

## Relationship to GitLab Issues

After creating user stories in Taiga:
1. Each high-level task (Smart Contract/Backend/Frontend) will be decomposed into detailed GitLab issues
2. GitLab issues contain the technical specifications (endpoints, contracts, schemas)
3. GitLab issues link back to the Taiga user story
4. Taiga shows implementation progress without technical details

**Separation principle**: Stories = what and why (product layer), GitLab = how (technical layer)
