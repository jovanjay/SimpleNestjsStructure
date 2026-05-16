# SimpleNestjsStructure

A reference NestJS project demonstrating a clean, scalable folder structure and layered backend architecture. Intended as a starting point for Node.js + NestJS backend applications.

> Each placeholder directory contains a `.gitignore` file so the folder is tracked by Git without committing empty directories.

---

## Architecture Overview

This project follows a **modular layered architecture** built on NestJS's dependency injection system. Responsibilities are divided across distinct layers:

| Layer | Location | Responsibility |
|---|---|---|
| **Entry point** | `src/main.ts` | Bootstrap the app, global middleware |
| **Root module** | `src/app.module.ts` | Wire all feature modules together |
| **Feature modules** | `src/modules/` | Self-contained vertical slices (users, auth, etc.) |
| **Config** | `src/config/` | Environment-aware configuration |
| **Database** | `src/database/` | DB connection, providers, entities |
| **Common** | `src/common/` | Shared cross-cutting concerns |
| **Interfaces** | `src/interfaces/` | Shared TypeScript contracts |

---

## Folder Structure

```
src/
├── main.ts                         # App bootstrap, global pipes/filters/interceptors
├── app.module.ts                   # Root module — imports all feature modules
├── app.controller.ts               # Root health-check / landing route
├── app.service.ts                  # Root service
│
├── common/                         # Shared cross-cutting concerns
│   ├── decorators/                 # Custom parameter & method decorators
│   ├── guards/                     # Auth / role guards (implements CanActivate)
│   ├── interceptors/               # Logging, transform, timeout interceptors
│   └── utils/                      # Pure helper functions
│
├── config/                         # Application configuration
│   ├── config.module.ts            # ConfigModule setup (@nestjs/config)
│   ├── config.service.ts           # Typed env access wrapper
│   └── constants.ts                # App-wide string/number constants
│
├── modules/                        # Feature modules (one folder per domain)
│   │
│   ├── users/                      # Users feature module
│   │   ├── users.module.ts
│   │   ├── users.controller.ts     # Route handlers (HTTP layer)
│   │   ├── users.service.ts        # Business logic
│   │   ├── users.entity.ts         # TypeORM / Mongoose entity
│   │   └── dto/
│   │       ├── create-user.dto.ts  # Input shape + validation rules
│   │       └── update-user.dto.ts
│   │
│   └── auth/                       # Auth feature module
│       ├── auth.module.ts
│       ├── auth.controller.ts
│       ├── auth.service.ts         # Token generation, credential validation
│       ├── jwt.strategy.ts         # Passport JWT strategy
│       └── dto/
│           └── login.dto.ts
│
├── database/                       # Database layer
│   ├── database.module.ts          # TypeORM / Mongoose module config
│   ├── database.providers.ts       # Connection factory providers
│   └── entities/                   # Shared / base entities
│
└── interfaces/                     # Shared TypeScript interfaces & types
    └── user.interface.ts
```

---

## Key Architectural Decisions

### Modular vertical slices
Each feature (e.g. `users`, `auth`) lives in its own module directory and owns its controller, service, entity, and DTOs. Modules are registered in `AppModule`, keeping concerns isolated and independently testable.

### Separation of layers
- **Controllers** handle HTTP concerns only (routing, request parsing, response shaping).
- **Services** contain all business logic and are injected via NestJS's DI container.
- **DTOs** define and validate the shape of incoming data using `class-validator`.
- **Entities** represent the persistence model, kept separate from the API contract.

### Shared `common/` layer
Cross-cutting concerns — guards, interceptors, decorators, utilities — live in `common/` so they can be reused across any module without creating circular dependencies.

### Config isolation
Environment variables are never accessed directly in business code. A `ConfigService` wrapper in `src/config/` provides typed, centralised access to all configuration values.

---

## Tech Stack

| | |
|---|---|
| Runtime | Node.js |
| Framework | NestJS 11 |
| Language | TypeScript 5 |
| HTTP Adapter | Express (via `@nestjs/platform-express`) |
| Reactive | RxJS 7 |
| Linting | ESLint 9 (flat config) + `typescript-eslint` |
| Formatting | Prettier |
| Testing | Jest + ts-jest + Supertest |

---

## Getting Started

**Install dependencies**
```bash
npm install
```

**Run in development**
```bash
# standard
npm run start

# watch mode (auto-reload)
npm run start:dev

# debug mode
npm run start:debug
```

**Run in production**
```bash
npm run build
npm run start:prod
```

---

## Testing

```bash
# unit tests
npm run test

# unit tests in watch mode
npm run test:watch

# end-to-end tests
npm run test:e2e

# coverage report
npm run test:cov
```

---

## License

[MIT](https://opensource.org/licenses/MIT)
