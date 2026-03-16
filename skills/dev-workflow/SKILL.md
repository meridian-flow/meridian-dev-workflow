---
name: dev-workflow
description: Development lifecycle orchestration — sequencing design, review, planning, and implementation phases with multi-agent coordination. Use this whenever you're working on a feature, refactor, or bug fix that involves more than one step. Activate for any task that benefits from phased execution, subagent delegation, or structured review — even if the user doesn't explicitly ask for a "workflow."
---
# Dev Workflow

You orchestrate work phases, delegate execution and review, and keep state visible so future agents can resume cleanly.

This skill defines phase sequencing and judgment. Use `architecture-design` and `plan-implementation` for phase-specific craft. `__meridian-work-coordination` owns work setup, status semantics, and artifact placement.

## Phases of Work

Three kinds of work — **design**, **planning**, and **implementation**. Review is woven through all of them, not a separate phase.

Track current focus with `meridian work update --status <status>`: `designing`, `planning`, `implementing`, `done`.

Review looks different at each stage:
- During design: stress-test the approach — does it handle edge cases, will it conflict with other work, are there simpler alternatives?
- During planning: sanity-check phase boundaries, dependencies, and scope.
- During implementation: code review for design drift, verification that the build is clean, testing that the thing actually works.

Not every task needs all three phases. Scale to the work.

## Scaling Ceremony

The biggest mistake is over-coordinating simple work or under-coordinating complex work. Two questions help calibrate:

1. **How much surface area?** More files and modules touched → more value from design and planning upfront.
2. **How reversible are mistakes?** Schema changes, API contracts, and public interfaces are expensive to get wrong — worth a design review. Internal refactors are cheap to fix — lighter process.

Skip straight to implementation when the intent is obvious and the blast radius is small. Add design when there are genuine tradeoffs. Use all three phases when work crosses module boundaries, changes interfaces, or requires coordination with other efforts.

## Design

Read `architecture-design` for method. Before designing, check `meridian work list` — if another work item touches the same area, read its design doc so efforts don't conflict. Surface significant overlap to the user.

When the design feels solid, fan out reviewers to stress-test it. Read `review-orchestration` for how to choose focus areas, select models, and synthesize findings.

## Planning

Read `plan-implementation` for phase decomposition, dependencies, and staffing.

## Implementation

The loop per phase: **Code → Test → Review → Fix if needed.**

Spawn a coder with full context (design docs, phase spec, relevant source). Then launch testers appropriate to what the phase changed — match testing to what could actually go wrong. Not every phase needs every kind of testing.

Fan out reviewers for the things testing can't catch. Read `review-orchestration` for focus areas and model selection. If fixes surface, spawn targeted corrections and re-review — but cap rework at three cycles. If it's still unstable, the problem is likely structural; escalate to the user.

When coders or reviewers surface out-of-scope findings, don't derail the active phase. Spawn an investigator to handle them in parallel — it'll quick-fix trivial items or file GH issues for the rest.

## Keeping State Visible

The point of tracking artifacts is resumability — a future agent (or you, after compaction) should be able to read `$MERIDIAN_WORK_DIR/` and understand what happened, what was decided, and what's left.

```
$MERIDIAN_WORK_DIR/
  overview.md              # Problem, approach, architecture
  decision-log.md          # Why choices were made, what was rejected
  implementation-log.md    # Bugs found, surprises, deferred items, coordination notes
  plan/
    phase-1-slug.md        # Per-phase specs
```

Both logs are append-only — if a decision gets reversed, record a new entry that supersedes the old one. Keep entries concrete (file paths, error messages, evidence) rather than vague. Cross-reference between logs when a finding leads to a decision.

Before marking work `done`, confirm phases are reviewed, tests pass, and deferred items are tracked.

## Cross-Workspace Coordination

When multiple work items are active, run `meridian work list` before design, use `meridian work sessions <name>` to see which sessions have touched each item, and read overlapping design docs. Log coordination in `implementation-log.md`. Use separate git worktrees to isolate parallel efforts.
