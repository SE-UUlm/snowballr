# AGENTS

## Overview

This repository is the **umbrella** of the SnowballR project. It contains the project-level documentation
(wiki) and the production/development deployment setup (Caddy reverse proxy and Docker Compose). It does not
contain application source code — those live in the dedicated repositories below. Prefer referencing the wiki
and existing docs over restating them here. If you must summarize, keep it short and point to the canonical
page.

### SnowballR repositories

- Organization: https://github.com/SE-UUlm
- SnowballR (umbrella repo): https://github.com/SE-UUlm/snowballr
- SnowballR API: https://github.com/SE-UUlm/snowballr-api
- SnowballR Backend: https://github.com/SE-UUlm/snowballr-backend
- SnowballR Frontend: https://github.com/SE-UUlm/snowballr-frontend
- SnowballR CI: https://github.com/SE-UUlm/snowballr-ci
- SnowballR Mock Backend: https://github.com/SE-UUlm/snowballr-mock-backend
- SnowballR Backend (legacy): https://github.com/SE-UUlm/snowballr-backend-old

### Canonical documentation and what each covers

- README.md — project overview, badges, and pointers
- wiki/Home.md — SLR / snowballing introduction and high-level requirements
- wiki/Getting-Started.md — prerequisites; points contributors to the frontend / backend repos for actually running them
- wiki/Architecture.md — system-level architecture, technology overview, and database schema (component-diagram, ER-diagram)
- wiki/Contributing.md — contribution workflow, deployment topology, routing, versioning and release procedure, Teamscale integration
- wiki/Processes.md — user account lifecycle, project lifecycle, criterion lifecycle, project invitation, full SLR process

## Structure

```
.
├── wiki/                       # canonical project-level documentation
│   ├── Home.md                 # entry page (also the GitHub wiki landing page)
│   ├── Getting-Started.md
│   ├── Architecture.md
│   ├── Contributing.md         # contains the canonical release procedure for all repos
│   ├── Processes.md
│   ├── _Sidebar.md / _Footer.md
│   └── assets/                 # diagrams referenced by wiki pages
├── images/                     # project logos used in READMEs
├── compose.yaml                # production + development deployment stack
├── Caddyfile                   # reverse-proxy routing (mounted into the caddy container)
├── Dockerfile.proxy            # gRPC-web proxy image used by compose.yaml
├── directory.krpref            # Kotlin/IDE workspace metadata
├── .github/workflows/          # deployment, release, wiki publishing, linting
├── markdownlint.json           # markdown lint config shared by wiki.yml
├── CHANGELOG.md
├── LICENSE
└── SnowballR Diagrams.drawio   # source of the diagrams in wiki/assets/
```

## Where to look

| Task                               | Location                                                      | Notes                                                       |
| ---------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------------- |
| Project overview                   | README.md                                                     | Badges and high-level pointers.                             |
| SLR / snowballing concepts         | wiki/Home.md                                                  | Definitions, requirements.                                  |
| First-time setup pointers          | wiki/Getting-Started.md                                       | Defers to frontend / backend repos for actual run commands. |
| System architecture                | wiki/Architecture.md                                          | Frontend, backend, API, database, Caddy reverse proxy.      |
| Contribution workflow / release    | wiki/Contributing.md                                          | Canonical release procedure used by all SnowballR repos.    |
| Deployment topology and routing    | wiki/Contributing.md#deployment                               | Service overview, networks, Caddy routing (mermaid).        |
| Process diagrams (lifecycle / SLR) | wiki/Processes.md                                             | User, project, criterion, invitation, SLR diagrams.         |
| Production / development stack     | compose.yaml                                                  | Profiles: `production`, `development`.                      |
| Reverse-proxy routing config       | Caddyfile                                                     | Mounted into the `caddy` service at runtime.                |
| gRPC-web proxy image               | Dockerfile.proxy                                              | Built by the `proxy` and `proxy-development` services.      |
| Diagram sources                    | SnowballR Diagrams.drawio                                     | Edit here, then export into wiki/assets/.                   |
| Deployment workflows               | .github/workflows/deploy.yml, deploy-dev.yml, deploy-prod.yml | Reusable deploy; nightly dev; prod on `v*.*.*` tag.         |
| Wiki publishing                    | .github/workflows/wiki.yml                                    | Markdown lint + publish to the GitHub Wiki on main.         |
| Release automation                 | .github/workflows/release.yml                                 | Delegates to snowballr-ci `release.yml@v1.1.0`.             |

