---
name: planning
type: reference
description: Use when decomposing work into phases — plan structure, verification levels, execution model.
model-invocable: true
---

# Plan Execution

Design describes target state. Plan describes execution delta. The plan is a
delta, not a restatement of the full system.

See `resources/execution-model.md` for the phase/subphase execution diagram.

## Phase

An independently testable checkpoint. Ends with a full gate.

A good phase is:

- **Independently testable** (most important): completion can be verified
  without waiting on later phases.
- **Bounded to specific files/modules**: ownership is clear and cross-phase
  collision risk stays low.
- **Right-sized**: substantial enough to matter, small enough for a phase exit
  gate to be meaningful.

## Subphase

A smaller execution chunk inside a phase. May be sequential or parallel. Can
temporarily break unfinished behavior — the phase exit gate catches what light
verification missed.

- **Smaller than a phase, larger than a commit.**
- **Independently verifiable at a light level** — compiles, existing tests
  still pass, contract of the subphase holds.

Phases can be flat when they are already small. Subphases are explicit when they
add checkpoint value.

## Verification Levels

- **Between subphases — coder self-review.** The coder runs the project's
  verification suite and reviews its own diff before reporting (see
  `/reflection`). No separate verifier or reviewer spawn. Quick feedback
  while context is still loaded.
- **Phase exit gate — full verification.** `@probe` and `@reviewer`
  are the default lanes. Add `@coder with `/testing` `resources/integration-patterns.md``
  or `@coder with `/testing` `resources/unit-patterns.md`` only when the phase
  introduces a durable boundary, composition risk, or narrow logic risk that
  higher-tier verification cannot cover cheaply. All gate lanes run in
  parallel (`--bg` + `spawn wait`).

## Probe and Diagnosis Lanes

Not all work in a phase is implementation:

- **Probe** — when a subphase depends on runtime behavior that isn't
  well-understood, schedule a `@probe` probe before the coding step.
- **Diagnosis** — when a subphase addresses a failure with unclear root cause,
  schedule an `@investigator` step before the coding step.

## Fix-Cycle Routing

Route findings to the right specialist, not always back to @coder:

- **Implementation bugs** → back to coder
- **Unclear runtime behavior** → `@probe` probe
- **Root-cause uncertainty** → `@investigator`
