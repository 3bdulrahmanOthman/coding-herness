# Autonomous Engineering Execution and Governance

> Status: consolidated architecture specification derived from prior Coding Harness and agent-workflow discussions. This defines target behavior and contracts; it does not claim that any runtime, adapter, or integration is implemented. External systems named here are research subjects or optional adapters unless explicitly accepted elsewhere.

## 1. Purpose and architectural placement

Coding Harness must support more than launching coding agents. It should coordinate bounded engineering work from an initial feature/task through decomposition, isolated implementation, independent verification, review, remediation, pull-request/CI integration, and final acceptance.

This capability spans the three product planes:
- **Developer Workbench:** human-facing plans, approvals, delegation tree, status, diffs, evidence, PR/CI state, and recovery controls.
- **Operational Runtime:** durable tasks/runs, process and session lifecycle, worktrees/branches, execution, events, artifacts, cancellation, retries, and GitHub/SCM operations.
- **Engineering Intelligence:** task interpretation, dependency-aware decomposition, project context, specialist selection, engineering contracts, risk analysis, verification strategy, evidence evaluation, and learning.

The planes communicate through versioned contracts. Do not collapse all responsibilities into one orchestrator process or confuse agent execution with engineering correctness.

## 2. Target end-to-end execution lifecycle

1. **Intake:** capture goal, intended outcome, constraints, repository/workspace, user priorities, and ambiguity.
2. **Understand:** inspect repository state, project memory, accepted decisions, architecture, relevant code, tests, and available runtime/harness capabilities.
3. **Specify:** produce a task/feature specification with scope, non-goals, assumptions, acceptance criteria, risks, and required evidence.
4. **Plan:** decompose the work into dependency-linked slices; identify ownership boundaries, integration points, verification gates, and critical path.
5. **Govern:** apply policy to select workflow, autonomy level, tools, providers, permissions, budgets, and approval requirements.
6. **Assign:** select the lead/orchestrator and only the relevant specialists based on capability, stack, evidence, and available adapters.
7. **Isolate:** assign each independently mutable slice a worktree/branch or another verified isolation boundary; prevent conflicting ownership.
8. **Execute:** run agents through supported harness adapters; stream durable events and collect structured outputs/artifacts.
9. **Verify:** run deterministic checks and independent review against the slice contract. An agent's self-report is not evidence.
10. **Remediate:** route failures to bounded fix/retest loops with explicit retry limits, idempotency, and escalation conditions.
11. **Integrate:** reconcile branches, resolve conflicts, run integration checks, and preserve traceability from slice to parent task.
12. **Review and deliver:** create/update PRs where authorized, inspect CI/status, summarize diffs and evidence, and expose residual risks.
13. **Accept:** evaluate all required gates and human approvals before merge/release or other consequential actions.
14. **Learn:** propose evidence-backed updates to project memory, procedures, and evaluation datasets under memory-governance policy.

The lifecycle is a state machine with explicit transitions, not a rigid one-way prompt chain. New evidence may return work to specification, planning, implementation, or verification.

## 3. Hierarchical agent topology

The target topology is one accountable lead/orchestrator coordinating dynamically selected specialists. A typical feature may be decomposed into backend, frontend, data, test, security, documentation, and review slices, but these are examples—not a fixed roster.

- **Lead / Orchestrator:** owns task outcome, decomposition, assignments, integration, escalation, and final report.
- **Planner / Architect:** creates dependency graph, interfaces, acceptance criteria, risks, and execution plan.
- **Implementation specialists:** own bounded slices such as backend, frontend, database, or infrastructure.
- **Test / Verification specialist:** independently derives and executes checks; does not merely repeat implementation-agent claims.
- **Reviewer / Security / Performance specialists:** inspect relevant risk dimensions independently when selected.
- **Nested specialists:** may be delegated by a specialist only when the parent contract allows it and global limits remain enforced.

Every child task has one parent, an explicit owner, a bounded scope, and a contract. The orchestrator maintains a dependency DAG and prevents circular delegation, unbounded recursion, and duplicate file ownership.

## 4. Contracts and invariants

### Task and slice contract

Each task/slice must carry:
- stable task ID, parent ID, project/repository/worktree/revision identity;
- objective, user outcome, scope, non-scope, assumptions, and accepted decisions;
- dependencies, owned files/interfaces, and conflict boundaries;
- required inputs/evidence and expected artifacts;
- acceptance criteria and verification plan;
- allowed tools, provider/harness capabilities, permission scope, and approval gates;
- time, token/cost, concurrency, retry, and nesting budgets;
- completion schema, escalation conditions, and cancellation behavior.

### Core invariants

- No merge/acceptance transition without required evidence and passing gates.
- No unbounded agent spawning, nested delegation, retry loops, or resource consumption.
- No concurrent writes to the same worktree/files unless a deliberate coordination protocol is defined.
- No silent provider/model fallback that violates user privacy, cost, or capability policy.
- No irreversible or external side effect without the required approval.
- No task completion claim based only on agent narration.
- Every state transition, tool invocation, approval, failure, retry, and artifact is traceable.
- Accepted architecture decisions and repository constraints are injected into relevant task contracts.
- Cancellation and recovery must preserve a truthful, inspectable state.

## 5. Governance and workflow selection

Governance is a cross-cutting policy/control plane, not another autonomous coding agent. It evaluates risk and selects the minimum sufficient workflow and autonomy level.

