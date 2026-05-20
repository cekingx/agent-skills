# Testing Patterns

## Philosophy

Services are pure functions of their dependencies. Test them directly — no HTTP layer, no NestJS testing module, no bootstrapping. Mock `*Provider` dependencies entirely. Assert both success and error return values.

## Unit Testing Services

Instantiate the service manually — do **not** use `Test.createTestingModule` for unit tests.

`Test.createTestingModule` is for testing the NestJS DI container itself (guards, interceptors, module wiring, full HTTP pipeline). For a service that takes dependencies and returns a value or error, it is overkill — it spins up a DI container just to call one function.

Direct instantiation is:
- **Faster** — no container bootstrapping
- **Explicit** — the test reads exactly like the code it tests
- **Better at failing** — if the constructor signature changes, TypeScript catches it immediately; `createTestingModule` can hide mismatches

Mock repositories and providers with Jest:

```ts
describe('UserService', () => {
  let service: UserService;
  let userRepo: jest.Mocked<Repository<User>>;
  let emailProvider: jest.Mocked<EmailProvider>;

  beforeEach(() => {
    userRepo = {
      findOne: jest.fn(),
      save: jest.fn(),
    } as any;

    emailProvider = {
      sendWelcomeEmail: jest.fn(),
    } as any;

    service = new UserService(userRepo, emailProvider);
  });
});
```

No `Test.createTestingModule()` needed for unit tests. Direct instantiation is faster and clearer.

## Rules

### Test both the success path and every error path

```ts
describe('findUser', () => {
  it('returns the user when found', async () => {
    const user = { id: '1', name: 'Alice' } as User;
    userRepo.findOne.mockResolvedValue(user);

    const result = await service.findUser('1');

    expect(result).toBe(user);
  });

  it('returns NotFoundException when user does not exist', async () => {
    userRepo.findOne.mockResolvedValue(null);

    const result = await service.findUser('1');

    expect(result).toBeInstanceOf(NotFoundException);
  });
});
```

### Assert error type with `instanceof`, not message strings

```ts
// ✅ robust — type is what matters
expect(result).toBeInstanceOf(NotFoundException);

// ❌ fragile — message can change
expect((result as NotFoundException).message).toBe('User not found: 1');
```

Message assertions are acceptable only when the message content is part of the contract (e.g. it's shown to the user).

### Mock `*Provider` entirely — never let them run in unit tests

```ts
// ✅ full mock — unit test doesn't care about email internals
emailProvider.sendWelcomeEmail.mockResolvedValue(undefined);

// ❌ wrong — real EmailProvider makes network calls
const emailProvider = new EmailProvider();
```

### Test that errors from chained services bubble up

```ts
it('returns NotFoundException from userService when user is missing', async () => {
  const error = new NotFoundException('User not found: 99');
  userService.findUser.mockResolvedValue(error);

  const result = await orderService.createOrder('99', dto);

  expect(result).toBe(error); // same instance, not re-wrapped
});
```

This verifies the bubble-up pattern — the caller doesn't swallow or re-map the error.

### Test that services log before returning errors

```ts
it('logs a warning when user is not found', async () => {
  const loggerWarnSpy = jest.spyOn(service['logger'], 'warn');
  userRepo.findOne.mockResolvedValue(null);

  await service.findUser('1');

  expect(loggerWarnSpy).toHaveBeenCalledWith(expect.stringContaining('1'));
});
```

### E2E tests verify HTTP status codes

E2E tests cover the controller layer — they confirm that returned errors become the right HTTP responses:

```ts
it('GET /users/:id returns 404 when user does not exist', async () => {
  await request(app.getHttpServer())
    .get('/users/nonexistent')
    .expect(404);
});
```

E2E tests don't test business logic — that's covered in unit tests. They test that the controller correctly throws what the service returns.

### File structure

```
src/
  users/
    tests/
      users.service.spec.ts    ← unit tests
      users.e2e.spec.ts        ← e2e tests (optional per feature)
```

Keep test files next to the code they test.
