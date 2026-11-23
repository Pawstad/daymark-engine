# Daymark Backend — Technology Stack

**Document Version:** 1.0  
**Status:** Approved  
**Repository:** `daymark-backend`  
**Scope:** MVP  
**Architecture:** Modular Monolith  
**Last Updated:** 2026-11-23

---

# 1. Purpose

This document defines the technology stack, architectural approach, project structure, infrastructure integrations, and technical conventions for the Daymark Backend Engine.

The backend is responsible for providing the central application logic and services consumed by the Daymark Mobile and Web applications.

Its primary responsibilities include:

- Business logic and business rule enforcement.
- User authentication and authorization.
- Habit management.
- Schedule management.
- Focus session management.
- Habit completion validation.
- Reminder processing.
- Progress calculation and reporting.
- Data persistence and integrity.
- REST API exposure.
- Background job processing.

This document serves as the technical reference for backend implementation and maintenance.

## 1.1 Technology Selection Principles

Technology decisions must prioritize:

1. Maintainability and simplicity.
2. Security and data integrity.
3. Clear separation of responsibilities.
4. Testability.
5. Reliability.
6. Ecosystem maturity and community support.
7. Compatibility with AI-assisted development.
8. Avoidance of premature complexity.

---

# 2. Technology Stack Overview

| Category | Technology | Purpose |
|---|---|---|
| Programming Language | Go (Golang) | Backend development |
| Architecture | Modular Monolith | Application architecture |
| HTTP Router | Chi | HTTP routing |
| API Architecture | REST API | Client-server communication |
| API Specification | OpenAPI 3.x | API contract |
| API Documentation | Swagger UI | Interactive API documentation |
| API Code Generation | oapi-codegen | Generate Go API-related code |
| Database | PostgreSQL | Primary data storage |
| Database Driver | pgx | PostgreSQL connectivity |
| Query Generation | sqlc | Type-safe SQL code generation |
| Database Migration | Goose | Database schema versioning |
| Cache | Redis | Caching and temporary data |
| Redis Client | go-redis | Redis integration |
| Background Jobs | River | Persistent background processing |
| Authentication | Opaque Access & Refresh Tokens | Session-based authentication |
| Password Hashing | Argon2id | Secure password storage |
| Validation | go-playground/validator | Request and configuration validation |
| Logging | Go slog | Structured logging |
| Configuration | caarlos0/env | Environment configuration |
| Containerization | Docker | Application containerization |
| Local Infrastructure | Docker Compose | Local service orchestration |
| Version Control | Git | Source control |
| Repository Hosting | GitHub | Collaboration and repository management |
| CI/CD | GitHub Actions | Automated validation and workflows |

All dependency versions must be evaluated against current stable releases during repository initialization. Versions must be pinned and reviewed before adoption.

---

# 3. Backend Architecture

## 3.1 Architecture Pattern

Daymark Backend uses a **Modular Monolith** architecture.

The application runs as one logical backend system while its internal codebase is divided into business modules.

Each module owns its related business logic and exposes clearly defined interfaces to other modules.

The architecture is intended to provide:

- Clear business boundaries.
- Independent feature development.
- Easier testing and maintenance.
- Controlled dependencies between modules.
- A straightforward deployment model.

Microservices are not part of the MVP architecture.

## 3.2 Business Modules

The initial backend modules are:

| Module | Responsibility |
|---|---|
| Auth | Registration, login, logout, token management, and sessions. |
| User | User profile and account information. |
| Habit | Habit creation, configuration, frequency, and lifecycle. |
| Schedule | Personal schedule and activity planning. |
| Focus | Pomodoro and focus session lifecycle. |
| Completion | Habit completion records and completion validation. |
| Reminder | Reminder configuration and notification scheduling. |
| Progress | Habit consistency, focus statistics, and progress summaries. |

Modules may be expanded or reorganized when justified by actual business requirements.

## 3.3 Application Entry Points

The backend has two primary entry points:

**API Server**

Responsible for receiving HTTP requests and exposing REST API endpoints.

