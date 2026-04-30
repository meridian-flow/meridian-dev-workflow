---
name: design-principles
description: >
  Design-facing operating guidance — problem framing, requirements validation,
  probing discipline, and edge-case thinking. Load when evaluating requirements,
  producing design packages, or deciding whether a problem is well-understood
  before committing to a solution.
---

# Spec-Driven Development

The workflow is spec-driven: requirements -> behavioral spec -> architecture ->
implementation verified against spec. The spec is the contract between design
and implementation, not a formality.

- Requirements capture the problem in solution-free terms (what outcome, not
  what feature).
- Design translates requirements into testable behavioral statements (EARS).
- Architecture serves the spec — every structural decision traces back to a
  behavioral requirement.
- Implementation verifies against claimed EARS statement IDs. Untraceable code
  is scope creep.
- Reviews check spec alignment, not just code correctness.

The spec is what makes the chain auditable. Without it, design is opinion,
implementation is guesswork, and review has no reference point.

# Treat Requirements as Hypotheses

The stated problem is rarely the real problem. Users describe solutions they
imagined (Y) when they need something different (X) — the XY Problem. Every
requirement is a hypothesis until validated.

- Ask for the outcome, not the feature. "What are you trying to achieve?" beats
  "What do you want built?" (Jobs-to-Be-Done framing.)
- Probe with "why" iteratively to reach the underlying need. The first answer
  is surface-level.
- When a requirement creates more problems than it solves, push back with
  evidence and propose alternatives.
- Gate design on a solution-free problem statement — a description of the
  outcome that doesn't presuppose a technical implementation.

# Edge-Case Thinking

- Edge cases, failures, boundaries are first-class requirements.
- Design artifacts must enumerate edge cases before implementation.
- Add phase-specific edge cases before each implementation phase.
- Testers add independent edge cases beyond claimed EARS IDs.
- Happy-path-only is incomplete.

# Probe Before Committing

- If two credible options exist and wrong choice is expensive, run the cheapest probe.
- Probe load-bearing runtime behavior before refactor/delete.
- Prototype library vs hand-rolled enough to compare LOC/failure modes/ergonomics.
- Identify assumptions that can 2x scope; probe those first.
- "Find out during implementation" is a risk flag.
- Match probe effort to reversibility.

# Probe Integration Boundaries

You cannot deduce your way to correctness at system boundaries. Real systems
have undocumented behaviors, version-specific quirks, and implicit contracts
that only probing reveals.

- Before adapter work, probe real system behavior.
- Verify config fields end-to-end against installed tool/version.
- Extract and reference real schema/spec when available.
- Add protocol-level observability from the start.
- Internal tests passing is necessary, not sufficient.
- Failure traces must show sent payload, received payload, mismatch point.

# Diagram First

A picture is worth a thousand words. Diagrams communicate structure,
relationships, and flows faster and more concretely than prose. Default to
diagrams for anything spatial — module boundaries, data flows, state machines,
dependency graphs, sequence interactions, system topology.

- Prefer mermaid diagrams — they're versionable, diffable, and verifiable with
  `meridian mermaid check`.
- Use tree structures for hierarchical decomposition.
- Diagrams in design docs, KB pages, and architecture docs are not decoration —
  they're the primary communication channel. Prose supplements diagrams, not the
  other way around.
- When reviewing, flag structural explanations that would be clearer as a
  diagram.
