# Engineering Intelligence — Complete Capability Specification

> Status: Approved product/architecture direction and implementation specification. This document does not claim runtime capabilities are implemented or validated.

## 1. Mission and boundaries

Engineering Intelligence (EI) is the engineering knowledge, reasoning-support, and verification layer used by Coding Harness. It augments coding agents; it is not itself a model, prompt collection, generic workflow bundle, or replacement for source control and developer judgment.

EI helps agents understand projects, retrieve evidence, plan safe changes, use deterministic engineering tools, coordinate specialists, verify outcomes, and preserve approved knowledge across tasks and sessions.

The layer remains model-, provider-, and coding-harness-agnostic through explicit capability adapters. It must work on resource-constrained machines and support local and remote/cloud model configurations subject to user policy.

These are cooperating subsystems, not necessarily separate processes or microservices. Keep a modular architecture and add process boundaries only for concrete isolation, platform, or scaling needs.

## 2. Complete capability map

1. Repository and codebase understanding.
2. Project, architecture, runtime, and dependency understanding.
3. Knowledge ingestion, normalization, provenance, and lifecycle.
4. Project memory and cross-session continuity.
5. Hybrid retrieval and context engineering.
6. Deterministic engineering tools and tool orchestration.
7. Planning, task decomposition, and specialist-agent orchestration.
8. Model/provider routing and coding-harness interoperability.
9. Verification, review, and evidence-backed completion.
10. Learning from outcomes without silently rewriting accepted project policy.
11. Security, privacy, governance, observability, and evaluation.
12. Unified extensions and external integrations.

## 3. Repository and codebase understanding

### Repository discovery
- Detect repository roots, Git metadata, branches, worktrees, submodules, monorepos, package/workspace boundaries, generated output, vendored dependencies, and ignored paths.
- Detect languages, frameworks, build systems, package managers, test runners, deployment/runtime configuration, and infrastructure components.
- Respect ignore rules, generated-file policies, secrets exclusions, and user-defined indexing boundaries.
- Provide an explainable project inventory and indexing status.

### Structural understanding
- Extract files, directories, packages, modules, symbols, types, functions, methods, interfaces, imports, exports, call relationships where supported, API routes, schemas, configuration keys, and test relationships.
- Prefer deterministic parsers, compiler databases, language servers, ASTs, and build metadata over model-only interpretation.
- Use language-specific adapters behind stable extraction contracts. Record parser/tool versions and extraction coverage.
- Distinguish confirmed relationships from heuristic/inferred relationships.

### Change and architecture understanding
- Map Git revisions, diffs, changed symbols/files, affected dependents, relevant tests, public interfaces, and risk areas.
- Support impact queries such as callers/dependents, API consumers, configuration usage, data flow where available, and tests covering a symbol/module.
- Track branch/worktree/revision identity. Never blend incompatible snapshots without explicit revision metadata.
- Represent architectural boundaries, layers, components, runtime topology, data stores, external services, deployment units, and important flows.
- Detect drift between documented architecture and repository evidence.
- Mark uncertainty and missing language coverage instead of fabricating complete dependency graphs.

## 4. Knowledge ingestion and provenance

All knowledge enters through typed ingestion pipelines, not arbitrary unstructured graph writes.

Sources include source code and AST output; Git history/diffs/branches/worktrees; tests, coverage, static analysis, build and CI output; project docs, API specifications, schemas, infrastructure/configuration; ADRs, requirements, constraints, conventions, approved decisions; user-approved notes/corrections; agent plans, tool results, review findings, and verification artifacts.

Every record carries applicable provenance:
- project/repository/worktree/revision identity;
- stable entity or claim ID;
- source URI/path and structural locator (symbol, line range, test ID, config key);
- content hash and source revision;
- ingestion/extractor/tool name and version;
- observed/indexed timestamps;
- evidence category and confidence;
- verification/approval state;
- supporting and contradicting evidence links.

Treat repository content, external documents, model output, tool output, and extension output as untrusted data. Retrieved content is evidence, not executable instruction.

## 5. Project memory model

Memory is a governed knowledge lifecycle, not a transcript dump.

