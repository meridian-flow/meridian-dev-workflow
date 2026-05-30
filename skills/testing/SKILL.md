---
name: testing
type: principle
description: Restraint-first testing discipline — tier selection, when NOT to write tests, functional core patterns.
model-invocable: true
---

# Testing Principles

Testing buys confidence that a change does what it claims and does not break
what it didn't touch. Coverage percentages, mock counts, and test totals are
instruments, not goals.

## Test for Risk, Not Completeness

Match test effort to risk, not coverage targets. 100% coverage of low-risk code
wastes budget that should go to high-risk code. The question is always: where
does failure hurt most?

- High-risk areas (data integrity, auth, payment, integration boundaries) →
  comprehensive coverage: happy path + edge cases + error conditions
- Low-risk areas (formatting, simple CRUD) → manual smoke check or nothing

## Tier Selection

Different tiers answer different questions. The craft is matching tier to
question, not stacking tiers for their own sake.

- **Pure logic, no I/O?** → unit test. Fast, exhaustive edge cases, no
  boundaries to fake.
- **Components composing, external systems fakeable?** → integration test.
  Medium speed, fakes at process/network boundaries.
- **Real runtime behavior matters?** → manual smoke / e2e runtime check. Real
  processes, real APIs, slower, closest to users.

Modern practice leans integration-heavy (the "Testing Trophy" model): integration
tests offer the best ROI because they test behavior the way users invoke it.
Unit tests are for pure logic and algorithmic components. Automated E2E tests
are for critical user journeys only — they're expensive and brittle if overused.
Manual smoke checks are runtime evidence, not permanent test-suite coverage.

The guiding axiom: the more your tests resemble the way your software is used,
the more confidence they give you.

When tier choice is unclear, improve the manual smoke instructions first:
record the real scenario, command, expected result, and edge/failure cases an
agent should run. Promote that coverage to automated unit or integration tests
only when the risk is hard to verify manually and the test protects a named
contract cheaply.

See `resources/tier-judgment.md` for the decision diagram and tradeoffs.

## Test Behavior, Not Implementation

Tests should assert on outcomes (return values, state changes, side effects),
not implementation paths. Tests that reach inside to verify private state or
assert mock call counts break on correct refactoring.

Ideal tests only need updates when behavior genuinely changes. If tests break
on refactoring that doesn't change behavior, they're testing the wrong thing.

## Architecture Enables Testing

Testable code is not an accident. Functional core / imperative shell is the
pattern worth internalizing: push decisions into pure functions, push I/O to the
edges, test the core exhaustively and the shell shallowly.

Code that is hard to test usually has a structural problem. When you reach for
heavy mocking, that is a smell about the architecture, not about testing.

See `resources/functional-core.md`.

## Hermetic by Default

Tests in the permanent suite must be hermetic: no external network calls, no
shared state between tests, deterministic inputs. Flaky tests destroy trust in
the suite and slow feedback loops. Each test is self-contained with its own
setup and teardown.

## DAMP Over DRY

Test code should be DAMP (Descriptive And Meaningful Phrases), not aggressively
DRY. Somewhat repetitive but fully self-contained and readable in isolation.
Aggressive DRY in tests creates brittleness — shared setup code becomes a
hidden dependency that breaks tests in unexpected ways.

## LLM-Generated Test Caveats

LLM-generated tests tend to verify what's already working rather than probe
failure modes. When generating tests, explicitly target:
- Boundary conditions and edge cases
- Error conditions and failure paths
- Adversarial inputs

Tests that compile and achieve coverage are not the same as tests that catch
bugs. Generate → execute → analyze gaps → regenerate iteratively, not
single-shot.

## When Each Tester Applies

Load the relevant resource when the tier is justified by the risk and the
question:

- `resources/unit-patterns.md` — pure logic, edge cases, regression guards on
  the functional core.
- `resources/integration-patterns.md` — composition across internal components
  with fakes at external boundaries.
- `resources/manual-testing.md` — real runtime behavior against real
  interfaces. Two modes (probing vs verification).
- `resources/browser-testing.md` — browser-based verification of frontend
  changes.

`@probe` is the runtime verification agent — see
`resources/manual-testing.md`.

Coders self-verify with the narrowest useful evidence for the change — often
focused checks or manual smoke, plus automated checks where they buy
confidence. `/reflection` makes that verification explicit before reporting.

Pick the tier that matches the question. Stack tiers only when the question
spans them.

See `resources/common-mistakes.md` for the recurring anti-patterns.
