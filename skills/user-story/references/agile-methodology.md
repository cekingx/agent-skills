# My Agile Method

## Overview

This is a customized Agile methodology adapted for smart contract and backend development in a non-Agile organization where requirements come from the Board of Directors (BOD) with fixed deadlines.

## Purpose

To bring structure and manageability to top-down product requirements while maintaining focus on delivering user value incrementally.

## Three-Level Hierarchy

### 1. Epic

**Definition:** A collection of related user stories that represents a larger feature or BOD initiative.

**Characteristics:**

- Originates from BOD ideas and requirements
- Too large to complete in a single sprint
- Has an associated deadline from BOD
- Represents a significant product feature or capability

**Example:**

- "Token Staking Platform"
- "Multi-signature Wallet Feature"
- "NFT Marketplace Integration"

---

### 2. User Story

**Definition:** The smallest functionality that brings value to the end user.

**Characteristics:**

- Delivers clear, tangible value to users
- Estimated using story points (relative sizing)
- Written from user perspective
- Follows the format: "As a [user type], I want [goal] so that [benefit]"

**Examples:**

- "As a token holder, I want to transfer tokens to another wallet so that I can send funds to others"
- "As a user, I want to see my token balance so that I know how many tokens I have"
- "As a staker, I want to stake my tokens so that I can earn rewards"

**Key Rule:** If a user story is complete, the user should be able to do something they couldn't do before or have a better experience.

---

### 3. Task

**Definition:** A specific implementation work item for a single technology layer, containing enough technical detail to guide a developer.

**Characteristics:**

- Scoped to one technology layer (Smart Contract, Backend, or Frontend)
- Contains named functions, endpoints, or components and their purpose
- Includes full endpoint specifications for new APIs
- Has a concrete implementation checklist
- One or more tasks combine to complete a user story

**Examples:**

- Smart Contract: Implement staking function
- Backend: Create staking API endpoint
- Frontend: Build staking UI component

**Key Rule:** Tasks capture *how* to build at a planning level — they guide developers without replacing their work. Do not write full implementations in tasks.

---

## Product and Technical Separation

**Important:** Understanding the separation between product requirements and technical implementation is critical.

### Product Layer

**Contains:** Epics, User Stories

**Purpose:**
- Documents **what** needs to be built and **why** (business value)
- Provides user perspective and acceptance criteria
- Used for stakeholder communication and planning

**Audience:** BOD, product team, stakeholders, developers

### Technical Layer

**Contains:** Tasks with full implementation specifications per technology layer

**Purpose:**
- Documents **how** to build it (technical details)
- Contains endpoints, function signatures, database schema changes
- Tracks daily development work
- Manages code-level implementation details

**Audience:** Developers

**Why this separation?**

- Keeps product discussions focused on user value, not implementation details
- Prevents overwhelming stakeholders with technical minutiae
- Allows developers to manage technical details where they work
- Provides visibility into implementation progress without excessive detail

---

## Workflow

### When BOD Provides an Idea/Requirement:

1. **Define the Epic**
   - Capture the BOD's idea as an Epic
   - Note the deadline
   - Clarify the overall objective

2. **Break Down into User Stories**
   - Ask: "What value does the user get?"
   - Identify all the user-facing functionalities needed
   - Write each as a user story from the user's perspective
   - Estimate each user story using story points (relative sizing)

3. **Create PRD**
   - Document epics and user stories in the PRD
   - Include acceptance criteria from user perspective
   - Add high-level technical context (but not detailed specs)
   - Stop here — do NOT include detailed implementation tasks in the PRD

4. **Decompose into Tasks (Technical Planning)**
   - For each user story, create one task per technology layer involved:
     - Smart Contract Task (if needed)
     - Backend Task (if needed)
     - Frontend Task (if needed)
   - Each task includes:
     - Named functions, endpoints, or components and their purpose
     - Full specifications for new API endpoints
     - Implementation notes (dependencies, security, edge cases)
     - Concrete implementation checklist

