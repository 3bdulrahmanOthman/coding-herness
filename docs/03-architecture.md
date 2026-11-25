# System Architecture

## 1. Architectural direction

The platform is a modular system with a clear separation between operational execution and engineering intelligence.

The current target architecture is:

```text
Desktop / CLI / API
        |
        v
Operational Runtime (Go; Shipyard reuse evaluated)
  - projects, boxes, worktrees, sessions
  - process and agent lifecycle
  - execution, task/run coordination
  - operational state and event journal
        |
        | versioned local IPC contract
        v
Rust Intelligence Sidecar
  - repository discovery and indexing
  - symbol/dependency extraction
  - knowledge and context services
  - retrieval and engineering analysis
  - graph access and query orchestration
        |
        +---------------------+
        |                     |
        v                     v
      Grafeo                SQLite
   graph knowledge       system state
   (only graph engine)   (not graph fallback)
```

This is a target boundary, not a claim that these processes or interfaces already exist.

## 2. Operational runtime

The operational runtime owns process-oriented concerns: project/worktree lifecycle, sessions, terminals, agent process control, task execution, remote boxes, and durable operational events.

Shipyard is a candidate foundation for these concerns. The project will inspect compatibility and reuse boundaries before deciding whether to integrate at the API/CLI level, package components, or implement an adapter. Coding Harness is not currently declared a Shipyard fork or a mandatory dependency.

Avoid rewriting mature operational functionality without a measured reason.

## 3. Rust Intelligence Sidecar

The sidecar is a separately supervised local process responsible for intelligence workloads and Grafeo ownership.

Responsibilities:
- repository scanning and incremental indexing;
- language-specific extraction adapters;
- graph schema and migrations;
- graph reads/writes and query execution;
- evidence and provenance management;
- context retrieval and packet assembly;
- resource limits, health reporting, and diagnostics;
- versioned API contract.

The sidecar should use Grafeo through its native Rust API. Do not introduce Go CGO as the integration boundary.

The Go runtime (or desktop supervisor where appropriate) manages sidecar lifecycle. The sidecar owns its database handles and must not permit competing processes to mutate the same graph store without verified engine guarantees.

## 4. IPC

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

## 5. State and storage boundaries

SQLite stores application/system state such as local configuration, project registry, job metadata, user preferences, and operational references where appropriate.

Grafeo stores the derived engineering knowledge graph. SQLite is **not** a graph fallback. If Grafeo cannot start, ordinary project/session functions may continue where possible, but graph-dependent capabilities must report degraded/unavailable status.

Source files and Git history remain authoritative. Graph records are derived, rebuildable representations with revision and provenance metadata.

## 6. Project isolation and database lifecycle

The preferred design to evaluate is one graph database per project/workspace, which can simplify isolation, export, deletion, and rebuild. This is not yet a final storage-layout ADR because Grafeo's current file, concurrency, and backup semantics must be validated.

The implementation must define:
- canonical database location and ownership;
- project identity and relocation behavior;
- open/close lifecycle;
- WAL/checkpoint behavior based on the pinned Grafeo API;
- backup and restore using documented safe procedures;
- crash recovery and integrity checks;
- schema migration and rollback strategy;
- disk quotas and cleanup policy.

Never copy a live database or WAL files as a backup unless the engine documents that procedure as safe.

## 7. Failure model

- Sidecar unavailable: operational features continue where independent; graph and intelligence features expose a clear unavailable state.
- Graph database corrupt: preserve source repository and SQLite system state; report integrity failure and offer controlled rebuild/restore.
- Index stale: expose indexed revision and freshness; never present stale facts as current.
- Provider unavailable: use only explicitly configured alternate providers or deterministic capabilities; do not silently change model/data policy.
- IPC incompatible: fail closed with a clear version mismatch, not undefined behavior.

## 8. Packaging and platforms

The long-term desktop target is Windows, macOS, and Linux. Tauri/React is a plausible UI approach if the Shipyard evaluation supports it. Platform packaging, signing, native process supervision, filesystem permissions, and IPC adapters remain platform-specific.

Do not assume a desktop binary can be reused unchanged on iOS or Android. Mobile is a future companion surface requiring separate product and platform design.

## 9. Architecture constraints

- Modular monolith inside the intelligence sidecar until scale or isolation justifies more services.
- No distributed microservices by default.
- Stable interfaces between Go runtime, Rust sidecar, storage, model providers, and extensions.
- No direct model-provider calls scattered throughout core domain logic.
- No graph-engine abstraction designed to support unselected alternatives; isolate Grafeo behind a maintainable internal repository/service boundary without building a multi-engine framework.
