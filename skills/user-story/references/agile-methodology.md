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

### 3. Task (Two Types)

#### A. Taiga Tasks (High-Level Implementation Categories)

**Definition:** High-level implementation areas that organize work by technology layer.

**Characteristics:**

- Represents a category of implementation work (Backend, Frontend, Smart Contract)
- Contains links to multiple detailed GitLab issues
- Tracked in Taiga alongside user stories for visibility
- Marked complete when all related GitLab issues are done

**Examples:**

- Smart Contract Implementation
- Backend Implementation
- Frontend Implementation

#### B. GitLab Issues (Detailed Technical Tasks)

**Definition:** Specific technical work items needed to implement a user story.

**Characteristics:**

- Technical in nature (not user-facing by itself)
- Contains detailed implementation specifications
- Sized appropriately for the work required
- Multiple GitLab issues combine to complete a Taiga task

**Examples:**

- Write smart contract staking function
- Create backend API endpoint for rewards
- Add unit tests for claim function
- Deploy contract to testnet
- Build React component for staking UI
- Add error handling for validation
- Update API documentation

---

## PRD, Taiga, and GitLab Separation

**Important:** Understanding the separation between product requirements and technical implementation is critical.

### Product Layer (PRD + Taiga)

**Contains:** Epics, User Stories, High-Level Tasks (implementation categories)

**Purpose:**
- Documents **what** needs to be built and **why** (business value)
- Provides user perspective and acceptance criteria
- Shows implementation progress by technology layer
- Used for stakeholder communication and planning

**Audience:** BOD, product team, stakeholders, developers

### Technical Layer (GitLab Issues)

**Contains:** Detailed technical tasks with full implementation specifications

**Purpose:**
- Documents **how** to build it (technical details)
- Contains endpoints, function signatures, database schemas
- Tracks daily development work
- Manages code-level implementation details

**Audience:** Developers only

**Why this separation?**

- Keeps product discussions focused on user value, not implementation details
- Prevents overwhelming stakeholders with technical minutiae
- Allows developers to manage technical details where they work (GitLab)
- Provides visibility into implementation progress without excessive detail

---

## Workflow

### When BOD Provides an Idea/Requirement:

1. **Define the Epic**
   - Capture the BOD's idea as an Epic
   - Note the deadline
   - Clarify the overall objective

2. **Break Down into User Stories (in PRD/Taiga)**
   - Ask: "What value does the user get?"
   - Identify all the user-facing functionalities needed
   - Write each as a user story from the user's perspective
   - Estimate each user story using story points (relative sizing)

3. **Create PRD**
   - Document epics and user stories in the PRD
   - Include acceptance criteria from user perspective
   - Add high-level technical context (but not detailed specs)
   - Stop here - do NOT include detailed implementation tasks in the PRD

4. **Add to Taiga**
   - Create epic in Taiga
   - Add user stories under the epic
   - For each user story, create high-level tasks by implementation layer:
     - Smart Contract Implementation (if needed)
     - Backend Implementation (if needed)
     - Frontend Implementation (if needed)

5. **Decompose into GitLab Issues (Technical Planning)**
   - For each Taiga task, create detailed GitLab issues
   - Size each GitLab issue appropriately for the work required
   - Include full technical specifications:
     - Endpoints with request/response examples
     - Smart contract function signatures
     - Database schema changes
     - Component structure
     - Testing requirements
   - Link each GitLab issue back to the Taiga user story

6. **Estimate and Communicate**
   - Sum up user story points to get epic estimate
   - Use historical velocity to convert story points to timeline
   - Compare estimated timeline against BOD deadline
   - Push back with data if deadline is unrealistic
   - Build in buffer time for code review, testing, and unexpected issues

7. **Execute and Track**
   - Work through GitLab issues systematically
   - Update Taiga task completion as GitLab issues finish
   - Mark Taiga tasks complete when all related GitLab issues are done
   - Mark user story complete when all Taiga tasks are done
   - Communicate progress to stakeholders using user stories (not individual GitLab issues)
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

Keep a visible backlog in Taiga to track progress and communicate status to stakeholders. Use GitLab for detailed technical work.

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
- **Clarity:** Clear separation between business value (user stories) and technical work (GitLab issues)
- **Communication:** Provides a framework to discuss scope and timelines with BOD
- **Manageability:** Large projects become manageable through decomposition
- **Tracking:** Easy to track progress at multiple levels
- **Flexibility:** Can adapt to changing priorities while maintaining structure
- **Focus:** Stakeholders see what matters to them; developers work where technical details belong
- **Visibility:** High-level implementation progress visible in Taiga without overwhelming detail

---

## Tools and Tracking

### Three-System Approach:

**1. PRD (Product Requirements Document)**

- **Purpose:** Document epics and user stories for stakeholders
- **Format:** Markdown document or Google Docs
- **Contains:** Business value, user stories, acceptance criteria, high-level technical context
- **Audience:** BOD, team members, stakeholders
- **Does NOT contain:** Detailed technical specifications, implementation tasks

**2. Taiga (Product & Sprint Tracking)**

