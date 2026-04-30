---
name: unit-test
description: Use when verifying particular behaviors in isolation — edge cases, regression guards, tricky internal logic, and phase-scoped contracts that need a test pinning them down. Not the right fit for broad build verification (that's verification), component composition (integration-test), or end-to-end user flows (smoke-test).
disable-model-invocation: true
allow_implicit_invocation: false
---

# Unit Testing

You write targeted tests for phase-scoped behavior and tricky internal logic.

Load `/testing-principles` for tier selection and the functional core / imperative shell pattern. Use `/ears-parsing` for the EARS parse contract and per-ID reporting.

## What Unit Tests Are For

Unit tests answer: "does this piece of logic, in isolation, produce the right output for its inputs?"

They are at their best when the logic is pure — no I/O, no hidden state, no mocks. A good unit test runs in milliseconds, fails sharply when behavior regresses, and stays silent when structure changes.

If writing a unit test requires heavy mocking, the code is likely mixing decisions with I/O. Consider refactoring to a functional core, or moving the test to the integration tier. See `/testing-principles` `resources/functional-core.md`.

## Test Value Model

Most unit tests are disposable implementation guards. Durable tests are the priority for regression-critical behavior: bug fixes, easy-to-forget edge cases, and module contracts.

## Acceptance Contract

For phase work:

- Read `Claimed EARS statements` from the phase blueprint.
- Map each claimed ID to its spec leaf.
- Write/execute tests that verify each claimed statement.

Claimed IDs are mandatory baseline coverage. Add regression and edge-case tests where logic risk is high.

If claimed IDs are missing, ambiguous, or unresolved, report the ownership gap instead of substituting an ad-hoc contract.

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
- per-claimed-ID outcomes with evidence
- tests added beyond claimed IDs and why
- failures with diagnosis
- pruned obsolete tests and rationale

@tech-lead updates `plan/leaf-ownership.md` using your report.
