---
name: dev-principles
description: Use when designing, implementing, reviewing, or refactoring code as shared operating guidance — covers patterns LLM agents systematically get wrong (refactoring discipline, abstraction judgment, deletion courage, dependency judgment, design probing, edge-case thinking, integration-boundary probing). Loaded into orchestrators to trigger proactive refactoring and into reviewers to guide what to flag.
---

# Spec-Driven Development

The workflow is spec-driven: requirements → behavioral spec → architecture →
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

This applies at every handoff: dev-orchestrator questioning user intent,
design-orchestrator questioning stated requirements, reviewers questioning
whether the implementation serves the original need.

# Refactor Early, Refactor Continuously

Context decays. Refactoring later means re-learning what you already knew. Kent Beck: "First make the change easy, then make the easy change."

- Run @refactor-reviewer in design review and again in final implementation review.
- Fix refactor findings in active loop. Do not defer by default.
- Do preparatory refactors before feature work.
- Keep refactor and feature commits separate.
- Treat falling refactor rate as risk signal.

# Edge-Case Thinking

- Edge cases, failures, boundaries are first-class requirements.
- Design artifacts must enumerate edge cases before implementation.
- Add phase-specific edge cases before each implementation phase.
- Testers add independent edge cases beyond claimed EARS IDs.
- Happy-path-only is incomplete.

# Abstraction Judgment

Premature abstraction locks in the wrong axis of variation. Two cases look similar by coincidence; three reveal the true pattern. Sandi Metz: "Duplication is far cheaper than the wrong abstraction."

- Two similar cases: keep duplicated.
- Three+ true semantic matches: extract abstraction.
- Surface similarity != semantic similarity.
- New parameters/branches in abstraction: wrong-abstraction signal.
- Fix wrong abstraction immediately: inline/delete and re-form.
- DRY hard on utilities, cautious on business logic.

# Delete Aggressively

Dead code makes the codebase harder to navigate for both LLMs and humans. LLMs default to preserving code — passivity is the failure mode, not over-deletion.

- Think about deletion aggressively, especially during review.
- Flag dead code, speculative helpers, unused abstractions.
- Probe untested behavior before proposing deletion.
- Challenge preserved code with no clear reason.
- Confirm with human before executing deletions.

# Depend Deliberately

- Good dependency deletes more code than it adds.
- Choose deps that collapse subsystems, not primitive swaps.
- Evaluate total ownership: code + cognitive load + failures + test matrix.
- Adoption criteria: active maintenance, mature API, battle-tested fit, scope match.
- Do not add dep for trivial problems.
- Reject reflex frames (`stdlib-only` or `always-dep`).

# Follow Existing Patterns

- Read surrounding code first.
- Reuse established project patterns.
- Avoid introducing new idioms for routine tasks.
- Prefer consistency over cleverness.

# Probe Before Committing

- If two credible options exist and wrong choice is expensive, run the cheapest probe.
- Probe load-bearing runtime behavior before refactor/delete.
- Prototype library vs hand-rolled enough to compare LOC/failure modes/ergonomics.
- Identify assumptions that can 2x scope; probe those first.
- "Find out during implementation" is a risk flag.
- Match probe effort to reversibility.

# Probe Integration Boundaries

You cannot deduce your way to correctness at system boundaries. Real systems have undocumented behaviors, version-specific quirks, and implicit contracts that only probing reveals.

- Before adapter work, probe real system behavior.
- Verify config fields end-to-end against installed tool/version.
- Extract and reference real schema/spec when available.
- Add protocol-level observability from the start.
- Internal tests passing is necessary, not sufficient.
- Failure traces must show sent payload, received payload, mismatch point.

# No Regressions

- Changes must not break existing behavior unless explicitly intended.
- Reviewers and orchestrators verify this before merge.
- Bugs get regression tests before the fix.

# Keep Docs Current

- Update docs in same change as behavior.
- Re-verify external-tool references when versions change.

# Chesterton's Fence

- Understand why code exists before removing it.
- Investigate uncertainty; do not preserve/delete blindly.

## Name Constraint Before Deleting

- Check introducing history: `git log -S "<symbol>"`, `git log --follow <file>`.
- Find tests that explicitly exercise the code.
- Find decision logs/work artifacts referencing the behavior.
- If constraint is unknown: delete cautiously.
- If constraint is known: code is load-bearing until replacement exists.

