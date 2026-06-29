---
name: "project-doc-sync"
description: "Keeps Zhihua's project control and delivery docs aligned with code changes. Invoke after changes affect behavior, APIs, data, tests, or deployment."
---

# Project Doc Sync

This skill keeps Zhihua's code, product control documents, acceptance criteria, and delivery manuals aligned.

Use it after code changes that affect product behavior, API contracts, data models, state machines, permissions, tests, deployment, runtime configuration, production readiness, or project status.

## Core Principle

Documentation is part of the system. Update the authoritative document, not every document.

Avoid duplicating the same fact across many files. Prefer the project's existing authority-source rules.

## JobVideo Documentation Map

For `Private/JobVideo_Platform`, start from:

- `产品研发控制/00-研发控制入口.md`

Update the authoritative area:

- Product phase or priority: `00-研发控制入口.md` or `02-阶段规划/`
- Business rule or feature behavior: `03-功能定义/`
- Code structure: `04-代码结构/`
- Frontend page or interaction: `05-前端规划/`
- API path/request/response/error: `06-接口契约/`
- Table, field, index, or constraint: `07-数据模型/`
- Acceptance or regression coverage: `08-验收与测试/`
- Major decision: `09-决策记录/`
- Agent execution status or rules: `10-Agent协作控制/`
- Deployment/runtime: `交付运行手册/`

Respect the authority-source table in `00-研发控制入口.md`.

## SafetyHub Documentation Map

For `Public/NF-SafetyHub`, start from:

- `产品研发控制/SafetyHub当前开发进展和下一步规划.md`
- `README.md`

Update the relevant area:

- Current phase/status: `产品研发控制/SafetyHub当前开发进展和下一步规划.md`
- Feature planning: `产品研发控制/SafetyHub功能定义规划.md`
- Implementation phase planning: `产品研发控制/SafetyHub实现阶段规划.md`
- Testing guidance: `产品研发控制/SafetyHub测试验证指导.md`
- Code architecture: `产品研发控制/SafetyHub代码结构框架规划.md`
- Deployment and operations: `交付运行手册/`
- Docker/offline bundle instructions: `docker离线部署/` and related delivery docs

## When Documentation Is Required

Update docs when changes affect:

- API behavior or endpoint contract
- Database schema or migration expectations
- Auth, permissions, roles, APIKey behavior, or state transitions
- User-facing flow or frontend page behavior
- Test baseline or acceptance criteria
- Deployment steps, env vars, Docker, Nginx, offline packaging
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
- Preserve Chinese project documentation style where the file is Chinese.
- Do not rewrite unrelated sections.
- Add dates only when the document already uses dated status entries.
- Prefer updating status bullets and tables over adding long prose.
- Keep future/pending work distinct from completed work.

## Output Expectations

When finishing development work, report:

- Whether docs were required
- Which authoritative docs were updated
- Which docs were intentionally not touched and why
