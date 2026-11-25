# ADR-008: Evaluate Shipyard as an Operational Foundation

- Status: **Accepted direction; reuse mechanism open**
- Date: 2026-11-25

## Context

The upstream cosscom/shipyard repository already provides substantial operational functionality for coding-agent work: boxes, worktrees, sessions, terminals, process execution, orchestration, durable runs, hooks, flows, plugins, and a Tauri/React desktop application.

Rebuilding these capabilities would duplicate mature work and expand scope.

## Decision

Evaluate the original upstream Shipyard as a candidate operational foundation and reuse suitable capabilities through supported interfaces or carefully chosen integration seams.

Coding Harness is not assumed to be a fork, and no direct source dependency or code copy is approved by this ADR.

## Consequences

- Inspect actual source, APIs, lifecycle, security, licensing, tests, and release packaging before selecting an integration method.
- Keep the Rust Intelligence Sidecar and its Grafeo ownership as a distinct intelligence boundary.
- Avoid duplicating existing operational orchestration where it meets requirements.
- Record the final reuse/integration decision in a follow-up ADR after source-level audit.
