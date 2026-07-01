---
name: "docker-oneclick-packager"
description: "Packages projects for Docker one-click and offline intranet deployment. Invoke when asked to containerize, deploy, or create Docker deploy bundles."
---

# Docker One-Click Packager

## 中文速览

- 用途：把项目整理成 Docker 一键部署包，优先支持内网/离线交付。
- 适用：需要 Dockerfile、Compose、环境模板、部署脚本、离线包、部署说明时。
- 不适用：只改业务逻辑、只验证生产风险、或只是小幅 Dockerfile 修补且不需要交付包的场景。

This skill guides an Agent to transform a specified software project into a Docker-based one-click deployment package. The default target is an intranet/offline deployment bundle that can also support online deployment when Docker Hub or a registry is available.

This skill is part of Zhihua's profile-driven development system. Use `zhihua-dev-profile` for long-term deployment preferences and `production-backend-verification` for production-sensitive backend validation; this skill owns packaging artifacts and operator-facing deployment bundles.

This skill owns Dockerfile, Compose, env templates, deployment scripts, offline bundles, and operator handoff docs. It does not own backend behavior, production invariants, package selection, or runtime command safety; use the corresponding focused skills for those concerns.

## When To Invoke

Invoke this skill when the user asks to:

- Package a project for Docker deployment
- Generate a one-click deployment method for a service
- Create an offline intranet deployment bundle
- Add Dockerfile, Docker Compose, deployment scripts, or environment templates
- Make a project configurable by port, domain, data directory, or service name
- Deploy multiple similar services on the same server without conflicts
- Standardize deployment artifacts for Agent-assisted delivery

Do not invoke this skill for small Dockerfile edits unless the user explicitly wants reusable packaging or deployment automation.

For production-sensitive services, pair this skill with `production-backend-verification` before finalizing deployment artifacts.

## Core Goal

Produce deployment artifacts that let an operator copy a generated package to a server and run one command to start the service.

The generated result must support:

1. Intranet/offline deployment by default
2. Online deployment as a compatible mode
3. Configurable host ports through `.env`
4. Multiple services on the same server without container, port, network, or data directory conflicts
5. Persistent data outside containers
6. Clear usage instructions generated every time
7. Relative paths inside project artifacts unless an operator-facing server path is intentionally configurable in `.env`

## Required First Steps

Before editing files, inspect the target project and identify:

- Runtime type: Python, Node.js, static frontend, Java, Go, or other
- Existing startup command and listening port
- Existing package manager and lock files
- Existing Docker files or deployment scripts
- Required environment variables
- Database, cache, object storage, upload directory, or other stateful dependencies
- Health check endpoint or a safe fallback endpoint
- Build output directory if the project is compiled
- Whether the project is already behind Nginx or another reverse proxy

Use existing conventions and dependencies. Do not assume a framework or package manager exists without checking project files.

## Output Directory Rules

Prefer a self-contained deployment directory in the target project, for example:

```text
deploy/
  docker/
    Dockerfile
    docker-compose.yml
    nginx.conf
  scripts/
    build-offline-bundle.sh
    deploy.sh
    install.sh
  env/
    intranet.env.example
    online.env.example
  README.md
```

If the project already has deployment directories, extend the existing structure instead of creating a parallel one.

All paths inside generated files must be relative to the project or deployment bundle whenever possible. Examples:

```env
APP_DATA_DIR=./data/app
POSTGRES_DATA_DIR=./data/postgres
APP_CONFIG_FILE=./config/production.yaml
```

For production server paths, expose them as `.env` values rather than hard-coding them in scripts:

```env
DEPLOY_DATA_ROOT=./data
APP_DATA_DIR=${DEPLOY_DATA_ROOT}/app
POSTGRES_DATA_DIR=${DEPLOY_DATA_ROOT}/postgres
```

## Required Artifacts

Generate or update these artifacts unless the project already has equivalent files:

1. `Dockerfile` for the application image
2. `docker-compose.yml` for local/online deployment
3. `.env.example` or deployment-specific env examples
4. `deploy.sh` for one-command online deployment
5. `build-offline-bundle.sh` for creating an intranet bundle
6. `install.sh` embedded in the offline bundle
7. `README.md` or an existing deployment guide update with usage instructions
8. Optional reverse proxy config such as `nginx.conf` when useful

