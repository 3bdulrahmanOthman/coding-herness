# Cross-Document Consistency Audit

- Status: **Reviewed; identified inconsistencies corrected; technical validation remains open**
- Branch reviewed: `main`
- Audit scope: README, all 16 numbered product/architecture documents, and all 8 ADRs present at the audit snapshot; open Issues #1–#14 were also inspected for alignment.
- This is a documentation consistency review, not a source-code audit of upstream Shipyard or AgenticOS, and not evidence that planned capabilities are implemented.

## 1. Findings corrected in this pass

| ID | Severity | Finding | Resolution |
|---|---|---|---|
| AUD-001 | High | Architecture diagram showed SQLite as the direct/authoritative store for Operational Runtime state, while the detailed storage boundary made SQLite optional and runtime-owned. | Updated the diagram: Operational Runtime owns task/run state and events; persistence backend remains subject to audit; SQLite is optional for runtime-owned metadata if validated and remains application/system storage. |
| AUD-002 | Medium | Architecture diagram left the first-release desktop/CLI/API mix entirely open, obscuring the accepted desktop-first direction and explicit Windows target. | Updated the diagram to desktop-first, Windows target, with exact feature/package scope still open. |
| AUD-003 | Medium | Foundation non-goal could be read as rejecting Shipyard as a source foundation, conflicting with the accepted Shipyard foundation decision. | Clarified that blind forking, wholesale vendoring, and unreviewed direct dependency are rejected; Shipyard remains the selected foundation and reuse is per-capability after audit. |
| AUD-004 | High | Implementation Requirements and Research Register described SQLite as application/system-only without carrying the explicit allowance for runtime-owned durable task/run metadata where the audited runtime design validates SQLite. | Aligned both documents with the accepted storage ownership rule. |
| AUD-005 | Medium | Autonomous execution research listed Shipyard alongside DSH/Sandcastle as though the choice of runtime foundation itself remained open. | Clarified that Shipyard is selected; only its per-capability integration/reuse mechanisms remain open. |

## 2. Canonical decisions after reconciliation

1. **Product:** Coding Harness is a developer workbench and agent-engineering platform with Developer Workbench, Operational Runtime, Engineering Intelligence, and a Unified Extensions Ecosystem.
2. **Foundation:** original upstream Shipyard is the selected operational/workbench foundation. The project is not required to be a GitHub fork. Per-capability reuse can be API/CLI, plugin/adapter, selective source reuse, or replacement of a specific unsuitable component after audit.
3. **AgenticOS:** capability/design-pattern reference only. Adopt/adapt selected features; do not import its complete multi-tenant server backend.
4. **Engineering Intelligence:** includes Code Intelligence and Engineering Memory as first-class domains; they are not separate products or parallel stores. The 22-domain taxonomy is logical capability organization, not 22 processes or always-on agents.
5. **Graph:** Grafeo is the sole selected graph engine, integrated through its native Rust API inside the Rust Intelligence Sidecar. Graph fallback is explicitly none.
6. **SQLite:** application/system state store; it may back Operational Runtime-owned task/run metadata only if the Shipyard/runtime audit validates that choice. It is never a graph fallback.
7. **Runtime ownership:** Operational Runtime owns authoritative live task/run/workflow state, process/session lifecycle, event journal, retries, cancellation, and recovery. Engineering Intelligence may keep derived evidence/relationship references, not a competing live execution store.
8. **Languages:** Go for validated operational runtime responsibilities; Rust for EI/Grafeo sidecar and measured native workloads; TypeScript/React for the workbench. No wholesale rewrite based on language-performance assumptions.
9. **Agents:** one accountable Orchestrator with dynamically selected specialists, bounded hierarchical delegation, capability-aware provider/harness adapters, and evidence-validated integration.
10. **Workflows:** one canonical versioned Workflow Spec and one durable execution substrate. Visual workflows, routines, hooks/flows, CLI/API, and event triggers are authoring/trigger surfaces, not competing executors.
11. **Extensions:** one Unified Extensions Ecosystem with native, WASM, isolated-process, remote/MCP, and UI extension tiers; security and platform validation precede broad third-party execution.
12. **Platforms/UI:** Windows desktop is an explicit target; macOS/Linux remain platform targets to validate. Tauri usage is not proof of Windows readiness. Preserve and extend Shipyard's UI language. Mobile is not a v1 commitment.

## 3. Open items that are not documentation contradictions

These remain valid validation work and must not be marked as completed decisions:

- Shipyard source/API/test/security/license/platform audit and per-capability reuse matrix.
- Grafeo version/toolchain POC, persistence/WAL, concurrency, recovery, backup/restore, memory/RSS, and platform validation.
- IPC transport, authentication, compatibility, supervision, and sidecar lifecycle.
- Graph Schema v1, entity/relationship properties, provenance, migrations, and deletion semantics.
- Retrieval/index placement: lexical, semantic/vector, graph, and direct deterministic lookup.
- First-release desktop feature/package scope and complementary CLI capabilities.
- AgenticOS feature-by-feature source audit and provenance.
- Workflow Spec semantics and migration of Shipyard flows/hooks.
- Extension manifests, WASM host API, sandbox threat model, process isolation, signing, revocation, and marketplace readiness.
- Provider/harness capability matrix, including reduced-capability adapters and native subagent differences.
- Windows clean-install packaging, signing, process/terminal behavior, IPC, sidecar, update, rollback, and recovery.
- Evaluation datasets, hardware baselines, quality thresholds, resource budgets, and acceptance gates.

## 4. GitHub Issues alignment

Issues #1–#14 are still open and cover the major validation tracks. They are consistent with the accepted product direction at a high level. However:

- The issues do not yet constitute a fully dependency-linked implementation backlog. Issue #5 is the intended consolidation point for dependency mapping and acceptance criteria.
- Some issue descriptions use the shorthand “SQLite system state only.” Interpret this as “SQLite is not a graph fallback”; the precise storage rule is the canonical rule in this audit and the reconciled architecture documents: optional runtime-owned task/run persistence only after validation.
- Issues are validation tasks, not proof that Shipyard/Grafeo/AgenticOS behavior has been tested.
- No issue should be closed merely because the corresponding architecture has been documented.

## 5. Verification boundary

The audit checked document presence and cross-document wording/decision alignment in the repository snapshot. It did not execute code, validate Grafeo, inspect the full upstream source trees, run platform builds, or prove runtime behavior. Those remain assigned to the existing technical-validation issues.

## 6. Maintenance rule

When an accepted decision changes:
1. Update its canonical ADR or decision register entry first.
2. Update dependent architecture/specification/requirements/roadmap documents in the same change.
3. Update affected GitHub Issues and acceptance criteria.
4. Re-run this cross-document audit.
5. Keep “Accepted”, “Validated”, “Proposed”, “Research”, “Rejected”, and “Superseded” states distinct.
