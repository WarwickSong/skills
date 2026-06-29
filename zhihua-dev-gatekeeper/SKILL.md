---
name: "zhihua-dev-gatekeeper"
description: "Routes Zhihua's coding work through project docs, mature-package checks, tests, and delivery rules. Invoke before non-trivial code changes."
---

# Zhihua Dev Gatekeeper

This is the default entry skill for non-trivial development work in Zhihua's projects under `C:\Coding`, especially `Private/JobVideo_Platform` and `Public/NF-SafetyHub`.

Use this skill before implementing features, fixing bugs, changing architecture, adding dependencies, modifying deployment behavior, or updating production-facing workflows.

## Core Principle

Do not jump straight into code. First understand the project intent, current phase, existing patterns, available dependencies, tests, and delivery constraints.

Prefer:

1. Existing project code and utilities
2. Existing dependencies already present in the project
3. Mature external packages with clear justification
4. Small custom code only when the above options are unsuitable

## Project Routing

### JobVideo Platform

When working in `Private/JobVideo_Platform`:

1. Read `README.md` for product and stack overview.
2. Read `产品研发控制/00-研发控制入口.md` before planning meaningful changes.
3. For feature work, read the relevant files under:
   - `03-功能定义/`
   - `05-前端规划/`
   - `06-接口契约/`
   - `07-数据模型/`
   - `08-验收与测试/`
   - `10-Agent协作控制/`
4. Respect the existing FastAPI + SQLAlchemy + Vue 3 + Vite architecture.
5. For backend behavior changes, prefer adding or updating pytest coverage in `jobvideo_backend/tests/`.
6. For frontend changes, verify the affected Vue view/component and the Vite build path.

### NF-SafetyHub

When working in `Public/NF-SafetyHub`:

1. Read `README.md` for architecture, commands, and deployment model.
2. Read `产品研发控制/SafetyHub当前开发进展和下一步规划.md` before changing scope, production behavior, concurrency, relay, APIKey governance, Docker, or deployment.
3. Treat the current automated test baseline as a production safety asset.
4. For relay, streaming, header policy, APIKey, archive/audit, concurrency, and production config changes, add or update focused pytest coverage.
5. Do not expand paused phases unless the user explicitly asks.
6. Preserve transparent proxy invariants and production deployment constraints.

## Mandatory Workflow

For non-trivial changes:

1. Identify the target project and affected layer.
2. Load the relevant project control documents.
3. Search existing code for similar implementation patterns.
4. Invoke the mature-package-first mindset before writing new abstractions.
5. Decide whether the task needs one of the companion skills:
   - `mature-package-first-dev`
   - `fastapi-sqlalchemy-tdd`
   - `vue-vite-product-ui-review`
   - `safetyhub-production-verification`
   - `project-doc-sync`
6. Make the smallest coherent change.
7. Run the most specific reasonable validation first.
8. Update documentation when behavior, APIs, data models, deployment, or project status changes.

## Companion Skill Selection

Use `mature-package-first-dev` when:

- A new helper, abstraction, parser, queue, client, scheduler, validator, retry layer, UI utility, or deployment script is being considered.
- A new dependency might be introduced.
- The implementation feels like something a mature package or existing project code may already solve.

Use `fastapi-sqlalchemy-tdd` when:

- Changing FastAPI routes, schemas, ORM models, database access, auth, state machines, APIKey logic, relay logic, or backend tests.

Use `vue-vite-product-ui-review` when:

- Changing JobVideo Vue pages, navigation, forms, upload flows, profile/company pages, video feed/detail pages, or user-facing UI states.

Use `safetyhub-production-verification` when:

- Changing SafetyHub concurrency, Docker, PostgreSQL, upstream relay, streaming, archive queues, APIKey providers, production config, or offline deployment.

Use `project-doc-sync` when:

- Code changes affect product behavior, API contracts, data models, deployment steps, acceptance criteria, or current project status.

## Output Expectations

When planning or reporting development work, include:

- Target project and affected layer
- Existing pattern or dependency reused
- Whether a new package was considered or rejected
- Test or validation path
- Documentation updates, if needed

Keep reports concise and operational.