Every generated deployment flow must include operator instructions. Never leave scripts undocumented.

## Docker Compose Requirements

Compose files must be safe for multiple independent services on one server.

Required rules:

- Use `COMPOSE_PROJECT_NAME` through `.env` or document `docker compose -p <name>`
- Do not hard-code `container_name` unless it is parameterized with `${COMPOSE_PROJECT_NAME}`
- Do not hard-code host ports; use variables such as `${APP_HTTP_PORT:-8080}:80`
- Persist stateful data through relative bind mounts or named volumes
- Add health checks for application and database services where possible
- Use `restart: unless-stopped` for long-running services
- Keep internal container ports stable and expose host ports through `.env`
- Keep service names predictable, such as `app`, `nginx`, `postgres`, `redis`
- Do not assume the final host port, project name, or data directory is unique on the deployment server
- Expose all values that may conflict on the deployment server through `.env`

Multi-Agent deployment model:

- Packaging Agents may run on separate machines and cannot know what other packages will be deployed on the final server
- Packaging Agents must not claim that their default host port is globally safe
- Packaging Agents must provide default suggestions, required internal ports, and configurable variables only
- The final deployment Agent on the target server is responsible for assigning unique host ports, unique `COMPOSE_PROJECT_NAME` values, and unique data roots across all packages
- Each package must include machine-readable or clearly tabular deployment requirements so the final deployment Agent can coordinate multiple packages

Port conflict rule:

- Containers may reuse the same internal port, such as `80`, `8000`, or `3000`, because each Compose project has its own Docker network
- Host ports must be unique on the same server, such as `8081:80`, `8082:80`, and `8083:80`
- Deployment scripts must check whether the configured host ports are already occupied on the target server before starting services when the target OS supports `ss`, `netstat`, or `lsof`
- If a configured host port is occupied, deployment scripts should stop with a clear message and tell the final deployment Agent which `.env` variable to change
- If multiple services need public `80/443`, only the host-level reverse proxy should bind `80/443`; project Compose files should bind unique backend ports

Preferred pattern:

```yaml
services:
  app:
    image: ${APP_IMAGE:-my-service-app:latest}
    build:
      context: .
      dockerfile: deploy/docker/Dockerfile
    env_file:
      - .env
    expose:
      - "8000"
    volumes:
      - ${APP_DATA_DIR:-./data/app}:/app/data
    restart: unless-stopped
```

Avoid this pattern for reusable multi-instance deployments:

```yaml
container_name: my-service-app
ports:
  - "80:80"
```

## Environment File Requirements

Environment examples must be safe to commit.

Required variables:

```env
COMPOSE_PROJECT_NAME=my-service
APP_IMAGE=my-service-app:latest
APP_HTTP_PORT=8080
APP_DOMAIN=example.local
DEPLOY_DATA_ROOT=./data
APP_DATA_DIR=./data/app
APP_LOG_DIR=./data/logs
OFFLINE_SKIP_BUILD=true
```

Local storage mapping rules:

- Map every persistent application directory from a configurable host path to a stable container path
- Prefer `${DEPLOY_DATA_ROOT}`-based relative host paths in examples, such as `${APP_DATA_DIR:-./data/app}:/app/data`
- Use separate host directories for app data, uploads, logs, database files, cache files, and backups when they have different lifecycle needs
- Never store required runtime data only inside the container writable layer
- Create required host directories in `deploy.sh` and offline `install.sh` before `docker compose up`
- Document which directories must be backed up before upgrade

Example volume mapping:

```yaml
volumes:
  - ${APP_DATA_DIR:-./data/app}:/app/data
  - ${APP_UPLOAD_DIR:-./data/uploads}:/app/uploads
  - ${APP_LOG_DIR:-./data/logs}:/app/logs
```

For databases, include only placeholders:

```env
POSTGRES_DB=my_service
POSTGRES_USER=my_service
POSTGRES_PASSWORD=replace-with-strong-password
POSTGRES_DATA_DIR=./data/postgres
DATABASE_URL=postgresql://my_service:replace-with-strong-password@postgres:5432/my_service
```

