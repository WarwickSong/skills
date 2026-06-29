---
name: "fastapi-sqlalchemy-tdd"
description: "Guides FastAPI and SQLAlchemy changes with contract-aware pytest coverage. Invoke for backend routes, schemas, models, DB, auth, and APIs."
---

# FastAPI SQLAlchemy TDD

This skill guides backend development for Zhihua's FastAPI projects, especially JobVideo and NF-SafetyHub.

Use it for route changes, schemas, ORM models, database migrations, auth flows, state machines, relay behavior, APIKey governance, storage, archive/audit logic, and backend bug fixes.

## Principles

- Contract first: understand the API, data model, state machine, and acceptance criteria before coding.
- Tests protect behavior: add or update focused pytest coverage for meaningful backend behavior changes.
- Small vertical slices: prefer one endpoint or behavior path at a time.
- Reuse existing test utilities and fixtures before adding new ones.
- Keep FastAPI dependency injection, Pydantic schemas, SQLAlchemy models, and service logic separated according to existing project patterns.

## JobVideo Backend Workflow

When changing `Private/JobVideo_Platform/jobvideo_backend`:

1. Read the relevant project docs:
   - `产品研发控制/00-研发控制入口.md`
   - `03-功能定义/` for business rules
   - `06-接口契约/` for API shape
   - `07-数据模型/` for table fields and constraints
   - `08-验收与测试/` for expected coverage
2. Inspect adjacent modules under `app/<feature>/`.
3. Reuse the existing module pattern: `models.py`, `schemas.py`, `routes.py`, optional `services.py`.
4. Update or add tests in `jobvideo_backend/tests/`.
5. For database changes, prefer explicit migration/compatibility handling consistent with existing scripts.

## SafetyHub Backend Workflow

When changing `Public/NF-SafetyHub`:

1. Read `README.md` and `产品研发控制/SafetyHub当前开发进展和下一步规划.md`.
2. Identify the affected subsystem:
   - `proxy/` for transparent relay and streaming
   - `engine/` for scanning rules
   - `governance/` for APIKey and providers
   - `middleware/` for request identity, limits, and concurrency
   - `runtime/` for shared clients, queues, and caches
   - `storage/` for persistence
   - `admin/` for admin APIs and static UI
3. Preserve documented invariants, especially transparent relay behavior and production safety boundaries.
4. Add or update focused tests under `tests/`.
5. Prefer `make test` or targeted `pytest tests/test_x.py` validation.

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
- Weakening SafetyHub production safeguards for local convenience
