---
name: "fastapi-sqlalchemy-tdd"
description: "Guides FastAPI and SQLAlchemy changes with contract-aware pytest coverage. Invoke for backend routes, schemas, models, DB, auth, and APIs."
---

# FastAPI SQLAlchemy TDD

This skill guides Python backend development in projects that use FastAPI, SQLAlchemy, Pydantic, pytest, or adjacent patterns. It is based on `zhihua-dev-profile`, not a single repository.

Use it for route changes, schemas, ORM models, database migrations, auth flows, state machines, API contracts, relay/proxy behavior, governance logic, storage, archive/audit logic, and backend bug fixes.

## Inferred Backend Tendencies

- API behavior should be contract-aware.
- Data model and state transitions should be explicit.
- Tests should protect meaningful behavior, especially regressions and failure paths.
- Existing module structure should be reused before new abstractions are introduced.
- Production implications matter for backend infrastructure.

## Generic Backend Workflow

When changing a FastAPI/SQLAlchemy backend:

1. Read repository instructions and overview docs.
2. Identify authoritative API, data model, feature, or acceptance docs if present.
3. Inspect adjacent modules before designing a new shape.
4. Reuse the existing project pattern for routes, schemas, models, services, dependencies, middleware, and tests.
5. Apply `mature-package-first-dev` before adding dependencies or infrastructure helpers.
6. Add or update focused pytest coverage for behavior changes.
7. Use `code-runtime-env` before running pytest, migrations, local services, or installing backend dependencies.
8. Run the smallest relevant validation first.
9. Use `project-doc-sync` if contracts, data models, behavior, or deployment changed.

## Common Patterns to Detect

Look for the project's existing equivalents of:

- `models.py`, `schemas.py`, `routes.py`, `services.py`
- FastAPI dependency injection and auth dependencies
- SQLAlchemy session management and migration scripts
- Pydantic request/response schema conventions
- Test fixtures, API clients, and test utility modules
- Middleware and runtime infrastructure
- Archive/audit/logging conventions

## Test Strategy

For each behavior change, decide the smallest useful test:

- Route contract test for request/response behavior
- Service/unit test for business rules
- Persistence test for model/schema changes
- Regression test for a fixed bug
- Failure-path test for auth, permissions, invalid states, queue limits, upstream errors, or config errors

Use red-green-refactor when feasible:

1. Write or adjust the test to describe the expected behavior.
2. Confirm it fails when practical.
3. Implement the minimal fix.
4. Confirm the test passes.
5. Run adjacent tests if the change touches shared code.

## Production-Sensitive Backend Work

For relay/proxy, APIKey, concurrency, archive/audit, Docker, PostgreSQL, high-load, or deployment-sensitive changes:

- Identify production invariants before coding.
- Prefer focused regression tests.
- Consider connection pools, queue limits, backpressure, timeouts, secret handling, and deployment packaging.
- If the project involves production-sensitive backend or AI-infrastructure behavior, consider `production-backend-verification`.
- If Docker/deployment artifacts must be created or changed, involve `docker-oneclick-packager` after behavior-level verification.

## Backend Change Checklist

- Existing route/service/model pattern checked
- Existing dependency capability checked before custom code
- API contract respected or documented
- Data model change handled safely
- Error response behavior considered
- Permission/state transition behavior considered
- Tests added or updated when behavior changes
- Docs updated through `project-doc-sync` when needed

## Avoid

- Adding a new framework or large dependency without a mature-package decision note
- Mixing persistence, request parsing, and business rules in one route when adjacent code separates them
- Changing public API behavior without updating contracts and tests
- Weakening production safeguards for local convenience
