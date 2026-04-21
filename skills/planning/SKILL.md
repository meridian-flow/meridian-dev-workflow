---
name: planning
description: >
  Shared definitions for plan execution — phases, subphases, verification
  levels, probe/diagnosis lanes, and the execution model. Loaded by both
  @planner (to produce plans) and @impl-orchestrator (to execute them).
---

# Plan Execution

Design describes target state. Plan describes execution delta. The plan is a
delta, not a restatement of the full system.

See `resources/execution-model.md` for the phase/subphase execution diagram.

## Phase

An independently testable stopping point. Ends with a full gate.

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

- **Between subphases — light verification.** `@verifier` (build + existing
  tests) and light `@reviewer -m codex` (code quality + task adherence). Quick
  feedback to keep momentum. Not the place for full review fan-out or smoke
  tests.
- **Phase exit gate — full verification.** `@verifier` (full), `@smoke-tester`,
  `@unit-tester` or `@integration-tester` as applicable (temporary gate tests —
  deleted after verification), `@reviewer` (one general review). Save
  @refactor-reviewer for the final gate.

## Probe and Diagnosis Lanes

Not all work in a phase is implementation:

- **Probe** — when a subphase depends on runtime behavior that isn't
  well-understood, schedule a `@smoke-tester` probe before the coding step.
- **Diagnosis** — when a subphase addresses a failure with unclear root cause,
  schedule an `@investigator` step before the coding step.

## Fix-Cycle Routing

Route findings to the right specialist, not always back to @coder:

- **Implementation bugs** → back to coder
- **Unclear runtime behavior** → `@smoke-tester` probe
- **Root-cause uncertainty** → `@investigator`
