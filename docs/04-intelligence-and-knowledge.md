# Engineering Intelligence, Memory, and Knowledge

## 1. Purpose

The intelligence layer should turn repository and project information into a structured, queryable, evidence-backed model that helps agents retrieve relevant engineering context without loading the entire project into every prompt.

A graph is not a substitute for source code, documentation, Git, or a search index. It is a derived representation of meaningful entities and their relationships.

## 2. Graph engine decision

**Grafeo is the sole selected graph engine.** It is integrated inside the Rust Intelligence Sidecar using its native Rust API.

**Graph fallback: none.** Do not silently substitute Neo4j, LadybugDB, SQLite plus petgraph, or another graph engine. If Grafeo is unavailable, graph-dependent features are unavailable/degraded and must say so.

This decision is recorded in ADR-001. A technical proof of concept validates whether the chosen integration meets requirements; it does not reopen the choice by default. A fundamental blocker must be documented and brought back for an explicit product decision.

Grafeo is pre-1.0 software at the time of this foundation. Pin the exact release/commit proven by the POC, record the Rust toolchain and feature flags, and treat upgrades as controlled compatibility and migration events. Do not rely on an unverified version number or API assumption.

## 3. System state versus project knowledge

### SQLite
Use SQLite for application/system state: configuration, local project registry, job metadata, user preferences, migration bookkeeping, and other transactional state that does not require graph traversal.

### Grafeo
Use Grafeo for engineering entities and relationships: modules, symbols, dependencies, requirements, decisions, tests, APIs, and evidence links.

SQLite is not an alternative graph engine. No SQLite+petgraph fallback is approved.

## 4. Knowledge model

Candidate entity types:
- Project, Repository, Revision, Module, Package, Directory, File;
- Symbol, Function, Method, Class, Interface, Type;
- Dependency, API, Endpoint, DatabaseEntity, Configuration;
- Requirement, ArchitectureDecision, Constraint, Convention;
- Bug, Risk, Test, Documentation, KnowledgeClaim;
- Task, AgentRun, VerificationResult.

Candidate relationship types:
- CONTAINS, DEFINES, CALLS, IMPORTS, EXTENDS, IMPLEMENTS;
- DEPENDS_ON, EXPOSES, READS, WRITES, CONFIGURES;
- TESTS, DOCUMENTS, REFERENCES, AFFECTS;
- SUPPORTED_BY, CONTRADICTS, SUPERSEDES, INVALIDATES.

The schema should be versioned and validated. Entity and relationship types must be added based on real query/use cases, not speculative exhaustiveness.

## 5. Evidence and provenance

Every derived claim should preserve as much of the following as applies:
- stable entity/claim ID;
- project and repository identity;
- source file and symbol;
- line range or structural locator;
- Git commit/revision and content hash;
- extraction method/tool and version;
- observed-at and indexed-at timestamps;
- confidence and verification status;
- source category (code, test, docs, user-approved decision, tool output);
- links to supporting evidence.

Do not treat model-generated interpretation as verified fact merely because it was stored.

## 6. Knowledge lifecycle

Use explicit states such as:
- Discovered — extracted or proposed, not yet accepted;
- Verified — supported by deterministic evidence or review;
- Accepted — approved as useful project knowledge;
- Superseded — replaced by a newer decision/fact;
- Deprecated — no longer recommended/current;
- Invalidated — source changed or evidence no longer supports it.

Keep history for important decisions and claims. Do not erase contradictory historical decisions; connect them through SUPERSEDES or CONTRADICTS and expose the current accepted state.

## 7. Indexing strategy

- Initial indexing is staged and observable.
- Incremental indexing uses Git changes, content hashes, and dependency invalidation.
- Deleted and renamed files must invalidate or relocate derived entities.
- Re-index only affected files and dependent relationships where safe.
- Support cancellation, resumability, and bounded parallelism.
- Track the repository revision represented by each index snapshot.
- Never mix data from incompatible revisions without explicit version metadata.

## 8. What not to store

Do not turn every token, line, terminal output, chat message, or transient log into a graph node. Store compact, meaningful engineering entities and relationships.

Do not embed every entity. Consider embeddings selectively for semantic material such as architecture decisions, important documentation, requirements, and curated knowledge. Deterministic relationships such as imports and calls generally do not need embeddings.

## 9. Retrieval and context assembly

A task should produce a retrieval plan containing:
- task intent and affected area;
- relevant symbols, files, modules, and dependencies;
- applicable constraints and architecture decisions;
- related tests and prior verified failures;
- required evidence and freshness;
- unresolved ambiguity.

Retrieval should combine graph traversal, lexical search, semantic retrieval where justified, and deterministic repository tools. Rank results by relevance, authority, freshness, confidence, and proximity to the task.

Each context packet should include evidence references, indexed revision, reasons for inclusion, and warnings for stale or conflicting data.

The exact division between Grafeo text search, SQLite FTS5, and any dedicated search index remains a POC question. Avoid maintaining duplicate indexes without a measured need.

## 10. Resource management

Benchmark complete sidecar process RSS, not only engine-reported memory. Measure indexing and query workloads across small, medium, large, and dense graphs. Track CPU, disk I/O, latency percentiles, cache behavior, spill behavior, and recovery.

Engine-level memory limits do not necessarily equal total process memory. Set budgets from measured workloads and provide cancellation, backpressure, and clear resource-exhaustion errors.

## 11. Backup, restore, and rebuild

Git/source remains authoritative. Graph data is derived and should be rebuildable. Define safe close/checkpoint/backup/restore behavior using the exact pinned Grafeo release. Test forced termination, restart, integrity checks, and interrupted indexing before production use.
