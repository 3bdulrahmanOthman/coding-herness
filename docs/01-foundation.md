# Project Foundation

## 1. Product definition

Coding Harness is an **Engineering Intelligence Augmentation Platform**: a system that augments coding agents with structured repository understanding, durable engineering knowledge, context selection, planning support, specialist delegation, controlled execution, and evidence-based verification.

The platform should improve the engineering process around different models and coding agents. It does not claim that architecture or tools can turn an incapable model into an expert. It can, however, reduce avoidable failure through better information, deterministic tooling, decomposition, constraints, feedback, and verification.

## 2. Problem

Coding agents often operate with fragmented or short-lived context. In large or unfamiliar projects they may:
- miss architectural constraints and cross-module dependencies;
- repeatedly rediscover decisions and repository facts;
- load excessive context or overlook the small set of relevant facts;
- make changes without understanding their impact;
- delegate work without clear contracts or evidence;
- report completion without sufficient verification;
- behave differently across models, providers, and execution harnesses.

The platform addresses these problems as an engineering system, not by relying on a single larger prompt.

## 3. Vision

Make high-quality engineering workflows available to coding agents across providers and repositories, with durable project understanding, transparent reasoning inputs, controlled autonomy, and verifiable results.

## 4. Mission

Build a portable intelligence and orchestration layer that can:
1. understand a repository and its engineering structure;
2. preserve decisions, constraints, and verified knowledge;
3. retrieve task-relevant context efficiently;
4. plan and decompose work;
5. select and coordinate suitable specialist agents;
6. execute through approved tools and environments;
7. verify changes against explicit acceptance criteria;
8. update knowledge based on evidence and outcomes.

## 5. Product outcomes

- Less repeated repository discovery.
- More accurate impact analysis and implementation plans.
- Better task decomposition and specialist selection.
- Fewer unsupported completion claims.
- Reusable workflows across compatible coding harnesses.
- Transparent provenance for retrieved knowledge and engineering claims.
- Predictable resource use and safe degradation.

These are desired outcomes, not measured achievements. They require evaluation against representative projects and baselines.

## 6. Non-goals

The initial platform is not intended to:
- train or replace foundation models;
- guarantee expert-level output from every model;
- require one specific commercial model provider;
- store every source line, token, prompt, or chat message in a graph;
- replace Git, source files, issue trackers, CI, or project databases;
- execute arbitrary repository instructions without trust and permission checks;
- require cloud infrastructure for core local project understanding;
- become a generic no-code automation platform;
- fork Shipyard or copy its source code as an assumed starting point.

## 7. Users and operating modes

Primary users are software engineers using one or more coding agents on personal or team projects. The platform should support:
- an individual developer working locally;
- a developer using remote development machines;
- multiple agents collaborating on one project;
- teams with shared engineering conventions and controlled knowledge;
- different models and harnesses connected through supported adapters.

## 8. Core principles

### Engineering evidence
Claims about a codebase should link to inspectable evidence: repository revision, file, symbol, line range where practical, and extraction method.

### Human-directed autonomy
Users define permissions and approval boundaries. Autonomy is configurable and bounded, not an unconditional default.

### Deterministic first where possible
Use parsers, Git, static analysis, tests, dependency metadata, schemas, and explicit rules for tasks that do not require probabilistic reasoning.

### Model-assisted where useful
Use models for semantic interpretation, synthesis, planning, explanation, and ambiguous reasoning. Keep model providers behind contracts.

### Incremental and resource-aware
Index changes rather than repeatedly scanning everything. Bound memory, concurrency, context size, and execution time.

### Portable contracts
Core capabilities should not be coupled to one model vendor, coding agent, or UI.

### Explicit failure
Unavailable intelligence services must be reported as unavailable. Never disguise a missing graph capability by silently switching to another graph engine.

## 9. Success measures

The project should establish baselines and then measure:
- repository fact retrieval precision and recall;
- evidence coverage of generated claims;
- task planning quality against expert-reviewed rubrics;
- change impact detection precision/recall;
- task completion and regression rates;
- verification coverage and false-positive rate;
- repeated-discovery reduction;
- context tokens consumed per task;
- indexing throughput and incremental update cost;
- sidecar RSS, CPU, disk I/O, and query latency;
- recovery time and data integrity after forced termination.

No numeric performance target is frozen until representative workloads and baseline measurements exist.

## 10. Product maturity

The current state is **foundation/documentation**. Any capability described in future tense or as a requirement is not yet implemented unless a later implementation record explicitly says otherwise.
