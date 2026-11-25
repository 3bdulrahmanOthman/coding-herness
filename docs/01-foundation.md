# Project Foundation

## 1. Product definition

Coding Harness is a **developer workbench and agent-engineering platform** composed of three cooperating planes:

1. **Developer Workbench:** the user-facing experience for projects, workspaces, sessions, tasks, plans, permissions, agent activity, diffs, evidence, settings, and diagnostics.
2. **Operational Runtime:** the execution foundation for projects, worktrees, sessions, terminals, agent processes, tasks/runs, event history, hooks/flows, artifacts, and supported remote execution.
3. **Engineering Intelligence:** a modular system of intelligence domains for repository/code/architecture understanding, knowledge ingestion, governed memory, retrieval/context, planning, specialist selection, tools, verification, learning, security, and evaluation.

A Unified Extensions Ecosystem connects optional tools, MCP integrations, skills, agent/workflow packages, model providers, harness adapters, intelligence providers, parsers/indexers, and UI plugins.

Coding Harness is not merely an Engineering Intelligence library, prompt/skill bundle, MCP collection, model, or thin wrapper around one coding agent. Its product value comes from the integration of an operational environment, an intelligence layer, and a developer-facing workbench.

The complete scope and 22-domain intelligence taxonomy are defined in [docs/13-platform-scope-and-intelligence-taxonomy.md](13-platform-scope-and-intelligence-taxonomy.md). The research and decision boundaries are tracked in [docs/14-research-and-decision-register.md](14-research-and-decision-register.md).

## 2. Problem

Coding agents and engineering workflows often operate with fragmented or short-lived context and disconnected tools. In large or unfamiliar projects they may:
- miss architectural constraints and cross-module dependencies;
- repeatedly rediscover decisions and repository facts;
- load excessive context or overlook the small set of relevant facts;
- make changes without understanding their impact;
- delegate work without clear contracts or evidence;
- execute commands without sufficiently scoped permissions or durable traceability;
- report completion without sufficient verification;
- behave differently across models, providers, and execution harnesses;
- lack a unified developer experience for sessions, plans, worktrees, diffs, evidence, and approvals.

The platform addresses these problems as an integrated engineering product, not by relying on a single larger prompt or model.

## 3. Vision

Provide a coherent, resource-aware developer environment in which people can direct coding agents across projects and providers, while the system supplies durable project understanding, controlled execution, transparent evidence, specialist collaboration, and verifiable outcomes.

## 4. Mission

Build a portable platform that can:
1. provide a clear workbench for projects, workspaces, tasks, sessions, plans, and review;
2. manage or integrate operational project, worktree, process, agent, and execution lifecycles;
3. understand repositories, code structure, architecture, dependencies, and runtime configuration;
4. preserve decisions, constraints, and verified knowledge with provenance and lifecycle governance;
5. retrieve task-relevant context efficiently;
6. plan and decompose engineering work;
7. select and coordinate suitable specialist agents and tools;
8. execute through approved tools, harnesses, and environments;
9. verify changes against explicit acceptance criteria;
10. learn from evidence and feedback without silently rewriting accepted project policy;
11. support extensibility, provider choice, privacy, and constrained hardware.

## 5. Product outcomes

- Less repeated repository discovery and context waste.
- More accurate architecture understanding and change impact analysis.
- Better task decomposition and specialist selection.
- Safer, traceable execution across supported environments.
- Fewer unsupported completion claims and regressions.
- Reusable workflows across compatible coding harnesses and providers.
- A unified developer experience for plans, sessions, worktrees, diffs, permissions, and evidence.
- Transparent provenance for retrieved knowledge and engineering claims.
- Predictable resource use and explicit degradation behavior.

These are desired outcomes, not measured achievements. They require evaluation against representative projects and baselines.

## 6. Non-goals

The initial platform is not intended to:
- train or replace foundation models;
- guarantee expert-level output from every model;
- require one specific commercial model provider;
- store every source line, token, prompt, or chat message in a graph;
- replace Git, source files, issue trackers, CI, or project databases;
- execute arbitrary repository instructions without trust and permission checks;
- require cloud infrastructure or paid AI for core deterministic capabilities;
- become a generic no-code automation platform;
- fork Shipyard or copy its source code as an assumed starting point;
- turn every intelligence domain into a separate microservice or always-running agent;
- assume mobile parity with desktop.

## 7. Users and operating modes

Primary users are software engineers using one or more coding agents on personal or team projects. The platform should investigate/support:
- an individual developer working locally on constrained or powerful hardware;
- a developer using remote development machines or execution boxes;
- multiple agents collaborating on one project with bounded permissions;
- teams with shared engineering conventions and controlled knowledge;
- different models and harnesses connected through supported adapters.

Team synchronization, shared memory, remote intelligence placement, and mobile companion behavior remain open product decisions, not v1 commitments.

## 8. Core principles

### Evidence and source of truth
Claims about a codebase should link to inspectable evidence: repository revision, file, symbol, line range where practical, and extraction method. Git/source/tests and accepted decisions remain authoritative.

### Human-directed autonomy
Users define permissions and approval boundaries. Autonomy is configurable and bounded, not an unconditional default.

### Deterministic first where possible
Use parsers, Git, static analysis, tests, dependency metadata, schemas, and explicit rules for tasks that do not require probabilistic reasoning.

### Model-assisted where useful
Use models for semantic interpretation, synthesis, planning, explanation, and ambiguous reasoning. Keep model providers behind contracts.

### Modular intelligence, unified product
Intelligence domains have explicit responsibilities and contracts, but they are not automatically separate services or agents. The workbench, operational runtime, and intelligence layer must remain cohesive.

### Incremental and resource-aware
Index changes rather than repeatedly scanning everything. Bound memory, concurrency, context size, and execution time. Do not require cloud inference for deterministic core capabilities.

### Portable contracts
Core capabilities should not be coupled to one model vendor, coding agent, UI, or operating system.

### Explicit failure
Unavailable intelligence services must be reported as unavailable. Never disguise a missing graph capability by silently switching to another graph engine.

### Reuse before rewrite
Audit Shipyard's actual capabilities before rebuilding project, worktree, session, process, event, or orchestration infrastructure.

## 9. Success measures

The project should establish baselines and then measure:
- workbench task completion, usability, and review/approval clarity;
- runtime reliability, session/process lifecycle correctness, and recovery;
- repository fact retrieval precision and recall;
- evidence coverage of generated claims;
- architecture and change-impact detection precision/recall;
- task planning quality against expert-reviewed rubrics;
- task completion and regression rates;
- verification coverage and false-positive/false-success rate;
- repeated-discovery reduction and context tokens consumed per task;
- indexing throughput and incremental update cost;
- sidecar/runtime RSS, CPU, disk I/O, startup, idle footprint, and query latency;
- agent selection/delegation quality, conflict rate, and rework;
- provider cost/latency and privacy-policy compliance;
- extension failures and security events;
- recovery time and data integrity after forced termination.

No numeric performance target is frozen until representative workloads and baseline measurements exist.

## 10. Product maturity

The current state is **foundation/documentation and validation planning**. Runtime implementation has not started. Any capability described in future tense or as a requirement is not implemented unless a later implementation record explicitly says otherwise.
