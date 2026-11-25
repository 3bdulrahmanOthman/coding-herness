# ADR-007: Orchestrator with Dynamic Specialist Agents

- Status: **Accepted**
- Date: 2026-11-25

## Context

Complex engineering work benefits from focused roles, but spawning every specialist on every task wastes tokens, compute, and coordination effort.

## Decision

Use a central orchestrator that dynamically selects and delegates to specialist agents according to task needs and declared capabilities.

Specialists have explicit roles, tools, skills, context requirements, output contracts, permissions, and verification criteria.

## Consequences

- Delegation is bounded by depth, concurrency, budget, and permission.
- Parallel work requires clear ownership boundaries.
- Harness/provider adapters must advertise real capabilities.
- Do not claim universal cross-provider subagent support.
- The orchestrator integrates and verifies specialist outputs before reporting completion.
