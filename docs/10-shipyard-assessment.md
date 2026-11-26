# Shipyard Upstream Assessment

## 1. Repository identity

- Upstream: https://github.com/cosscom/shipyard
- Default branch observed through GitHub Connector: `main`
- Repository is public and accessible for read.
- Coding Harness is not declared as a fork. The user's prior fork was deleted.
- This assessment is based on upstream files inspected through GitHub on 2026-11-25. It is not a complete source-code audit.

## 2. Inspected upstream files

- `README.md`
- `go.mod`
- `app/package.json`
- `docs/guides/orchestration.mdx`
- `docs/guides/project-config.mdx`

The upstream README describes four major pieces:
- `berthd`: daemon on development boxes for worktrees, sessions, services, hooks, flows, and kits.
- `berth`: laptop CLI/background agent and app API connectivity.
- `app/`: Tauri + React desktop application.
- `plugins/`: built-in React plugins using a plugin SDK.

The inspected `go.mod` declares module path `github.com/cosscom/shipyard`, Go version `1.27.0`, and includes `modernc.org/sqlite` among dependencies. Do not assume module renaming or code vendoring is appropriate.

The inspected app package uses Tauri 2, React, TypeScript, Vite, and pnpm. The upstream README lists Go, Node, pnpm, and Rust as build prerequisites.

## 3. Existing capabilities relevant to Coding Harness

The upstream orchestration guide documents:
- starting tasks/worktrees and agents;
- prompting running sessions;
- waiting for agent turns;
- executing checks in worktrees;
- durable runs and event journaling;
- handoff and review patterns;
- broadcast to multiple agents;
- trying multiple implementations with a judge;
- flows, hooks, and plugin orchestration;
- agent completion notifications.

The project-config guide documents repository-level worktree setup, ports, environment, services, agents, hooks, and flows, plus explicit trust before repository-provided commands are run.

These are meaningful existing operational capabilities. Coding Harness should avoid duplicating them without a clear gap.

## 4. Candidate reuse boundaries

Potentially reusable through integration or adapters:
- worktree and project lifecycle;
- session and agent process lifecycle;
- execution and test command infrastructure;
- durable task/run journal;
- agent orchestration primitives;
- hooks, flows, and event integration;
- desktop shell and plugin UI patterns;
- CLI/API connectivity.

These are candidates only. The exact reuse method must be selected after source-level inspection of packages, APIs, security boundaries, and release architecture.

## 5. What must remain Coding Harness intelligence

The proposed differentiating layer includes:
- structured engineering knowledge and Grafeo graph;
- repository semantic indexing and evidence provenance;
- architecture/decision memory and lifecycle;
- task-specific context planning and retrieval;
- engineering impact analysis;
- specialist capability contracts and selection policy;
- evidence-based verification and knowledge updates;
- unified extension/provider contracts where upstream facilities do not satisfy requirements.

## 6. Risks and constraints

- Shipyard's current API and internal package boundaries may evolve.
- Direct source coupling creates maintenance and licensing obligations.
- A plugin API may not expose the lifecycle or data needed by the intelligence runtime.
- The Go daemon and Rust sidecar need a robust cross-platform lifecycle and IPC contract.
- Shipyard's own trust model and permissions must be understood before adding new execution paths.
- Desktop/mobile parity must not be assumed.

## 7. Required next source audit

Inspect the actual source tree and tests for:
1. Go package layout and daemon lifecycle;
2. CLI-to-daemon communication and API contracts;
3. local/remote authentication and transport;
4. process supervision and cancellation;
5. SQLite schema and migration ownership;
6. app Tauri commands and sidecar packaging;
7. plugin SDK and extension permissions;
8. release workflows and supported OS/architecture matrix;
9. test strategy and integration seams.

Then write a reuse decision comparing:
- external integration via supported APIs;
- adapter/plugin integration;
- vendoring or code reuse (only if licensing and maintenance justify it);
- independent operational implementation only for genuine gaps.

Do not call the current documentation inspection a full source audit.


## 6. AgenticOS comparison and product direction

Shipyard remains the chosen foundation for operational runtime and workbench UX. AgenticOS is not a replacement foundation; it is a reference for selected higher-level capabilities: versioned agent definitions, capability-based configuration, Skills/Context, Routines, visual workflows, approvals, budgets, audit, delegation, structured tables, and interactive Apps. Its full multi-tenant Python/FastAPI/PostgreSQL backend is not part of the target architecture.

The product will preserve Shipyard's current UI language and extend its navigation and surfaces. A Windows desktop edition is a committed product target, but current upstream Tauri usage does not prove Windows readiness; a dedicated porting and release validation track is required.

See [16-shipyard-agenticos-integration-plan.md](16-shipyard-agenticos-integration-plan.md) for the capability matrix, one-engine workflow decision, extension tiers, language boundaries, and validation plan.
