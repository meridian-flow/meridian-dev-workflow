---
name: agent-staffing
type: reference
description: Use when composing a team for a design or implementation phase — choosing which agents to spawn, how many, what runs in parallel, which review focus areas to cover, and how to scale effort to complexity. Also use when deciding whether a task is small enough to skip orchestration ceremony or large enough to warrant fan-out.
detail: Agent selection heuristics for design and implementation phases.
model-invocable: false
---

# Agent Staffing

Compose the right team for each phase. The goal is coverage across
perspectives, not redundant passes from the same angle.

Each manager/lead has its own staffing patterns in its body — this skill covers
the general principles that apply everywhere.

## General Principles

**Delegation is the default for managers and leads.** They route implementation and verification to specialists. Direct edits are limited to coordination artifacts explicitly named in the agent body. If no team composition was provided by your caller, compose one yourself using the agent catalogs below.

**Scale effort to risk.** Two questions: how expensive is it to be wrong here,
and how much harder is it to fix later than now? High-risk areas get more
coverage and more diverse perspectives. Low-risk areas get lighter treatment.

**Review convergence.** Review loops run until convergence (no new substantive
findings), not a fixed number of passes. The lead can stop early but must log
the reasoning.

**Route by work type.** Not everything is a coding task. Probe unclear behavior
with `@smoke-tester`, diagnose root causes with `@investigator`, implement with
coder variants. Plans that treat everything as coder work produce guesswork at
boundaries.

## Model Selection

Profile defaults are correct for most roles — don't override with `-m` unless
you have a specific reason. Run `meridian mars models list` to see configured
families and strengths; add `--live` when local harness availability matters.

## Fan-Out vs Parallel Lanes

- **Fan-out** — *same* prompt, *same* files, *different* models. Convergent
  signal on a high-stakes call. Reserved for critical decisions.
- **Parallel lanes** — *different* prompts (different focus areas), usually
  default model each. Most review staffing uses parallel lanes.

## Integration Boundaries

When a phase talks to an external system:
- Probe the real system before coding against assumed behavior
- `@smoke-tester` is mandatory for every integration phase
- Per-target coverage: N targets means N tested, not one-of-three

## Agent Catalogs

See resources for detailed catalogs:

- `resources/reviewers.md` — @reviewer, @alignment-reviewer, @simplify-reviewer
- `resources/testers.md` — @smoke-tester, @browser-tester, unit-test and integration-test skills
- `resources/builders.md` — @coders, @architects, @web-researchers, @explorers
- `resources/maintainers.md` — @kb-writer, @kb-maintainer, @tech-writer, @investigator