### Memory categories
- Structural: repository map, modules, symbols, dependencies, interfaces.
- Architectural: components, boundaries, design rationale, data flows.
- Decision: accepted/rejected decisions, ADRs, constraints, supersession history.
- Convention: coding standards, patterns, build/test commands, project practices.
- Operational: environments, workflows, known commands, deployment constraints, non-secret facts.
- Task: plans, affected areas, decisions, artifacts, unresolved follow-ups.
- Failure: verified bugs, root causes, regressions, failed approaches, regression tests.
- Verification: test coverage, validation gaps, reproducible checks, evidence.
- Preference/policy: explicit user/team-approved preferences and permissions, with scope and precedence.

### Memory trust and write policy
- Use explicit states such as Candidate, Observed, Verified, Accepted, Superseded, Expired, and Invalidated.
- Model-generated claims are never promoted to Accepted solely by generation or repetition.
- Define which events can create/update each memory category.
- Require evidence and source revision for repository-derived facts.
- Require explicit approval for durable user/team preferences or policy changes unless narrowly authorized.
- Preserve history for important decisions and contradictory evidence.
- Support correction, expiry, invalidation, deletion, and rebuild.
- Prevent stale or low-confidence memory from overriding current source code, tests, or accepted ADRs.

### Storage boundaries
- Grafeo is the sole graph engine, owned by the Rust Intelligence Sidecar. No graph fallback.
- SQLite stores application/system state only (project registry, settings, jobs, migration bookkeeping, task metadata, extension registry metadata); it is not a graph fallback.
- Source repositories and Git remain authoritative. Derived graph/index/memory records must be rebuildable where possible.
- Store large raw artifacts as files/object artifacts with stable references and hashes, not wholesale graph properties.

## 6. Knowledge graph and retrieval

Use Grafeo to represent meaningful engineering entities and typed relationships, not every token or line. Graph Schema v1 must be driven by actual retrieval, impact-analysis, traceability, and verification queries.

Candidate entities: Project, Repository, Worktree, Revision, Directory, File, Package, Module, Symbol, Type, Function, Method, API, Endpoint, DataEntity, ConfigKey, Dependency, Requirement, Constraint, Convention, ArchitectureComponent, ArchitectureDecision, Test, Build, DeploymentUnit, ExternalService, KnowledgeClaim, Evidence, Task, AgentRun, ToolExecution, Finding, VerificationResult.

Candidate relationships: CONTAINS, DEFINES, IMPORTS, CALLS, EXTENDS, IMPLEMENTS, DEPENDS_ON, EXPOSES, READS, WRITES, CONFIGURES, TESTS, COVERS, DOCUMENTS, REFERENCES, AFFECTS, INTRODUCES, REMOVES, SUPPORTED_BY, CONTRADICTS, SUPERSEDES, INVALIDATES, PRODUCED_BY, VERIFIED_BY.

Schema v1 must define stable IDs, project/revision scoping, required properties, constraints where supported, provenance edges, lifecycle states, indexes, migrations, and deletion semantics. The candidate vocabulary is not a requirement to implement every type immediately.

### Retrieval modes
- Graph traversal for typed relationships, dependency paths, architecture and impact queries.
- Lexical retrieval for exact identifiers, paths, errors, symbols, and code terms.
- Semantic retrieval for conceptual similarity across docs, ADRs, requirements, and curated knowledge.
- Deterministic direct lookup through Git, filesystem, language servers, AST indexes, tests, and build tools.
- Optional temporal/revision-aware retrieval for historical questions and change analysis.

Do not assume one database/index must implement every retrieval mode. Select components through measured results and avoid duplicate indexes without a demonstrated need. Grafeo remains the only graph engine.

### Ranking and context assembly
Rank evidence by task relevance, source authority, revision freshness, verification state, confidence, structural proximity, and diversity. Deduplicate overlapping evidence and respect token/context budgets.

Every context packet includes project/task scope; evidence and source locators; revision/index freshness; inclusion rationale; confidence/verification state; conflicts and missing coverage; unresolved questions; and token/size accounting. Retrieval must explain why evidence was returned and support reproducible evaluation.

