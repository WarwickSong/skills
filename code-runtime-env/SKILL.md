---
name: "code-runtime-env"
description: "Selects safe project runtime environments before running code. Invoke when executing code, tests, scripts, or installing dependencies."
---

# Code Runtime Environment

## 中文速览

- 用途：在运行代码、测试、构建、脚本或安装依赖前，选择安全的项目运行环境。
- 适用：执行 Python/Node/Conda/Docker/package manager 命令前，尤其是不确定该用哪个环境时。
- 不适用：只读代码、静态分析、纯文档修改且不需要执行命令的场景。

This skill guides an Agent to safely choose the correct runtime environment before running code, tests, scripts, package managers, or dependency installation commands.

This skill is part of Zhihua's profile-driven development system. `zhihua-dev-gatekeeper` routes here whenever execution or dependency changes are needed; this skill owns runtime selection and command-environment safety.

## When To Invoke

Invoke this skill when the Agent is about to:

- Run Python, Node.js, shell, Java, Go, Rust, or other project code
- Run tests, linters, formatters, migrations, build commands, or local services
- Install, update, or remove dependencies
- Execute package manager commands such as `pip`, `python`, `pytest`, `npm`, `pnpm`, `yarn`, `poetry`, `uv`, `conda`, `go`, `cargo`, `mvn`, or `gradle`
- Reproduce a bug with runtime evidence
- Start a project command where the correct environment is unclear

Do not invoke this skill for read-only source inspection, static code review, or documentation-only edits unless command execution is required.

When paired with `mature-package-first-dev`, decide what should be installed there first, then use this skill to choose where and how to install or run it safely.

## Core Rule

Never default to the system global runtime for project commands or dependency installation.

Before running commands, discover project-local and user-managed environments, then choose the safest matching environment. If discovery finds multiple plausible environments or no safe environment, ask the user with concrete options before running or installing anything.

## Environment Discovery Order

Inspect the target project from the command working directory upward to the project root.

1. Project-local virtual environments
   - `.venv/`
   - `venv/`
   - `env/`
   - `.env/` only when it clearly contains a runtime environment, not configuration files
2. Python environment managers
   - `.python-version`
   - `pyproject.toml`
   - `poetry.lock`
   - `uv.lock`
   - `Pipfile` and `Pipfile.lock`
   - `requirements.txt` or requirement variants
3. Conda environments
   - try active conda first, then `conda run -n <env> ...` or `conda env list` to list available environments
4. Language-specific local tooling
   - `node_modules/`, `package.json`, lock files, and package manager fields
   - `.nvmrc`, `.node-version`, `.tool-versions`
   - `.ruby-version`, `Gemfile`
   - `go.mod`, `Cargo.toml`, `pom.xml`, `build.gradle`, or equivalent manifests
5. Containerized runtime definitions
   - `Dockerfile`
   - `docker-compose.yml` or `compose.yml`
   - devcontainer configuration

Prefer environments that are explicitly tied to the current project over globally installed tools.

## Python Selection Rules

Use this priority when running Python commands:

1. If a project-local virtual environment exists, use its interpreter directly, such as `.venv/bin/python`, and its tools through `python -m ...` when possible.
2. If Poetry or uv is configured and the project does not expose a local venv, use the project manager command, such as `poetry run ...` or `uv run ...`.
3. If a Conda environment file exists, check whether a matching Conda environment is already available before using it.
4. If an active Conda environment clearly matches the project, use `conda run -n <env> ...` or the active environment consistently.
5. If only global `python`, `pip`, or `pytest` are available, do not use them for project commands until the user chooses an option.

Install Python dependencies only inside the selected virtual environment, Poetry environment, uv environment, or Conda environment. Prefer `python -m pip` over bare `pip` after selecting the interpreter.

## Conda Handling

When Conda may be relevant:

- Detect whether `conda` or `mamba` exists before suggesting Conda commands
- List available environments only as a discovery step
- Match environments by project name, explicit environment file name, or documented setup instructions
- Do not create or modify a Conda environment unless the user explicitly chooses that option
- Do not install packages into `base` unless the user explicitly requests it and no safer project environment exists

## Dependency Installation Rules

Before installing dependencies:

1. Identify the package manager from lock files and manifests
2. Prefer reproducible install commands that respect lock files
3. Confirm the command targets the selected project environment
4. Avoid global install flags unless the project explicitly requires them
5. If a command would write outside the project or selected environment, ask the user first

Examples of safer patterns:

```bash
.venv/bin/python -m pip install -r requirements.txt
poetry install
uv sync
conda env update -n project-env -f environment.yml
npm ci
pnpm install --frozen-lockfile
```

## User Question Policy

After scanning, ask the user when:

- No project-local or managed environment is found
- Multiple plausible environments are found
- The only available option is a system global runtime
- Installing dependencies would affect a global environment
- The environment name cannot be confidently matched to the project
- Creating a new environment appears necessary

Offer concrete choices instead of open-ended questions. Example options:

- Use detected `.venv`
- Use Conda environment `<name>`
- Create or update environment from `environment.yml`
- Create project-local `.venv`
- Run read-only command with system runtime only this time
- Skip running and provide manual command instructions

## Command Construction Rules

When possible, run tools through the selected runtime:

- Python: `<venv>/bin/python -m pytest`, `<venv>/bin/python -m pip`, `poetry run pytest`, `uv run pytest`
- Conda: `conda run -n <env> python -m pytest`
- Node.js: use the package manager implied by lock files, such as `npm`, `pnpm`, or `yarn`
- Project scripts: prefer manifest scripts such as `npm test`, `pnpm lint`, or documented commands

Keep command working directories explicit. Do not rely on accidental shell state from a previous terminal.

## Reporting Back

When reporting command execution, briefly state:

- Which environment was selected
- Why it was selected
- Whether any dependency installation occurred
- Any uncertainty or fallback that remains

This makes environment choices reviewable and prevents accidental global changes.
