---
name: dev-principles
description: >
  Implementation operating guidance — refactoring discipline, abstraction
  judgment, deletion courage, dependency judgment, pattern adherence. Load
  when implementing, reviewing, or refactoring code. Design-facing guidance
  (spec-driven, requirements-as-hypotheses, probing, edge-case thinking)
  lives in design-principles.
disable-model-invocation: true
allow_implicit_invocation: false
---

# Refactor Early, Refactor Continuously

Context decays. Refactoring later means re-learning what you already knew. Kent Beck: "First make the change easy, then make the easy change."

- Run @refactor-reviewer in design review and again in final implementation review.
- Fix refactor findings in active loop. Do not defer by default.
- Do preparatory refactors before feature work.
- Keep refactor and feature commits separate.
- Treat falling refactor rate as risk signal.

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