## 7. Deterministic engineering tools

Use deterministic tools wherever they answer or validate a question more reliably than model inference.

Categories: filesystem/repository inspection; Git history/diff/blame/worktree; AST/symbol/reference/call-graph/language-server queries; package/dependency/vulnerability inspection; format/lint/typecheck/compile/build; unit/integration/E2E/regression/property tests; static/security/secret/license/SBOM scans; API/schema/migration validation; profiling/benchmarks/resource measurement; docs/generated-artifact checks; authorized runtime/log/trace inspection.

Define a typed tool registry and contracts: input/output schema, capability, supported OS, required permissions, side effects, timeout, cancellation, resource limits, idempotency, provenance, and error taxonomy. Normalize tool outputs into evidence artifacts. Destructive or externally consequential tools require authorization and policy checks.

## 8. Planning and agent orchestration

The Orchestrator owns intent understanding, planning, decomposition, context acquisition, specialist selection, delegation, progress tracking, integration, verification planning, escalation, and final reporting.

Select specialists dynamically by task, project stack, capability declarations, tool availability, model/harness compatibility, permissions, quality history, latency, and cost/resource budgets. Candidate roles include repository analyst, architect, planner, frontend/backend/database engineer, security reviewer, test engineer, performance engineer, documentation specialist, reviewer, and release engineer. This is not a fixed mandatory roster.

Every delegated task specifies objective, scope/non-scope, project revision/worktree, constraints/ADRs, evidence, expected artifacts, acceptance criteria, tests, ownership boundaries, tools/permissions, budgets, and escalation conditions.

Agent outputs are structured and include changed files, decisions, assumptions, evidence, commands/tests, results, failures, and remaining risks. The Orchestrator validates output before integration.

Support sequential handoff, bounded independent parallelism, review agents, plan critique, adversarial review, implementation/verification loops, and human escalation. Prevent circular delegation and unbounded spawning. Use isolated worktrees where supported; track retries, idempotency, cancellation, ownership, and merge conflicts.

## 9. Model and provider interoperability

Define a provider-neutral model capability contract. Track text, structured output, tool calling, streaming, context window, multimodal input, reasoning controls, and provider limitations. Separate provider transport/authentication from model capability and policy.

Support explicit provider configuration; policy-based routing by task/capability/privacy/context/quality/latency/cost; local and cloud providers; user-controlled fallback only among explicitly permitted providers; retry/backoff/rate limits/timeouts/cancellation; token/cost/latency accounting; data residency/retention constraints; reproducible provider configuration in task traces.

Never silently send private repository content to a cloud provider or silently switch provider/model against user policy. Deterministic capabilities remain available when model providers are unavailable.

## 10. Coding-harness interoperability

Treat coding harnesses as execution environments behind adapters. Discover and advertise capabilities instead of assuming a universal feature set.

The adapter contract covers, where supported: create/resume/cancel sessions; submit instructions and receive events; expose tools/MCP and skills; create worktrees or isolated contexts; read/write files and run commands under permission policy; report changed files, diffs, artifacts, usage, and exit states; support native subagents/delegation where available.

Normalize task state, events, errors, artifacts, and verification metadata. Support reduced-capability harnesses without pretending unsupported features exist. Keep specialist definitions independent of any one harness.

## 11. Verification and quality control

Verification is a first-class stage, not a final conversational assertion.

- Derive a verification plan from task, changed files, risk, and acceptance criteria.
- Select deterministic checks (tests, typecheck, lint, build, static/security analysis, migration checks) based on repository configuration and change impact.
- Use independent review/adversarial analysis when risk warrants it.
- Verify claimed commands actually ran; preserve exit codes, logs, environment, revision, and artifact references.
- Distinguish Passed, Failed, Blocked, Not Run, and Inconclusive.
- Detect flaky tests; retries do not create an unqualified pass.
- Check diff scope, unintended changes, secrets, dependency/license impact, and generated artifacts.
- Produce evidence-backed completion reports with residual risks and unverified claims.
- No agent may claim completion solely because it generated a plausible patch or explanation.

