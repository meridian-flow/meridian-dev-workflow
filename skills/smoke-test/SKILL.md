---
name: smoke-test
type: reference
description: >
  Use when you need to understand or verify runtime behavior against the real
  system — CLI invocations, HTTP requests, integration flows, race probes,
  interruption recovery. Two modes: probing (how does this behave?) and
  verification (does this change work?). Mandatory for integration boundaries.
model-invocable: false
---

# Smoke Testing

Run the real system and observe what happens. Real processes, real filesystem,
real network. In this workflow, smoke testing means manual runtime verification
by an agent unless the caller explicitly asks for an automated e2e test suite.

Load `/testing-principles` for tier selection. Use `/ears-parsing` for
per-pattern parsing and report format.

Check README, AGENTS/CLAUDE guidance, build files, and existing smoke/e2e
guides for canonical invocation patterns. Many projects keep manual smoke
guides as markdown in `tests/e2e/` or `tests/smoke/` — check conventions before
assuming what format to use.

## Two Modes

| Mode | Phase | Question |
|---|---|---|
| **Probing** | Research / design | "How does this system behave today?" |
| **Verification** | Implementation | "Does this change work correctly?" |

Recognize the mode from the prompt. The tools are identical; the intent differs.

### Probing

Exploratory. Run commands to map behavior, find constraints, surface surprises.

- Exercise normal paths, note what the system actually does
- Probe edges: boundaries, failure, unusual input
- Document discovered behavior, constraints, and anything that contradicts
  stated assumptions

Report findings as observations, not pass/fail.

### Verification

Confirmatory. The phase claims specific EARS statements; produce evidence for
or against them.

- Read the phase blueprint's `Claimed EARS statements`
- Load referenced spec leaves
- Treat claimed IDs as mandatory baseline coverage
- Verify each claimed ID, then probe beyond the claim

Report per-ID outcomes (`verified` / `falsified` / `unparseable` / `blocked`).

## Execution

Run in `$MERIDIAN_TASK_DIR` — the work item's bound task dir, which may be
the project root, a sibling worktree, or another sibling checkout. Use
`cd "$MERIDIAN_TASK_DIR"` or `git -C "$MERIDIAN_TASK_DIR" …` for git, build,
and runtime commands. Do not create, move, or switch worktrees; the caller
owns workspace placement. Inspect `git status` and the working tree before
probing so you know what is already in flight. Use disposable environments
(temp repo, throwaway config) only for destructive probes or clean-baseline
comparisons that would contaminate the workspace.

Respect the shared workspace: do not reset, revert, stash, or clean the working
tree. Do not delete untracked files without ownership confirmation. Keep
generated artifacts and logs contained, and report their paths so cleanup is
explicit.

Go adversarial after the happy path: bad input, interruption, sequencing,
boundary conditions, invalid state, and fresh-state variants. Focus on
user-visible behavior: exit codes, error messages, output shape, on-disk side
effects.

## Reporting

**Probing:** discovered behavior, constraints, surprises, exact manual commands
and outputs, open questions.

**Verification:** per-claimed-ID outcomes with manual runtime evidence, exact
commands and outputs, exploratory findings beyond claimed IDs, coverage gaps.

Report smoke results as manual runtime evidence. Do not summarize automated
pytest/unit/integration/type/lint checks as smoke coverage.

@tech-lead owns `plan/leaf-ownership.md` updates based on verification reports.
