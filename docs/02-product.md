# Product Capabilities

## Product scope

Coding Harness is a developer workbench and agent-engineering platform made of three cooperating planes: Developer Workbench, Operational Runtime, and Engineering Intelligence. The unified extensions ecosystem connects optional third-party and user-defined capabilities across these planes. See [Product Scope and Intelligence Taxonomy](13-platform-scope-and-intelligence-taxonomy.md) for the full 22-domain map and end-to-end workflow.

## Developer Workbench

Target capabilities:
- Project/workspace onboarding, discovery, switching, and lifecycle.
- Repository trust review, initial indexing progress, and index health.
- Worktree/branch visibility and task isolation.
- Agent sessions, terminals, task/run timeline, progress, cancellation, and artifacts where supported by the runtime.
- Task intake, plan/acceptance-criteria review, permission approvals, and escalation.
- Specialist delegation tree, agent events, handoffs, and collaboration review.
- Diff/file review, evidence browser, test results, and residual-risk reporting.
- Project architecture/dependency views and governed knowledge/memory review.
- Extension management, provider/harness configuration, settings, diagnostics, logs, updates, and recovery.

The first shipped surface (desktop, CLI, or both) remains open. Tauri 2 + React + TypeScript is a candidate desktop stack, not an accepted final choice. Mobile is a possible later companion, not a v1 commitment.

## Operational Runtime

Target capabilities:
- Project/workspace and environment lifecycle.
- Git worktrees, branches, task isolation, and repository operations.
- Agent/session process lifecycle and supported harness invocation.
- Terminal and command execution under permission policy.
- Durable tasks/runs, event journal, progress, artifacts, cancellation, retries, and recovery.
- Hooks, flows, and operational automation.
- Remote boxes/execution targets where supported by the selected foundation.
- Runtime health, logs, process supervision, and upgrade compatibility.

The original Shipyard upstream is a candidate reuse foundation. Its actual implementation, APIs, tests, license, security, and platform support must be audited before choosing integration boundaries. Do not assume a fork or direct code reuse.

## Engineering Intelligence

### Repository and software understanding
- Discover repository structure, languages, frameworks, packages, build systems, test entry points, monorepos, and worktrees.
- Parse symbols and relationships where language tooling permits.
- Understand dependencies, imports, calls, API boundaries, data models, runtime configuration, and test coverage.
- Track Git revisions and invalidate stale derived facts.
- Produce architecture and impact reports with source evidence.

### Knowledge, memory, and context
- Ingest code, Git, tests, CI, docs, ADRs, requirements, tool results, reviews, and user-approved inputs.
- Record architecture decisions, requirements, constraints, conventions, known risks, and verified project facts.
- Preserve provenance, confidence, timestamps, revisions, and lifecycle state.
- Distinguish accepted decisions from hypotheses and observations.
- Retrieve relevant files, symbols, decisions, dependencies, tests, and prior outcomes.
- Rank evidence by relevance, freshness, authority, and confidence.
- Assemble bounded context packets and explain inclusion, uncertainty, and missing evidence.

### Planning, agents, and collaboration
- Convert goals into outcomes, constraints, subtasks, dependencies, and acceptance criteria.
- Select execution strategies and appropriate agents/tools.
- Dynamically select specialist roles rather than spawning all specialists by default.
- Coordinate parallel work only when boundaries and integration costs are understood.
- Keep task state durable and recoverable.
- Preserve human approval gates for high-impact actions.

### Deterministic tools and execution support
- Use Git, filesystem, AST, language servers, dependency metadata, tests, linters, type checks, builds, static/security analysis, and profiling.
- Expose typed tool contracts with permissions, side effects, timeout, cancellation, and provenance.
- Normalize tool results into evidence artifacts.

### Verification and learning
- Derive verification plans from acceptance criteria and repository conventions.
- Run relevant tests, linters, type checks, static analysis, and targeted review through the runtime.
- Compare expected and actual changes.
- Distinguish Passed, Failed, Blocked, Not Run, and Inconclusive.
- Record failures and successful approaches as evidence-backed knowledge candidates.
- Do not automatically promote a model-generated claim to verified knowledge.

## Unified Extensions Ecosystem

A coherent ecosystem covers Native Tools, MCP Servers/Adapters, Skills, Agent Packages, Workflow Packages, Intelligence Providers, Memory Providers, Model Providers, Coding-Harness Adapters, parser/indexer packages, and UI Plugins. Core capabilities remain native; MCP is an interoperability mechanism, not a prerequisite.

## User workflow (target)

1. Connect or open a repository/workspace and review trust boundaries.
2. Discover the repository and establish an observable initial index.
3. Review detected stack, architecture, commands, coverage gaps, and proposed knowledge.
4. Describe an engineering task and clarify missing requirements.
5. Retrieve evidence and build a bounded context packet.
6. Review a plan, risks, dependencies, acceptance criteria, and permission scope.
7. Execute directly or delegate to dynamically selected specialists.
8. Collect events, diffs, artifacts, and verification results.
9. Review evidence, skipped checks, and unresolved risks.
10. Approve, reject, or revise proposed durable knowledge updates.

## Capability maturity labels

- **Implemented:** shipped and tested.
- **In progress:** implementation exists but is incomplete.
- **Planned:** accepted direction, not implemented.
- **Experimental:** available behind explicit opt-in and not yet stable.
- **Research:** under investigation; no product commitment.

## Requirements quality

Every major capability should have a user problem, intended outcome, functional and non-functional requirements, permission/trust implications, failure modes, acceptance criteria, observable metrics, dependencies, and rollout plan.
