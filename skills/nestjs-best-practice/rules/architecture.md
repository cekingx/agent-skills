# Architecture & Modules

## The Core Separation

Every feature has two kinds of services:

**Business services** — contain logic, are fully unit-testable, return `Promise<Data | HttpException>`.

**Provider services** (`*Provider`) — wrap untestable side-effects: HTTP clients, email, queues, external APIs, file storage. They are injected into business services and mocked entirely in unit tests.

```
UserService         ← business logic, testable
UserRepository      ← TypeORM, mocked in unit tests
EmailProvider       ← sends email, mocked in unit tests
StripeProvider      ← calls Stripe API, mocked in unit tests
```

## Rules

### Name side-effect services with `*Provider`

Any service that touches something outside the process boundary (network, disk, queue) is a `*Provider`:

```ts
// ✅
@Injectable()
export class EmailProvider {
  async sendWelcomeEmail(to: string): Promise<void | InternalServerErrorException> { ... }
}

@Injectable()
export class StripeProvider {
  async createCustomer(email: string): Promise<StripeCustomer | InternalServerErrorException> { ... }
}
```

`*Provider` services also follow the return-error-as-value pattern. They never throw.

### Business services depend on Providers, not the other way around

```ts
// ✅ correct direction
@Injectable()
export class UserService {
  constructor(
    @InjectRepository(User) private readonly userRepo: Repository<User>,
    private readonly emailProvider: EmailProvider,
  ) {}
}

// ❌ wrong — Provider should not know about business services
@Injectable()
export class EmailProvider {
  constructor(private readonly userService: UserService) {} // circular, wrong
}
```

### Start top-down — split only when the boundary is obvious

Don't create modules speculatively. Start with fewer, larger modules and split only when a domain boundary becomes clearly visible. Over-modularizing early creates shallow modules that cost more than they're worth.

A module justifies its existence when it **hides meaningful complexity** — not just when it groups related files together. Ask: does this module hide something from the rest of the app, or does it just reorganize it?

```
// ✅ earns its module — hides the full payments domain
PaymentsModule (PaymentsService, StripeProvider, PaymentEntity, WebhookHandler)

// ❌ shallow — just groups helpers, hides nothing
UtilsModule (CryptoService, SlugService, DateService)
```

Signals that a boundary has become obvious:
- It owns a clearly separate domain concept (users, payments, notifications)
- It could change or be replaced without touching other modules
- It has enough internal complexity that hiding it behind a module boundary simplifies the rest of the codebase

### Avoid shallow modules

A shallow module is one that doesn't hide anything meaningful — it only groups services that happen to be related. The abstraction costs more than it saves.

```ts
// ❌ shallow — no domain, no hidden complexity, just re-exports
@Module({
  providers: [CryptoService, SlugService, DateService],
  exports: [CryptoService, SlugService, DateService],
})
export class UtilsModule {}
```

If services don't share a domain, keep them where they're used. If multiple modules need a utility, inject it directly or move it to the module that owns it most naturally.

### Module structure when a boundary is clear

```
src/
  users/
    users.module.ts
    users.controller.ts
    users.service.ts
    users.provider.ts       ← if needed
    user.entity.ts
    dto/
    tests/
  orders/
    orders.module.ts
    ...
```

### Modules depend one-way

If `OrdersModule` needs user data, it imports `UsersModule` (which exports `UsersService`). If two modules need each other, the boundary is wrong — merge them or extract the shared concern into the module that owns it most naturally.

```ts
// ✅ one-way dependency
@Module({
  imports: [UsersModule],
  controllers: [OrdersController],
  providers: [OrdersService],
})
export class OrdersModule {}
```

### Controllers are thin

Controllers do exactly three things:
1. Declare the route and extract request data (`@Param`, `@Body`, `@Query`)
2. Call the service
3. Check `instanceof Error`, throw or return

No business logic, no logging, no error mapping in controllers.

### Keep modules focused

A module should own one domain concept. If a module's `providers` array grows beyond ~6 services, consider splitting it.
