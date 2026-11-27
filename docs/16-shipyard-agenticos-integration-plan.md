# Shipyard Foundation, AgenticOS Capability Adoption, and Workbench Integration

> Status: approved product direction and integration plan. This document distinguishes upstream facts, accepted product decisions, and validation work. It does not claim that any planned feature is implemented.

## 1. Product decision

**Shipyard is the operational and desktop-workbench foundation. AgenticOS is a source of selected product patterns and capabilities to adapt, improve, and integrate into Coding Harness.** Coding Harness is not a rewrite of either upstream project and will not import AgenticOS's complete server backend.

- Upstream foundation: [cosscom/shipyard](https://github.com/cosscom/shipyard), MIT.
- Capability reference: [vstorm-co/agenticos](https://github.com/vstorm-co/agenticos), Apache-2.0.
- Product repository: [3bdulrahmanOthman/coding-herness](https://github.com/3bdulrahmanOthman/coding-herness).

Shipyard currently describes `berthd` (box daemon), `berth` (CLI/background agent), a Tauri/React desktop app, worktrees, terminals, sessions, services, hooks/flows, automations, and React plugins. These are candidates for selective reuse and extension after source-level validation, not assumptions that every subsystem is production-ready for our target.

AgenticOS currently documents a self-hosted multi-user agent platform built around FastAPI, PostgreSQL/pgvector, Pydantic AI, and a Next.js console. Its useful patterns include versioned agent specs, capability-based agent configuration, Skills/Context/Memory, visual workflows, routines, approvals, budgets, audit trails, delegation, interactive Apps, and browser capabilities. We adopt selected patterns, not its backend architecture.

## 2. Product planes and ownership

1. **Developer Workbench** — Shipyard-derived UX and desktop application: projects, workspaces, sessions, chat, agent activity, task/delegation tree, plans, approvals, diffs, evidence, artifacts/Apps, settings, diagnostics, and extension management.
2. **Operational Runtime** — reuse and evolve Shipyard's Go operational capabilities where the audit confirms suitability: project/box/worktree/session/process lifecycle, terminals, services, durable task/run events, hooks, operational automation, and remote boxes where supported.
3. **Engineering Intelligence** — Rust Intelligence Sidecar: repository/code/architecture understanding, Engineering Memory, knowledge graph, retrieval/context, planning support, impact analysis, deterministic analysis, verification evidence, and learning.
4. **Unified Extensions & Capability Ecosystem** — native tools, MCP, Skills, agent packages, workflow packages, model/provider adapters, harness adapters, memory/intelligence providers, parsers/indexers, and UI plugins.

These are ownership boundaries, not a requirement for one process per domain. Engineering Memory and Code Intelligence are first-class parts of Engineering Intelligence, not separate competing products.

## 3. Capability adoption matrix

| AgenticOS pattern | Coding Harness treatment |
|---|---|
| Agent Builder / agent specifications | Adopt versioned, schema-validated Agent Specs; add capability declarations, permissions, budgets, compatibility, test fixtures, and rollback |
| Skills | Adopt on-demand, progressive-disclosure skill loading; support scripts/resources with explicit trust and execution permissions |
| Context | Adopt reusable project/user context with scope, provenance, revision, and secret exclusion |
| Memory | Integrate with Engineering Memory lifecycle; do not create a parallel memory store |
| Knowledge bases / RAG | Integrate with repository knowledge and retrieval; avoid a second generic document-RAG subsystem |
| Visual Workflows | Adopt a visual authoring surface over one canonical workflow definition and execution substrate |
| Routines / watches | Adopt schedule, event, webhook, and manual triggers on the same durable workflow runtime |
| Virtual Tables | Add optional structured task/data artifacts with schema, provenance, import/export, and UI views |
| Delegation | Integrate with the hierarchical Orchestrator and specialist capability registry |
| Budgets | Enforce model/tool/time/concurrency budgets before and during execution |
| Approvals | Integrate with Progressive Governance and resumable approval states |
| Audit trail | Record actor, agent, provider, extension, policy, action, evidence, timestamps, and outcome |
| Interactive Apps / visuals | Adopt a sandboxed Artifact/App model with preview, versioning, permissions, and explicit publish/share actions |
| Browser automation | Optional isolated capability/extension; never silently grant access to user sessions or credentials |
| Multi-tenant organization backend | Do not import; keep the local-first product core. Team/server collaboration is a separate future deployment mode |
| AgenticOS Python/FastAPI/Postgres backend | Do not port wholesale; reimplement required capabilities behind our native contracts |

## 4. Workflow architecture: one engine, multiple authoring surfaces

Avoid two competing workflow engines (Shipyard operational flows and a separate AgenticOS-style visual workflow engine).

Define one canonical, versioned Workflow Spec and one durable execution substrate. Shipyard hooks/flows, visual canvas, routines, CLI, API, and event triggers become authoring/trigger adapters that compile or invoke the same workflow model.

The canonical model should support:
- Typed steps: deterministic tool, agent invocation, specialist delegation, condition, transform, wait, approval, human input, subworkflow, and terminal result.
- Explicit input/output schemas and data contracts.
- DAG dependencies, bounded branches/loops, and explicit retry policies.
- Event, schedule, webhook, manual, and task-completion triggers.
- Durable run state, append-only event history, checkpoints where justified, cancellation, timeout, idempotency, replay policy, and crash recovery.
- Human approval pauses that resume the same run safely.
- Budgets, concurrency limits, permissions, secrets references, and trust policy.
- Version-pinned definitions and immutable run snapshots.
- Evidence/artifact outputs and observability.
- Compatibility adapters for existing Shipyard flows; migration must be incremental and preserve existing behavior.

The Operational Runtime owns the single durable execution substrate, authoritative workflow/run/task state, event journal, retries, cancellation, and recovery. Engineering Intelligence plans and selects workflows, supplies context and specialists, and evaluates evidence; it does not run a competing workflow engine. Governance authorizes transitions and side effects but is not another executor. Grafeo may retain derived task/run/evidence relationships, never the canonical live run state.

Do not add an entirely new workflow service before auditing Shipyard's flow engine. First determine whether its current engine can host the canonical model or whether a narrow runtime extension is required. The visual builder is an editor, not a second executor.

## 5. Agent specifications and dynamic collaboration

Use one versioned Agent Spec format. It should describe:
- Identity, purpose, instructions, input/output contracts, and version.
- Declared capabilities and tool references.
- Model/provider preferences and capability requirements (not hard-coded vendor assumptions).
- Skills, context sources, and memory scopes.
- Permission profile, trust level, allowed side effects, and approval policy.
- Budget, timeout, concurrency, delegation depth, and retry constraints.
- Evaluation examples, acceptance tests, and compatibility metadata.

The Orchestrator selects only the specialists required by the task. Specialists may delegate only within an explicit parent contract and global recursion/concurrency budgets. A specialist is a role/configuration executed by an available provider or harness, not necessarily a permanently running process.

Collaboration modes include sequential handoff, parallel independent review, adversarial critique, plan review, implementation/review pairs, and human escalation. Use them when expected quality gain justifies latency and cost; do not spawn a panel for every task.

## 6. Unified Extensions: execution tiers

Keep one catalog, manifest, permission model, lifecycle, and discovery UX, but support distinct execution tiers:

1. **Tier 0 — Native trusted capabilities:** compiled/bundled code, tightly versioned with the app; smallest overhead and used for core deterministic features.
2. **Tier 1 — Sandboxed/WASM extensions:** preferred for portable third-party logic when required APIs fit the WASM host interface; capability-scoped host functions, quotas, and no ambient access.
3. **Tier 2 — Isolated local processes:** Rust/Go or other supported extension executables over versioned IPC for integrations needing OS APIs, native libraries, or independent lifecycle.
4. **Tier 3 — Remote/MCP services:** external tools and services with explicit network, credential, data-egress, and approval policies.
5. **UI extensions:** separate frontend extension contract with declarative contribution points; never grant backend privileges merely because a plugin renders UI.

The manifest declares tier, entry point, version, permissions, capabilities, supported platforms, dependencies, resource limits, secrets, network access, integrity/signature metadata, and compatibility. Installation, enablement, updates, rollback, revocation, and audit are governed. MCP is an interoperability adapter, not the internal domain architecture.

A marketplace/catalog is a product capability to design for, but public distribution, trust review, signing, publisher verification, and revocation must be validated before enabling broad third-party execution.

## 7. Desktop, Windows, and platform strategy

**A Windows desktop edition is an explicit product target.** Do not infer readiness from Tauri usage or upstream build files.

The platform plan is:
- Windows: build and ship a supported desktop edition; validate installer, signing, auto-update, filesystem/process permissions, terminal behavior, worktree operations, named-pipe/local IPC, sidecar lifecycle, and recovery on clean Windows installations.
- macOS and Linux: preserve and validate upstream-compatible desktop targets, packaging, permissions, and sidecar behavior.
- CLI/runtime components: keep OS-specific process, shell, terminal, path, and service behavior behind platform adapters.
- Rust Intelligence Sidecar: package per target OS/architecture and verify Grafeo native dependencies and data-directory semantics.
- Mobile: not part of this desktop commitment; defer until separately justified.

The existing Shipyard desktop app is a starting point for assessment, not proof that the Windows product is complete. Windows is delivered through explicit porting, integration, packaging, and release gates.

## 8. Language and process boundaries

- **Go:** retain Shipyard's operational runtime/daemon/CLI responsibilities where source audit and benchmarks support reuse.
- **Rust:** Engineering Intelligence Sidecar, Grafeo native integration, indexing/analysis hot paths, bounded graph algorithms, and native components where profiling justifies them.
- **TypeScript/React:** existing Shipyard workbench and UI plugins; evolve the current visual language and interaction patterns rather than redesigning from scratch.
- **Other languages:** allowed for isolated adapters only when a capability or ecosystem advantage justifies the maintenance and packaging cost.

Do not rewrite all of Shipyard in Rust and do not add a language merely for theoretical speed. Benchmark end-to-end startup, idle RSS, indexing, retrieval, IPC, and task throughput.

## 9. Workbench UX integration

Preserve Shipyard's recognizable layout, navigation, density, typography, spacing, and interaction language. Extend its information architecture rather than replacing it.

Integrate:
- Projects/workspaces and worktrees.
- Chat/session surface with selectable agent/provider/harness.
- Agent roster and task/delegation tree.
- Workflow builder, run history, routines, and automation.
- Skills, context, memory, knowledge, and project intelligence.
- Apps/artifacts and structured tables.
- Approvals, budgets, audit/activity, diffs, evidence, and review inbox.
- Extension catalog/manager and capability configuration.
- Settings, provider connections, diagnostics, and platform status.

New pages and panels must use existing design tokens and reusable components. UI changes require screenshot-based regression against the current Shipyard UI and accessibility/keyboard review.

## 10. Engineering Intelligence integration

Engineering Memory and Code Intelligence remain in the existing Engineering Intelligence taxonomy:
- Code Intelligence extracts language-aware symbols, references, types, calls, APIs, schemas, and source evidence.
- Engineering Memory stores governed structural, architectural, decision, procedural, operational, failure, and verification knowledge with provenance, confidence, revision, freshness, correction, supersession, expiry, and deletion.
- Retrieval combines exact/lexical, graph, and optional semantic retrieval with explicit ranking and evidence.
- Context Engineering creates bounded, explainable context packets.
- Verification and Learning use actual tests, static analysis, review outcomes, and execution evidence; model claims alone never become verified facts.

No duplicate memory/RAG subsystem is to be created for the AgenticOS-inspired features.

## 11. Validation gates and open technical work

Before implementation choices are frozen:
1. Audit Shipyard source, tests, APIs, plugin SDK, flow engine, security, license/notices, release pipeline, and Windows/macOS/Linux support.
2. Audit AgenticOS feature implementations and current documentation for the selected patterns; record source references and avoid copying backend assumptions.
3. Define the canonical Agent Spec and Workflow Spec, including migrations from existing Shipyard constructs.
4. Prototype durable workflow pause/resume, approvals, event triggers, and bounded agent delegation.
5. Validate extension tiers, especially WASM host capabilities and process isolation, with threat-model tests.
6. Build and package a Windows desktop vertical slice including Go runtime, Rust/Grafeo sidecar, IPC, installer, update, and recovery.
7. Validate UI integration using the existing Shipyard design system and screenshot regression.
8. Benchmark memory, startup, indexing, retrieval, workflow execution, and concurrency on representative low-resource and standard hardware.

No numeric RAM requirement or performance claim is approved until measured on named hardware and datasets.

## 12. Licensing and provenance

Shipyard is MIT and AgenticOS is Apache-2.0 according to their current repository metadata. Reuse patterns and code are different decisions: document whether each adopted item is (a) independently reimplemented from observed behavior, (b) adapted from source, or (c) used through an integration. Preserve required notices and assess Apache-2.0 NOTICE/patent/attribution obligations and dependency licenses before copying code. Do not copy assets, UI text, or code without a provenance review.

## 13. Acceptance criteria

- Shipyard remains the clear operational/workbench foundation.
- AgenticOS contributions are mapped to explicit capabilities and owners; its full server backend is not imported.
- Exactly one canonical workflow definition and execution substrate serves visual workflows, routines, hooks/flows, CLI/API, and events.
- Agent Specs and Workflow Specs are versioned, validated, testable, and permission-aware.
- Engineering Memory and Code Intelligence remain integrated in Engineering Intelligence without parallel stores.
- Extensions share discovery, manifests, permissions, lifecycle, and audit while using tier-appropriate isolation.
- Windows desktop is a committed target with demonstrated clean-install, runtime, sidecar, IPC, update, and recovery evidence.
- Shipyard's existing UI language is preserved and expanded consistently.
- Planned/research capabilities are not represented as implemented.