**Background Worker**

Responsible for executing asynchronous and scheduled jobs through River.

Both entry points share the same business modules and infrastructure packages.

They may run as separate processes or containers.

---

# 4. Backend Project Structure

The backend follows a feature-oriented Modular Monolith structure.

```text
daymark-backend/
│
├── cmd/
│   ├── api/
│   │   └── main.go
│   │
│   └── worker/
│       └── main.go
│
├── config/
│   └── config.go
│
├── db/
│   ├── migrations/
│   └── queries/
│
├── internal/
│   │
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── handler.go
│   │   │   ├── service.go
│   │   │   ├── repository.go
│   │   │   ├── model.go
│   │   │   ├── dto.go
│   │   │   ├── mapper.go
│   │   │   └── errors.go
│   │   │
│   │   ├── user/
│   │   ├── habit/
│   │   ├── schedule/
│   │   ├── focus/
│   │   ├── completion/
│   │   ├── reminder/
│   │   └── progress/
│   │
│   ├── middleware/
│   │   ├── authentication.go
│   │   ├── authorization.go
│   │   ├── cors.go
│   │   ├── rate_limit.go
│   │   └── request_id.go
│   │
│   ├── shared/
│   │   ├── errors/
│   │   ├── response/
│   │   ├── pagination/
│   │   └── validation/
│   │
│   └── platform/
│       ├── database/
│       ├── cache/
│       ├── jobs/
│       ├── logger/
│       └── clock/
│
├── api/
│   └── openapi.yaml
│
├── scripts/
├── tests/
│
├── .env
├── .env.example
├── .gitignore
├── Dockerfile
├── docker-compose.yml
├── go.mod
└── go.sum
```

The `vendor/` directory is optional. It should only be generated when dependency vendoring is explicitly required.

## 4.1 Directory Responsibilities

### `cmd/`

Contains application entry points.

- `cmd/api/`: API server initialization.
- `cmd/worker/`: River worker initialization.

Entry points should focus on application startup, dependency initialization, and graceful shutdown.

They should not contain business logic.

### `config/`

Contains application configuration loading and validation.

Examples:

- Application environment.
- HTTP server configuration.
- PostgreSQL configuration.
- Redis configuration.
- Authentication configuration.
- River configuration.
- Logging configuration.

Configuration values are loaded from environment variables.

### `db/`

Contains database resources.

- `migrations/`: Goose migration files.
- `queries/`: SQL queries consumed by sqlc.

Generated sqlc code should be placed in a designated package, such as `internal/platform/database/`, based on the sqlc configuration.

### `internal/modules/`

Contains business modules.

Each module owns its business rules and related components.

Example:

```text
internal/modules/habit/
├── handler.go
├── service.go
├── repository.go
├── model.go
├── dto.go
├── mapper.go
└── errors.go
```

| Component | Responsibility |
|---|---|
| Handler | Handles HTTP requests and responses. |
| Service | Executes business logic and business rules. |
| Repository | Handles persistence operations. |
| Model | Defines domain-related data structures. |
| DTO | Defines API request and response structures. |
| Mapper | Converts between domain models and DTOs. |
| Errors | Defines module-specific errors. |

Not every module must contain every file. Add components only when necessary.

### `internal/middleware/`

Contains HTTP middleware shared across endpoints.

Examples:

- Authentication.
- Authorization.
- CORS.
- Rate limiting.
- Request ID.
- HTTP request logging.

Middleware must not contain feature-specific business logic.

### `internal/shared/`

Contains reusable components shared across multiple business modules.

Examples:

- Common application errors.
- Standard API response structures.
- Pagination utilities.
- Generic validation helpers.

Avoid turning this directory into a general-purpose utility dumping ground.

### `internal/platform/`

Contains infrastructure implementations and technical integrations.

| Directory | Responsibility |
|---|---|
| database/ | Database connections, transaction management, and sqlc integration. |
| cache/ | Redis client and caching utilities. |
| jobs/ | River client, job registration, and worker infrastructure. |
| logger/ | Application logging setup. |
| clock/ | Time abstraction for deterministic testing. |

