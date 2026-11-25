# Agents and Orchestration

## 1. Agent model

Use an **Orchestrator + dynamically selected specialist agents** architecture.

The orchestrator owns task understanding, planning, decomposition, delegation decisions, integration, and final verification. It should not spawn every specialist for every request. It selects only agents whose capabilities are relevant.

A specialist definition should include:
- stable role and purpose;
- capabilities and exclusions;
- supported models/providers/harnesses;
- tools and skills required;
- context requirements;
- input and output contract;
- expected artifacts;
- verification criteria;
- permission profile and resource budget.

## 2. Candidate specialist roles

Examples, not a fixed mandatory roster:
- Repository Analyst;
- Software Architect;
- Planner/Decomposer;
- Backend Engineer;
- Frontend Engineer;
- Database Engineer;
- Security Reviewer;
- Test Engineer;
- Performance Engineer;
- Documentation Specialist;
- Code Reviewer;
- Integration/Release Engineer.

Role selection must be based on task needs, project stack, agent availability, and required permissions.

## 3. Task contract

Every delegated task should state:
- objective and user outcome;
- scope and explicit non-scope;
- relevant repository revision/worktree;
- accepted decisions and constraints;
- inputs and evidence;
- expected outputs and format;
- acceptance criteria;
- tests/checks to run;
- files or interfaces owned;
- allowed tools and permissions;
- deadline/resource budget where applicable;
- escalation conditions.

The orchestrator validates the specialist's output against the contract before integration.

## 4. Delegation and parallelism

Parallelize only independent work with clear ownership boundaries. Avoid multiple agents editing the same files or changing shared architecture contracts concurrently.

The orchestrator should:
- detect dependencies between subtasks;
- assign isolated worktrees/branches where supported;
- cap concurrent agents by resource and cost budgets;
- track task and agent states;
- support cancellation and retries with idempotency;
- collect results asynchronously;
- reconcile conflicts and integrate through review.

## 5. Collaboration patterns

Supported target patterns include:
- sequential handoff;
- independent parallel attempts;
- review by a separate specialist;
- broadcast of a bounded question to selected agents;
- plan critique and adversarial review;
- iterative implementation-verification loops;
- escalation to a human when assumptions or permissions are insufficient.

Collaboration must not create circular delegation or unbounded agent spawning. Each run needs a traceable parent task and bounded depth/budget.

## 6. Model and harness agnosticism

The platform should separate specialist role definitions from the underlying model or coding harness. An adapter advertises actual capabilities, such as:
- start/resume a session;
- create isolated work;
- send a prompt;
- receive completion or wait for turn;
- access tools/MCP;
- stream events;
- cancel;
- report artifacts and usage.

Do not claim universal cross-provider subagent support. Some harnesses expose native subagents; others only expose sessions, CLI processes, or MCP tools. The adapter must declare what is genuinely supported, and the orchestrator must adapt accordingly.

A specialist package may be portable as a role, prompt, contract, and tool requirement while its execution adapter remains provider-specific.

## 7. Verification and completion

A specialist's statement that work is complete is not proof. The orchestrator checks:
- requested files/artifacts exist;
- diff matches scope;
- acceptance criteria are met;
- required tests and checks ran;
- results and exit codes are captured;
- security-sensitive changes receive appropriate review;
- unresolved risks are disclosed.

The final report distinguishes verified results, assumptions, skipped checks, and outstanding work.

## 8. Human collaboration

Users can inspect plans, agent assignments, tool permissions, execution status, diffs, evidence, and verification. Approval gates are configurable by risk category and operation. The system must never impersonate the user or make consequential external commitments without authorization.

## 9. Reuse of operational orchestration

Shipyard already documents durable runs, agent-to-agent orchestration, wait/send/check, handoff, review, broadcast, and flows. Evaluate these existing capabilities as potential operational primitives rather than reimplementing them automatically. The intelligence layer should add engineering context, task contracts, knowledge retrieval, specialist policy, and evidence validation where those are absent.

## 10. Autonomous execution lifecycle and governance

Use [Autonomous Engineering Execution and Governance](15-autonomous-engineering-execution-and-governance.md) as the detailed target lifecycle and contract reference. It extends this agent model with:
- feature/task specifications and dependency-linked slices;
- hierarchical lead/specialist/nested-specialist delegation;
- isolated worktree ownership and explicit integration boundaries;
- independent verification, remediation loops, review, PR/CI, and acceptance gates;
- progressive governance, controlled workflow selection, evidence-first transitions, context health, bounded budgets, and recovery.

These are platform requirements, not a mandate to run every stage or specialist for every task. The orchestrator selects the minimum sufficient workflow based on task risk and available capabilities.
