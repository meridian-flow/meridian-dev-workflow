---
name: refactoring-principles
description: >
  Shared guidance for judging and improving code structure. Use when
  designing, implementing, or reviewing code where extensibility,
  maintainability, and agent execution speed matter, especially when
  deciding whether to refactor early, how to judge structural debt, and
  how to distinguish real design problems from stylistic preferences.
disable-model-invocation: true
allow_implicit_invocation: false
---

# Refactoring Principles

Refactoring is behavior-preserving structural improvement. Its purpose is not
tidiness for its own sake; it is to make the next change smaller, safer, and
easier to reason about.

In agent-heavy development, refactoring matters more, not less. Execution is
cheap, so structural debt compounds quickly: hidden coupling, weak locality,
unclear naming, and sprawling edit surfaces slow every future task and make
regressions more likely. Good structure keeps change narrow and predictable so
both humans and agents can work quickly without spraying edits across the
codebase.

## What Good Structure Optimizes For

Good structure minimizes the cost and risk of the next change.

Prefer structures that:
- keep related concepts local
- make names communicate intent clearly
- align abstractions with real axes of variation
- reduce cross-file edit fan-out
- make behavior easy to grep, trace, and verify
- isolate compatibility and legacy concerns from ordinary code paths

A structural problem is real when it makes future work broader, riskier, or
harder to understand. "Messy" is not enough; the issue should have a concrete
maintenance cost.

## Core Judgments

Refactor early when the current structure is impeding current work. It is
usually cheaper to improve the shape of the code while context is fresh than to
preserve a bad structure and rediscover its constraints later.

Prefer small, behavior-preserving moves over large redesigns. The goal is to
improve locality, clarity, and changeability without taking on unnecessary
correctness risk.

Duplication can be preferable to the wrong abstraction, but intentional
duplication must be discoverable. When similar logic stays separate on purpose,
make the relationship explicit when it would not otherwise be obvious, so future
changes do not silently drift apart.

An abstraction is suspect when it keeps growing flags, branches, special cases,
or caller-specific behavior to absorb new work. That usually means the code
captured superficial similarity instead of the real shared concept.

When code appears obsolete, ask what constraint it is carrying. If it carries no
real behavior, remove it. If it carries a real but poorly expressed constraint,
make that constraint explicit in a clearer concept or boundary, then remove or
reshape the old code.

Legacy and deprecated behavior should be explicit, localized, and removable.
Compatibility paths should not be smeared through ordinary code. If old
behavior must remain, isolate it behind a clearly named boundary, mark it as
deprecated where the language supports it, and explain what still depends on it
and what ends its lifetime.

Comments are support, not structure. Use short comments when the reason code
remains would otherwise be unclear, especially for intentional duplication,
temporary compatibility, or deprecated paths. Prefer names, boundaries, and
tests first; use comments to record the non-obvious reason and exit condition.

## Structural Risk Signals

Be suspicious when:
- one change requires touching many files
- one module changes for unrelated reasons
- understanding one behavior requires loading too many concepts at once
- a single function or class accumulates too many responsibilities
- the same distinction is implemented with repeated branching in many places
- interfaces leak representation details that should stay local
- naming hides the real domain concept
- compatibility behavior is mixed into ordinary paths instead of isolated
- old code is preserved without a clear reason, owner, or exit condition

## What Refactoring Should Produce

A successful refactor makes the codebase easier to extend without increasing
ambiguity.

After refactoring, it should be easier to:
- add a new case without scattered edits
- remove or replace old behavior safely
- understand where a concept lives
- test the right boundary
- reason about what can change independently

Refactoring is not about maximizing cleverness or abstraction density. It is
about preserving behavior while improving clarity, locality, and extensibility.