## 12. Learning and feedback loops

Use outcomes, reviewer corrections, test failures, and user feedback to improve retrieval, tool selection, planning, and specialist selection. Keep evaluation data separate from production memory where appropriate. Govern durable policy changes. Preserve negative results as scoped evidence; a failed approach in one repository/version is not universally invalid.

## 13. Unified extensions ecosystem

Unify capability types while preserving explicit type contracts. Candidate types: tools, MCP servers/adapters, skills, specialist-agent definitions, workflows, parsers/indexers, model providers, and harness adapters.

Define manifests, version compatibility, dependency resolution, permissions, publisher/provenance metadata, integrity/signatures, sandbox/isolation, install/update/rollback/disable/uninstall, revocation, audit logs, and resource budgets. Native core capabilities must remain usable without third-party extensions. MCP is interoperability, not a substitute for core domain services.

## 14. Security, privacy, and trust

Treat repository files, docs, MCP results, model output, and extension output as untrusted. Defend against prompt injection and tool-output instruction injection. Enforce project/worktree boundaries in memory, retrieval, tools, and logs. Apply least privilege, explicit tool permissions, human approval for consequential operations, and per-extension isolation. Protect credentials; never place secrets in graph memory or context packets by default. Provide retention, export, deletion, local/cloud processing controls, and auditability. Validate IPC clients, payloads, paths, symlinks, and process ownership.

## 15. Observability and evaluation

Trace tasks across retrieval, planning, model calls, tool executions, agent delegation, verification, and memory writes using correlated IDs.

Measure indexing coverage/freshness/duration/incremental correctness; retrieval precision/recall/evidence coverage/stale-conflict rate; context tokens and useful-evidence density; task success against acceptance criteria; verification outcomes and false-success rate; agent selection/delegation/rework/conflicts; latency/throughput/provider cost/CPU/RSS/disk/recovery; extension failures and security events.

Use representative repositories and expert-labeled tasks. Compare against a baseline harness without EI. Keep datasets, versions, hardware, methodology, and raw results reproducible. Do not publish unsupported performance or quality claims.

## 16. Resource-aware and deployment strategy

Support low-resource machines through incremental indexing, bounded concurrency, cancellation, backpressure, selective embeddings, caching, and configurable budgets. Keep local-first deterministic capabilities functional without cloud AI. Allow cloud models as optional configured providers; do not require paid models for core deterministic indexing/retrieval/verification. Make data movement explicit. Measure complete process RSS and workload costs, not only library metrics.

## 17. Versioned contracts to define

Specify contracts for Project/Repository/Revision identity; ingestion/extraction events; knowledge entities/relationships/evidence/memory states; retrieval requests/responses/context packets; deterministic tool invocations/results; task plans/subtasks/agent capabilities/results; model/provider capabilities/invocations; harness sessions/events; verification plans/results/reports; extension manifests/permissions/lifecycle; telemetry/trace correlation.

Contracts must be typed, versioned, testable, and independent of a single model provider or coding harness.

## 18. Non-goals and guardrails

- Do not store whole chat histories or every source token as graph memory.
- Do not let model guesses silently become accepted project facts.
- Do not replace Git, source code, tests, or docs as sources of truth.
- Do not build a multi-graph-engine abstraction; Grafeo is the sole selected graph engine.
- Do not make all capabilities depend on cloud AI.
- Do not assume every harness supports native subagents, tools, streaming, or worktrees.
- Do not build marketplace breadth before core security, indexing, retrieval, and verification are proven.
- Do not describe planned capabilities as implemented.

## 19. Implementation sequencing

Technical validation establishes Shipyard reuse boundaries, Grafeo behavior, IPC/lifecycle, Graph Schema v1, and implementation issues before runtime implementation. The POC validates the accepted Grafeo choice; it does not authorize an automatic graph fallback. Implementation issues must trace back to this specification and implementation requirements.


## 20. Relationship to the complete Coding Harness product

