# Coding Harness

**An Engineering Intelligence Augmentation Platform for AI-assisted software engineering.**

Coding Harness aims to make coding agents more effective at real software engineering work: understanding unfamiliar repositories, reasoning over architecture and dependencies, planning changes, coordinating specialist agents, executing work safely, verifying outcomes, and retaining useful engineering knowledge across sessions.

It is intended to be model-, provider-, and harness-agnostic. It is not another model, a prompt collection, or merely a workflow wrapper.

> **Project status:** Foundation and architecture definition. Runtime implementation has not started. Architecture decisions are recorded separately from implementation details that still require proof-of-concept validation.

## Start here

- [Project foundation](docs/01-foundation.md)
- [Product capabilities](docs/02-product.md)
- [System architecture](docs/03-architecture.md)
- [Engineering intelligence and knowledge](docs/04-intelligence-and-knowledge.md)
- [Agents and orchestration](docs/05-agents-and-orchestration.md)
- [Unified extensions ecosystem](docs/06-extensions-ecosystem.md)
- [Security and trust](docs/07-security.md)
- [Implementation requirements](docs/08-implementation-requirements.md)
- [Roadmap](docs/09-roadmap.md)
- [Shipyard upstream assessment](docs/10-shipyard-assessment.md)
- [Architecture decision records](docs/adr/README.md)

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
| Operational foundation | Evaluate and reuse suitable Shipyard concepts/components; no assumption of a fork or mandatory dependency |

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

## Current scope

The initial project phase is architecture, contracts, requirements, and validation planning. It does not claim that the runtime, sidecar, graph schema, UI, or provider integrations already exist.

## License and ownership

No license is declared by this foundation. A license and contribution policy must be selected deliberately before external contributions or redistribution are invited.
