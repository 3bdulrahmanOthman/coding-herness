# ADR-008: Shipyard as the Operational and Workbench Foundation

- Status: **Accepted product direction; per-capability reuse mechanism open**
- Date: 2026-11-25

## Context

The upstream cosscom/shipyard repository already provides substantial operational functionality for coding-agent work: boxes, worktrees, sessions, terminals, process execution, orchestration, durable runs, hooks, flows, plugins, and a Tauri/React desktop application.

Rebuilding these capabilities would duplicate mature work and expand scope.

## Decision

Use the original upstream Shipyard as the selected operational and desktop-workbench foundation for Coding Harness. Reuse suitable capabilities through supported interfaces or carefully chosen integration seams. The product foundation is accepted; the method used for each capability (API, plugin/adapter, selective source reuse, or replacement where unsuitable) remains open until the source-level audit.

Coding Harness is not assumed to be a fork. This ADR does not approve wholesale source copying or a mandatory direct source dependency.

## Consequences

- Inspect actual source, APIs, lifecycle, security, licensing, tests, and release packaging before selecting an integration method.
- Keep the Rust Intelligence Sidecar and its Grafeo ownership as a distinct intelligence boundary.
- Avoid duplicating existing operational orchestration where it meets requirements.
- Record per-capability reuse/integration decisions in a follow-up ADR after source-level audit.