Target policy dimensions:
- **Progressive governance:** low-risk reversible work can use lighter gates; high-impact, security-sensitive, destructive, or external actions require stronger review and explicit approval.
- **Controlled workflow selection:** choose among direct execution, plan-first, test-first, parallel slices, sequential handoff, independent review, or human escalation based on task shape and evidence.
- **Evidence-first invariant:** stage transitions depend on verifiable artifacts, not status text or confidence claims.
- **Context health:** track context freshness, completeness, token budget, compaction/summarization loss, and whether critical constraints/evidence survived handoff.
- **Checkpointing:** persist checkpoints where task duration, interruption risk, or recovery requirements justify them; do not add a complex checkpoint subsystem without a demonstrated need.
- **Policy plane:** centralize permissions, provider/data movement, autonomy, resource budgets, extension trust, and approval decisions.
- **Runtime identity/attestation:** where supported, record which runtime, harness, agent, provider, extension versions, and policy produced an action.
- **Incident recovery:** classify failures, preserve artifacts, support safe resume/retry/rollback, and escalate when state is ambiguous.
- **Production readiness:** require security, reliability, observability, recovery, and platform gates before stable release.

These are target requirements to refine and validate. Their detailed policy schema and runtime enforcement points remain implementation work.

## 6. Orchestration versus execution versus governance

Keep the following responsibilities distinct:

| Concern | Owner |
|---|---|
| Task interpretation, decomposition, specialist choice, engineering contracts | Engineering Intelligence |
| Durable run state, agent process/session lifecycle, worktrees, commands, events, retries, cancellation | Operational Runtime |
| Permissions, invariants, transition gates, autonomy, budgets, approval policy | Governance/policy services, shared across planes |
| Plans, status, delegation tree, approvals, diffs, evidence, diagnostics | Developer Workbench |
| Agent loop, prompt/tool execution, provider-specific features | Selected coding-harness adapter |
| Pull-request and CI operations | SCM/GitHub integration adapter under runtime permissions |
| Final acceptance decision | Policy gates plus authorized human approval where required |

The orchestrator coordinates but does not replace runtime durability, deterministic verification, or policy enforcement.

## 7. External systems researched—not preselected

Prior discussions considered:
- **DeepSeek Harness (DSH):** a candidate agent runtime/workflow system. Prior discussion described subagent/workflow primitives such as parallel, pipeline, phase, nested agents, lifecycle controls, and JavaScript customization. These capabilities and current APIs must be verified against the exact upstream version; persistent resume, isolation, retries/recovery, review gates, GitHub lifecycle, and global budgets were not established as turnkey.
- **Sandcastle:** a candidate orchestration/execution library for isolated agent runs, sandboxes, branches/worktrees, parallel execution, review, retries, and merge workflows. It is not the engineering-governance authority and should only be integrated if a current source audit demonstrates a real gap and a maintainable seam.
- **Codex, Claude Code, and other coding harnesses:** adapters must advertise actual capabilities (sessions, subagents, tools, hooks, MCP, worktrees, streaming, cancellation, artifacts, usage). Do not assume feature parity.
- **AOS:** a label used in prior discussions for the governance/acceptance concept; no explicit full expansion was established. Do not invent an expansion or assume a separate product/process.

No external system above is an accepted mandatory dependency. Reuse is conditional on source/API, license, security, platform, maintenance, and capability validation.

## 8. Relationship to skills, MCP, workflows, and specifications

- Skills encode reusable procedures and domain guidance; they do not by themselves provide durable orchestration, isolation, state transitions, or recovery.
- MCP provides tool interoperability; it is not the orchestration architecture and must not be required for native core capabilities.
- Workflow packages encode reusable execution patterns and gates; the runtime must execute them with durable state and policy enforcement.
- Project specifications, architecture decisions, task plans, and acceptance criteria are first-class versioned artifacts connected to code, tests, PRs, and verification evidence.
- Existing ESM/skills/MCPs/AGENTS.md and external tools should be audited and composed before creating overlapping skills or replacing functioning capabilities.
- The harness should select which skills, MCP tools, workflows, providers, and agents are relevant, pass only necessary context, and preserve budgets to reduce wandering and token waste.

## 9. Recovery, observability, and evaluation

Persist enough information to explain and safely resume a run:
- task graph and state transitions;
- selected workflow, agent/harness/provider and capability snapshot;
- worktree/branch ownership and revision references;
- approvals and policy decisions;
- prompts/context references subject to privacy/retention policy;
- tool invocations, exit codes, artifacts, test results, PR/CI references;
- retry lineage, failure classification, and recovery actions.

Evaluation should measure task success, acceptance-gate correctness, regression/rework, evidence coverage, agent coordination overhead, conflict rate, false-success rate, recovery success, token/model cost, latency, CPU/RSS/disk, and human intervention. Benchmarks must be reproducible and compare against a baseline workflow.

## 10. Open decisions and validation

- Whether DSH, Sandcastle, Shipyard, or another runtime is reused, and at which boundary.
- Exact workflow/state-machine schema and durable event model.
- Exact worktree isolation and nested delegation policy.
- Which SCM/CI providers are first-class and what permissions they require.
- Context health/compaction metrics and checkpoint triggers.
- Policy schema, risk tiers, approval rules, and attestation mechanism.
- Global budgets and scheduling/fairness policy.
- Which capabilities are native, adapter-provided, or deferred after source-level validation.

See [Research and Decision Register](14-research-and-decision-register.md), [Agents and Orchestration](05-agents-and-orchestration.md), and [Implementation Requirements](08-implementation-requirements.md).