Security rules:

- Never put real passwords, API keys, tokens, or private URLs into committed templates
- Use `replace-with-...` placeholders for secrets
- If copying a real `.env` into an offline bundle is required, make the script read from an operator-provided file and document the risk
- Do not print secrets in scripts or logs

## Offline Bundle Requirements

The offline bundle is the default delivery mode.

The build script should create a timestamped bundle like:

```text
offline-bundles/<project>_offline_bundle_YYYYMMDD_HHMMSS/
  images.tar
  project/
  install.sh
  SHA256SUMS
```

The compressed output should be:

```text
offline-bundles/<project>_offline_bundle_YYYYMMDD_HHMMSS.tar.gz
offline-bundles/<project>_offline_bundle_YYYYMMDD_HHMMSS.tar.gz.sha256
```

The bundle build script must:

1. Build the application image
2. Pull required base service images unless `SKIP_PULL=true`
3. Save all required images with `docker save`
4. Copy project deployment files into the bundle
5. Copy `README.md`, `部署须知.md`, and all deployment instructions into the bundle
6. Copy a deployment env file into the bundle as `.env` or include an `.env.example` and stop with a clear message
7. Exclude `.git`, virtual environments, dependency caches, test caches, local data directories, logs, and existing bundles
8. Generate checksum files
9. Print exact upload and install commands

The install script inside the bundle must:

1. Check Docker and Docker Compose availability
2. Load `images.tar`
3. Verify `.env` exists or create it from an example only when safe
4. Create required data directories
5. Start services with `docker compose up -d` or `docker compose up --no-build -d`
6. Run non-destructive database migrations or initialization if the project provides them
7. Run health checks
8. Print service URL, status command, log command, restart command, and stop command

Database import must be opt-in. Never clear or overwrite existing server data by default.

## Online Deployment Requirements

Online deployment should reuse the same Compose files and `.env` model.

The online `deploy.sh` should:

1. Check Docker availability
2. Copy `.env.example` to `.env` only if `.env` does not exist, then instruct the operator to edit it
3. Create data directories
4. Build or pull images according to `.env`
5. Start services
6. Run health checks
7. Print usage commands

Online mode must not require changing source code if the offline mode already works.

## Reverse Proxy Guidance

If the application needs one public HTTP entry, include an internal Nginx service when useful.

If multiple services share one server:

- Each Compose project should bind a unique host port, such as `8081`, `8082`, `8083`
- A host-level Nginx, Caddy, Traefik, or gateway can own `80/443`
- The host-level proxy should route by domain to each service's configured host port

Do not make multiple projects bind `80` or `443` directly unless only one service is deployed on the server.

## Health Check Rules

Prefer a real health endpoint:

```text
/health
/healthz
/health/ready
/api/health
```

If no health endpoint exists, use a safe HTTP GET to `/` only if it does not mutate state.

If no HTTP health check is possible, use a process-level check and clearly document the limitation.

## Application-Specific Guidance

### Python or FastAPI

- Detect `requirements.txt`, `pyproject.toml`, `poetry.lock`, or `uv.lock`
- Use the existing package manager
- Use `uvicorn`, `gunicorn`, or the existing startup command
- Keep module path configurable when uncertain, such as `APP_MODULE=main:app`

### Node.js

- Detect `package-lock.json`, `pnpm-lock.yaml`, or `yarn.lock`
- Use the matching package manager
- Build only when the project has a build script
- Use `npm start`, `pnpm start`, or the existing production command

### Static Frontend

- Build static files in a builder stage
- Serve with Nginx or another lightweight static server
- Make the host port configurable

### Java

- Detect Maven or Gradle
- Use a build stage to produce the jar
- Run the final jar in a slim runtime image

### Go

- Use a builder stage to compile the binary
- Copy only the binary and required runtime assets into the final image

## Required Usage Instructions

Every time this skill creates or updates deployment artifacts, generate usage instructions in the project. Prefer updating an existing deployment guide; otherwise create a focused deployment README inside the deployment directory.

