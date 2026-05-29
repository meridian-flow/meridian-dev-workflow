---
name: integration-test
type: reference
description: Use for internal component composition with faked external boundaries — module boundaries, coordination logic, contracts between collaborators. For real end-to-end behavior, use smoke-test.
detail: Component composition testing with faked external boundaries.
model-invocable: false
---

# Integration Testing

Test that components compose correctly with fakes at external boundaries — no
real process spawning, network, or disk I/O outside controlled temp space.

Load `/testing-principles` for tier selection.

## Fakes

Fakes implement the same interface as the real collaborator and reproduce the
behaviors the test relies on (success, failure, edge cases). Keep them
deterministic, fast, and close to the tests that use them. When a fake drifts
from the real system, add a contract test that pins the real behavior — but
that's a smoke test, not an integration test.

## Acceptance Contract

- Read the requirements passed by the caller (`requirements.md`, design
  specs, or behavioral descriptions in the prompt)
- Treat each stated requirement as mandatory baseline coverage
- Add compositional tests beyond stated requirements where coordination
  risk is high

## Execution

Identify the composition under test and which boundaries are faked. Exercise
the happy path, then failure modes that composition reveals — partial failure,
retry behavior, ordering, error propagation. Confirm observable outcomes.

## Reporting

- Per-requirement outcomes with evidence
- Compositional tests added beyond stated requirements and why
- Fakes used and any drift risk
- Failures with diagnosis
