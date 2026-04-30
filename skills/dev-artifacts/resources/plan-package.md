# Plan Package: Phase Files and Artifact Contracts

Reference for authoring the on-disk `plan/` and `design/` packages. Load when writing, reviewing, or resuming from these artifacts.

## Phase File Structure

Each `plan/phase-N-<slug>.md` contains, in order:

1. **Scope** — what this phase delivers, boundaries, files/modules touched.
2. **Dependencies** — prior phases, refactor entries, external preconditions.
3. **Subphases** (when the phase has them) — numbered `N.1`, `N.2`, ... Each subphase blueprint includes:
   - scope (what this subphase does)
   - files / modules touched
   - dependencies on prior subphases
   - light verification criteria (what `@verifier` must confirm before moving on)
   - estimated size (helps judge if it should be split further)
4. **Phase Exit Gate** — full tester lanes and reviewer focus areas that must pass before the phase commits.
5. **Claimed EARS statements** — IDs this phase owns.
6. **Touched refactor IDs** — entries from `design/refactors.md` this phase handles.

Phases without subphases may omit section 3; the phase exit gate still applies.

## Artifact Contract

- `requirements.md`: user intent, constraints, success criteria.
- `design/spec/`: EARS contract with stable IDs.
- `design/architecture/`: implementation shape tied to spec.
- `design/refactors.md`: structural refactor agenda.
- `design/feasibility.md`: probe evidence and assumption verdicts.
- `plan/overview.md`: rounds, staffing, fanout, refactor handling.
- `plan/phase-N-<slug>.md`: per-phase scope, subphases, exit gate, claims.
- `plan/leaf-ownership.md`: exclusive phase ownership for each spec leaf.
- `plan/status.md`: phase and subphase lifecycle truth.
- `plan/pre-planning-notes.md`: runtime observations before planner spawn.
- `plan/preservation-hint.md`: redesign carry-over guidance.

## `plan/overview.md` Contract

Include:

- `Parallelism Posture` and `Cause`
- round list
- per-round justifications tied to concrete constraints
- refactor-handling table covering every `R0N`
- Mermaid fanout matching the round list
- explicit staffing contract for per-phase teams and final review loop

## Staffing Contract

Every plan must include staffing concrete enough for @tech-lead to execute directly:

1. **Per-phase teams** — implementer variant (`@coder`, `@refactor-coder`, `@frontend-coder`), tester lanes (`@verifier`, `@smoke-tester`, `@unit-tester`, `@integration-tester`, `@browser-tester` as applicable), probe/diagnosis steps where behavior is unclear.
2. **Final review loop** — reviewer focus areas, `@refactor-reviewer` (full change set), and `@smoke-tester` (end-to-end).
3. **Escalation policy** — when findings require routing to `@smoke-tester` (behavioral), `@investigator` (root-cause), or redesign escalation to @product-manager.

A plan without staffing drives execution toward coder-only behavior with weak review loops.

## Phase Blueprint Contract

Each phase file includes:

- scope and boundaries
- touched files/modules
- claimed EARS statement IDs
- touched refactor IDs
- dependencies
- subphases (when applicable) with scope, files touched, dependencies on prior subphases, light verification criteria, and estimated size
- tester lanes for the phase exit gate
- phase exit criteria

## `plan/leaf-ownership.md` Contract

- One row per EARS statement ID (`S<subsystem>.<section>.<letter><number>`)
- Complete + exclusive ownership
- Status, tester lane, evidence pointer
- Revised annotation propagation on redesign cycles (`revised: <reason>`)
