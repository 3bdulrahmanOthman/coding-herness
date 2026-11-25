# System Architecture

## 1. Architectural direction

Coding Harness is composed of a developer-facing workbench, an operational execution runtime, and a modular Engineering Intelligence system. These are product/ownership boundaries; they do not mandate one process per capability.

```text
Developer Workbench (desktop / CLI / API — first-release mix open)
        |                         |
        v                         v
Operational Runtime  <---->  Engineering Intelligence API
(Go; Shipyard reuse              |
 evaluated)                      v
  - projects/workspaces       Rust Intelligence Sidecar
  - boxes/worktrees           - repository/code/architecture intelligence
  - sessions/terminals        - knowledge ingestion and governed memory
  - agent processes           - graph/retrieval/context services
  - tasks/runs/events         - deterministic analysis and tool coordination
  - hooks/flows               - planning/agent intelligence support
  - execution/artifacts       - verification evidence and learning
        |                          |
        |                          +----------+
        |                                     |
        v                                     v
  Operational state                         Grafeo
  and event journal                      graph knowledge
                                           (sole graph engine)
        |
        v
      SQLite
  application/system state only
  (not graph fallback)
```

This is a target architecture, not a claim that processes or interfaces already exist.

## 2. Developer Workbench

The workbench is the user-facing control and review surface for projects, workspaces, sessions, tasks, plans, agent activity, tools, permissions, diffs, evidence, settings, and diagnostics. It should consume stable runtime and intelligence APIs rather than embedding domain logic in the UI.

The first usable surface (desktop, CLI, or both) remains open. Tauri 2 + React + TypeScript is a candidate desktop approach informed by the Shipyard ecosystem, not a final approved stack decision. Mobile is not a v1 assumption.

## 3. Operational runtime

The operational runtime owns process-oriented concerns: project/workspace/worktree lifecycle, sessions, terminals, agent process control, task execution, remote boxes, durable operational events, artifacts, hooks, and flows.

The original Shipyard upstream is a candidate foundation. Inspect source, tests, APIs, security, licensing, packaging, and OS support before selecting API integration, plugin/adapter integration, selective reuse, or independent implementation. Avoid rewriting mature operational functionality without a measured reason.

## 4. Rust Intelligence Sidecar

The sidecar is a separately supervised local process responsible for intelligence workloads and Grafeo ownership.

Responsibilities:
- repository discovery and incremental indexing;
- language-specific extraction adapters;
- architecture/dependency/impact analysis;
- knowledge ingestion, provenance, and memory lifecycle;
- graph schema and migrations;
- graph reads/writes and query execution;
- lexical/semantic retrieval coordination and context packet assembly;
- deterministic engineering analysis/tool coordination;
- planning and specialist-selection support;
- verification evidence normalization and learning signals;
- resource limits, health reporting, and diagnostics;
- versioned API contract.

The sidecar should use Grafeo through its native Rust API. Do not introduce Go CGO as the integration boundary. These are logical modules within the sidecar unless concrete isolation or scale requirements justify another process.

The Go runtime (or desktop supervisor where appropriate) manages sidecar lifecycle. The sidecar owns its database handles and must not permit competing processes to mutate the same graph store without verified engine guarantees.

## 5. IPC

Use a versioned, local-only IPC contract. Select the transport after inspecting Shipyard's existing APIs and validating Windows, macOS, and Linux behavior.

Candidate transports include Unix domain sockets, Windows named pipes, or an authenticated loopback HTTP interface. These are options for evaluation, not an approved final transport.

Requirements:
- explicit protocol and schema version;
- request IDs, deadlines, cancellation, and bounded payload sizes;
- structured error codes and health endpoints;
- authentication/authorization appropriate to the local threat model;
- no unauthenticated network-wide listener;
- compatibility policy between runtime and sidecar versions;
- graceful shutdown and startup readiness;
- observable logs without secrets or raw private prompts.

## 6. State and storage boundaries

SQLite stores application/system state such as local configuration, project registry, job/task metadata, user preferences, extension registry metadata, and migration bookkeeping.

Grafeo stores the derived engineering knowledge graph. SQLite is **not** a graph fallback. If Grafeo cannot start, ordinary project/session functions may continue where possible, but graph-dependent capabilities must report degraded/unavailable status.

Source files and Git history remain authoritative. Graph records are derived, rebuildable representations with revision and provenance metadata. Large raw artifacts should be stored as files/object artifacts with stable references and hashes.

Graph traversal, lexical search, semantic/vector retrieval, and artifact storage are distinct workloads. Their exact storage/index placement remains open pending measured POC results.

## 7. Project isolation and database lifecycle

The preferred design to evaluate is one graph database per project/workspace, which can simplify isolation, export, deletion, and rebuild. This is not yet a final storage-layout ADR because Grafeo's current file, concurrency, and backup semantics must be validated.

The implementation must define canonical database location and ownership, project identity and relocation behavior, open/close lifecycle, WAL/checkpoint behavior based on the pinned Grafeo API, safe backup/restore, crash recovery, schema migration, disk quotas, and cleanup policy.

Never copy a live database or WAL files as a backup unless the engine documents that procedure as safe.

## 8. Failure model

- Sidecar unavailable: operational features continue where independent; graph and intelligence features expose a clear unavailable state.
- Graph database corrupt: preserve source repository and SQLite system state; report integrity failure and offer controlled rebuild/restore.
- Index stale: expose indexed revision and freshness; never present stale facts as current.
- Provider unavailable: use only explicitly configured alternate providers or deterministic capabilities; do not silently change model/data policy.
- IPC incompatible: fail closed with a clear version mismatch, not undefined behavior.
- UI unavailable: CLI/API and operational processes may continue where their contracts permit.

## 9. Packaging and platforms

The long-term desktop target is Windows, macOS, and Linux. Validate UI packaging, signing, runtime/sidecar binaries, process supervision, filesystem permissions, IPC, updates, and recovery separately per OS. Tauri/React remains a candidate, not a frozen decision.

Do not assume a desktop binary can be reused unchanged on iOS or Android. Mobile is a future companion surface requiring separate product and platform design.

## 10. Architecture constraints

- Modular architecture; do not split intelligence domains into microservices without a concrete need.
- Operational lifecycle remains independent from intelligence availability where possible.
- Modular monolith inside the intelligence sidecar until scale or isolation justifies more services.
- Stable interfaces between workbench, Go runtime, Rust sidecar, storage, model providers, and extensions.
- No distributed microservices by default.
- No direct model-provider calls scattered throughout core domain logic.
- No graph-engine abstraction designed to support unselected alternatives; isolate Grafeo behind a maintainable internal repository/service boundary without building a multi-engine framework.

## 11. Autonomous engineering execution and governance

The end-to-end execution lifecycle, hierarchical delegation contracts, governance/policy plane, evidence-gated transitions, context health, bounded retries, recovery, and external runtime evaluation are specified in [Autonomous Engineering Execution and Governance](15-autonomous-engineering-execution-and-governance.md).

The Workbench, Operational Runtime, Engineering Intelligence, harness adapters, SCM integration, and governance services retain distinct ownership. The orchestrator coordinates these boundaries; it does not replace durable runtime state, deterministic verification, or policy enforcement.
