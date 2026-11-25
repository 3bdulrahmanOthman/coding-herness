# Roadmap

The roadmap is phased and evidence-driven. Dates and numeric performance commitments are intentionally omitted until scope and baselines are validated.

## Phase 0 — Foundation (current)

- Project vision, scope, principles, and product capabilities.
- Architecture boundaries and ADRs.
- Shipyard upstream assessment.
- Grafeo sidecar integration requirements.
- Security, testing, and resource requirements.

**Exit criteria:** architecture and open questions are understood; no runtime implementation assumptions are presented as proven.

## Phase 1 — Technical validation

- Inspect Shipyard implementation and identify integration options.
- Build a minimal Grafeo Rust POC.
- Validate persistence, queries, recovery, memory controls, and supported desktop platforms.
- Validate local IPC options and lifecycle supervision.
- Define graph schema v1 and evidence model.
- Establish representative benchmark datasets and baselines.

**Exit criteria:** POC report, pinned Grafeo version, tested IPC/lifecycle proposal, and approved implementation plan.

## Phase 2 — Core platform skeleton

- Operational runtime adapter boundary.
- Rust Intelligence Sidecar skeleton.
- Versioned IPC contract and health lifecycle.
- SQLite system-state schema.
- Grafeo database lifecycle and schema migration.
- Basic repository discovery and index status.

**Exit criteria:** app/runtime can start, monitor, and stop the sidecar; project/system state remains usable if intelligence is unavailable.

## Phase 3 — Repository intelligence MVP

- Git-aware incremental indexing.
- File/module/symbol/dependency extraction for selected languages.
- Evidence and provenance.
- Graph query services.
- Basic impact analysis and explainable retrieval.
- Context packet generation with freshness indicators.

**Exit criteria:** evaluated retrieval and impact-analysis quality on representative repositories.

## Phase 4 — Agent intelligence and verification

- Orchestrator and specialist registry.
- Task contracts and dynamic specialist selection.
- Planning and bounded delegation.
- Agent adapters for selected harnesses.
- Verification plans and evidence-backed completion reports.

**Exit criteria:** end-to-end engineering tasks demonstrate measurable improvement over a baseline workflow.

## Phase 5 — Extensions and provider ecosystem

- Unified extension manifests and lifecycle.
- MCP interoperability adapters.
- Model/provider adapters.
- Skills, agent packages, and workflow packages.
- Permission review and package integrity.

**Exit criteria:** third-party capability integration is manageable, auditable, and secure.

## Phase 6 — Productization

- Desktop experience and CLI/API polish.
- Multi-project workflows and team policies.
- Packaging, signing, updates, backups, diagnostics, and support.
- Optional mobile companion exploration.

**Exit criteria:** supported platform releases meet reliability, security, and usability requirements.

## Prioritization rule

Prioritize work that improves correctness, evidence, retrieval quality, safety, and measurable engineering outcomes. Avoid building marketplace breadth, speculative agents, or elaborate UI before core indexing and verification are proven.
