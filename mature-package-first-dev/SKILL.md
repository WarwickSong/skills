---
name: "mature-package-first-dev"
description: "Checks existing code, current dependencies, and mature packages before custom implementation. Invoke before building new technical capability."
---

# Mature Package First Development

## 中文速览

- 用途：在写新能力或加依赖前，先检查已有代码、已有依赖、框架能力和成熟包，避免重复造轮子。
- 适用：新增工具函数、抽象层、解析器、客户端、部署脚本、依赖包或基础设施能力前。
- 不适用：明确的小改动、纯文案修改、已经确定复用现有实现且不新增技术能力的场景。

This skill prevents unnecessary custom code and repeated wheel-building during development. It is domain-profile aware, not project-specific. Use `zhihua-dev-profile` as the profile source.

Use it before implementing a new technical capability, adding a helper module, designing infrastructure, creating deployment scripts, writing parsers/validators/clients, or introducing a new dependency.

This skill owns package and reuse decisions only. Use domain skills for implementation details, `code-runtime-env` for installation or command execution, and `project-doc-sync` when dependency or behavior changes affect authoritative docs.

## Decision Order

Always evaluate options in this order:

1. Existing project code, utilities, scripts, and documented patterns
2. Current dependencies already installed in the project
3. Framework-native features or mature external packages
4. Small custom implementation

Custom implementation is acceptable only when it is simpler, safer, more maintainable, or avoids unacceptable deployment/security costs.

## Profile-Aware Scope

Use `zhihua-dev-profile` to identify relevant domains without maintaining a separate domain list here. Be especially strict when package choices affect production-sensitive backend paths, deployment/offline delivery, auth/security, persistent data, or user-facing frontend bundle size.

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

If the decision changes project setup, runtime assumptions, API behavior, or deployment instructions, involve `project-doc-sync` after implementation.

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
