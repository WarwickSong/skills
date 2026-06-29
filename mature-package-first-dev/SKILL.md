---
name: "mature-package-first-dev"
description: "Checks existing code, current dependencies, and mature packages before custom implementation. Invoke before building new technical capability."
---

# Mature Package First Development

This skill prevents unnecessary custom code and repeated wheel-building during development. It is domain-profile aware, not project-specific. Use `zhihua-dev-profile` as the profile source.

Use it before implementing a new technical capability, adding a helper module, designing infrastructure, creating deployment scripts, writing parsers/validators/clients, or introducing a new dependency.

## Decision Order

Always evaluate options in this order:

1. Existing project code, utilities, scripts, and documented patterns
2. Current dependencies already installed in the project
3. Framework-native features or mature external packages
4. Small custom implementation

Custom implementation is acceptable only when it is simpler, safer, more maintainable, or avoids unacceptable deployment/security costs.

## Profile-Aware Domains

Zhihua's inferred domains in `zhihua-dev-profile` mean package checks should be especially careful for:

- Python web services, FastAPI, Pydantic, SQLAlchemy, pytest, httpx, async I/O
- API contracts, auth, permissions, state transitions, audit/archive behavior
- Vue/Vite frontend UI, routing, forms, upload/media flows, browser APIs
- LLM/AI infrastructure, proxying, streaming, APIKey governance, observability
- Docker, Nginx, PostgreSQL, offline/intranet delivery, production verification
- Project-control documents, acceptance criteria, deployment manuals, and agent workflows

This domain list is summarized from `zhihua-dev-profile`. If future work reveals new recurring domains, update `zhihua-dev-profile` through `zhihua-skill-evolution`; do not maintain a separate divergent list here.

## Required Checks

### 1. Project-local Search

Search for:

- Similar modules, utilities, service classes, middleware, scripts, tests, and docs
- Existing naming conventions and architectural seams
- Existing validation, retry, queueing, auth, API client, upload, deployment, or test patterns
- Existing project-control or delivery documents that already define the intended approach

### 2. Existing Dependency Check

Inspect dependency files before proposing a new package:

- Python: `requirements.txt`, `pyproject.toml`, lock files, imports in adjacent code
- Node/frontend: `package.json`, lock files, existing imports in source code
- Deployment: `Dockerfile`, `docker-compose.yml`, `Makefile`, shell/PowerShell scripts, CI config

Do not assume a package is available just because it is common.

### 3. Mature Package Evaluation

If a new package may help, evaluate:

- Fit for the exact task
- Maintenance activity and ecosystem trust
- License compatibility
- Security and supply-chain risk
- Runtime footprint and transitive complexity
- Windows/Linux/Docker compatibility
- Offline or intranet deployment impact
- Testability
- Whether the package reduces long-term code ownership

Production-sensitive backend infrastructure requires a stricter bar than product UI convenience.

If the decision is to install, update, or remove a dependency, hand off to `code-runtime-env` before running package-manager commands.

If the dependency affects Docker images, offline/intranet bundles, or deployment artifacts, also involve `docker-oneclick-packager` or `production-backend-verification` as appropriate.

## Decision Template

Before coding, produce a short decision note when the choice is non-obvious:

```text
Mature-package check:
- Inferred domain(s): ...
- Existing project option: ...
- Existing dependency option: ...
- External package option: ...
- Decision: reuse existing / extend dependency / add package / custom code
- Reason: ...
- Validation: ...
```

## Biases

- Prefer framework-native capabilities over custom wrappers.
- Prefer the project's existing style over a theoretically cleaner but inconsistent abstraction.
- Avoid adding packages for small convenience functions.
- Avoid hand-rolling security-sensitive code when a trusted library exists.
- Avoid adding heavy dependencies to production-sensitive paths without strong justification.
- Avoid adding frontend libraries unless they clearly improve UX or reduce substantial complexity.

## Red Flags

Pause and reconsider when:

- The implementation introduces a new abstraction with no tests.
- A new dependency is added but only used once for a trivial operation.
- The package complicates Docker/offline/intranet deployment.
- The package overlaps with already-present framework or dependency capabilities.
- The custom code recreates auth, crypto, retry, queueing, parsing, validation, protocol, or browser behavior poorly.
