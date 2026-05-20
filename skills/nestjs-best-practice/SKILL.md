---
name: nestjs-best-practices
description: "Apply this skill whenever writing, reviewing, or refactoring NestJS TypeScript code. This includes creating or modifying controllers, services, modules, providers, guards, interceptors, filters, and database entities. Triggers for error handling patterns, service return types, testability concerns, logging, module architecture, and TypeORM entity design. Also use for NestJS code reviews and refactoring existing NestJS code to follow best practices. Covers any task involving NestJS backend TypeScript patterns."
license: MIT
metadata:
  author: your-name
---

# NestJS Best Practices

Best practices for NestJS, prioritized by impact. Each rule teaches what to do and why.

## Consistency First

Before applying any rule, check what the application already does. The best choice is the one the codebase already uses. Inconsistency is worse than a suboptimal pattern.

Check sibling files, related controllers, services, or tests for established patterns. If one exists, follow it. These rules are defaults for when no pattern exists yet, not overrides.

## Quick Reference

### 1. Error Handling → `rules/error-handling.md`

- Services return `Promise<Data | HttpException>` — never throw inside a service
- Use NestJS built-in exceptions (`NotFoundException`, `BadRequestException`, etc.) as return values
- Controllers do a single `instanceof Error` check, then `throw result`
- Chained services bubble errors up as-is — no re-mapping unless the service needs to handle it
- Log errors in the service using NestJS `Logger`, before returning them
- Never throw in a service — throwing is the controller's responsibility

### 2. Architecture & Modules → `rules/architecture.md`

- Isolate untestable side-effects (HTTP clients, email, queues, external APIs) into `*Provider` services
- Services contain business logic and are fully unit-testable
- `*Provider` services are integration-tested or mocked entirely in unit tests
- One module per feature; shared utilities go in a `SharedModule`
- Never import feature modules into each other — communicate via shared services or events

### 3. Testing Patterns → `rules/testing.md`

- Unit test services by calling them directly — no HTTP, no NestJS testing module needed
- Mock `*Provider` dependencies entirely in unit tests
- Assert return values including error cases — test that errors are returned, not thrown
- Use `instanceof` assertions to verify correct error types are returned
- E2E tests cover controller behavior: correct HTTP status codes from thrown exceptions


## How to Apply

1. Identify the file type and select relevant sections (e.g., service → §1, §2; controller → §1; test → §3; module → §2)
2. Check sibling files for existing patterns — follow those first per **Consistency First**
3. When in doubt about a pattern, prefer the one that is easier to unit test