Infrastructure implementations must not own business rules.

---

# 5. Dependency and Layering Rules

The backend must maintain clear dependencies between components.

## 5.1 General Rules

1. Handler communicates with Service.
2. Service executes business logic and depends on repository abstractions.
3. Repository handles persistence and database queries.
4. Business modules must not freely access another module's internal repository implementation.
5. Shared components must not depend on business modules.
6. Platform packages provide infrastructure implementations.
7. Business rules must remain independent of HTTP and database implementation details.
8. Avoid unnecessary interfaces and excessive abstraction.

## 5.2 Module Communication

Modules should communicate through clearly defined interfaces or service contracts.

For example:

If the Reminder module needs information about a Habit, it should interact through an explicitly defined contract rather than accessing the Habit module's internal repository directly.

The goal is to preserve module ownership and avoid tightly coupled implementations.

## 5.3 Helper and Utility Placement

Do not create a global `utils/` directory by default.

Use the following placement rules:

| Component Type | Location |
|---|---|
| Habit-specific helper | `modules/habit/` |
| Authentication-specific helper | `modules/auth/` |
| Shared generic component | `shared/` |
| Database helper | `platform/database/` |
| Redis helper | `platform/cache/` |
| HTTP middleware | `middleware/` |
| River integration | `platform/jobs/` |

Every helper should have a clear responsibility and ownership.

---

# 6. Database

## 6.1 PostgreSQL

PostgreSQL is the primary source of truth for Daymark.

It stores:

- User accounts.
- Authentication sessions.
- Habits and habit categories.
- Schedules.
- Focus sessions.
- Habit completion records.
- Reminder records.
- Progress-related data.

## 6.2 Database Technologies

| Technology | Responsibility |
|---|---|
| PostgreSQL | Primary relational database |
| pgx | PostgreSQL driver and connection management |
| sqlc | Type-safe database access code generation |
| Goose | Database migration management |

## 6.3 Database Conventions

- Use UUID identifiers for primary keys.
- Use `TIMESTAMPTZ` for timestamps representing actual moments in time.
- Use `DATE` for date-only business concepts.
- Define foreign key constraints.
- Use database transactions for operations requiring atomicity.
- Create indexes based on actual query patterns.
- Apply schema changes through Goose migrations.
- Use sqlc for type-safe database access.
- Avoid unnecessary database queries and N+1 access patterns.

Database constraints should enforce important data integrity requirements wherever appropriate.

## 6.4 Time Handling

Store actual timestamps consistently and distinguish them from date-only concepts.

Examples:

- Focus session start and end: `TIMESTAMPTZ`.
- Schedule start and end: `TIMESTAMPTZ` when representing specific moments.
- Habit occurrence date: `DATE` where appropriate.

Timezone interpretation must follow the business meaning of each feature.

Do not assume that every date or timestamp represents the same kind of time.

---

# 7. Caching

## 7.1 Redis

Redis is used as a supporting infrastructure component, not as the primary source of truth.

Potential use cases:

- Frequently accessed data.
- Authentication lookup caching.
- Rate limiting.
- Temporary application state.
- Short-lived coordination data.

## 7.2 Redis Integration

Use `go-redis` as the Go Redis client.

Redis operations should be encapsulated inside the platform cache package.

Business modules should not depend directly on Redis implementation details.

## 7.3 Caching Strategy

Use cache-aside where appropriate:

1. Application checks Redis.
2. If data exists, return cached data.
3. If data is missing, retrieve it from PostgreSQL.
4. Store the result in Redis with an appropriate expiration.
5. Invalidate or update the cache when underlying data changes.

Caching should be introduced based on actual access patterns and performance requirements.

Do not cache every database query by default.

---

# 8. Authentication and Authorization

## 8.1 Authentication Strategy

Daymark uses opaque access tokens and refresh tokens.

The authentication system must support:

- Registration.
- Login.
- Logout.
- Token expiration.
- Refresh token rotation.
- Session revocation.
- Refresh token reuse detection.
- Session management.
- Logout-all functionality.

