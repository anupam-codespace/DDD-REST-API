# DDD-REST-API

This repository documents a practical way to structure a **maintainable Go service (modular monolith)** before introducing microservices.

## Goals

- Keep domain logic independent from frameworks and databases.
- Make code easy to test and evolve.
- Create clear module boundaries that can later be extracted into microservices.

## Recommended service shape (before microservices)

Build one deployable service, but split code by **domain boundaries** (bounded contexts), not by technical layers alone.

```text
/cmd/api/main.go
/internal
  /platform             # shared technical concerns (config, logger, db, http)
  /<context-a>
    /domain             # entities, value objects, domain services, domain errors
    /application        # use cases, command/query handlers, DTO mapping
    /infrastructure     # repository implementations, external clients
    /interfaces/http    # handlers, request/response models, routing
  /<context-b>
    /domain
    /application
    /infrastructure
    /interfaces/http
/pkg                    # optional: truly reusable libraries
```

## Dependency direction

Enforce inward dependencies:

- `interfaces` -> `application` -> `domain`
- `infrastructure` depends on `domain` contracts (interfaces), not the other way around.
- `domain` depends on nothing outside the standard library where possible.

## Practical conventions

- Define repository interfaces in `domain` or `application`; implement them in `infrastructure`.
- Keep HTTP/database models separate from domain models.
- Use constructors to inject dependencies explicitly.
- Use transactions at the application/use-case boundary.
- Keep cross-context calls behind interfaces or application services.

## Testing strategy

- **Domain tests**: pure unit tests (fast, no mocks if possible).
- **Application tests**: use-case tests with fakes/stubs for repositories.
- **Interface/HTTP tests**: handler tests for validation and mapping.
- **Infrastructure tests**: focused integration tests for DB/external clients.

## When to split into microservices

Extract a bounded context only when you have clear signals:

- Independent scaling/deployment needs.
- Team ownership boundaries.
- Runtime isolation requirements.
- Well-defined APIs/events already in place.

If these signals are weak, keep the modular monolith and improve internal boundaries first.
