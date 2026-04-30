---
name: integration-test
description: Use when verifying that internal components compose correctly — module boundaries, coordination logic, contracts between collaborators — with fakes at external system boundaries. The middle tier between unit and smoke. Not the right fit for pure logic (unit-test) or real runtime behavior against live systems (smoke-test).
disable-model-invocation: true
allow_implicit_invocation: false
---

# Integration Testing

You verify that components compose correctly at speeds faster than end-to-end tests allow.

Load `/testing-principles` for tier selection and architecture judgment. Use `/ears-parsing` for EARS parse contract and per-ID reporting.

## What Integration Tests Are For

Integration tests answer: "do these pieces agree on the contract when they work together?"

- Multiple internal modules cooperate to produce behavior.
- External systems (processes, network, databases, third-party APIs) are replaced with fakes.
- The test exercises the composition without paying the cost of real I/O.

## Fakes at External Boundaries

A fake is a drop-in replacement for an external system that behaves like the real thing for the cases the test cares about. The defining characteristic: no real process spawning, no real network, no real disk outside of well-controlled temp space.

Good fakes:
- implement the same interface as the real collaborator
- reproduce the behaviors the test relies on (success, failure, edge cases)
- are deterministic and fast
- live in the test codebase, close to the tests that use them

Bad fakes:
- drift from the real implementation's behavior over time
- return success for every call regardless of input
- leak real I/O (spawning a subprocess, opening a socket)

When a fake and the real system drift, add a contract test that pins the real system's behavior — but that is a smoke test, not an integration test.

## When to Use Integration Tests

Use when:
- code coordinates multiple internal components and the bugs live in the coordination
- external systems can be faked without losing the test's value
- unit tests would require so much mocking they test nothing
- you need faster feedback than smoke tests give

Do not use when:
- the logic is pure — a unit test is faster and sharper
- the behavior only exists in the real runtime — use a smoke test
- the contract with an external API is what matters — use a contract test or smoke test

## Acceptance Contract

When spawned for a phase:

- Read the phase blueprint's `Claimed EARS statements` list.
- Load referenced spec leaves.
- Treat claimed IDs as mandatory baseline coverage.
- Add compositional tests beyond the claimed IDs where coordination risk is high.

If claimed IDs are missing or unresolved, stop and report a planning/ownership gap.

## How to Execute

- Identify the composition under test (which modules, which boundary is faked).
- Stand up or reuse fakes for the external systems.
- Exercise the happy path, then the failure modes that composition reveals (partial failure, retry behavior, ordering, error propagation).
- Confirm observable outcomes, not internal calls.

## Reporting

Return:
- per-claimed-ID outcomes (`verified` / `falsified` / `unparseable` / `blocked`)
- compositional tests added beyond claimed IDs and why
- fakes used, what they simulate, and any drift risk
- failures with diagnosis

@tech-lead owns `plan/leaf-ownership.md` updates based on tester reports.
