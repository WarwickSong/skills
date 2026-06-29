---
name: "zhihua-dev-gatekeeper"
description: "Routes Zhihua's coding work through inferred domain profile, mature-package checks, tests, and delivery rules. Invoke before non-trivial code changes."
---

# Zhihua Dev Gatekeeper

This is the default entry skill for non-trivial development work. It is not tied to a single repository. It uses the current codebase plus `zhihua-dev-profile` to choose the right workflow.

Use this skill before implementing features, fixing bugs, changing architecture, adding dependencies, modifying deployment behavior, or updating production-facing workflows.

## Development Profile Source

Use `zhihua-dev-profile` as the authoritative source for inferred domains, preferences, evidence, and monotonic profile updates.

Do not duplicate the full profile here. Mention only the relevant domains and preferences for the current task.

## Repository Discovery

For any project, first infer its local conventions:

1. Look for project-level instructions such as `AGENTS.md`, `README.md`, `.trae/`, `.agents/`, `CLAUDE.md`, or equivalent docs.
2. Identify stack and commands from dependency/config files such as `requirements.txt`, `pyproject.toml`, `package.json`, `Dockerfile`, `docker-compose.yml`, `Makefile`, CI files, and test config.
3. Identify product/control documentation if present, such as PRDs, specs, architecture docs, API contracts, data-model docs, deployment manuals, acceptance tests, or task boards.
4. Search existing code for similar implementation patterns before designing new abstractions.
5. Decide whether the task should use a companion skill.

## Task Weight Routing

Do not force every request through the full workflow.

### Quick Path

Use a lightweight path for:

- Pure explanations or Q&A
- Tiny single-file edits
- Copy changes, wording changes, or non-behavioral docs cleanup
- Small style tweaks with no dependency, API, data, or deployment impact

Quick path still requires respecting local file conventions, but it does not require mature-package analysis, broad doc reading, or full validation.

### Standard Path

Use the standard workflow for behavior changes, feature work, bug fixes, tests, API/data changes, dependency changes, and user-facing UI changes.

### Strict Path

Use the strict path for production-sensitive backend, security, auth, APIKey, data migration, deployment, Docker, offline/intranet delivery, high-concurrency, or LLM/AI infrastructure changes. Strict path requires explicit validation and documentation consideration.

## Mandatory Workflow

For non-trivial changes:

1. Identify the target project, affected layer, and relevant inferred domain(s).
2. Load repository instructions and authoritative docs when present.
3. Search existing code for similar patterns.
4. Apply `mature-package-first-dev` before creating new technical capability or adding dependencies.
5. Choose the most specific companion skill available.
6. Make the smallest coherent change.
7. Run the most specific reasonable validation first.
8. Use `project-doc-sync` when behavior, APIs, data models, deployment, or status changes.

## Companion Skill Selection

Use `mature-package-first-dev` when:

- A helper, abstraction, parser, queue, client, scheduler, validator, retry layer, UI utility, or deployment script is being considered.
- A new dependency might be introduced.
- The implementation feels like something an existing project pattern or mature package may already solve.

Use `agent-dev-control` when:

- Starting a new software project, organizing an existing project for repeated Agent-assisted development, or creating lightweight project-control docs.
- Requirements, API contracts, data models, acceptance criteria, or task status are drifting across sessions.

Use `code-runtime-env` when:

- Running code, tests, scripts, builds, migrations, local services, package managers, or dependency installation commands.
- The correct Python, Node, Conda, container, or project-local runtime is unclear.

Use `fastapi-sqlalchemy-tdd` when:

- Working on Python backend services using FastAPI, SQLAlchemy, Pydantic, pytest, auth, state machines, API contracts, or database-backed behavior.

Use `vue-vite-product-ui-review` when:

- Working on Vue 3 / Vite product UI, forms, navigation, upload/media flows, dashboards, role-based pages, or user-facing states.

Use `production-backend-verification` when:

- The task has production-sensitive traits: LLM/API proxying, transparent relay, streaming, APIKey governance, concurrency gates, archive queues, Docker, PostgreSQL, offline/intranet deployment, or high-concurrency verification.

Use `docker-oneclick-packager` when:

- The user asks to containerize, deploy, create Docker/Compose artifacts, make one-click deployment scripts, or build offline/intranet deployment bundles.
- Deployment packaging needs reusable operator-facing docs, env templates, port/data-dir configurability, or multi-service coexistence.

Use `project-doc-sync` when:

- Code changes affect product behavior, API contracts, data models, deployment steps, acceptance criteria, project status, or operating procedures.

Use `zhihua-skill-evolution` when:

- Real work reveals the inferred profile is incomplete, a domain should be added, a workflow misfires, or a rule should be tuned.

## Current-Project Evidence

Current-project evidence is maintained in `zhihua-dev-profile`. When future projects show new domains, use `zhihua-skill-evolution` to update that profile instead of editing routing rules ad hoc.

## Output Expectations

When planning or reporting development work, include:

- Target project and inferred domain(s)
- Existing pattern or dependency reused
- Whether a new package was considered or rejected
- Test or validation path
- Documentation updates, if needed

Keep reports concise and operational.
