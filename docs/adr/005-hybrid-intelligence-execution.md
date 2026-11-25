# ADR-005: Hybrid Intelligence Execution

- Status: **Accepted**
- Date: 2026-11-25

## Context

Not every engineering task needs an LLM. Deterministic tools are often more reliable for parsing, dependency inspection, Git operations, tests, policy enforcement, and structured validation. Models are valuable for semantic reasoning, synthesis, planning, and ambiguous interpretation.

## Decision

Use a hybrid execution model: deterministic native capabilities and model-based reasoning are first-class parts of the platform.

## Consequences

- Keep domain logic and provider adapters separate.
- Prefer deterministic evidence where available.
- Use models where semantic reasoning adds value.
- Record uncertainty and provenance.
- Do not imply that orchestration guarantees model correctness.
