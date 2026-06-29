---
name: "safetyhub-production-verification"
description: "Verifies NF-SafetyHub production-sensitive changes. Invoke for relay, concurrency, Docker, PostgreSQL, APIKey, deployment, and load testing."
---

# SafetyHub Production Verification

This skill protects NF-SafetyHub production readiness.

Use it when changing `Public/NF-SafetyHub` production-sensitive areas: transparent relay, streaming, header policy, APIKey governance, KeyProvider, concurrency limits, request limits, archive queues, shared upstream clients, PostgreSQL, Docker, Nginx, offline deployment, production config, load testing, and operational docs.

## Mandatory Context

Before planning or modifying production-sensitive behavior:

1. Read `README.md`.
2. Read `产品研发控制/SafetyHub当前开发进展和下一步规划.md`.
3. Identify whether the change affects a completed phase, current Stage 6A production verification, or a paused future phase.
4. Do not expand paused phases unless explicitly requested.

## Production Invariants

Preserve these invariants unless the user explicitly approves a design change:

- `/v1/*` transparent relay remains OpenAI-compatible.
- Non-desensitized request bodies should preserve byte-level behavior where documented.
- Streaming SSE behavior remains compatible with mainstream LLM clients.
- Header policy must avoid hop-by-hop and invalid content headers.
- Blocked requests must not consume upstream quota.
- Archive/audit failures should not block the main response path unless the current design says otherwise.
- Admin, health, and runtime endpoints should not be blocked by `/v1/*` concurrency queues.
- Production config must fail safely for missing or weak critical secrets.
- Docker production startup must not use reload mode.

## Verification Planning

For each production-sensitive change, define:

- Affected subsystem
- Expected invariant preserved or improved
- Targeted pytest file(s)
- Manual or integration verification, if needed
- Deployment/offline bundle impact
- Rollback or mitigation path when relevant

## Test Selection

Prefer focused tests first:

- `tests/test_relay.py` for relay behavior
- `tests/test_header_policy.py` for response/request header handling
- `tests/test_api_keys.py` for APIKey and upstream key mapping
- `tests/test_concurrency_limit.py` for request queue behavior
- `tests/test_archive.py` and `tests/test_audit.py` for persistence paths
- `tests/test_health.py` for readiness/liveness
- Admin tests for dashboard/API behavior

Run broader tests only after focused tests pass or when shared production code changed.

## Load and Deployment Readiness

For Stage 6A work, validate or document:

- Worker count and container resources
- `V1_MAX_INFLIGHT`, `V1_MAX_QUEUE_SIZE`, and timeout behavior
- Upstream connection pool settings
- PostgreSQL connection stability
- Archive queue capacity and drop behavior
- Admin responsiveness during `/v1/*` load
- Docker Compose and Nginx config consistency
- Offline deployment package rebuild requirements
- Production secret and logging safety

## Mature Package Rule

Before adding packages to SafetyHub production paths:

1. Check if current dependencies already cover the need.
2. Evaluate Docker and offline deployment impact.
3. Avoid adding heavy dependencies for small runtime behavior.
4. Prefer mature security/crypto libraries over custom security-sensitive code when dependency cost is acceptable.

## Output Expectations

Reports should include:

- Production risk level: low / medium / high
- Invariants checked
- Tests run or recommended
- Deployment docs affected
- Follow-up production verification if runtime-only validation is required
