# Coding Harness — Product Scope and Intelligence Taxonomy

> Status: Consolidated product scope derived from the project discussions and accepted architecture direction. Capability domains are logical boundaries, not a mandate for separate processes, microservices, or always-running agents. Items marked as research/open are not approved implementation choices.

## 1. Product definition

Coding Harness is a developer workbench and agent-engineering platform. It combines three cooperating planes:

1. **Developer Workbench and Product Experience** — the surfaces through which developers manage projects, workspaces, sessions, plans, permissions, agent activity, diffs, evidence, settings, and diagnostics.
2. **Operational Runtime** — the execution foundation for projects, worktrees, sessions, terminals, agent processes, tasks/runs, hooks, flows, remote execution environments, and durable operational events. Reuse suitable upstream Shipyard capabilities if the source audit confirms a safe and maintainable integration seam.
3. **Engineering Intelligence** — the intelligence system that understands software projects, maintains governed knowledge, retrieves task-relevant evidence, plans engineering work, selects specialists, guides tools, verifies outcomes, and learns from feedback.

The product is not merely an Engineering Intelligence library, a prompt/skill bundle, an MCP collection, a model, or a thin wrapper around one coding agent. The product value comes from the combination of an operational environment, an intelligence layer, and a usable developer-facing workbench.

## 2. Product planes and ownership

### 2.1 Developer Workbench

Target responsibilities:
- Project and workspace discovery, creation, opening, switching, and lifecycle visibility.
- Repository connection, onboarding, trust review, initial indexing progress, and index health.
- Worktree and branch visibility, including task isolation and change ownership.
- Agent/session management: create, resume, inspect, interrupt, cancel, and review sessions where the selected runtime supports them.
- Terminal, command, task/run, and execution output visibility.
- Task intake, plan review, acceptance criteria, approval gates, and progress.
- Specialist assignments, delegation tree, event timeline, and inter-agent handoffs.
- Diff/file review, artifacts, test results, verification evidence, and residual-risk summaries.
- Tool/extension installation and management, permissions, provider configuration, and harness connections.
- Project intelligence views: architecture map, dependency/impact exploration, knowledge/evidence browser, and memory/decision review.
- Global settings, model/provider policy, resource budgets, privacy controls, logs, diagnostics, updates, and recovery guidance.

The initial surface (desktop, CLI, or both) remains an explicit product decision. Tauri 2 + React + TypeScript is a candidate desktop stack inherited from the Shipyard evaluation context, not a frozen decision. Do not claim mobile parity; a mobile companion is a later exploration only.

### 2.2 Operational Runtime

Owns operational lifecycle and execution concerns:
- Project/workspace and environment lifecycle.
- Git worktrees, branches, task isolation, and repository operations.
- Agent/session process lifecycle and supported harness invocation.
- Terminal and command execution under permission policy.
- Durable tasks/runs, event journal, progress, artifacts, cancellation, retries, and recovery.
- Hooks, flows, and operational automation.
- Remote boxes or remote execution targets when supported by the chosen operational foundation.
- Runtime health, logs, resource/process supervision, and upgrade compatibility.

Shipyard upstream is an operational reference and candidate reuse foundation. The project must audit actual source, tests, APIs, license, security, platform support, and packaging before choosing API integration, plugin/adapter integration, selective reuse, or independent implementation. No fork, source copy, or mandatory dependency is presumed.

### 2.3 Engineering Intelligence

Owns project understanding, engineering knowledge, task intelligence, and evidence quality. It must be callable by the runtime and usable independently of a specific UI, model provider, or coding harness.

## 3. Engineering Intelligence domains

The following taxonomy makes the previously discussed intelligence areas explicit. A domain may be implemented as a Rust module, library, service boundary, or extension contract depending on measured need; it is not automatically a separate process or autonomous agent.

### A. Project and software understanding

1. **Repository Intelligence** — repository discovery, Git/worktree/revision awareness, monorepo/package boundaries, file inventories, ignore rules, generated/vendor detection, and index coverage.
2. **Code Intelligence** — language-aware parsing, ASTs, symbols, references, imports/exports, call relationships, types, APIs, schemas, and language-server/compiler integration.
3. **Architecture Intelligence** — component boundaries, layers, runtime topology, data flows, external services, deployment units, architectural conventions, ADR alignment, and architecture drift.
4. **Dependency and Impact Intelligence** — dependency graphs, callers/consumers, affected modules/APIs/configuration/data, change blast radius, test selection hints, and risk areas.
5. **Runtime and Environment Intelligence** — build/test/run entry points, environment/configuration understanding, service topology, deployment/runtime constraints, and operational diagnostics, without exposing secrets as memory.