- **Purpose:** Track epics, user stories, and high-level implementation progress
- **Contains:** 
  - Epics with deadlines
  - User stories with acceptance criteria
  - High-level tasks (Smart Contract, Backend, Frontend)
  - Links to related GitLab issues
- **Audience:** Product team, stakeholders, developers
- **Does NOT contain:** Detailed technical specifications, code-level implementation details

**3. GitLab Issues (Technical Implementation)**

- **Purpose:** Manage detailed technical implementation work
- **Contains:**
  - Detailed task breakdown (~1 day each)
  - Full technical specifications (endpoints, contracts, schemas)
  - Implementation notes and requirements
  - Testing checklists
  - Code-level details
- **Audience:** Developers only
- **Links back to:** Taiga user story

### Linking Between Systems:

**From Taiga to GitLab:**
- Each Taiga task (e.g., "Backend Implementation") contains links to multiple GitLab issues
- Example: Backend Implementation → GitLab #101, #102, #103

**From GitLab to Taiga:**
- Each GitLab issue header includes: "Implements: Taiga User Story #45"
- This creates bidirectional traceability

### Minimum Tracking Needed:

**In PRD:**
- Epic name and deadline
- Epic description and business value
- List of user stories with acceptance criteria
- User story estimates (in story points)

**In Taiga:**
- Epic with deadline
- User stories with acceptance criteria and priority
- High-level tasks per user story (by implementation layer)
- Links to related GitLab issues
- Status of each user story and task

**In GitLab Issues:**
- Tasks broken down from each Taiga task
- Full technical specifications
- Task estimates (in hours or days for time tracking)
- Task status (To Do, In Progress, Done)
- Link back to Taiga user story
- Actual time spent vs. estimated time (for improving future estimates)

---

## Example Structure

```text
Epic: Token Staking Platform (Deadline: 6 weeks)
│
├── User Story: As a token holder, I want to stake my tokens so that I can earn rewards
│   │
│   ├── Taiga Task: Smart Contract Implementation
│   │   ├── GitLab Issue #101: Design staking contract architecture
│   │   ├── GitLab Issue #102: Implement staking function
│   │   ├── GitLab Issue #103: Implement unstaking function
│   │   └── GitLab Issue #104: Write smart contract unit tests
│   │
│   ├── Taiga Task: Backend Implementation
│   │   ├── GitLab Issue #105: Create staking API endpoint
│   │   ├── GitLab Issue #106: Add staking event listeners
│   │   └── GitLab Issue #107: Write backend integration tests
│   │
│   └── Taiga Task: Frontend Implementation
│       ├── GitLab Issue #108: Build staking UI component
│       ├── GitLab Issue #109: Integrate with backend API
│       └── GitLab Issue #110: Add loading and error states
│
├── User Story: As a staker, I want to view my staking rewards so that I know how much I've earned
│   │
│   ├── Taiga Task: Smart Contract Implementation
│   │   ├── GitLab Issue #111: Implement reward calculation logic
│   │   └── GitLab Issue #112: Create rewards query function
│   │
│   ├── Taiga Task: Backend Implementation
│   │   ├── GitLab Issue #113: Build rewards API endpoint
│   │   └── GitLab Issue #114: Add integration tests
│   │
│   └── Taiga Task: Frontend Implementation
│       └── GitLab Issue #115: Display rewards in UI
│
└── User Story: As a staker, I want to claim my rewards so that I can receive my earned tokens
    │
    ├── Taiga Task: Smart Contract Implementation
    │   ├── GitLab Issue #116: Implement claim function
    │   ├── GitLab Issue #117: Add claim validation logic
    │   └── GitLab Issue #118: Write tests for claim functionality
    │
    ├── Taiga Task: Backend Implementation
    │   ├── GitLab Issue #119: Create claim API endpoint
    │   └── GitLab Issue #120: Add claim event listeners
    │
    └── Taiga Task: Frontend Implementation
        └── GitLab Issue #121: Build claim button and confirmation UI
```

---

## Common Patterns

### When a User Story Needs Only Backend

Some user stories may only require backend or smart contract work (e.g., API-only features, cron jobs, background processes):

```text
User Story: As a system, I want to automatically calculate rewards daily

├── Taiga Task: Smart Contract Implementation
│   └── (GitLab issues for reward calculation)
│
└── Taiga Task: Backend Implementation
    └── (GitLab issues for cron job, event processing)

(No Frontend Task needed)
```

### When a User Story Spans Multiple Repositories

If your smart contracts, backend, and frontend are in separate repos, GitLab issues live in their respective repositories but all link back to the same Taiga user story.

### Handling Dependencies

If a frontend task depends on backend completion:
- Note the dependency in the GitLab issue description
- Use GitLab's "blocks/blocked by" relationships
- Taiga task status reflects overall progress across all GitLab issues

---

**Note:** This document is the result of a collaboration between myself and Claude, an AI assistant by Anthropic, to develop a practical Agile methodology tailored to my work context as a smart contract and backend developer working with Taiga and GitLab.
