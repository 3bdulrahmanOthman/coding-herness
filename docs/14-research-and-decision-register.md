# Research and Decision Register

> Purpose: preserve the technical alternatives and research questions discussed for Coding Harness, while distinguishing decisions from hypotheses. This register is not a substitute for source-level audits, official documentation checks, or reproducible benchmarks.

## 1. Accepted decisions (do not silently reopen)

| Area | Accepted direction | Validation still required |
|---|---|---|
| Graph engine | Grafeo is the sole graph engine | Pin exact release/toolchain; validate API, persistence, query behavior, memory, concurrency, recovery, backup/restore, and platforms |
| Graph integration | Native Rust API inside a Rust Intelligence Sidecar | POC, packaging, lifecycle, IPC, supervision |
| Graph fallback | None | Define visible degraded/unavailable behavior |
| System state | SQLite only for application/system state | Schema, migrations, backup, ownership |
| Intelligence execution | Hybrid deterministic + model-based | Capability routing, evaluation, failure behavior |
| Extensions | Unified Extensions Ecosystem | Manifest, permissions, lifecycle, compatibility, sandboxing |
| Agent topology | Orchestrator with dynamically selected specialists | Capability registry, contracts, bounded delegation, evaluation |
| Operational/workbench foundation | Shipyard is the selected product foundation | Complete source/test/security/license/platform audit; per-capability reuse mechanism remains open |

## 2. Data and knowledge architecture research

### 2.1 Distinct workloads

Do not use the word "database" as if one store must solve every problem. Evaluate separately:
- Graph traversal: typed entities/relationships, dependency paths, architecture and impact queries.
- Lexical search: exact symbols, identifiers, paths, error messages, code fragments, and configuration keys.
- Semantic retrieval: conceptual similarity across architecture docs, ADRs, requirements, curated notes, and selected code/documentation.
- Durable system state: transactions, settings, jobs, project registry, extension metadata.
- Artifact storage: large logs, reports, patches, generated outputs, and benchmark files.
- Source-of-truth storage: repository files and Git history.

### 2.2 Alternatives and constraints previously discussed

- Neo4j was explored but its operational footprint was a concern for a resource-constrained, local-first desktop product. It was not selected.
- LadybugDB was discussed as a potentially lighter graph alternative; it was not selected.
- SQLite was discussed as a lightweight persistence/system-state option, but explicitly not approved as a graph fallback.
- Grafeo was selected as the sole graph engine, with native Rust integration through the sidecar and no fallback.
- SQLite remains useful for application/system state, not graph semantics.
- Graphiti and other external memory/retrieval systems were discussed as possible integration/provider ideas, not as mandatory core dependencies or replacements for Grafeo.
- FTS5, Grafeo text-search capabilities, dedicated lexical indexes, embeddings, and vector retrieval need workload-based comparison. Do not assume one index should store all content.

### 2.3 Required research outputs

- Query workload catalog tied to actual product use cases.
- Dataset fixtures representing small, medium, large, and dense repositories/graphs.
- Comparison of exact-match, lexical, graph, and semantic retrieval quality.
- Index duplication, update cost, disk/RSS, query latency, and rebuild cost.
- Provenance, revision, deletion, rename, and stale-data behavior.
- Reproducible decision record with explicit trade-offs.

## 3. Runtime and operational foundation research

Inspect the original upstream Shipyard repository, not the deleted fork. Audit:
- Go daemon, CLI, API, project/box/worktree/session lifecycle.
- Agent process management, terminal/command execution, event journaling, task/run durability, retries, cancellation, and recovery.
- Hooks, flows, agent orchestration, plugin SDK, desktop shell, and UI extension model.
- Tauri commands, sidecar packaging, app updates, signing, OS support, CI, and release process.
- SQLite usage and schema/migration ownership.
- Authentication, local/remote transport, trust prompts, permissions, sandboxing, and threat model.
- License, notices, dependency license obligations, contribution provenance, and redistribution constraints.

Compare supported API integration, plugin/adapter integration, carefully selected source reuse, and independent implementation only for genuine gaps. Do not copy/vendor code before license and maintenance review.

## 4. Model and harness research

Create a capability matrix rather than assuming provider parity.

