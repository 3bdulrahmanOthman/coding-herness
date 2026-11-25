# Coding Harness

**A developer workbench and agent-engineering platform combining Operational Runtime, Engineering Intelligence, and a unified extensions ecosystem.**

Coding Harness aims to make serious software engineering workflows more reliable across coding agents, models, providers, and repositories. It combines project/workspace/session execution capabilities, a developer-facing workbench, and a modular intelligence system for project understanding, memory, retrieval, planning, orchestration, deterministic tools, verification, and learning.

It is not merely an Engineering Intelligence library, a prompt/skill bundle, an MCP collection, or a wrapper around one model or coding harness.

> **Project status:** Product foundation, architecture, and validation planning. Runtime implementation has not started. Planned capabilities are not implemented or validated unless a later implementation record says so.

## Start here

- [Project foundation](docs/01-foundation.md)
- [Product capabilities](docs/02-product.md)
- [System architecture](docs/03-architecture.md)
- [Engineering intelligence and knowledge](docs/04-intelligence-and-knowledge.md)
- [Complete Engineering Intelligence specification](docs/12-engineering-intelligence-spec.md)
- [Product scope and 22-domain intelligence taxonomy](docs/13-platform-scope-and-intelligence-taxonomy.md)
- [Research and decision register](docs/14-research-and-decision-register.md)
- [Agents and orchestration](docs/05-agents-and-orchestration.md)
- [Unified extensions ecosystem](docs/06-extensions-ecosystem.md)
- [Security and trust](docs/07-security.md)
- [Implementation requirements](docs/08-implementation-requirements.md)
- [Roadmap](docs/09-roadmap.md)
- [Open technical questions](docs/11-open-questions.md)
- [Shipyard upstream assessment](docs/10-shipyard-assessment.md)
- [Architecture decision records](docs/adr/README.md)

## Product planes

| Plane | Responsibility |
|---|---|
| Developer Workbench | Project/workspace UX, sessions, plans, agent activity, diffs, evidence, permissions, settings, diagnostics |
| Operational Runtime | Projects, worktrees, sessions, terminals, agent processes, tasks/runs, hooks/flows, remote execution where supported |
| Engineering Intelligence | Repository/code/architecture understanding, knowledge/memory, retrieval/context, planning, specialist selection, tools, verification, learning |
| Unified Extensions Ecosystem | Typed integrations for tools, MCP, skills, agents, workflows, intelligence/memory/model providers, harness adapters, parsers, and UI plugins |

The intelligence taxonomy is a set of logical capability domains, not a requirement for separate microservices or always-running agents.

## Approved architecture decisions

| Area | Decision |
|---|---|
| Graph engine | **Grafeo** — the only selected graph engine |
| Grafeo integration | **Rust Intelligence Sidecar**, using Grafeo's native Rust API |
| Graph fallback | **None** — no alternative graph engine |
| System state | SQLite for application/system state, not as a graph fallback |
| Intelligence execution | Hybrid: deterministic native capabilities plus model-based reasoning |
| Extensions | Unified extensions ecosystem |
| Agent topology | Orchestrator with dynamically selected specialist agents |
| Operational foundation | Evaluate and reuse suitable original Shipyard capabilities; no assumption of a fork or mandatory dependency |

See the ADRs for rationale, consequences, and validation requirements.

## Design principles

1. Evidence over unsupported assertions.
2. Source repositories and Git remain the source of truth; indexes are derived and rebuildable.
3. Local-first and resource-aware operation.
4. Explicit permissions, trust boundaries, and human approval for consequential actions.
5. Modular architecture with stable contracts and replaceable providers.
6. Incremental work and retrieval rather than indiscriminate context loading.
7. Verification is part of execution, not an optional final prompt.
8. Fail visibly and safely; do not silently substitute a different graph engine.
9. Core capabilities must not require paid/cloud AI or third-party MCPs.
10. Do not duplicate mature operational runtime features before auditing Shipyard.

## License and ownership

No license is declared by this foundation. A license and contribution policy must be selected deliberately before external contributions or redistribution are invited.

- [Autonomous Engineering Execution and Governance](docs/15-autonomous-engineering-execution-and-governance.md)

The product scope includes three cooperating planes—Developer Workbench, Operational Runtime, and Engineering Intelligence—plus a unified extensions ecosystem. The intelligence taxonomy defines 22 logical domains. The execution-governance specification covers hierarchical agents, durable workflows, evidence gates, approvals, isolation, recovery, and end-to-end acceptance.
