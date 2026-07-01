---
name: "agent-dev-control"
description: "Creates reusable Agent-friendly project R&D control docs. Invoke when starting a new code project or organizing development governance."
---

# Agent Development Control

## 中文速览

- 用途：为新项目或长期开发项目建立轻量的“研发控制”文档体系。
- 适用：项目刚开始、需求容易漂移、需要产品/接口/数据/验收/Agent 协作文档时。
- 不适用：一次性小改动、简单 bug 修复、只需要同步已有文档的场景。

This skill helps an Agent set up a lightweight, reusable project R&D control system for software projects. Use it to turn vague product or engineering direction into a maintainable set of documents that guides future Agent-assisted development.

This skill is part of Zhihua's profile-driven development system. Use `zhihua-dev-profile` for long-term preferences and `zhihua-dev-gatekeeper` for routing; this skill owns the project-control documentation setup itself.

## When To Invoke

Invoke this skill when the user wants to:

- Start a new software project with Agent-assisted development
- Organize product, engineering, API, data, and testing plans
- Create a reusable project control directory similar to `产品研发控制`
- Improve continuity across multiple Agent coding sessions
- Reduce context drift, duplicated requirements, or conflicting documents
- Establish lightweight governance before implementing features

Do not invoke this skill for small one-off code edits, typo fixes, or isolated debugging tasks unless the user explicitly asks to improve project governance.

If project-control docs already exist, prefer `project-doc-sync` for behavior/API/data/deployment documentation updates instead of recreating the control structure.

If creating project-control docs reveals a new recurring development domain or preference, use `zhihua-skill-evolution` to update `zhihua-dev-profile` rather than embedding that profile only in the project docs.

## Core Idea

Create a project control directory that acts as the single navigation and governance layer for Agent development.

The directory should not become a heavy documentation system. It should provide:

1. A single entry document
2. Stage-based delivery planning
3. Feature-level authoritative definitions
4. API contracts
5. Data model contracts
6. Acceptance and testing guidance
7. Decision records
8. Lightweight Agent collaboration controls

## Recommended Directory Structure

Use a project-specific directory name. For Chinese projects, `产品研发控制/` is appropriate. For English projects, use `project-control/` or `dev-control/`.

```text
产品研发控制/
  00-研发控制入口.md
  00-文档治理指南.md
  01-产品总览/
    产品定位与核心抽象.md
  02-阶段规划/
    阶段总览.md
    阶段0-基础基线.md
    阶段1-核心闭环.md
  03-功能定义/
    功能索引.md
    F1-核心功能.md
  04-代码结构/
    当前代码结构.md
    目标代码结构.md
  05-前端规划/
    页面总览.md
    前端技术方案.md
    开发阶段.md
  06-接口契约/
    接口总览.md
    阶段1-接口清单.md
  07-数据模型/
    当前数据模型.md
    阶段1-数据模型.md
  08-验收与测试/
    阶段1-验收用例.md
  09-决策记录/
    ADR-01-示例决策.md
  10-Agent协作控制/
    Agent开发执行协议.md
    阶段1-任务状态板.md
    轻量代码映射.md
    变更记录规则.md
```

Adjust the structure to the project size. For small projects, start with only the entry, governance guide, stage plan, feature definition, acceptance checklist, and Agent collaboration controls.

## Implementation Workflow

When setting up the control system, follow this order:

1. Identify the product goal, target users, and core value loop
2. Define the current stage and its acceptance criteria
3. Create the entry document as the only required starting point
4. Create the governance guide with single-source-of-truth rules
5. Split feature definitions by stable feature IDs, not by temporary tasks
6. Define API contracts only for the current or next implementation stage
7. Define data models only where implementation needs them
8. Add acceptance cases that can verify the current stage
9. Add Agent collaboration controls for execution protocol, task status, code mapping, and change rules
10. Keep all documents short enough for Agents to read selectively

## Entry Document Rules

The entry document should contain:

- Product positioning
- Current stage
- Current stage goals
- Current stage acceptance criteria
- Document index
- Single-source-of-truth rules

Keep it concise. Ideally, the entry document should stay under 80 to 120 lines.

The entry document is a navigation layer, not a place for full feature specs.

## Single Source Of Truth

Use this authority model:

| Content Type | Authoritative Source | Other Documents |
|--------------|----------------------|-----------------|
| Product goal, stage boundary, priority | Entry document and stage plan | Summary only |
| Business rules, permissions, states | Feature definition | Reference only |
| API paths, requests, responses, errors | API contract | Do not duplicate |
| Tables, fields, indexes, constraints | Data model document | Explain meaning only |
| Page routes and interactions | Frontend plan | Do not duplicate backend rules |
| Acceptance scenarios | Acceptance and testing docs | Summary only |
| Major tradeoffs | ADR documents | Reference decision ID |
| Agent execution flow and task state | Agent collaboration controls | No business fact authority |

If two documents fully describe the same fact, consolidate the fact into one authoritative document and replace the other with a reference.

## Agent Collaboration Controls

Always include a lightweight `10-Agent协作控制/` or equivalent directory when the project will be developed across multiple Agent sessions.

### Agent Execution Protocol

Define what an Agent should read before, during, and after implementation.

