---
name: "interview-driven-development"
description: "Uses interviewer-style questioning to improve development quality. Invoke for complex builds, architecture choices, AI/data projects, or interview-ready work."
---

# Interview-Driven Development

## 中文速览

- 用途：用“面试官追问”的方式提升复杂开发任务的方案质量、可解释性和可验证性。
- 适用：复杂项目、架构取舍、AI/数据系统、作品集、面试讲解、需要深入权衡的开发任务。
- 不适用：拼写修正、一行小改、机械性文件调整、没有明显设计选择的简单任务。

This skill helps an agent develop software, data workflows, AI systems, prototypes, or technical artifacts through an interviewer-style reasoning loop. The agent acts as both builder and critical interviewer: it asks high-quality questions, researches better methods when needed, compares trade-offs, verifies implementation quality, and explains the key reasoning to the user in a useful but concise way.

The goal is not to expose every internal thought. The goal is to improve the project and help the user learn how to explain the project.

## When to Invoke

Invoke this skill when a task has one or more of these characteristics:

- The user asks to build, design, refactor, optimize, or improve a non-trivial project.
- The task involves AI agents, LLM apps, RAG, data analysis, automation, evaluation, recommendation, search, or complex UI/product behavior.
- There are multiple plausible technical approaches and meaningful trade-offs.
- The project may be used for interviews, portfolios, demos, internal reviews, or learning.
- The user asks for higher quality, deeper thinking, best practices, research, architecture judgment, or interview preparation.
- The agent notices uncertainty around correctness, evaluation, maintainability, scalability, usability, or business fit.

Do not invoke this skill for tiny mechanical tasks such as spelling fixes, one-line edits, simple file conversion, or obvious implementation steps with no meaningful design choice.

## Core Principle

Treat the development process like a strong technical interview.

For every important design or implementation decision, ask:

- What problem is this actually solving?
- What would a strong interviewer challenge here?
- Is there a better or more standard way to do this?
- What assumptions does this solution depend on?
- What are the edge cases and failure modes?
- How will we know the result is better?
- How does this become a real project change rather than just a good explanation?
- How can the user later explain this decision clearly?

## Engagement Policy

Avoid interrupting the user for every task. Choose the interaction level based on task complexity.

### Fast Build

Use when the task is small, clear, or time-sensitive.

- Do only necessary project-context research.
- Avoid extra discussion unless there is a blocker.
- Final response: concise summary of the result.

### Interview-Aware Build

Use as the default for medium or complex development tasks.

- Ask interviewer-style questions internally.
- Research project context and external best practices when they affect correctness, maintainability, compatibility, or user value.
- Compare key alternatives before choosing.
- Final response: summarize the most important questions, answers, trade-offs, and project changes.

### Interview-Ready Build

Use when the project has learning, portfolio, demo, or interview value.

- Proactively ask the user whether they want this mode if the task seems suitable.
- Research stronger approaches and current best practices.
- Compare multiple approaches explicitly.
- Make project changes that improve not only functionality but also explainability, evaluation, and future extensibility.
- Final response: include a short interview-preparation section with likely interviewer questions and strong answer angles.

When useful, ask the user to choose:

> This task has some project/interview value. Would you like me to do a fast build, a standard interview-aware build, or an interview-ready build with deeper trade-off notes?

If the user already signals a mode, do not ask again. Interpret phrases like “quickly,” “just do it,” “认真做,” “考虑周全,” “按面试项目标准做,” “帮我准备怎么讲,” and “只要结果” as mode preferences.

## Development Loop

Use this loop during the task. Keep it lightweight unless the user selected Interview-Ready Build.

### 1. Understand the Real Task

Clarify internally:

- What outcome does the user need?
- What counts as success?
- What constraints must be preserved?
- What is unknown or risky?
- What existing project conventions must be followed?

If the ambiguity would materially change the work, ask the user. Otherwise make a reasonable assumption and proceed.

### 2. Ask Interviewer-Style Questions

Generate a small set of high-value questions from these categories:

