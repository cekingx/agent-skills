---
name: task-decomposer
description: Decompose user stories into detailed technical task breakdowns organized by technology layer (Smart Contract, Backend, Frontend). Use this skill when the user provides a user story and needs it broken down into concrete implementation tasks. Triggers include requests like "decompose this user story", "break down into tasks", "create implementation tasks", or "break this down by layer". Does NOT generate full implementations — only structured task lists with enough technical context to guide developers.
---

# Task Decomposer

Decompose user stories into layer-specific implementation tasks with enough technical context to guide developers — without writing the full implementation.

## When to Use This Skill

Use when user provides:
- A user story that needs technical breakdown
- Request to decompose or break down work into tasks
- Request to split high-level functionality into concrete work items per layer

## Scope Boundaries

**Do:**
- List what needs to be built per layer
- Name the functions, endpoints, and components involved
- Describe behavior and purpose of each item
- Note access control, security concerns, dependencies
- Write concrete checklist items developers can act on

**Do NOT:**
- Write full smart contract function implementations or bodies
- Write full SQL table definitions
- Write frontend component code or state management logic
- Generate anything that replaces actual development work

**Exception — Backend new API endpoints:**
- Write the full endpoint specification: method, path, request body, response body, and error codes
- For existing endpoints being reused, just reference them by method + path

## Core Methodology

Read the full methodology in [references/agile-methodology.md](references/agile-methodology.md) before decomposing tasks.

**Key principles:**
- **One Task Per Layer**: Create separate tasks for Smart Contract, Backend, and Frontend work
- **Name, Don't Implement**: Name functions/endpoints/components and describe what they do — not how they're coded
- **Guide, Don't Replace**: The output should help a developer plan their work, not do it for them

## Decomposition Process

### 1. Understand the User Story

- Read the complete user story
- Identify acceptance criteria
- Determine which technology layers are needed (Smart Contract, Backend, Frontend)

### 2. Identify Implementation Layers

- **Smart Contract**: Blockchain logic, token operations, state management
- **Backend**: APIs, event listeners, database operations, business logic
- **Frontend**: UI components, state management, API integration, user interactions

### 3. Create One Task Per Layer

Create **separate tasks** for each technology layer involved.

Example for "As a user, I want to stake tokens":
- Task #1: Implement Staking Smart Contract
- Task #2: Create Staking Backend API
- Task #3: Build Staking Frontend UI

### 4. Describe Technical Scope (Without Implementing)

For each task, list what needs to exist and why — not how to build it:

**For Smart Contract Tasks:**
- Functions needed (name, purpose, access level)
- Events to emit (name, what triggers them)
- State variables required
- Security concerns to address (e.g., reentrancy, overflow)

**For Backend Tasks:**
- New endpoints: write full specification (method, path, request body, response body, error codes)
- Existing endpoints being reused: reference by method + path only
- Database changes required (table or field names, not full DDL)
- Event listeners needed
- Error cases to handle

**For Frontend Tasks:**
- Components to create or modify (name, purpose)
- State to manage (what data, not how it's structured)
- API calls to make
- UX considerations (loading states, error messages, validation)

### 5. Note Dependencies and Risks

- Which tasks block others
- Security considerations
- Third-party libraries or tools needed
- Known edge cases or gotchas

### 6. Create Implementation Checklist

Concrete, actionable checklist items — no time estimates:
- Design/architecture review
- Core implementation work items
- Error handling and validation
- Testing (unit, integration)
- Code review and documentation

### 7. Use the Template and Examples

Copy structure from [assets/task-decomposer-template.md](assets/task-decomposer-template.md).

Reference examples:
- [assets/example-smart-contract-task.md](assets/example-smart-contract-task.md)
- [assets/example-backend-task.md](assets/example-backend-task.md)
- [assets/example-frontend-task.md](assets/example-frontend-task.md)

## Output Format

Output as markdown following the task template structure:
1. Task header with layer
2. User story context
3. Technical scope (what, not how)
4. Notes on dependencies and risks
5. Implementation checklist
6. Related tasks

## Quality Checklist

Before finalizing:
- [ ] Each layer has its own separate task
- [ ] Functions/endpoints/components are named and described, not implemented
- [ ] Implementation checklist items are concrete and actionable
- [ ] Dependencies between tasks are noted
- [ ] Security considerations are mentioned, not solved
- [ ] No full code, schemas, or implementations included

## Common Patterns

### Pattern 1: Full-Stack User Story

Creates 3 tasks — Smart Contract, Backend, Frontend

### Pattern 2: Backend-Only User Story

Creates 1 task — Backend (e.g., cron jobs, background processing)

### Pattern 3: API + UI User Story

Creates 2 tasks — Backend (if API doesn't exist), Frontend
