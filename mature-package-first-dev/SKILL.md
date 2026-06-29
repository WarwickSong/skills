---
name: "mature-package-first-dev"
description: "Checks existing code, current dependencies, and mature packages before custom implementation. Invoke before building new technical capability."
---

# Mature Package First Development

This skill prevents unnecessary custom code and repeated wheel-building during development.

Use it before implementing a new technical capability, adding a helper module, designing infrastructure, creating deployment scripts, writing parsers/validators/clients, or introducing a new dependency.

## Decision Order

Always evaluate options in this order:

1. Existing project code, utilities, scripts, and documented patterns
2. Current dependencies already installed in the project
3. Mature external packages or framework-native features
4. Small custom implementation

Custom implementation is acceptable only when it is simpler, safer, more maintainable, or avoids unacceptable deployment/security costs.

## Required Checks

### 1. Project-local Search

Search for:

- Similar modules, utilities, service classes, middleware, scripts, tests, and docs
- Existing naming conventions and architectural seams
- Existing validation, retry, queueing, auth, API client, upload, deployment, or test patterns

For JobVideo, check:

- `jobvideo_backend/app/`
- `jobvideo_backend/tests/`
- `jobvideo_frontend/src/`
- `产品研发控制/`
- `交付运行手册/`

For SafetyHub, check:

- `proxy/`, `engine/`, `governance/`, `middleware/`, `runtime/`, `storage/`, `admin/`
- `tests/`
- `产品研发控制/`
- `交付运行手册/`
- Docker and offline deployment scripts

### 2. Existing Dependency Check

Inspect dependency files before proposing a new package:

- Python: `requirements.txt`, imports in adjacent code
- Node/Vue: `package.json`, existing imports in `src/`
- Deployment: `Dockerfile`, `docker-compose.yml`, `Makefile`, shell/PowerShell scripts

Do not assume a package is available just because it is common.

### 3. Mature Package Evaluation

If a new package may help, evaluate:

- Fit for the exact task
- Maintenance activity and ecosystem trust
- License compatibility
- Security and supply-chain risk
- Runtime footprint and transitive complexity
- Windows/Linux/Docker compatibility
- Offline deployment impact
- Testability
- Whether the package reduces long-term code ownership

For SafetyHub, also evaluate production and offline deployment consequences.

For JobVideo, also evaluate simplicity and maintainability for a product still evolving quickly.

## Decision Template

Before coding, produce a short decision note when the choice is non-obvious:

```text
Mature-package check:
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
- Avoid adding heavy dependencies to SafetyHub production paths without strong justification.
- Avoid adding frontend libraries to JobVideo unless they clearly improve UX or reduce complexity.

## Red Flags

Pause and reconsider when:

- The implementation introduces a new abstraction with no tests.
- A new dependency is added but only used once for a trivial operation.
- The package complicates Docker/offline deployment.
- The package overlaps with FastAPI, SQLAlchemy, httpx, pytest, Vue, Vite, or Axios capabilities already present.
- The custom code recreates auth, crypto, retry, queueing, parsing, validation, or protocol behavior poorly.
