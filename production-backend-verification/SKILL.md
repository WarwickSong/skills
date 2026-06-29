---
name: "production-backend-verification"
description: "Verifies production-sensitive backend and AI-infra changes. Invoke for relay, concurrency, Docker, PostgreSQL, APIKey, deployment, and load testing."
---

# Production Backend Verification

This skill protects production-sensitive backend and AI-infrastructure work.

Use it when changing production-sensitive areas such as transparent relay, streaming, header policy, APIKey governance, credential-provider integrations, concurrency limits, request limits, archive queues, shared upstream clients, PostgreSQL, Docker, Nginx, offline/intranet deployment, production config, load testing, and operational docs.

## Profile Context

This Skill implements the `production-sensitive backend systems` domain from `zhihua-dev-profile`.

Examples include:

- LLM or API proxy services
- Safety/governance/audit layers
- APIKey or credential-management flows
- High-concurrency FastAPI services
- Docker/PostgreSQL/Nginx production deployments
- Offline or intranet delivery packages

## Mandatory Context

Before planning or modifying production-sensitive behavior:

1. Read repository instructions and overview docs.
2. Identify production, deployment, API, runtime, security, or acceptance docs if present.
3. Identify whether the change affects completed behavior, current production-readiness work, or paused/future scope.
4. Do not expand paused/future scope unless explicitly requested.
5. Use `code-runtime-env` before running tests, load scripts, Docker commands, package managers, or service startup commands.

## Quick Path

Use a lighter path only when the change is clearly low risk:

- Documentation wording that does not change commands or production behavior
- Test naming or small test refactor without behavior change
- Local-only script cleanup that does not affect deployment

For quick-path work, still check the affected file's local context and report why deeper production verification was not needed.

## Production Invariants

Preserve applicable invariants unless the user explicitly approves a design change:

- Transparent relay or proxy paths preserve client compatibility.
- Streaming behavior remains compatible with target clients.
- Header policy avoids hop-by-hop and invalid content headers.
- Blocked or locally handled requests do not consume upstream quota when that is a stated design goal.
- Archive/audit failures should not block the main response path unless the current design says otherwise.
- Admin, health, and runtime endpoints should remain responsive during primary-path load when designed that way.
- Production config fails safely for missing or weak critical secrets.
- Docker production startup must not use development reload mode.

## Verification Planning

For each production-sensitive change, define:

- Affected subsystem
- Expected invariant preserved or improved
- Targeted test file(s)
- Manual or integration verification, if needed
- Deployment/offline bundle impact
- Rollback or mitigation path when relevant

## Test Selection

Prefer focused tests first:

- Relay/proxy tests for transparent forwarding, streaming, and payload compatibility
- Header-policy tests for response/request header handling
- APIKey/auth/governance tests for credential mapping and access control
- Concurrency/request-limit tests for queue behavior and backpressure
- Archive/audit/persistence tests for non-blocking operational records
- Health/readiness tests for process and dependency status
- Admin/runtime tests for operational visibility

Run broader tests only after focused tests pass or when shared production code changed.

## Load and Deployment Readiness

For production-readiness work, validate or document:

- Worker count and container resources
- In-flight, queue, timeout, and backpressure behavior
- Upstream connection pool settings
- PostgreSQL or durable storage stability
- Archive/audit queue capacity and drop behavior
- Admin/runtime responsiveness during primary-path load
- Docker Compose and Nginx config consistency
- Offline or intranet deployment package rebuild requirements
- Production secret and logging safety

## Mature Package Rule

Before adding packages to production-sensitive paths:

1. Check if current dependencies already cover the need.
2. Evaluate Docker and offline/intranet deployment impact.
3. Avoid heavy dependencies for small runtime behavior.
4. Prefer mature security/crypto libraries over custom security-sensitive code when dependency cost is acceptable.
5. Use `mature-package-first-dev` for the dependency decision and `code-runtime-env` for safe installation or execution.

## Packaging and Documentation Handoff

- Use `docker-oneclick-packager` when production verification leads to Docker, Compose, Nginx, env-template, or offline/intranet bundle changes.
- Use `project-doc-sync` when production behavior, deployment steps, runtime config, capacity assumptions, or runbooks change.

## Output Expectations

Reports should include:

- Production risk level: low / medium / high
- Invariants checked
- Tests run or recommended
- Deployment docs affected
- Follow-up production verification if runtime-only validation is required
