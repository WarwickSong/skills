---
name: "zhihua-skill-evolution"
description: "Improves Zhihua's custom skills from real-use feedback. Invoke when workflows misfire, feel too heavy, miss checks, or need new rules."
---

# Zhihua Skill Evolution

This meta-skill maintains and improves Zhihua's custom Skill system in `C:\Coding\.trae\skills`.

Use it when the user gives feedback about how a Skill behaved in real work, asks to refine a Skill, reports that a workflow was too heavy or too light, notices a missing check, or wants a repeated lesson turned into a reusable rule.

## Scope

This skill may update custom Skills such as:

- `zhihua-dev-gatekeeper`
- `mature-package-first-dev`
- `fastapi-sqlalchemy-tdd`
- `vue-vite-product-ui-review`
- `safetyhub-production-verification`
- `project-doc-sync`
- Other user-maintained Skills in `C:\Coding\.trae\skills`

Do not modify built-in Skills or unrelated project code unless the user explicitly asks.

## Core Principle

Skills should evolve from real usage, not speculation.

Prefer small, precise updates that improve triggering, reduce repeated mistakes, preserve context discipline, and keep each Skill focused.

Avoid turning one Skill into a giant all-purpose policy document.

## Feedback Signals

Invoke this skill when the user says or implies:

- "这个 Skill 没触发对"
- "以后遇到这种情况要先做 X"
- "这个流程太重 / 太轻"
- "这次漏读了某个文档"
- "这次不该新增依赖"
- "这里应该先跑测试 / 不该跑全量测试"
- "这个规则以后要记住"
- "把这次经验沉淀到 Skill 里"
- "优化一下我的 Skill"

Also invoke it after repeated mistakes, repeated manual corrections, or recurring project-specific patterns.

## Update Workflow

1. Identify the feedback type:
   - Trigger condition issue
   - Missing required check
   - Overly broad or heavy workflow
   - Missing project-specific rule
   - Bad output format
   - Incorrect companion Skill routing
   - New recurring project constraint
2. Locate the right target:
   - Entry/routing rule → `zhihua-dev-gatekeeper`
   - Dependency/package decision → `mature-package-first-dev`
   - Backend test/API/model rule → `fastapi-sqlalchemy-tdd`
   - Frontend UX/page rule → `vue-vite-product-ui-review`
   - SafetyHub production rule → `safetyhub-production-verification`
   - Documentation synchronization rule → `project-doc-sync`
   - Skill maintenance rule → this Skill
3. Decide whether to:
   - Edit an existing Skill
   - Add a short reference section
   - Split a new focused Skill
   - Do nothing and explain why the feedback should remain situational
4. Make the smallest useful change.
5. Validate that the updated Skill still has valid frontmatter and a clear description.
6. Summarize what changed and when the new rule will apply.

## Design Rules

- Keep `description` concise and trigger-oriented.
- Put project-specific rules in the most specific Skill possible.
- Put cross-project routing rules in `zhihua-dev-gatekeeper`.
- Do not duplicate the same rule across many Skills unless necessary for triggering.
- Prefer checklists and decision templates over long essays.
- Keep language consistent with the existing Skill file.
- Preserve existing sections unless they are clearly wrong or obsolete.

## When to Create a New Skill

Create a new Skill only when:

- The rule set has a distinct trigger and workflow.
- Adding it to an existing Skill would make that Skill unfocused.
- The user is likely to reuse it across multiple projects or sessions.
- The Skill can be described clearly in one sentence.

Do not create a new Skill for a one-off preference or a tiny reminder.

## Safety Rules

- Do not remove existing rules unless they are outdated, conflicting, or explicitly rejected by the user.
- Do not weaken production or testing safeguards without explicit user approval.
- Do not add package-specific recommendations without checking current project dependencies and deployment constraints.
- Do not rewrite all Skills just to improve wording.
- Do not modify user project code while maintaining Skills unless requested.

## Output Template

When applying feedback, report:

```text
Skill evolution update:
- Feedback captured: ...
- Updated Skill: ...
- Change type: trigger / checklist / routing / guardrail / output format
- New behavior: ...
- Not changed: ...
```

Keep the report concise.
