---
name: post-impl-capture
type: reference
description: Use after implementation ships — coordinates knowledge capture across .context/, KB, and docs layers.
model-invocable: true
---

# Post-Implementation Capture

After implementation ships, knowledge lives in conversation transcripts
(decisions, rejected alternatives, discovered constraints), design artifacts
(what was intended), and the code itself (what was built). Capture it before
transcripts compact and reasoning evaporates.

## Coordination Sequence

1. **Mine conversations.** Spawn @session-miner with --from to extract
   decisions, rejected alternatives, and intent from implementation sessions.

2. **Assess scope.** Read the work item's design artifacts (requirements.md,
   design/) to understand what was *intended*. Compare with changed files to
   see what was *built*. The delta between intent and outcome often reveals
   undocumented decisions.

3. **Spawn documentation agents in parallel:**
   - @code-mirror — pass changed source files (-f), session-miner
     findings, design artifacts (requirements.md, design/), and
     implementation session context (--from). Pass session-miner
     findings as the primary context. Add `--from` only when specific
     rationale phrasing matters and the digest doesn't capture it.
     Goal: ".context/ and AGENTS.md updated for all affected modules."
   - @kb-writer — pass session-miner findings, design artifacts, and
     conversation context (--from). Goal: "cross-cutting knowledge captured
     in KB."
   - @tech-writer — pass changed files and design artifacts. Goal:
     "user-facing docs updated for behavioral changes."

4. **Review coverage.** After all complete, check:
   - Did @code-mirror cover all modules with significant contract changes?
   - Did @kb-writer capture cross-cutting patterns and project-wide decisions?
   - Did @tech-writer update docs for user-visible changes?
   Spawn additional passes for gaps.

5. **Structural health.** Spawn @kb-maintainer targeting the KB and any
   .context/ directories that were created or heavily modified.

6. **Report.** Summarize what was captured, which layers were updated, and
   any remaining gaps.
