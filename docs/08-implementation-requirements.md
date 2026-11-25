# Implementation Requirements

This document converts the architecture direction into verifiable engineering requirements. It is a planning baseline, not evidence that implementation exists.

## 1. Foundation and repository hygiene

- Establish a documented module and package layout before runtime implementation.
- Define coding standards, formatting, linting, test conventions, release versioning, and contribution policy.
- Keep ADRs current when architectural decisions change.
- Separate product requirements, architecture, implementation tasks, and operational runbooks.

## 2. Runtime integration

- Inspect Shipyard's actual Go daemon, CLI, API, app lifecycle, process supervision, storage, security, and release packaging before selecting integration seams.
- Prefer stable public APIs or well-defined adapter boundaries over source-level coupling.
- Document supported Shipyard version/commit if a direct integration is selected.
- Keep operational lifecycle independent from intelligence availability.
- Define runtime/sidecar compatibility and upgrade behavior.

## 3. Rust sidecar

- Native Rust application with Grafeo native API.
- Single authoritative owner for each open graph database.
- Health, readiness, version, and diagnostics endpoints over approved local IPC.
- Graceful startup/shutdown, cancellation, bounded queues, and recovery.
- Structured errors and traceable request IDs.
- Resource budgets for indexing, queries, and concurrent jobs.
- No arbitrary remote listener by default.

## 4. Grafeo proof of concept

Validate against the exact pinned Grafeo version:
- Rust toolchain/MSRV and target platform support;
- database create/open/close and persistence;
- schema and query behavior;
- graph insert/update/delete and traversal;
- concurrent reads/writes and ownership semantics;
- WAL/checkpoint behavior if supported and enabled;
- crash recovery and integrity;
- memory limits, spill behavior, and process RSS;
- export/backup/restore;
- upgrade and schema migration strategy;
- vector/hybrid search only if required by a measured use case.

Do not publish performance claims based solely on vendor benchmarks. Record hardware, graph shape, dataset generator, warm/cold state, concurrency, query definitions, and repeated measurements.

## 5. Graph and indexing

- Stable project, repository, revision, entity, and relationship identifiers.
- Versioned schema and migration metadata.
- Provenance and evidence for derived facts.
- Incremental Git-aware indexing and invalidation.
- Correct rename/delete handling.
- Resumable indexing and bounded concurrency.
- Stale-index detection and explicit freshness status.
- Rebuild from source repository without losing SQLite system state.

## 6. SQLite system state

- Define schema, migrations, transactions, and backup policy.
- Store system metadata only; do not implement a hidden graph fallback.
- Maintain explicit references to graph database identity and status.
- Handle sidecar/graph unavailable state without corrupting system state.

## 7. Context and retrieval

- Task-to-retrieval-plan contract.
- Hybrid retrieval with explainable ranking.
- Evidence references and indexed revision in every context packet.
- Token/context budget and deduplication.
- Freshness and conflict handling.
- Evaluation datasets with expert-labeled relevant evidence.
- Measure precision, recall, evidence coverage, and context token cost.

## 8. Agent orchestration

- Agent capability registry and adapter contract.
- Specialist input/output contracts and permission profiles.
- Bounded delegation depth, parallelism, time, and cost.
- Durable task state and cancellation.
- Idempotent retries and conflict handling.
- Structured artifacts and verification reports.
- Explicit handling of harnesses without native subagent support.

## 9. Extension platform

- Versioned manifest schema.
- Dependency and compatibility resolver.
- Permission declaration and approval.
- Install/update/rollback/uninstall lifecycle.
- Package integrity and provenance.
- MCP adapters separate from native domain services.

## 10. Testing strategy

### Unit
Domain logic, schemas, graph mapping, ranking, parsers, policy decisions, and IPC serialization.

### Integration
Rust sidecar with Grafeo, SQLite state, Go runtime adapter, indexing pipeline, and extension loading.

### Contract
Runtime/sidecar compatibility, provider adapters, tool contracts, manifest validation, and API versioning.

### End-to-end
Repository onboarding, initial and incremental indexing, task planning, delegated execution, verification, restart, and recovery.

### Security
Threat cases in [Security](07-security.md).

### Performance and resilience
Benchmark small, medium, large, and dense graph shapes. Measure p50/p95/p99 latency, throughput, process RSS, CPU, I/O, indexing cost, recovery, and data integrity. Include forced termination and long-running soak tests.

## 11. Release gates

Do not ship a capability as stable until:
- requirements and acceptance criteria are met;
- supported platform matrix is tested;
- migration and rollback behavior is documented;
- security review is complete for the relevant risk;
- telemetry/diagnostics are adequate;
- failure modes are explicit;
- performance is measured on representative workloads.
