# ADR-004: SQLite for System State Only

- Status: **Accepted**
- Date: 2026-11-25

## Context

The platform needs lightweight durable transactional state in addition to its graph knowledge store.

## Decision

Use SQLite for application/system state such as configuration, project registry, job metadata, preferences, and migration bookkeeping. Use Grafeo for the engineering knowledge graph.

SQLite is not a graph fallback and must not be combined with petgraph as an alternative graph engine.

## Consequences

- Define clear ownership and references between system records and per-project graph stores.
- Sidecar/graph failure must not corrupt system state.
- The exact SQLite schema and database layout remain implementation design work.
