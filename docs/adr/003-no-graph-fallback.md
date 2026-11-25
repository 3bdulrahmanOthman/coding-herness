# ADR-003: No Graph Fallback

- Status: **Accepted**
- Date: 2026-11-25

## Context

A fallback graph engine would create duplicated semantics, query behavior, schema migrations, test matrices, and maintenance burden. The project explicitly selected Grafeo.

## Decision

There is **no graph-engine fallback**. Do not substitute Neo4j, LadybugDB, SQLite+petgraph, or another graph implementation if Grafeo is unavailable.

## Consequences

- Graph-dependent capabilities report unavailable/degraded status when Grafeo cannot operate.
- Source repositories and SQLite system state remain intact.
- The graph can be restored or rebuilt from authoritative source data when safe.
- Ordinary application/project/session features may continue if independent of graph services.
- No fallback engine or fallback graph abstraction should be implemented.
