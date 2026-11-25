# Product Capabilities

## Capability map

### Repository intelligence
- Discover repository structure, languages, frameworks, packages, build systems, and test entry points.
- Parse symbols and relationships where language tooling permits.
- Understand dependencies, imports, calls, API boundaries, data models, and test coverage.
- Track Git revisions and invalidate stale derived facts.
- Produce impact reports with source evidence.

### Project knowledge
- Record architecture decisions, requirements, constraints, conventions, known risks, and verified project facts.
- Preserve provenance, confidence, timestamps, and lifecycle state.
- Distinguish accepted decisions from hypotheses and observations.
- Track superseded and contradicted knowledge without erasing its history.

### Context engineering
- Translate a task into structured retrieval needs.
- Retrieve only relevant files, symbols, decisions, dependencies, and prior outcomes.
- Rank evidence by relevance, freshness, authority, and confidence.
- Assemble bounded context packets with citations/evidence references.
- Explain why each item was included and identify uncertainty or missing evidence.

### Planning and execution
- Convert user goals into outcomes, constraints, subtasks, dependencies, and acceptance criteria.
- Select execution strategies and appropriate agents/tools.
- Coordinate parallel work only when boundaries and integration costs are understood.
- Keep task state durable and recoverable.
- Preserve human approval gates for high-impact actions.

### Verification and learning
- Derive verification plans from acceptance criteria and repository conventions.
- Run tests, linters, type checks, static analysis, and targeted manual review.
- Compare expected and actual changes.
- Record failures and successful approaches as evidence-backed knowledge candidates.
- Do not automatically promote a model-generated claim to verified knowledge.

### User experience
The eventual product may include a desktop application, CLI, and APIs. The initial architecture should not assume that all interfaces ship simultaneously. A mobile companion is a possible later surface, not an initial platform commitment.

## User workflow (target)

1. Connect or open a repository.
2. Inspect repository metadata and establish an initial index.
3. Review discovered project facts and proposed knowledge.
4. Describe an engineering task.
5. Build a task-specific context packet.
6. Produce a plan and acceptance criteria.
7. Approve or adjust the plan and permission scope.
8. Execute directly or delegate to selected specialists.
9. Collect evidence and run verification.
10. Review the diff, test results, and unresolved risks.
11. Accept, reject, or revise proposed knowledge updates.

## Capability maturity labels

Use these labels in future plans and UI:
- **Implemented:** shipped and tested.
- **In progress:** implementation exists but is incomplete.
- **Planned:** accepted direction, not implemented.
- **Experimental:** available behind explicit opt-in and not yet stable.
- **Research:** under investigation; no product commitment.

## Requirements quality

Every major capability should eventually have:
- user problem and intended outcome;
- functional requirements;
- non-functional requirements;
- permission and trust implications;
- failure modes;
- acceptance criteria;
- observable metrics;
- dependencies and rollout plan.