This document specifies the Engineering Intelligence plane, not the entirety of Coding Harness. The overall product also includes a Developer Workbench and an Operational Runtime. Their responsibilities, end-to-end user workflow, and the 22 logical intelligence domains are defined in [docs/13-platform-scope-and-intelligence-taxonomy.md](13-platform-scope-and-intelligence-taxonomy.md). The research questions and technology alternatives are tracked in [docs/14-research-and-decision-register.md](14-research-and-decision-register.md).

The intelligence domains are logical capability boundaries. They do not imply one process, microservice, or autonomous agent per domain. Runtime process/session execution remains owned by the Operational Runtime; the intelligence sidecar supplies project understanding, knowledge, retrieval, reasoning support, and evidence services through versioned contracts.

## 21. Cross-cutting intelligence mechanisms and graph views

The following named concepts from the broader project discussions are explicit mechanisms/cross-cutting capabilities. They do not automatically add separate microservices, agents, or top-level taxonomy domains.

### 21.1 Engineering Reasoning Intelligence

A cross-domain reasoning capability combining repository, architecture, dependency/impact, knowledge, retrieval, planning, and verification evidence. It should support problem framing, hypothesis generation, root-cause investigation, alternative comparison, risk analysis, decision rationale, and iterative revision when new evidence appears. Reasoning outputs must distinguish facts, hypotheses, assumptions, and decisions and cite supporting evidence. It is not a substitute for deterministic checks.

### 21.2 Capability Amplification Intelligence

A cross-cutting layer that compensates for model/agent limitations through deterministic tools, project knowledge, retrieval, context engineering, specialist routing, capability-aware provider/harness selection, and independent verification. It should prefer deterministic evidence when available and explicitly expose unsupported capabilities rather than allowing an agent to pretend they exist.

### 21.3 Cognitive scaffolding and knowledge injection

- **Cognitive scaffolding:** externalize task state into structured goals, constraints, assumptions, dependencies, hypotheses, evidence, plans, acceptance criteria, and checklists so the model is not required to retain the entire problem in an opaque context window.
- **Knowledge injection:** assemble and inject only the task-relevant, revision-aware context packet, project rules, accepted decisions, tool contracts, and specialist instructions into the selected agent/harness. Preserve provenance and disclose missing or conflicting evidence.
- Context compaction/handoff must preserve critical constraints, accepted decisions, open questions, evidence references, and next actions; track context health and loss rather than assuming summaries are lossless.

### 21.4 Expanded memory and learning mechanisms

Project memory should account for:
- **Procedural memory:** reusable, evidence-backed steps for recurring engineering tasks and workflows.
- **Experience memory:** prior task outcomes, failures, diagnostics, remediation attempts, and what was verified to work.
- **Distillation:** compress repeated task traces and evidence into reusable procedures or project knowledge while preserving provenance, scope, confidence, and links to original evidence.
- **Teacher–student improvement:** optionally use a stronger model as a critic/evaluator or to propose distilled guidance for a smaller/cheaper model. This is an evaluation/knowledge-curation strategy, not an approved requirement to train or fine-tune models. No generated guidance becomes accepted policy without the relevant review/validation.
- **Capability assessment:** maintain measured/declared capability profiles for models, providers, harnesses, tools, and specialists; route tasks according to demonstrated support, quality, latency, privacy, and resource constraints.

### 21.5 Graph intelligence views

Grafeo remains one graph engine. The logical views/queries may include:
- Symbol and reference graph.
- Import, package, and dependency graph.
- Call graph where language/tool extraction is sufficiently reliable.
- Data-flow graph where supported by language/compiler analysis; mark partial coverage and uncertainty.
- Architecture/component/service graph.
- Requirement–code–test traceability graph.
- Change-impact graph linking changed entities to affected callers, consumers, contracts, tests, and architecture components.

These are logical projections/query families over the approved knowledge model, not separate graph databases. Every edge must carry extractor/source provenance, revision scope, and confidence/coverage metadata. Do not imply complete call/data-flow coverage across all languages.

### 21.6 Capability boundaries and validation status

These mechanisms are target requirements to map into the 22-domain taxonomy and implementation backlog. Their exact algorithms, schemas, quality thresholds, and model usage remain research/implementation tasks. They are not claims of existing runtime functionality.
