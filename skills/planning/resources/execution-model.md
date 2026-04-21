# Execution Model

The plan is executed by @impl-orchestrator as a nested loop: phases outside, subphases inside, with verification at both levels.

```mermaid
flowchart TB
    START([Read Plan]) --> PHASE

    subgraph PHASE["FOR EACH PHASE"]
        direction TB

        subgraph SUBPHASE["FOR EACH SUBPHASE"]
            PROBE{"Behavior unclear?"}
            PROBE -->|yes| SM["@smoke-tester<br/>(probing mode)"]
            SM --> IMPL
            PROBE -->|no| IMPL
            IMPL["@coder / @refactor-coder /<br/>@frontend-coder"]
            IMPL --> LV["Light @verifier<br/>(build + existing tests)"]
            LV --> LR["Light @reviewer -m codex<br/>(code quality + task adherence)"]
            LR --> ROUTE{"Issues?"}
            ROUTE -->|impl bug| IMPL
            ROUTE -->|unclear behavior| SM
            ROUTE -->|root-cause| INV["@investigator"]
            INV --> IMPL
            ROUTE -->|clean| NEXT_SUB([Next Subphase])
        end

        NEXT_SUB --> GATE

        subgraph GATE["PHASE EXIT GATE"]
            direction LR
            V["@verifier<br/>(full)"]
            ST["@smoke-tester<br/>(verify mode)"]
            UT["@unit-tester /<br/>@integration-tester<br/>(temp — delete after)"]
            RV["@reviewer<br/>(one general)"]
        end

        GATE --> PASS{"Gate passes?"}
        PASS -->|impl fix| IMPL
        PASS -->|behavioral| SM
        PASS -->|root-cause| INV
        PASS -->|yes| COMMIT([Commit phase])
        COMMIT --> NEXT_PHASE([Next Phase])
    end

    NEXT_PHASE --> FINAL

    subgraph FINAL["FINAL GATE"]
        FRV["@reviewer fan-out<br/>(focus areas incl.<br/>plan coverage +<br/>design alignment)"]
        FRFR["@refactor-reviewer<br/>(full change set)"]
        FST["@smoke-tester<br/>(end-to-end)"]
    end

    FINAL --> GAPS{"Gaps found?"}
    GAPS -->|yes, add phases| PHASE
    GAPS -->|no| DONE([Ship])
```

## Why Two Levels

- **Light verification between subphases** catches drift early, inside the phase, while context is fresh and the fix is cheap. Full fan-out between subphases would be over-processing.
- **Full gate at phase boundaries** enforces the real quality bar before the phase commits. Subphases can break things temporarily; the gate catches what slipped.
- **Final gate after all phases** proves the whole change set hangs together and matches the design. Discovered gaps feed back as new phases.

## Fix-Cycle Routing

Route findings to the right specialist, not always back to @coder:

- **Implementation bugs** → back to the coder (context is still fresh).
- **Unclear runtime behavior** → `@smoke-tester` probe before re-attempting the fix.
- **Root-cause uncertainty** → `@investigator` to diagnose before coding resumes.
- **Phase-gate findings** → route by type as above, then re-run affected gate lanes.
- **Final-gate gaps** → new phase appended to the plan, following the normal phase loop.

## Probe Before Coding

When a subphase depends on runtime behavior that isn't well-understood, spawn
`@smoke-tester` in probing mode before coding. Don't let @coder guess at system
behavior — probing is cheap, wrong assumptions are expensive.

## When Subphases Are Omitted

If the phase is small enough that a single coder session can finish it before intermediate verification would help, the phase can be flat — no subphases, just the phase exit gate. The plan should mark these explicitly rather than leaving the reader to infer.