- **Authenticity**: Does the implementation actually solve the problem, or only appear to?
- **Depth**: What technical detail would expose shallow understanding?
- **Boundary**: What inputs, states, users, or environments could break it?
- **Evaluation**: How do we know the change is better?
- **Alternatives**: What other methods could work better?
- **Trade-off**: What are we gaining and giving up?
- **Landing**: What must change in code, tests, docs, UX, or process for this to be real?
- **Narrative**: How can the user explain this project or decision later?

Only keep questions that can influence implementation or user understanding.

### 3. Research Better Methods When Needed

Research is required when it may affect:

- correctness;
- security;
- compatibility;
- current library/API usage;
- architecture choice;
- model, agent, or data evaluation method;
- performance or scalability;
- maintainability;
- user-facing behavior.

Prefer project-internal research first:

- existing code patterns;
- README and docs;
- tests;
- configuration;
- dependency versions;
- nearby implementations.

Use external research when internal context is insufficient or the topic depends on current best practices, official docs, library behavior, or ecosystem conventions.

Do not research merely to look thorough. Research only when it can change the decision or improve the explanation.

### 4. Compare Options and Choose

When there are multiple plausible approaches, compare them briefly using relevant dimensions:

- simplicity vs extensibility;
- speed vs maintainability;
- new dependency vs existing stack;
- generic abstraction vs task-specific implementation;
- automation vs control;
- accuracy vs cost;
- user experience vs implementation complexity;
- prototype value vs production readiness.

Then choose one approach and state the reason in plain language.

Preferred decision shape:

> I chose X because it fits the current constraints better than Y. Y may be better later if the project needs Z.

### 5. Implement and Verify

Turn the questioning into actual project changes.

Depending on the task, this may include:

- code changes;
- tests or validation scripts;
- edge-case handling;
- error states;
- docs or usage notes;
- evaluation metrics;
- logging or observability;
- simpler abstractions;
- clearer user-facing behavior.

If a good question does not lead to a change, explain why only if it matters to the user.

### 6. Report Back to the User

Keep the final response concise. Do not reveal hidden chain-of-thought or every internal question.

For Interview-Aware Build, include:

- what changed;
- key interviewer-style questions considered;
- answers or decisions;
- how those decisions affected the implementation;
- validation performed or not performed.

For Interview-Ready Build, also include:

- likely interviewer questions;
- strong answer angles;
- honest limitations;
- suggested next improvements.

## User-Facing Summary Template

Use this shape when helpful:

```markdown
完成了：
- [主要改动]

我用面试官式追问重点检查了：
- 问题：[关键问题]
  回答：[简短回答]
  落地：[因此做了什么]
- 问题：[关键问题]
  回答：[简短回答]
  落地：[因此做了什么]

如果用于面试/项目讲述，可以这样说：
- [项目亮点]
- [关键取舍]
- [不足与后续优化]
```

Use this only when the task benefits from it. For simple tasks, keep the response shorter.

## Quality Bar

A good use of this skill should produce at least one of these outcomes:

- a better implementation than the first obvious approach;
- a clearer explanation of why the chosen approach is appropriate;
- explicit handling of important edge cases;
- stronger evaluation or validation;
- reduced unnecessary complexity;
- better project narrative for the user;
- better preparation for technical interview follow-up questions.

If none of these outcomes apply, the skill probably should not have been used.

## Example Invocation

User: “帮我做一个 RAG demo，之后可能放进简历里。”

Agent should respond by offering a mode choice or choosing Interview-Ready Build:

> 这个项目有明显的面试和作品集价值。我可以按 Interview-Ready Build 来做：不仅实现 demo，还会比较检索、切分、评估和回答生成方案，最后帮你整理可能被追问的问题和回答。

During development, the agent should ask itself:

- How do we evaluate retrieval quality?
- Why this chunking strategy?
- What happens when the source document lacks the answer?
- How do we show this is better than direct LLM answering?
- What can the user say if asked why this design is reliable?

The final implementation should reflect those questions through code, evaluation, fallback behavior, and a concise explanation.