## 8.2 Token Security

- Generate tokens using cryptographically secure randomness.
- Store token hashes rather than plaintext tokens in persistent storage.
- Use short-lived access tokens.
- Use longer-lived refresh tokens with rotation.
- Revoke compromised or reused refresh token families.
- Apply rate limiting to authentication endpoints.
- Never log raw access or refresh tokens.
- Validate token expiration and revocation status.

Redis may be used as a token lookup cache, while PostgreSQL remains the persistent source of truth.

## 8.3 Password Security

Use Argon2id for password hashing.

Passwords must never be stored in plaintext or reversible encrypted form.

## 8.4 Authorization

Use role-based authorization where necessary and enforce resource ownership checks.

A user must not be able to access or modify another user's private habits, schedules, focus sessions, or related records by changing an identifier.

Authentication and authorization must be enforced on the backend, regardless of client-side restrictions.

---

# 9. Background Jobs

## 9.1 River

River is the approved background job processing library.

It uses PostgreSQL-backed job persistence and is suitable for Daymark's initial infrastructure.

Potential use cases:

- Scheduled reminders.
- Email verification.
- Password reset notifications.
- Expired session cleanup.
- Deferred application operations.

## 9.2 Job Processing Rules

- Jobs must be designed to be idempotent.
- Retry policies must be explicitly configured.
- Failed jobs must be observable.
- Jobs must have appropriate timeout and retry limits.
- Duplicate execution must not produce duplicate business effects.
- Job payloads should contain identifiers and necessary metadata rather than unnecessary sensitive data.

Where supported by the selected River integration, enqueue jobs within the same database transaction as the associated business operation.

River provides durable job processing but does not guarantee exactly-once execution.

Application-level idempotency remains necessary.

## 9.3 Worker Execution

The worker has its own entry point:

```text
cmd/worker/main.go
```

It may run as a separate Docker container or process while sharing the same backend codebase.

## 9.4 Reminder Processing

The Reminder module owns reminder business rules.

The platform jobs package owns River integration.

Reminder processing should:

1. Identify the reminder event.
2. Schedule or enqueue the appropriate job.
3. Validate that the reminder is still relevant when processed.
4. Apply idempotency protection.
5. Execute the configured delivery mechanism.
6. Record the result when appropriate.

The notification delivery provider remains a separate integration concern.

---

# 10. API Standards and Documentation

## 10.1 REST API

The backend exposes REST APIs consumed by the Daymark Mobile and Web applications.

API conventions:

- Use resource-oriented endpoints.
- Use appropriate HTTP methods and status codes.
- Apply consistent request and response structures.
- Validate request payloads.
- Use pagination for collection endpoints where appropriate.
- Return consistent error responses.
- Version APIs when breaking changes require it.
- Enforce authentication and authorization on protected endpoints.

## 10.2 OpenAPI

OpenAPI 3.x is the official API specification format and the single source of truth for Daymark's API contract.

The specification is maintained in:

```text
api/openapi.yaml
```

It defines:

- API endpoints and HTTP methods.
- Request parameters and payload schemas.
- Response schemas and status codes.
- Authentication requirements.
- Common error response structures.
- Reusable schema components.

Use oapi-codegen to generate Go API-related code where appropriate.

The frontend and mobile repositories may generate client types from the same specification.

The specification must remain synchronized with actual backend behavior.

## 10.3 Swagger UI

Swagger UI provides interactive API documentation based on the OpenAPI specification.

It allows developers to:

- Browse available API endpoints.
- Inspect request parameters and response schemas.
- Understand authentication requirements.
- Execute API requests directly from the documentation interface.
- Inspect API responses and error structures.

Recommended endpoints:

| Endpoint | Purpose |
|---|---|
| `/api/docs` | Interactive Swagger UI documentation |
| `/api/openapi.yaml` | OpenAPI specification file |

Implementation rules:

