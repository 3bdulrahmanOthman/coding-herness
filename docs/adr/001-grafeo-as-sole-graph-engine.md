# ADR-001: Grafeo as the Sole Graph Engine

- Status: **Accepted**
- Date: 2026-11-25

## Context

The project needs a graph database for structured engineering knowledge, relationships, dependency traversal, and graph-oriented retrieval. Neo4j's operational footprint was a concern for the intended local-first/resource-aware product. Other embedded graph options were explored.

## Decision

Use **Grafeo** as the only selected graph engine. Integrate it through its native Rust API inside the Rust Intelligence Sidecar.

## Consequences

- No multi-engine abstraction or engine-selection setting is required.
- The sidecar owns Grafeo database lifecycle and access.
- The exact Grafeo release, API surface, feature flags, persistence, concurrency, memory, backup, and platform support must be validated and pinned through a POC.
- Because Grafeo is pre-1.0, upgrades require explicit compatibility tests and schema migration planning.
- Vendor benchmark claims are not production guarantees; measure complete sidecar RSS and representative workloads.

## Validation

The POC must test persistence, schema/query behavior, graph mutations/traversal, concurrency, recovery, resource controls, backup/restore, and supported OS targets.

A fundamental blocker must be documented and presented for an explicit decision. Do not silently substitute another engine.