In multi-Agent delivery, the Packaging Agent must also create or update `部署须知.md`. This file is written for the final deployment Agent on the target server, not only for a human operator. It must describe what the package needs, what can be changed, and what must be coordinated with other packages.

Chinese is preferred for `部署须知.md` and Chinese deployment handoff docs because the target operator or final deployment Agent is likely to read Chinese. Keep filenames, commands, variables, service names, and code identifiers unchanged.

Recommended documentation files:

```text
deploy/
  README.md
  部署须知.md
```

`README.md` explains how this package is built and used. `部署须知.md` is the handoff contract for the final deployment Agent on the target server.

The instructions must include:

1. What files were generated
2. How to configure `.env`
3. How to build the offline bundle
4. How to upload the bundle to an intranet server
5. How to install with one command
6. How to change the host port
7. How to deploy multiple services on one server
8. How to check whether the chosen host port is already occupied
9. How local storage is mapped from host paths to container paths
10. How to check health
11. How to view logs
12. How to restart and stop services
13. Where persistent data is stored
14. Which data directories should be backed up
15. How to upgrade without deleting data
16. What not to commit, especially real secrets

Use relative paths in instructions when referring to project files.

The usage instructions must include a storage mapping table like:

| Purpose | `.env` variable | Host path | Container path | Backup required |
|---------|------------------|-----------|----------------|-----------------|
| App data | `APP_DATA_DIR` | `./data/app` | `/app/data` | Yes |
| Uploads | `APP_UPLOAD_DIR` | `./data/uploads` | `/app/uploads` | Yes |
| Logs | `APP_LOG_DIR` | `./data/logs` | `/app/logs` | Optional |

The usage instructions must include a port planning table like:

| Service instance | `COMPOSE_PROJECT_NAME` | Host port | Internal port | Public domain |
|------------------|------------------------|-----------|---------------|---------------|
| Service A | `service-a` | `8081` | `80` | `a.example.local` |
| Service B | `service-b` | `8082` | `80` | `b.example.local` |

`部署须知.md` must include a final-deployment handoff section like:

| Item | Package suggestion | Must final deployment Agent coordinate? | `.env` variable or file |
|------|--------------------|------------------------------------------|--------------------------|
| Compose project name | `my-service` | Yes | `COMPOSE_PROJECT_NAME` |
| Host HTTP port | `8080` | Yes | `APP_HTTP_PORT` |
| Public domain | `my-service.example.local` | Yes | `APP_DOMAIN` |
| Data root | `./data` | Yes | `DEPLOY_DATA_ROOT` |
| App internal port | `8000` | No, informational | Compose `app.expose` |
| Health path | `/health` | No, used for validation | `APP_HEALTH_PATH` |

`部署须知.md` must clearly state that suggested host ports and project names are not guaranteed to be unique. The final deployment Agent must inspect the target server, choose final values, edit `.env`, then run the install command.

## Validation Checklist

Before finishing, verify as much as practical:

- Required files exist
- Compose syntax is valid if Docker is available
- Environment example contains no real secrets
- Host ports are configurable
- No fixed `container_name` conflict exists, or names are parameterized
- Data directories are persistent and configurable
- Offline bundle script excludes local data and caches
- Install script does not delete existing data
- Usage instructions are present
- Generated paths are relative except documented operator-configurable server paths

If Docker is unavailable, still perform static validation by reading generated files and explain what command the operator should run later.

## Standard User Prompt

Users can ask:

```text
请使用 docker-oneclick-packager，对当前项目生成内网离线优先的一键 Docker 部署方案。要求端口可配置、路径使用相对路径、支持同一服务器部署多个服务，并生成完整使用说明。
```

For a specified project path:

```text
请使用 docker-oneclick-packager，对 <项目相对路径> 生成内网离线部署包能力。需要 Dockerfile、docker-compose、环境模板、离线打包脚本、一键安装脚本和使用说明，端口和数据目录都要可配置。
```

## Final Response Requirements

When reporting completion to the user, include:

- Files created or changed
- Default deployment mode
- How to configure the port
- How to build and install the offline bundle
- Validation performed
- Any assumptions or follow-up risks, especially secrets and data migration

Keep the response concise but educational enough for operators to understand the deployment model.
