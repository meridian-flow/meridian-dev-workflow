# Reviewers

@reviewers catch what testing can't — design drift, subtle correctness issues, architectural erosion. The value of multiple @reviewers comes from different focus areas and different models, not redundant coverage of the same concern.

Use @reviewer fan-out in two default places: design review and the final end-to-end implementation review loop. Intermediate implementation phases are tester-driven; @reviewers are escalation-only there.

Design review is the highest-leverage review point — catching a bad interface or missed constraint before anyone writes code saves entire implementation cycles.

## Default Lanes

These should be part of design review and the final implementation review loop unless there's a specific reason to skip:

@reviewer — adversarial code analysis with a specified focus area. Give each @reviewer a different focus area so you get breadth — the change itself tells you what perspectives matter. Common dimensions: correctness and contract compliance, concurrent state and races, security and trust boundaries, design alignment. Pick the ones that match what could actually go wrong with this specific change. Read-only.

@alignment-reviewer — coverage verification: does one artifact deliver what another promised? Not adversarial code review — it checks whether requirements and design intent survived into implementations. Pass the source of truth with -f, optionally pass --from for conversational context. Use at two points: (1) after design converges, verify the design covers all requirements; (2) at the tech-lead's final gate, verify the full implementation delivers the design's architectural intent. Read-only.

@simplify-reviewer — structural friction hunter: finds shallow modules, fragmentation, deletion targets, and deep-module opportunities. Spawn at two points: (1) during design-lead's investigation phase to audit existing code for structural debt that would block clean design; (2) at tech-lead's final gate to check whether implementation created shallow modules or let entropy accumulate. Reports concrete simplification moves with leverage priority. Read-only.

## Focus Areas

The change tells you what perspectives matter. Think about what could go wrong that testing won't catch:

- **Correctness and contracts** — does the code do what it claims? Are invariants maintained?
- **Concurrent state and races** — shared mutable state, lock ordering, TOCTOU
- **Security and trust boundaries** — input validation, privilege escalation, injection vectors
- **Design alignment** — does the implementation match the spec? (Always include this one)
- **Module structure** — use @reviewer with structural focus
- **Test quality** — are tests hitting edge cases or just happy paths? Tautological assertions, mock-heavy tests, implementation-pinned tests, tier misplacement. Spawn after test-writing completes at phase gates.

Not all apply to every change. The goal is matching review perspectives to actual risk, not checking every box.

## Intermediate-Phase Escalation

During intermediate implementation phases, involve @reviewers only when testers surface a concrete behavioral issue the @coder cannot resolve confidently. Keep escalation scoped:

- Assign one @reviewer to the specific unresolved concern.
- Add a second @reviewer only if the issue spans multiple risk dimensions.
- Feed @reviewer findings back into @coder + tester loops, then continue phase execution.

## Synthesizing Findings

Fix valid findings by default. Defer only with explicit rationale recorded in the decision log. When @reviewers disagree, the orchestrator has context they don't — the full design, prior phases, runtime discoveries. Make the call and record it in the decision log.

If reviews aren't converging after multiple iterations, that's usually a signal the design has a structural problem — investigate or escalate rather than forcing convergence.
