---
name: "zhihua-skill-evolution"
description: "Evolves Zhihua's skills from real-use feedback and inferred domain profile. Invoke when domains expand, workflows misfire, or rules need tuning."
---

# Zhihua Skill Evolution

This meta-skill maintains and improves Zhihua's custom Skill system in `C:\Coding\.trae\skills`.

Its job is not merely to patch individual rules. It should infer Zhihua's broader development domains, needs, and preferences from real projects and real usage, then adjust the Skill system around that inferred profile.

## Core Principle

Skills should evolve from evidence.

Current projects and feedback are evidence for a broader development profile, not hard-coded boundaries. The authoritative profile source is `zhihua-dev-profile`.

## Monotonic Domain Rule

The inferred domain set is monotonic:

- New domains may be added when real work provides evidence.
- Existing domains should not be removed, narrowed, or treated as obsolete unless the user explicitly says so.
- A domain may be marked lower-priority or less active, but it remains part of the historical profile.
- Project-specific rules should be generalized into domain-level rules when possible.

## Profile Source

Read and update `zhihua-dev-profile` when feedback changes inferred domains, preferences, or evidence.

Do not maintain a second copy of the full profile in this Skill.

## Feedback Signals

Invoke this skill when the user says or implies:

- "这个 Skill 没触发对"
- "这个规则太项目化了，要更通用"
- "这是我以后经常会做的领域"
- "以后遇到这种情况要先做 X"
- "这个流程太重 / 太轻"
- "这次漏读了某个文档"
- "这次不该新增依赖"
- "这里应该先跑测试 / 不该跑全量测试"
- "这个规则以后要记住"
- "把这次经验沉淀到 Skill 里"
- "优化一下我的 Skill"

Also invoke it after repeated mistakes, repeated manual corrections, or recurring project/domain patterns.

## Evolution Workflow

1. Extract evidence:
   - What happened?
   - Which project or task exposed it?
   - Is it a one-off, a project-specific rule, or evidence of a broader domain/preference?
2. Update `zhihua-dev-profile` if needed:
   - Add new domains monotonically.
   - Add or refine preferences.
   - Generalize current-project facts into reusable domain rules.
3. Locate the right target:
   - Inferred profile source → `zhihua-dev-profile`
   - Routing and task weight → `zhihua-dev-gatekeeper`
   - Project-control documentation setup → `agent-dev-control`
   - Dependency/package decision → `mature-package-first-dev`
   - Runtime environment and command execution safety → `code-runtime-env`
   - Backend test/API/model rule → `fastapi-sqlalchemy-tdd`
   - Frontend UX/page rule → `vue-vite-product-ui-review`
   - Production-sensitive backend / AI infrastructure rule → `production-backend-verification` or a new domain Skill
   - Docker one-click or offline/intranet packaging rule → `docker-oneclick-packager`
   - Documentation synchronization rule → `project-doc-sync`
   - Skill maintenance/evolution rule → this Skill
4. Decide whether to:
   - Edit an existing Skill
   - Add a domain-aware section
   - Split a new focused Skill
   - Do nothing and explain why the feedback should remain situational
5. Make the smallest useful change.
6. Validate that updated Skills still have valid frontmatter and clear trigger descriptions.
7. Summarize the profile implication and the rule change.

## Design Rules

- Keep `description` concise and trigger-oriented.
- Avoid hard-coding current project paths in general-purpose Skills.
- Use current project examples as evidence, not boundaries.
- Put profile/routing rules in `zhihua-dev-gatekeeper`.
- Put package/dependency discipline in `mature-package-first-dev`.
- Put domain-specific execution rules in focused domain Skills.
- Do not duplicate the same rule across many Skills unless necessary for triggering.
- Prefer checklists and decision templates over long essays.
- Preserve existing safeguards unless they conflict with stronger user feedback.

## Anti-Bloat Rules

- If a Skill grows beyond roughly 150 lines, consider moving stable background material into a `references/` file or splitting a focused new Skill.
- Do not paste the full development profile into multiple Skills; reference `zhihua-dev-profile` instead.
- Do not add project-specific paths to general Skills unless the path is itself the long-term convention being encoded.
- Prefer one precise trigger rule over several overlapping examples.
- If two Skills repeat the same checklist, keep it in the more general Skill and make the specific Skill reference it.

## Cross-Skill Consistency Check

When adding or modifying a Skill, check whether it should connect to these existing responsibilities:

- Profile/routing: `zhihua-dev-profile`, `zhihua-dev-gatekeeper`
- Project governance: `agent-dev-control`, `project-doc-sync`
- Dependency choice and execution: `mature-package-first-dev`, `code-runtime-env`
- Backend/API/data: `fastapi-sqlalchemy-tdd`
- Frontend/product UI: `vue-vite-product-ui-review`
- Production/deployment: `production-backend-verification`, `docker-oneclick-packager`

If a new Skill owns a domain that touches one of these responsibilities, add a concise handoff rule rather than duplicating the full workflow.

## Skill Update Protocol

Every Skill update should answer:

1. Evidence: what real project, task, or feedback caused this?
2. Abstraction level: one-off / project-specific / domain-level / profile-level?
3. Impact scope: which Skills should change and which should not?
4. Monotonicity: does this add a domain or refine a preference without removing existing domains?
5. Bloat check: should this be a short rule, reference file, or new focused Skill?

## When to Create a New Skill

Create a new Skill only when:

- The inferred domain has a distinct recurring workflow.
- Adding it to an existing Skill would make that Skill unfocused.
- The user is likely to reuse it across multiple projects or sessions.
- The Skill can be described clearly in one sentence.

Do not create a new Skill for a one-off preference or tiny reminder.

## Safety Rules

- Do not remove inferred domains without explicit user approval.
- Do not weaken production or testing safeguards without explicit user approval.
- Do not add package-specific recommendations without checking current project dependencies and deployment constraints.
- Do not rewrite all Skills just to improve wording.
- Do not modify user project code while maintaining Skills unless requested.

## Output Template

When applying feedback, report:

```text
Skill evolution update:
- Evidence captured: ...
- Profile implication: new domain / refined preference / routing correction / no profile change
- Updated Skill: ...
- Change type: trigger / checklist / routing / guardrail / output format / new domain
- New behavior: ...
- Not changed: ...
```

Keep the report concise.
