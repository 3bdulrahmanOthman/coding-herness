# ADR-002: Rust Intelligence Sidecar

- Status: **Accepted**
- Date: 2026-11-25

## Context

The operational runtime and engineering intelligence layer have different responsibilities and technology constraints. Grafeo offers a native Rust integration path. Rewriting the operational runtime would add unnecessary risk.

## Decision

Place graph and engineering-intelligence responsibilities in a separately supervised **Rust Intelligence Sidecar**. Use Grafeo's native Rust API. Keep operational lifecycle and agent/process orchestration in the Go operational layer where suitable.

## Consequences

- Define a versioned local IPC contract.
- Define startup, readiness, health, cancellation, graceful shutdown, restart, and compatibility behavior.
- The sidecar is the authoritative owner of its graph database handles.
- Avoid CGO as the Go/Rust integration boundary.
- Packaging and process supervision must be validated on Windows, macOS, and Linux.
- The sidecar can fail independently; operational features should remain available when they do not depend on graph intelligence.
