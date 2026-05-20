# Error Handling

The core pattern: **services return errors as values, controllers throw them.**

## The Pattern

Services return a union of the success type and a NestJS `HttpException` subclass:

```ts
async findUser(id: string): Promise<User | NotFoundException> {
  const user = await this.userRepository.findOne({ where: { id } });
  if (!user) {
    this.logger.warn(`User not found: ${id}`);
    return new NotFoundException(`User not found: ${id}`);
  }
  return user;
}
```

Controllers do exactly one thing with the result:

```ts
@Get(':id')
async getUser(@Param('id') id: string) {
  const result = await this.userService.findUser(id);
  if (result instanceof Error) throw result;
  return result;
}
```

## Rules

### Always return, never throw inside a service

```ts
// ✅ correct
async findUser(id: string): Promise<User | NotFoundException> {
  if (!user) return new NotFoundException('User not found');
  return user;
}

// ❌ wrong — throwing inside a service makes it harder to test
async findUser(id: string): Promise<User> {
  if (!user) throw new NotFoundException('User not found');
  return user;
}
```

### Use NestJS built-in HTTP exceptions

Do not create custom error classes. Use NestJS exceptions directly — they carry the correct HTTP status and are recognizable across the codebase.

Common exceptions:
- `NotFoundException` — 404, resource not found
- `BadRequestException` — 400, invalid input that passed validation
- `ForbiddenException` — 403, authenticated but not authorized
- `UnauthorizedException` — 401, not authenticated
- `ConflictException` — 409, state conflict (e.g. duplicate)
- `UnprocessableEntityException` — 422, business rule violation
- `InternalServerErrorException` — 500, unexpected failure

### Log in the service, before returning the error

```ts
// ✅ correct — log where the error originates
if (!user) {
  this.logger.warn(`User not found: ${id}`);
  return new NotFoundException(`User not found: ${id}`);
}

// ❌ wrong — logging in the controller loses context
const result = await this.userService.findUser(id);
if (result instanceof Error) {
  this.logger.warn('something failed'); // too late, too vague
  throw result;
}
```

Use `this.logger.warn()` for expected failures (not found, conflict).
Use `this.logger.error()` for unexpected failures (DB down, external API failure).

### Inject Logger per class

```ts
@Injectable()
export class UserService {
  private readonly logger = new Logger(UserService.name);
}
```

Never use `console.log`. Always use the NestJS `Logger`.

### Bubble errors up unchanged in chained services

When a service calls another service, propagate the error as-is unless this service specifically needs to handle it:

```ts
async createOrder(userId: string, dto: CreateOrderDto): Promise<Order | NotFoundException | BadRequestException> {
  // bubble up — OrderService doesn't own this error
  const user = await this.userService.findUser(userId);
  if (user instanceof Error) return user;

  // bubble up — same principle
  const item = await this.itemService.findItem(dto.itemId);
  if (item instanceof Error) return item;

  // this service owns this error
  if (item.stock < dto.quantity) {
    this.logger.warn(`Insufficient stock for item: ${dto.itemId}`);
    return new BadRequestException('Insufficient stock');
  }

  return this.orderRepository.save({ user, item, quantity: dto.quantity });
}
```

### Return type should list all possible errors

Be explicit in the return type signature. This documents what can go wrong and TypeScript will remind you when things change:

```ts
// ✅ explicit — callers know what errors to expect
async createOrder(userId: string, dto: CreateOrderDto): Promise<Order | NotFoundException | BadRequestException>

// ❌ vague — hides what errors can actually come back
async createOrder(userId: string, dto: CreateOrderDto): Promise<Order | Error>
```

### Controller pattern is always the same

Every controller method follows the same shape — no exceptions:

```ts
@Post()
async create(@Body() dto: CreateOrderDto) {
  const result = await this.orderService.createOrder(dto);
  if (result instanceof Error) throw result;
  return result;
}
```

Do not put business logic in controllers. Do not map errors in controllers. The controller's only job is: call service, check for error, throw or return.
