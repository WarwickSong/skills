---
name: "project-doc-sync"
description: "Keeps project control and delivery docs aligned with code changes. Invoke after changes affect behavior, APIs, data, tests, or deployment."
---

# Project Doc Sync

This skill keeps code, project-control documents, acceptance criteria, and delivery manuals aligned. It is not tied to a specific repository layout. Use `zhihua-dev-profile` for long-term documentation preferences.

Use it after code changes that affect product behavior, API contracts, data models, state machines, permissions, tests, deployment, runtime configuration, production readiness, or project status.

## Core Principle

Documentation is part of the system. Update the authoritative document, not every document.

Avoid duplicating the same fact across many files. Prefer the project's existing authority-source rules.

## Generic Documentation Discovery

For any project, look for authoritative docs such as:

- README or overview docs
- Product requirements, PRDs, specs, phase plans, task boards
- Architecture and code-structure docs
- API contracts and schema docs
- Data model and migration docs
- Frontend/page/UX plans
- Acceptance criteria and testing guidance
- ADRs or decision records
- Deployment, operations, Docker, offline/intranet, and runbooks
- Agent collaboration or project-control protocols

If the project defines a documentation authority map, follow it.

If no docs exist, do not invent a documentation system unless requested; instead, briefly report that no doc update target was found.

## Inferred Documentation Tendencies

Based on `zhihua-dev-profile`, Zhihua's work tends to benefit from:

- Clear authoritative sources for product behavior, APIs, data models, and deployment
- Concise status and phase tracking
- Explicit acceptance and testing notes
- Delivery/runbook updates when deployment behavior changes
- Keeping future plans separate from completed work

These tendencies apply generally. If future work reveals new documentation domains or preferences, update `zhihua-dev-profile` through `zhihua-skill-evolution`.

## When Documentation Is Required

Update docs when changes affect:

- API behavior or endpoint contract
- Database schema or migration expectations
- Auth, permissions, roles, APIKey behavior, or state transitions
- User-facing flow or frontend page behavior
- Test baseline or acceptance criteria
- Deployment steps, env vars, Docker, Nginx, offline packaging, or operational runbooks
- Production capacity, concurrency, queueing, relay, or upstream compatibility
- Project phase status, completed work, or known risks

## When Documentation Is Not Required

Usually skip docs for:

- Pure refactors with no behavior change
- Tiny internal cleanup
- Formatting-only changes
- Test-only refactors that do not change coverage meaning

Still mention that docs were checked and not needed.

## Update Style

- Keep updates concise and factual.
- Preserve the language and style of the existing document.
- Do not rewrite unrelated sections.
- Add dates only when the document already uses dated status entries.
- Prefer updating status bullets and tables over adding long prose.
- Keep future/pending work distinct from completed work.

## Known Project Patterns as Evidence

Current projects show useful documentation patterns such as product control directories, API contract docs, data model docs, acceptance tests, delivery manuals, and production status notes. Treat these as examples of Zhihua's preferred documentation discipline, not as hard-coded paths required for every project.

## Output Expectations

When finishing development work, report:

- Whether docs were required
- Which authoritative docs were updated
- Which docs were intentionally not touched and why
