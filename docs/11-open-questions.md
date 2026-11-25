# Open Technical Questions and Validation Register

These questions are intentionally separated from accepted architecture decisions. They concern implementation details, validation, and sequencing—not permission to silently change approved decisions.

## A. Shipyard integration

1. Which upstream APIs and package boundaries are stable enough for integration?
2. Should Coding Harness integrate with Shipyard through its CLI/API, a plugin, an adapter, or selected source reuse?
3. Which runtime owns the local desktop lifecycle and sidecar supervision?
4. How are remote boxes and intelligence sidecars represented? Is intelligence local-only initially, or can it run on a remote box?
5. Which Shipyard version/commit and license obligations apply if direct reuse is selected?

## B. Sidecar and IPC

1. Which transport best satisfies Windows, macOS, and Linux: named pipes, Unix sockets, or authenticated loopback?
2. What is the exact handshake, authentication, protocol versioning, cancellation, and compatibility policy?
3. Which process supervises the sidecar in desktop, CLI-only, and remote-box modes?
4. What are restart, idle shutdown, crash recovery, and upgrade semantics?

## C. Grafeo and graph storage

1. Which exact Grafeo release/commit and Rust toolchain pass the POC?
2. Which APIs/features are stable for schema, queries, mutations, vector search, and text search?
3. What are the actual persistence, WAL, checkpoint, concurrency, and backup guarantees?
4. Does per-project graph storage provide the best isolation and lifecycle, or is a different layout required?
5. How are database migrations and rollback handled across pre-1.0 Grafeo upgrades?
6. What memory controls exist, and how does engine memory compare with complete sidecar RSS?
7. Which graph shapes and query patterns represent realistic project workloads?

**Constraint:** Grafeo remains the only graph engine. A failed POC must be brought back as an explicit decision with evidence; it does not authorize an automatic fallback.

## D. Search and retrieval

1. Which workloads belong in Grafeo's text search versus SQLite FTS5 or another lexical index?
2. Which knowledge categories justify embeddings, and where should vectors live?
3. How are retrieval ranking, evidence freshness, confidence, and contradictory facts evaluated?
4. What context packet format is shared across model/harness adapters?

## E. Repository indexing

1. Which languages receive first-class AST/symbol extraction in the MVP?
2. Which parser/indexing technologies offer acceptable portability and maintenance?
3. How are generated files, vendored dependencies, monorepos, symlinks, and huge repositories handled?
4. How are Git branches, worktrees, and multiple revisions isolated in the graph?
5. Which events trigger incremental re-indexing and dependency invalidation?

## F. Agent and provider compatibility

1. Which coding harnesses are first-class in the initial release?
2. What is the minimum common adapter contract, and which capabilities remain optional?
3. How are model selection, cost/latency policy, context limits, and provider failures handled?
4. What are the maximum delegation depth, parallelism, and per-task budgets?
5. How are specialist outputs normalized and independently verified?

## G. Extensions and governance

1. What is the first manifest schema and versioning policy?
2. Which extension types can execute code, and what sandbox is available per platform?
3. How are signatures, publisher identity, permissions, dependencies, and revocation managed?
4. Which native capabilities must exist before any marketplace is considered?

## H. Product and platform

1. Is the first usable surface desktop, CLI, or both?
2. Which operating systems and architectures are release blockers for v1?
3. What is the minimum viable UI for reviewing evidence, plans, permissions, and agent work?
4. What team-sharing and synchronization needs are in scope for the first release?

## I. Evaluation gates

Before implementation decisions are frozen, define:
- representative public and private test repositories;
- expert-labeled retrieval and impact-analysis datasets;
- baseline coding-agent workflows;
- acceptable precision/recall and evidence coverage;
- performance test hardware and graph fixtures;
- security threat model and release criteria.

## Decision hygiene

When a question is resolved, record the result in an ADR or the relevant architecture document. Do not quietly convert a proposed answer into an accepted decision.
