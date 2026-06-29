---
name: "zhihua-dev-profile"
description: "Defines Zhihua's inferred development domains and preferences. Invoke when routing coding work or evolving custom skills."
---

# Zhihua Development Profile

This skill is the authoritative profile source for Zhihua's (志华) custom development Skill system.

Use it when routing coding work, deciding which companion Skill applies, evaluating whether a new domain should be added, or updating other custom Skills from real-use feedback.

## Core Rule

The profile is inferred from real projects and real usage. It is a long-term development tendency model, not a list of allowed repositories.

Current projects are evidence. They are not boundaries.

## Monotonic Domain Rule

The inferred domain set is monotonic:

- New domains may be added when real work provides evidence.
- Existing domains must not be removed, narrowed, or treated as obsolete unless the user explicitly says so.
- A domain may be marked lower-priority or less active, but it remains part of the historical profile.
- Project-specific facts should be generalized into reusable domain-level rules when possible.

## Current Inferred Domains

- Product-oriented web applications
- FastAPI backend services
- SQLAlchemy-backed persistence and schema evolution
- Contract-aware API development
- Vue/Vite frontend product UI
- Auth, roles, permissions, and state transitions
- Media/file handling and operational storage concerns
- LLM/AI infrastructure and proxy-style services
- Safety, governance, audit, and observability workflows
- Production-sensitive backend systems
- Docker, Nginx, PostgreSQL, deployment, offline/intranet delivery, and production verification
- Agent-assisted development governance and project-control documentation
- Runtime environment selection for safe code execution, tests, builds, scripts, and dependency installation
- Docker one-click packaging and reusable deployment artifact generation

## Current Inferred Preferences

- Prefer mature packages, framework-native features, and existing project patterns before custom code.
- Prefer existing project conventions before new abstractions.
- Prefer small vertical slices with tests over large unverified rewrites.
- Prefer explicit contracts, state machines, and acceptance criteria.
- Prefer production and delivery implications to be considered early, especially for backend infrastructure.
- Prefer authoritative docs instead of duplicated documentation.
- Prefer structured agent workflows that remain light enough for practical iteration.
- Prefer the smallest validation that gives meaningful confidence before broader tests.
- Prefer project-local or managed runtimes over global runtimes for execution and dependency installation.
- Prefer deployment packages that are configurable, reproducible, and safe for multiple services on one server.
- Prefer clear handoffs between Skills: routing, package choice, runtime execution, tests, documentation, and deployment should each be owned by the most specific relevant Skill.

## Current Evidence Log

Evidence should be used to infer domains and preferences, not to hard-code project paths into general Skills.

- A short-video recruiting product suggests product UI, role-based workflows, media handling, FastAPI, SQLAlchemy, Vue/Vite, and API-contract discipline.
- An LLM safety/proxy service suggests production-sensitive backend infrastructure, relay compatibility, APIKey governance, audit/archive, concurrency control, Docker/PostgreSQL, offline deployment, and verification discipline.

## Profile Update Protocol

When updating this profile:

1. State the evidence that triggered the update.
2. Decide whether it adds a domain, refines a preference, or only affects a specific Skill.
3. Preserve the monotonic domain rule.
4. Keep entries concise and reusable across projects.
5. Update routing or companion Skills only when the profile change affects behavior.

## Output Expectations

When this profile is used, mention only the relevant inferred domains and preferences. Do not quote the full profile unless the user asks.
