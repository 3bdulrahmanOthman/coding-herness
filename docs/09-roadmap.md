# Roadmap

The roadmap is phased and evidence-driven. Dates and numeric performance commitments are intentionally omitted until scope and baselines are validated. The roadmap covers the whole Coding Harness product, not only Engineering Intelligence.

## Phase 0 — Product and architecture foundation (current)

- Product definition across Developer Workbench, Operational Runtime, Engineering Intelligence, and Unified Extensions.
- Full intelligence-domain taxonomy and end-to-end workflow.
- Architecture boundaries, accepted ADRs, and research register.
- Shipyard upstream assessment and source-audit plan.
- Grafeo sidecar integration requirements.
- Security, testing, platform, and resource requirements.

**Exit criteria:** product scope and ownership boundaries are documented; accepted decisions are separated from open research; planned capabilities are not presented as implemented.

## Phase 1 — Technical validation and research

- Complete Shipyard source/API/test/security/license/platform audit.
- Build and benchmark a minimal Grafeo Rust POC.
- Validate persistence, query behavior, recovery, resource use, backup/restore, and supported desktop platforms.
- Select and test local IPC and sidecar lifecycle.
- Define Graph Schema v1, provenance, memory lifecycle, and retrieval query catalog.
- Research lexical/semantic/vector indexing placement and external memory integrations against actual workloads.
- Establish model-provider and coding-harness capability matrices.
- Establish representative datasets, baseline workflows, and evaluation methodology.
- Resolve first-release surface and operational/runtime integration boundaries.

**Exit criteria:** reproducible validation reports, pinned Grafeo release/toolchain, tested IPC/lifecycle proposal, source-audited Shipyard reuse decision, and approved implementation backlog.

## Phase 2 — Core platform skeleton

- Workbench/runtime/sidecar API boundaries.
- Operational runtime integration adapter.
- Rust Intelligence Sidecar skeleton and lifecycle supervision.
- Versioned IPC contract, health, readiness, cancellation, and diagnostics.
- SQLite system-state schema and migrations.
- Grafeo database lifecycle and schema migration.
- Project/workspace registry, repository onboarding, and basic index status.
- Minimum viable UI/CLI surface for project and runtime status.

**Exit criteria:** app/runtime can open a project and start/monitor/stop the sidecar; project/session/system state remains usable if intelligence is unavailable.

## Phase 3 — Repository and project intelligence MVP

- Git-aware incremental indexing.
- File/module/symbol/dependency extraction for selected languages.
- Architecture and runtime/configuration inventory.
- Evidence/provenance and governed knowledge lifecycle.
- Graph query services and validated lexical retrieval.
- Basic impact analysis and explainable retrieval.
- Context packet generation with freshness and token budgets.

**Exit criteria:** evaluated repository understanding, retrieval, and impact-analysis quality on representative repositories.

## Phase 4 — Task, agent, execution, and verification intelligence

- Task intake, clarification, planning, decomposition, and acceptance criteria.
- Orchestrator and specialist capability registry.
- Dynamic specialist selection, bounded delegation, and task contracts.
- Agent/harness adapters for selected environments.
- Deterministic tool registry and permission-aware execution.
- Verification plans, independent review, and evidence-backed completion reports.
- Workbench task timeline, diff/evidence review, and approval gates.

**Exit criteria:** end-to-end engineering tasks demonstrate measurable improvement over a baseline workflow with no unsupported completion claims.

## Phase 5 — Memory learning and extension/provider ecosystem

- Memory correction, supersession, invalidation, expiry, and approval workflows.
- Learning from review/test outcomes with policy governance.
- Unified extension manifests and lifecycle.
- MCP interoperability adapters and native tool exposure.
- Model/provider adapters and policy-aware routing.
- Skills, agent packages, workflow packages, parser/indexer extensions, harness adapters, and UI plugins.
- Permission review, package integrity, and sandboxing.

**Exit criteria:** integrations are compatible, auditable, secure, and do not become prerequisites for core capabilities.

## Phase 6 — Productization and advanced collaboration

- Desktop experience and CLI/API polish.
- Multi-project workflows, diagnostics, updates, recovery, and packaging.
- Resource budgets, performance tuning, and reproducible evaluation.
- Optional remote execution, team policies, synchronization, and shared knowledge research.
- Optional mobile companion exploration.
- Extension catalog/marketplace only after core governance and security are proven.

**Exit criteria:** supported releases meet reliability, security, usability, and resource targets established by evidence.

## Prioritization rule

Prioritize correctness, evidence, repository understanding, safe execution, retrieval quality, verification, and user control. Avoid marketplace breadth, speculative agent count, always-on intelligence services, and elaborate UI before the core end-to-end workflow is proven.

## Cross-phase execution governance workstream

This workstream applies across Phases 1–6 and is detailed in [Autonomous Engineering Execution and Governance](15-autonomous-engineering-execution-and-governance.md).

- Phase 1: validate DSH/Sandcastle/Shipyard capabilities and compare reuse boundaries; define task contracts, state transitions, policy requirements, and evaluation baselines.
- Phase 2: implement durable task/run state, event lineage, cancellation/recovery, worktree ownership, and minimum policy enforcement.
- Phase 3: connect repository evidence, project specifications, architecture constraints, and context packets to task/slice contracts.
- Phase 4: implement hierarchical orchestration, dynamic specialists, independent verification, bounded remediation, review, and SCM/CI workflows.
- Phase 5: add governed learning, context-health metrics, workflow packages, capability attestation where justified, and extension governance.
- Phase 6: harden production recovery, security, auditability, platform support, and optional remote/team execution.

No phase requires all agents, stages, or integrations to run for every task. Workflow selection is risk- and capability-driven.


## Cross-cutting upstream integration work

- Keep Shipyard as the operational/workbench foundation and complete its source/API/security/platform audit before reuse decisions.
- Adopt selected AgenticOS patterns through native contracts: versioned Agent Specs, Skills/Context, unified Engineering Memory, one workflow substrate with visual authoring and Routines, Virtual Tables, approvals, budgets, audit, and sandboxed Apps/artifacts.
- Avoid a second workflow executor: migrate/bridge existing Shipyard flows and new visual/routine definitions to one canonical Workflow Spec and durable run model.
- Implement the unified extension execution tiers (native, WASM where suitable, isolated process, remote/MCP, and UI extension) behind one manifest and governance system.
- Deliver a Windows desktop edition as an explicit target with clean-install, signing, process/terminal, IPC, Rust sidecar/Grafeo, update, and recovery gates. Preserve the Shipyard UI language.

See [Shipyard and AgenticOS integration plan](16-shipyard-agenticos-integration-plan.md).