Recommended reading order:

1. Entry document
2. Current stage plan
3. Relevant feature definition
4. Relevant API contract
5. Relevant data model
6. Relevant acceptance and testing document
7. Current task status board
8. Lightweight code map, only when code location is unclear

### Task Status Board

Track only coarse-grained stage tasks.

Recommended statuses:

- `待实现` / `To Implement`
- `实现中` / `In Progress`
- `待验证` / `Pending Verification`
- `已验证` / `Verified`
- `暂缓` / `Deferred`

Do not track every button, variable, internal helper, or visual detail.

### Lightweight Code Map

Use the code map as a navigation aid, not as a source of truth.

Track only:

- Main backend entry
- Main frontend entry
- Main test entry
- Status such as `待确认`, `尚未实现`, or `已确认`

Do not track every function, component, or utility file.

### Change Record Rules

Require records only when changes affect:

- Stage scope or priority
- Business rules
- Permissions
- State machines
- API contracts
- Data models
- Acceptance criteria
- Major technical decisions
- Key code entry locations

Do not record ordinary copy edits, style tweaks, local refactors, or variable renames.

## Templates

### Entry Document Template

```markdown
# <Project Name> 研发控制入口

> 本文档是研发控制的唯一入口。Agent 和开发者应先读本文件，再按需读取对应子目录的细节文档。

## 一、产品定位

<用 1 到 3 段说明项目是什么、服务谁、核心价值是什么。>

## 二、当前阶段

**阶段 N：<阶段名称>**

<说明当前阶段目标和边界。>

## 三、当前阶段核心目标

1. <目标 1>
2. <目标 2>
3. <目标 3>

## 四、当前阶段验收标准

- [ ] <验收项 1>
- [ ] <验收项 2>
- [ ] <验收项 3>

## 五、文档索引

| 目录 | 用途 | 何时读 |
|------|------|--------|
| `01-产品总览/` | 产品定位与核心抽象 | 首次了解项目时 |
| `02-阶段规划/` | 阶段目标、任务清单、验收标准 | 确定当前做什么时 |
| `03-功能定义/` | 功能输入输出、业务规则、验收标准 | 实现某功能前 |
| `06-接口契约/` | 接口路径、请求、响应、错误码 | 实现或调用 API 时 |
| `07-数据模型/` | 表结构、字段、索引、约束 | 修改数据库时 |
| `08-验收与测试/` | 验收场景和测试用例 | 测试和验收时 |
| `09-决策记录/` | 重大取舍和不可逆决策 | 理解设计背景时 |
| `10-Agent协作控制/` | 执行协议、任务状态、代码映射、变更规则 | Agent 执行任务前后 |

## 六、权威来源规则

<放置单一权威来源表。>
```

### Feature Definition Template

```markdown
# F<N>-<功能名称>

## 一、目标

<说明该功能解决什么问题。>

## 二、角色与权限

| 角色 | 可执行动作 | 限制 |
|------|------------|------|
| <角色> | <动作> | <限制> |

## 三、输入输出

| 场景 | 输入 | 输出 |
|------|------|------|
| <场景> | <输入> | <输出> |

## 四、业务规则

- <规则 1>
- <规则 2>

## 五、状态机

<如无状态机，写“不适用”。>

## 六、验收标准

- [ ] <验收项 1>
- [ ] <验收项 2>
```

### Agent Execution Protocol Template

```markdown
# Agent 开发执行协议

> 本文档定义 Agent 或开发者执行具体开发任务时的最小协作流程。

## 一、开发前读取顺序

1. `00-研发控制入口.md`
2. `02-阶段规划/` 对应阶段文档
3. `03-功能定义/` 对应功能文档
4. `06-接口契约/` 对应阶段文档
5. `07-数据模型/` 对应文档
6. `08-验收与测试/` 对应阶段文档
7. `10-Agent协作控制/阶段N-任务状态板.md`
8. `10-Agent协作控制/轻量代码映射.md`

## 二、开发中规则

- 以权威来源为准
- 不从摘要文档推导完整业务规则
- 不扩大当前阶段范围
- 文档与代码冲突时，先确认代码事实，再决定修改方向

## 三、开发后检查

- 是否符合当前阶段目标
- 是否符合对应功能定义
- 接口契约是否一致
- 数据模型是否需要同步
- 验收用例是否需要更新
- 任务状态是否需要更新
```

## Quality Bar

A good control system is:

- Easy for Agents to read selectively
- Stable enough to survive multiple coding sessions
- Explicit about authoritative sources
- Lightweight enough that humans will maintain it
- Focused on the current stage, not all future possibilities
- Connected to code through navigation, not exhaustive indexing

A bad control system is:

- A duplicate of every implementation detail
- A large document dump with no authority model
- A status board tracking every tiny UI or helper change
- A code map that must be updated for every function rename
- A planning system that delays implementation instead of guiding it

## Final Checklist

Before finishing setup, verify:

- Entry document exists and points to all important directories
- Governance guide defines single-source-of-truth rules
- Current stage is explicit
- Current stage has acceptance criteria
- Feature definitions are split by stable feature IDs
- Agent collaboration controls exist
- Code map is intentionally lightweight
- Change record rules avoid unnecessary bookkeeping
