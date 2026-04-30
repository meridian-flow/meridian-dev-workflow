---
name: agent-staffing
description: Use when composing a team for a design or implementation phase — choosing which agents to spawn, how many, what runs in parallel, which review focus areas to cover, and how to scale effort to complexity. Also use when deciding whether a task is small enough to skip orchestration ceremony or large enough to warrant fan-out.
disable-model-invocation: true
allow_implicit_invocation: false
---

# Agent Staffing

Compose the right team for each phase. The goal is coverage across
perspectives, not redundant passes from the same angle.

Each manager/lead has its own staffing patterns in its body — this skill covers
the general principles that apply everywhere.

## General Principles

**Delegation is mandatory for managers and leads.** They never write code
or edit source files directly. If no team composition was provided by your
caller, compose one yourself using the agent catalogs below.

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
you have a specific reason. Run `meridian mars models list` to see available
families and strengths.

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

- `resources/reviewers.md` — @reviewer, @refactor-reviewer, and @alignment-reviewer
- `resources/testers.md` — @verifier, @smoke-tester, @unit-tester, @integration-tester, @browser-tester
- `resources/builders.md` — @coders, @architects, @web-researchers, @explorers
- `resources/maintainers.md` — @kb-writer, @kb-maintainer, @tech-writer, @investigator