5. **Estimate and Communicate**
   - Sum up user story points to get epic estimate
   - Use historical velocity to convert story points to timeline
   - Compare estimated timeline against BOD deadline
   - Push back with data if deadline is unrealistic
   - Build in buffer time for code review, testing, and unexpected issues

6. **Execute and Track**
   - Work through tasks systematically
   - Mark tasks complete as implementation finishes
   - Mark user story complete when all its tasks are done
   - Communicate progress to stakeholders using user stories (not individual tasks)
   - Adjust estimates based on actual completion time

---

## Key Principles

### Focus on User Value

Every user story must deliver something meaningful to the end user. If it doesn't provide user value, it's a task, not a user story.

### Break Down Large Work

Big ideas from BOD are broken down into manageable pieces that can be estimated, tracked, and delivered incrementally.

### Separate Product from Implementation

Keep product requirements (what and why) separate from technical implementation (how). This allows each audience to focus on what matters to them.

### Maintain Visibility

Keep a visible backlog to track progress and communicate status to stakeholders.

### Build in Buffers

Always include time for:

- Code review
- Testing and bug fixes
- Unexpected complexity
- Deployment and configuration
- Documentation

### Adapt as Needed

This method is flexible. Adjust the process based on what works and what doesn't. The goal is to deliver value effectively, not to follow rules rigidly.

---

## Benefits of This Approach

- **Structure:** Brings order to top-down requirements
- **Clarity:** Clear separation between business value (user stories) and technical work (tasks)
- **Communication:** Provides a framework to discuss scope and timelines with BOD
- **Manageability:** Large projects become manageable through decomposition
- **Tracking:** Easy to track progress at multiple levels
- **Flexibility:** Can adapt to changing priorities while maintaining structure
- **Focus:** Stakeholders see what matters to them; developers work where technical details belong

---

## Tools and Tracking

Use whatever tools fit your team. The methodology is tool-agnostic. What matters is maintaining the three-level hierarchy and the product/technical separation.

### Minimum Tracking Needed:

**At the Product Level (PRD or equivalent):**
- Epic name and deadline
- Epic description and business value
- List of user stories with acceptance criteria
- User story estimates (in story points)
- Priority per user story

**At the Task Level (task tracker or equivalent):**
- Tasks organized by user story and technology layer
- Full technical specifications per task
- Implementation checklist
- Task status (To Do, In Progress, Done)
- Dependencies between tasks

---

## Example Structure

```text
Epic: Token Staking Platform (Deadline: 6 weeks)
│
├── User Story: As a token holder, I want to stake my tokens so that I can earn rewards
│   │
│   ├── Task: Smart Contract — Implement staking and unstaking functions
│   │
│   ├── Task: Backend — Create staking API and event listeners
│   │
│   └── Task: Frontend — Build staking UI component
│
├── User Story: As a staker, I want to view my staking rewards so that I know how much I've earned
│   │
│   ├── Task: Smart Contract — Implement reward calculation and query functions
│   │
│   ├── Task: Backend — Build rewards API endpoint
│   │
│   └── Task: Frontend — Display rewards in UI
│
└── User Story: As a staker, I want to claim my rewards so that I can receive my earned tokens
    │
    ├── Task: Smart Contract — Implement claim function and validation
    │
    ├── Task: Backend — Create claim API and event listeners
    │
    └── Task: Frontend — Build claim button and confirmation UI
```

---

## Common Patterns

### When a User Story Needs Only Backend

Some user stories may only require backend or smart contract work (e.g., API-only features, cron jobs, background processes):

```text
User Story: As a system, I want to automatically calculate rewards daily

├── Task: Smart Contract — Implement reward calculation logic
│
└── Task: Backend — Create cron job and event processing

(No Frontend Task needed)
```

### When a User Story Spans Multiple Repositories

If your smart contracts, backend, and frontend are in separate repos, tasks live in their respective repositories but all link back to the same user story.

### Handling Dependencies

If a frontend task depends on backend completion:
- Note the dependency in the task description
- Use your task tracker's dependency or blocking relationships

---

**Note:** This document is the result of a collaboration between myself and Claude, an AI assistant by Anthropic, to develop a practical Agile methodology tailored to my work context as a smart contract and backend developer.
