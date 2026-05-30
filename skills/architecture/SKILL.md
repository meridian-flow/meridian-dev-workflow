---
name: architecture
type: reference
description: Use when designing or reasoning about system boundaries — tradeoff dimensions, dependencies, structural risk.
model-invocable: true
---

# Architecture

## Boundaries

A good boundary has high cohesion within and loose coupling between. Types
that show up in practice:

- **Module** — code organization, import direction, visibility
- **Process** — separate runtime, communication over IPC/network
- **Trust** — authenticated vs unauthenticated, user vs admin, internal vs external
- **API/contract** — versioned interface that callers depend on

Boundaries are expensive to move once code builds on both sides. Getting them
wrong early means broad edits later.

## Dependencies

Direction matters more than count. Depend toward stability — things that
change less. When a volatile component depends on a stable one, changes stay
local. When a stable component depends on a volatile one, changes ripple.

Watch for: circular dependencies, stable code depending on volatile code,
leaking internal representation across a boundary.

## Tradeoff Dimensions

Not every dimension applies to every decision. Pick the ones that match what
could actually go wrong:

- **Reversibility** — how expensive to change this later?
- **Coupling** — how many things break when this changes?
- **Complexity** — total ownership cost: code, tests, failure modes, onboarding
- **Migration path** — how do you get from here to there?
- **Integration risk** — undocumented behavior, version-specific quirks, implicit contracts
- **Testability** — can the design be verified at natural seam points?
- **Scope boundaries** — what's in and what's out?

## Structural Risk

Wrong calls are expensive in proportion to how much code builds on top of
them. Highest-risk decisions: component boundaries, data model shape, trust
boundaries, and API contracts. Lower-risk: internal implementation within a
well-bounded module.

Match scrutiny to risk. A boundary decision that three teams build on
deserves tradeoff comparison and review. An internal helper function does not.