For models/providers, measure/record:
- Local vs cloud, supported OS/hardware, memory/VRAM, quantization and context limits.
- Tool calling, structured output, streaming, multimodal support, cancellation, rate limits, and reliability.
- Quality by task category, latency, cost, privacy/data retention, and offline behavior.
- Explicit provider routing and user-approved fallback behavior.

For coding harnesses, inspect:
- Session lifecycle, tool invocation, MCP/skills, file/command access, worktrees, streaming/events, cancellation, artifacts, usage metadata, native subagents, and remote execution.
- Adapter contract and capability degradation behavior.
- Security and permission boundaries for each integration.

## 5. Platform and product research

- Desktop shell and UI framework; desktop targets Windows/macOS/Linux.
- CLI/API scope and whether both ship with the first usable release.
- Project onboarding, workspace model, multi-project navigation, sessions, terminals, worktrees, agent timeline, plan approval, diff review, evidence browser, settings, diagnostics, and updates.
- Remote boxes, remote execution, team sharing, sync, and shared memory.
- Extension UI, manifests, signatures, sandboxing, permissions, lifecycle, and eventual catalog/marketplace.
- Accessibility, keyboard navigation, localization, data export/deletion, and upgrade/recovery experience.

## 6. Resource and performance research

Benchmark complete processes and realistic end-to-end workloads:
- Sidecar and runtime RSS, CPU, disk I/O, startup time, idle footprint, and power/resource behavior.
- Initial and incremental indexing throughput and cancellation/resume.
- Retrieval p50/p95/p99 latency and throughput.
- Graph storage growth, index duplication, backup/restore, crash recovery, and rebuild time.
- Local model hardware requirements separately from deterministic core requirements.
- Concurrent agents and tasks under constrained RAM/CPU.
- Backpressure, quotas, fairness, and graceful degradation.

Do not establish numeric budgets before representative hardware and datasets are selected.

## 7. Security and trust research

Threat model:
- Malicious repositories and executable project configuration.
- Prompt injection in source, docs, issues, webpages, MCP/tool output, and agent artifacts.
- Cross-project/worktree leakage and stale/poisoned memory.
- Extension compromise, dependency confusion, malicious updates, and publisher impersonation.
- IPC spoofing, path traversal, symlinks, command execution, process ownership, and secret leakage.
- Agent privilege escalation and unsafe external side effects.
- Cloud data movement, retention, telemetry, and user consent.

Required outputs: threat model, trust boundaries, capability permission matrix, platform sandbox matrix, secret-handling policy, audit events, retention/export/deletion rules, and security test suite.

## 8. Evaluation research

Establish baseline workflows and representative tasks. Measure:
- Repository understanding and extraction coverage.
- Retrieval precision/recall, evidence coverage, freshness, and conflict rate.
- Impact analysis precision/recall.
- Plan quality, task success, regression rate, and rework.
- Verification coverage and false-success rate.
- Agent selection/delegation efficiency and conflict rate.
- Context token cost, latency, model cost, CPU/RSS, disk, and recovery.
- Extension failures and security events.

Keep expert-labeled datasets, benchmark versions, environment details, raw results, and methodology reproducible.

## 9. Decision hygiene

Each result must be labeled one of:
- **Accepted:** explicitly approved direction.
- **Validated:** demonstrated by reproducible test/evidence.
- **Proposed:** recommendation awaiting approval.
- **Research:** investigation without product commitment.
- **Rejected:** explicitly ruled out, with reason.
- **Superseded:** replaced by a later decision.

A research result does not silently change an accepted ADR. A technical blocker must be presented with evidence for an explicit product decision.

## 10. Autonomous execution and governance research

Prior discussions explored a target system that can coordinate a complete feature lifecycle, not merely spawn subagents. The required investigation is captured in [Autonomous Engineering Execution and Governance](15-autonomous-engineering-execution-and-governance.md).

### 10.1 External runtime and orchestration candidates