### B. Knowledge and context

6. **Knowledge Ingestion and Provenance Intelligence** — typed ingestion from code, Git, tests, CI, docs, ADRs, issues, tools, reviews, and user-approved inputs; normalization, source locators, hashes, revisions, extractor versions, and trust metadata.
7. **Project Memory and Continuity Intelligence** — structural, architectural, decision, convention, operational, task, failure, verification, and approved preference/policy memory; lifecycle, correction, supersession, expiry, invalidation, deletion, and rebuild.
8. **Retrieval Intelligence** — graph, lexical, semantic, and deterministic retrieval; query planning, freshness/conflict handling, ranking, evidence diversity, and explainable retrieval.
9. **Context Engineering Intelligence** — task-specific context packets, token/size budgets, deduplication, evidence citations, inclusion rationale, uncertainty, and missing-evidence disclosure.

### C. Task reasoning and collaboration

10. **Planning and Decomposition Intelligence** — intent interpretation, requirement clarification, task decomposition, dependency ordering, acceptance criteria, risk assessment, and execution strategy.
11. **Agent Selection and Orchestration Intelligence** — dynamic specialist selection, capability matching, delegation contracts, bounded parallelism, worktree ownership, retries, cancellation, integration, and escalation.
12. **Collaboration and Review Intelligence** — plan critique, independent review, adversarial review, alternative implementation comparison, conflict reconciliation, and human handoff. These are selectable collaboration patterns, not mandatory agents for every task.

### D. Tools, execution, and quality

13. **Tool and Action Intelligence** — typed tool discovery/selection, capability and permission checks, deterministic tool invocation, side-effect awareness, idempotency, timeout/cancellation, and evidence normalization.
14. **Execution Intelligence** — execution strategy, command/check sequencing, runtime coordination, progress tracking, failure classification, and controlled retries. Actual process execution remains owned by the Operational Runtime.
15. **Verification and Quality Intelligence** — verification-plan generation, test/lint/type/build/security checks, diff-scope review, evidence capture, flaky-test treatment, and explicit Passed/Failed/Blocked/Not Run/Inconclusive outcomes.
16. **Learning and Feedback Intelligence** — use reviewer corrections, task outcomes, failures, and verification evidence to improve retrieval, planning, tool selection, and specialist routing without silently changing accepted policy.

### E. Model, platform, and governance

17. **Model and Provider Intelligence** — model capability discovery, local/cloud provider selection, policy-aware routing, context limits, structured output/tool support, privacy, latency/cost accounting, rate limits, retries, and explicit user-controlled fallback.
18. **Harness Interoperability Intelligence** — capability discovery and adapters for coding harnesses, sessions, tools, MCP, skills, worktrees, streaming, cancellation, artifacts, and native subagents where actually supported.
19. **Security, Trust, and Policy Intelligence** — repository/extension trust, prompt-injection resistance, project isolation, capability permissions, approval policy, secret handling, audit, retention, export, and deletion governance.
20. **Resource and Performance Intelligence** — resource-aware scheduling, indexing/query budgets, bounded concurrency, backpressure, caching, selective embeddings, local-vs-cloud workload choices, and CPU/RSS/disk/latency measurement.
21. **Observability and Evaluation Intelligence** — correlated traces, retrieval and evidence metrics, task outcomes, false-success rates, agent/rework metrics, reproducible benchmarks, diagnostics, and quality regression detection.
22. **Extension and Ecosystem Intelligence** — unified discovery, compatibility, dependency resolution, permissions, lifecycle, provenance, integrity, sandboxing, revocation, and governance across tools, MCP, skills, agents, workflows, providers, parsers/indexers, harness adapters, and UI plugins.

## 4. Shared contracts and evidence flow

All domains should communicate through typed, versioned contracts. At minimum define:
- Project, repository, workspace, worktree, and revision identity.
- Ingestion event, extracted entity, relationship, evidence, and provenance.
- Memory item, lifecycle state, approval, correction, and invalidation.
- Retrieval request/result and context packet.
- Architecture/dependency/impact query and result.
- Task plan, subtask, acceptance criterion, risk, and dependency.
- Agent capability, delegation request, structured result, and artifact.
- Tool capability, invocation, permission decision, result, and side-effect record.
- Model/provider capability, invocation, usage, and policy decision.
- Harness session, event, cancellation, artifact, and exit state.
- Verification plan, check result, evidence, and completion report.
- Extension manifest, compatibility, permission, and lifecycle event.
- Trace, metric, diagnostic, and audit event.

