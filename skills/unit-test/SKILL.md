---
name: unit-test
type: reference
description: Use for isolated logic, regression-critical edge cases, and module contracts. For component composition, use integration-test. For end-to-end behavior, use smoke-test.
model-invocable: false
---

# Unit Testing

You write targeted tests for phase-scoped behavior and tricky internal logic.

Load `/testing-principles` for tier selection and the functional core /
imperative shell pattern.

## Test Value Model

Most unit tests are disposable implementation guards. Durable tests are the priority for regression-critical behavior: bug fixes, easy-to-forget edge cases, and module contracts.

## Acceptance Contract

- Read the requirements passed by the caller (`requirements.md`, design
  specs, or behavioral descriptions in the prompt).
- Map each stated requirement to testable behavior.
- Write/execute tests that verify each requirement.

Stated requirements are mandatory baseline coverage. Add regression and
edge-case tests where logic risk is high.

## What to Prioritize

- bug-regression guards
- parsing / state-machine edge cases
- module contract boundaries
- failure and boundary inputs

Avoid low-value tests that mirror implementation details or duplicate equivalent coverage. See `/testing-principles` `resources/common-mistakes.md` for recurring anti-patterns.

## Authoring Craft

- Keep tests isolated, fast, and single-purpose.
- Test behavior through the module's contract, not its internals.
- Use clear names so failure output explains what broke.
- Follow existing project fixture and assertion patterns for the language and framework in use.
- Add short comments only when the edge case rationale is non-obvious.

## Pruning

Actively remove obsolete tests when your scope changes structure:

- tests for removed paths
- tests pinned to invalid internals
- vacuous tests that pass regardless of behavior
- redundant tests with weaker signal than an existing case

Report pruned tests and rationale.

## Reporting

Return:
- per-requirement outcomes with evidence
- tests added beyond stated requirements and why
- failures with diagnosis
- pruned obsolete tests and rationale