## Architecture and deployment

- **System overview:** Browser → Caddy → frontend / api-docs / gRPC-web proxy → backend → PostgreSQL
  (wiki/Architecture.md, wiki/Contributing.md#routing).
- **Two parallel stacks:** `production` (tagged backend version, stable) and `development` (`latest-dev` backend and
  frontend, may be unstable) are defined in `compose.yaml` via profiles.
- **Networks:** `snowballr-network` is internal-only; `snowballr-host` is used by `caddy` (external clients) and the
  `backend` (host email server). See wiki/Contributing.md#networks.
- **Deployment trigger:** development deploys nightly at 05:00 and on every push to `main`; production deploys on
  pushing a `v*.*.*` tag (`deploy-dev.yml`, `deploy-prod.yml`).
- **Backend version pin:** the `backend` service in `compose.yaml` pins a specific version
  (`ghcr.io/se-uulm/snowballr-backend:<version>`). When releasing a new SnowballR version, this pin must be updated
  (wiki/Contributing.md#release-procedure).

## Required environment variables (for deployment)

- `PROD_DOMAIN` — domain served by the production stack.
- `DEV_DOMAIN` — domain served by the development stack.
- `WORK_DIR` — base working directory for mounted volumes (database files, Caddyfile, Caddy data/config).

See wiki/Contributing.md#deployment for details.

## Boundaries

- Always do: prefer wiki references over restating things here; keep changes scoped to deployment, wiki, or
  project-level docs; follow the release procedure documented in wiki/Contributing.md.
- Ask first: bumping the pinned backend image version in `compose.yaml`; changes to `Caddyfile` or routing;
  adding / removing compose services or profiles; changes to deployment workflows or SSH/secret handling;
  changes to the diagrams under `wiki/assets/` (also update the `.drawio` source).
- Never do: commit secrets, real domain credentials, or contents of `*.env` files; touch `${WORK_DIR}/database*`
  data; bypass the Caddy reverse proxy by exposing service ports publicly; publish the wiki from a non-`main`
  branch.

## Commands (run from repo root)

### Compose

- Production stack: `docker compose --profile production up`
- Development stack: `docker compose --profile development up`
- Requires `PROD_DOMAIN`, `DEV_DOMAIN`, `WORK_DIR`, and the per-service `*.env` files (see compose.yaml).

### Wiki / Markdown

- Markdown lint and link check run in CI (.github/workflows/wiki.yml) using snowballr-ci's `lint-md@v1` action.
- The same workflow publishes the contents of `wiki/` to the GitHub Wiki on pushes to `main`.

### Release

- Follow wiki/Contributing.md#release-procedure: create `releases/vX.Y.Z`, update `CHANGELOG.md` (prefer
  `hallmark cc add major|minor|patch`), open a PR, tag the merge commit, and push the tag.
- Tagging `v*.*.*` triggers `.github/workflows/release.yml` (uses `SE-UUlm/snowballr-ci/.github/workflows/release.yml@v1.1.0`)
  and `deploy-prod.yml`.

## Style, checks, and tests

- **Style:** Follow `markdownlint.json` for all wiki/README/CHANGELOG content.
- **Diagrams:** Source diagrams in `SnowballR Diagrams.drawio`; export updated PNGs/SVGs into `wiki/assets/` when changed.
- **No application code** lives here, so there is no Kotlin/TS lint or test setup in this repo.

## Issues

- Use `.github/ISSUE_TEMPLATE` (when present) to pick the right template.

## PRs

- Use `.github/pull_request_template.md` (when present) for required sections.

## Git and CI conventions

- PRs to `main` must keep a linear history (`.github/workflows/git_conventions.yml`, via snowballr-ci's
  `ensure-linear-history@v1.0.0`). Rebase onto `main` before requesting review.
- Use merge commits when merging an approved PR — see wiki/Contributing.md#workflow.
- CI runs markdown lint, deploy (on schedule / push / tag), wiki publish, and release (`.github/workflows`).

## Conventional commits

Commit messages follow Conventional Commits with a short type prefix and optional scope. Common types in this repo
include: feat, fix, refactor, docs, chore, ci. Use lowercase types and keep the subject imperative and concise.