Every engineering claim should be traceable to evidence and a repository revision where applicable. Model-generated hypotheses must remain hypotheses until independently supported or explicitly accepted. Source code, Git, tests, and approved project decisions remain authoritative.

## 5. End-to-end product workflow

1. Open or connect a project/workspace and establish project identity and trust boundaries.
2. Discover repository structure and start observable, incremental indexing.
3. Present detected stack, architecture, dependencies, build/test commands, coverage gaps, and proposed knowledge for review.
4. Receive an engineering task and identify missing requirements or ambiguity.
5. Retrieve task-specific evidence and assemble a bounded context packet.
6. Produce a plan, risks, dependencies, acceptance criteria, and proposed permission scope.
7. Select a suitable execution strategy and only the specialists/tools/harness capabilities needed.
8. Execute through the Operational Runtime, using isolated worktrees where supported.
9. Collect events, diffs, artifacts, logs, and deterministic verification evidence.
10. Independently review and verify the result; expose skipped checks and residual risks.
11. Present a reviewable completion report and request approval for consequential actions.
12. Propose memory/knowledge updates with provenance; apply the appropriate approval policy.
13. Preserve task history and feedback for future retrieval and evaluation.

## 6. Data and storage boundaries

- **Grafeo** is the sole approved graph engine for structured engineering knowledge, owned by the Rust Intelligence Sidecar. No graph fallback.
- **SQLite** is for application/system state only: settings, project registry, job/task metadata, extension registry metadata, and migration bookkeeping. It is not a graph fallback.
- Source repositories and Git are authoritative. Graph and indexes are derived and rebuildable where possible.
- Large raw artifacts (logs, reports, patches, generated files) belong in file/object artifact storage with stable references and hashes, not as wholesale graph properties.
- Lexical search, semantic/vector search, and graph traversal are distinct retrieval workloads. Their exact storage/index placement remains a measured research/POC question; do not assume all belong in Grafeo or create duplicate indexes without evidence.
- External memory/retrieval systems may be optional providers or integrations. They must not silently replace the accepted Grafeo architecture or become mandatory dependencies for core functionality.

## 7. Deployment and resource principles

- Local-first deterministic project understanding and verification must remain useful without a paid cloud model.
- Support local and cloud model providers through explicit configuration and data-movement policy.
- Support resource-constrained devices with incremental indexing, bounded concurrency, cancellation, backpressure, caching, selective embeddings, and configurable budgets.
- Do not require every intelligence domain to run continuously. Schedule or invoke capabilities on demand.
- Desktop targets discussed are Windows, macOS, and Linux; validate packaging, signing, process supervision, filesystem permissions, and IPC separately per OS.
- Remote execution and remote intelligence placement require explicit trust, data residency, latency, and lifecycle decisions.
- Mobile is not an assumed v1 target.

## 8. Explicitly open / research-only areas

The following are not frozen decisions and must not be described as implemented or approved:
- Exact first-release product surface (desktop, CLI, or both) and final UI framework.
- Final Shipyard integration method and exact upstream revision/license implications.
- Exact first-class coding harnesses and capability matrix.
- Exact local/cloud model provider roster, routing policy, and model requirements.
- Exact graph schema v1 and Grafeo version/features after the POC.
- IPC transport and sidecar supervision details.
- Lexical index and semantic/vector storage placement; embedding strategy and local model requirements.
- External memory/retrieval integrations (including systems previously explored) and whether any offer measurable value beyond native capabilities.
- Team synchronization, shared memory, remote intelligence, and collaboration scope.
- Marketplace timing, extension sandbox technology, signing, and publisher governance.
- Supported languages and language-specific extraction technologies for MVP.
- Performance budgets and acceptance thresholds, to be established from representative benchmarks.

## 9. Product guardrails

- Do not collapse the whole product into Engineering Intelligence alone.
- Do not turn every intelligence domain into a separate microservice or agent.
- Do not duplicate Shipyard operational features before source-level audit.
- Do not make MCP or third-party plugins prerequisites for core native features.
- Do not require paid/cloud AI for deterministic core capabilities.
- Do not leak private repository data to a cloud provider without explicit policy.
- Do not promote generated guesses to accepted memory.
- Do not report a task as complete without verifiable evidence.
- Do not claim planned features are implemented.