1. Swagger UI must render documentation from the maintained OpenAPI specification.
2. OpenAPI remains the single source of truth.
3. Documentation must stay synchronized with actual backend behavior.
4. Protected endpoints must document their authentication requirements.
5. Swagger UI must not expose production secrets or sensitive internal information.
6. Interactive documentation should be restricted or disabled in production according to deployment security requirements.
7. The documentation interface must not bypass normal authentication or authorization controls.

Swagger UI is a documentation interface, not an alternative API implementation or authorization mechanism.

---

# 11. Configuration Management

## 11.1 Environment Variables

Environment configuration files are located in the repository root.

```text
daymark-backend/
├── .env
├── .env.example
├── config/
└── ...
```

Rules:

- `.env` contains local environment values and must not be committed.
- `.env.example` documents required environment variables without real credentials.
- Production credentials must be provided through the deployment environment or secret management system.
- Configuration must be validated when the application starts.
- Never hardcode credentials or sensitive configuration in source code.

## 11.2 Configuration Loading

Use `caarlos0/env` for environment variable parsing.

The configuration package is responsible for:

- Reading environment variables.
- Applying appropriate defaults.
- Validating required configuration.
- Providing typed configuration structures.
- Preventing invalid application startup configurations.

Example configuration categories:

- HTTP server.
- PostgreSQL.
- Redis.
- Authentication.
- River.
- Logging.
- Application environment.

---

# 12. Validation and Error Handling

## 12.1 Validation

Use `go-playground/validator` for request and configuration validation where appropriate.

Validation should distinguish between:

- Request format validation.
- Domain business rule validation.
- Database integrity constraints.

Business rules must be validated within the relevant service or domain logic, not exclusively through DTO validation.

## 12.2 Error Handling

Use a consistent application error structure.

Errors should distinguish between:

- Validation errors.
- Authentication errors.
- Authorization errors.
- Resource not found.
- Conflict errors.
- Business rule violations.
- Internal server errors.

Internal error details must not be exposed to clients.

Sensitive information such as SQL statements, credentials, and stack traces must not appear in public API responses.

---

# 13. Logging and Observability

## 13.1 Logging

Use Go's standard `log/slog` package for structured logging.

Logs should include relevant context:

- Timestamp.
- Log level.
- Request ID.
- Operation or module.
- Error context.
- Relevant execution metadata.

Do not log:

- Plaintext passwords.
- Raw access tokens.
- Raw refresh tokens.
- Sensitive personal information.
- Secrets or credentials.

## 13.2 Observability

Observability should be introduced incrementally.

Recommended capabilities:

- Structured application logging.
- Request ID and correlation identifiers.
- Error monitoring through Sentry or an equivalent service.
- OpenTelemetry instrumentation where useful.
- Metrics for API latency, error rates, and background job processing.

Observability must not expose sensitive user data.

---

# 14. Containerization

## 14.1 Docker

Docker is used to provide consistent backend application environments.

The backend should support containerized execution for:

- API server.
- Background worker.

The same application image may be reused with different startup commands.

## 14.2 Docker Compose

Docker Compose manages local development services.

Initial services:

- Backend API.
- Background worker.
- PostgreSQL.
- Redis.

The API and worker should share the same application configuration conventions while supporting different process-specific settings.

Production deployment configuration must be managed separately from local development configuration.

---

# 15. API and Backend Testing

Testing must validate requirements and expected behavior, not simply reproduce the current source code implementation.

## 15.1 Unit Testing

Use Go's standard testing package.

Unit tests should cover:

- Business logic.
- Business rule validation.
- Authentication logic.
- Habit completion criteria.
- Focus session state transitions.
- Reminder processing rules.
- Progress calculations.

## 15.2 Repository Integration Testing

Repository tests should validate:

- SQL query behavior.
- Database constraints.
- Transaction integrity.
- Filtering and pagination.
- Data persistence.
- Database error handling.

## 15.3 Handler and API Testing

Handler tests should cover:

- Request validation.
- Response structure.
- HTTP status codes.
- Authentication requirements.
- Authorization and ownership checks.
-