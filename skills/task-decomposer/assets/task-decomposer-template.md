## [User Story Title]

**Layer**: Backend / Frontend / Smart Contract

---

### User Story Context

**As a** [user type]
**I want to** [action/feature]
**So that** [benefit/value]

---

### Technical Scope

#### Endpoints (if Backend — new APIs)

For each new API endpoint, include full specification:

**[METHOD]** `/api/[path]` — [Description]
- **Authentication**: Required / None
- **Request**:
  ```json
  {
    "field": "type — description"
  }
  ```
- **Response (200)**:
  ```json
  {
    "field": "type — description"
  }
  ```
- **Error Codes**:
  - `400` — [Reason]
  - `401` — [Reason]
  - `500` — [Reason]

For existing endpoints being reused, just reference them by method + path without full spec.

#### Functions (if Smart Contract)

- `functionName(params)` — [Purpose, access level, events emitted]
- `functionName(params)` — [Purpose, access level]

#### Components (if Frontend)

- `ComponentName` — [Purpose, what data it displays or what action it handles]
- `ComponentName` — [Purpose]

#### Database Changes (if applicable)

- New table or fields needed: [name and purpose, not full schema]
- Migrations required: yes/no

---

### Notes

- [Dependencies on other tasks or layers]
- [Security considerations to address]
- [Performance considerations]
- [Third-party libraries or tools needed]
- [Known edge cases or gotchas]

---

### Implementation Tasks

- [ ] [Architecture or design review item]
- [ ] [Core implementation item]
- [ ] [Additional feature or logic item]
- [ ] [Error handling and validation]
- [ ] [Unit tests]
- [ ] [Integration tests]
- [ ] [Code review fixes]
- [ ] [Documentation update]

---

### Definition of Done

- [ ] Code implemented according to specifications
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Merged to main branch
- [ ] Deployed to staging
- [ ] QA verified