- **DeepSeek Harness (DSH):** prior discussion described Subagent/Workflow primitives (parallel, pipeline, phase, nested delegation, lifecycle controls, JavaScript customization). Verify exact upstream release and API. Do not assume durable resume, worktree isolation, retry/recovery, independent review gates, GitHub PR/CI lifecycle, or global budgets are complete.
- **Sandcastle:** previously discussed as a TypeScript orchestration library for isolated agent runs, branches/worktrees, parallel tasks, testing/review, retries, and merge workflows. Treat as a candidate, not a dependency or accepted architecture decision.
- **Shipyard:** selected operational/workbench foundation; its exact API/plugin/source-reuse boundaries remain subject to the source-audit issue. Do not conflate its process/workspace lifecycle with Engineering Intelligence.
- **Codex and Claude Code:** evaluate as harness adapters with capability discovery, not as equivalent runtimes.

### 10.2 Governance and workflow concepts carried forward

The following concepts appeared in earlier agent-operating-model discussions and should be evaluated as Coding Harness requirements:
- Progressive Governance and risk-tiered approval.
- Controlled Workflow Selection instead of one fixed pipeline.
- Evidence-first stage transitions and explicit acceptance invariants.
- Context Health/Compaction and preservation of critical context across handoffs.
- Checkpoints only where interruption/recovery needs justify them.
- A central policy plane for permissions, identity, budgets, data movement, and approvals.
- Runtime identity/attestation where feasible.
- Telemetry, detection, incident recovery, and production-readiness gates.

These concepts are target requirements for design/validation, not claims that an existing AOS implementation or separate product is part of this repository. “AOS” had no confirmed full expansion in the prior discussion.

### 10.3 Product/workflow history and status

- The early medium-sized Next.js SaaS/workspace concept with Skills, MCP, and project-wide Spec Kit tracking was exploratory; it does not establish the final product stack or first-release surface.
- The user emphasized composing existing ESM/skills/MCP/AGENTS.md and project workflows rather than adding a separate Harness skill or duplicating capabilities blindly.
- A core goal is to reduce agent wandering and token waste through scoped context, targeted research, workflow selection, budgets, and explicit stop/retry/handoff behavior.
- Preserve project specifications, architecture decisions, task plans, acceptance criteria, and evidence as linked/versioned artifacts.
- Skills are reusable procedures; MCP is tool interoperability; workflows are orchestration patterns; runtime is durable execution; governance decides whether transitions are allowed.

### 10.4 Required research outputs

- Current-source capability matrix and integration spike for DSH/Sandcastle/selected harnesses.
- Task/slice contract and state-machine schema with durable event model.
- Worktree ownership, nested delegation, retry, cancellation, and recovery tests.
- Risk-tiered policy/approval matrix and evidence-gate test suite.
- Context-health/compaction evaluation and token/cost budget methodology.
- End-to-end baseline tasks measuring quality, coordination overhead, false-success, recovery, and resource consumption.


## 11. Additional accepted product direction: Shipyard + AgenticOS

| Area | Direction | Remaining validation |
|---|---|---|
| Product foundation | Shipyard is the operational/workbench foundation | Source-level audit and per-capability selective reuse decisions |
| AgenticOS | Adopt selected patterns/capabilities; do not import its full server backend | Feature-by-feature source audit and provenance |
| Engineering Memory / Code Intelligence | First-class domains within Engineering Intelligence | Implement through the approved knowledge and indexing architecture; no parallel subsystem |
| Workflow architecture | One canonical Workflow Spec and one durable execution substrate | Audit Shipyard flows; validate pause/resume, triggers, approvals, retries, migration |
| Agent definitions | Versioned, schema-validated Agent Specs with capability, permission, budget, and test metadata | Contract and compatibility tests |
| Extensions | Unified ecosystem with native, WASM, isolated-process, remote/MCP, and UI tiers | WASM host API, sandbox, threat model, compatibility and packaging POC |
| Desktop target | Windows edition is explicitly required; macOS/Linux remain platform targets | Clean-install packaging, signing, terminal/process behavior, IPC, sidecar, updates, recovery |
| UI direction | Preserve Shipyard's visual language and extend it | Design-token inventory and screenshot/accessibility regression |
| Languages | Go operational runtime where validated; Rust intelligence sidecar; TypeScript/React workbench | Benchmark and upstream compatibility audit |

The detailed decision and implementation contract is [16-shipyard-agenticos-integration-plan.md](16-shipyard-agenticos-integration-plan.md). These decisions do not imply implementation is complete.
