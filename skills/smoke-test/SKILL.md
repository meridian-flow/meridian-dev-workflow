---
name: smoke-test
description: >
  Use when verifying user-facing behavior end-to-end — CLI invocations, HTTP requests, integration flows, race probes, interruption recovery. Two modes: probing (research phase, understand current behavior) and verification (implementation phase, prove correctness). Mandatory for changes that cross integration boundaries. Not the right fit for internal logic verification — that's unit testing.
disable-model-invocation: true
allow_implicit_invocation: false
---

# Smoke Testing

You validate real runtime behavior through user-facing flows.


## What Smoke Tests ARE and ARE NOT

**Smoke tests ARE:**
- Actually running the CLI/API/UI as a user would
- Real processes, real filesystem, real network when applicable
- Commands like `meridian spawn -a coder -p "test"` followed by verifying output
- Manual or scripted execution of user-facing workflows

**Smoke tests ARE NOT:**
- Pytest unit tests (those test internal logic in isolation)
- Pytest integration tests (those test component composition with fakes)
- Running `pytest tests/unit` and calling it "regression testing"

**Critical:** Many projects keep smoke test guides as markdown in `tests/e2e/` or `tests/smoke/`. These are MANUAL test checklists, not pytest-runnable files. Check the project's testing conventions before assuming what "smoke test" means for that codebase.

Load `/testing-principles` for tier selection. Use `/ears-parsing` for per-pattern parsing rule and report format.

## Two Modes, Same Toolset

Smoke testing shows up in two contexts. The tools and craft are identical; the intent differs.

| Mode | Spawned during | Question you're answering |
|---|---|---|
| **Probing** | Research phase | "How does this system behave today?" |
| **Verification** | Implementation phase | "Does this change work correctly?" |

Recognize the mode from the prompt. If the caller wants to understand existing behavior, you are probing. If the caller wants evidence a change meets its contract, you are verifying.

### Probing Mode

Exploratory. The question is open. You run commands to map behavior, find constraints, and surface surprises so the design or plan can incorporate them.

- Exercise normal paths and note what the system actually does.
- Probe edges: what happens at boundaries, under failure, with unusual input.
- Document discovered behavior, discovered constraints, and anything that contradicts stated assumptions.
- You are not proving the system correct. You are describing it accurately.

Report findings as observations, not pass/fail.

### Verification Mode

Confirmatory. The question is closed — the phase claims specific EARS statements; your job is evidence for or against them.

- Read the phase blueprint's `Claimed EARS statements`.
- Load referenced spec leaves.
- Treat claimed IDs as mandatory baseline coverage.
- Verify each claimed ID against concrete evidence, then run exploratory probes beyond the claim.

Report per-ID outcomes (`verified` / `falsified` / `unparseable` / `blocked`).

## Before You Test (Both Modes)

Check README, AGENTS/CLAUDE guidance, build files, and existing smoke/e2e tests for canonical invocation patterns. Match established conventions unless there is a concrete reason to diverge.

## How to Execute

- Run real commands and real workflows.
- Build disposable environments when honesty requires fresh state (temp repo, throwaway config, stub dependency, clean process boundary).
- Cover relevant backend/provider variants explicitly; report what was and was not tested.
- Go adversarial after the happy path: bad input, interruption, sequencing mistakes, and boundary conditions.
- Focus on user-visible behavior: exit codes, error messages, output shape, and on-disk side effects.

## Reporting

**Probing:**
- discovered behavior and constraints
- surprises worth surfacing to design/plan
- exact commands and outputs
- open questions

**Verification:**
- per-claimed-ID outcomes with evidence
- exact commands and outputs
- exploratory findings beyond claimed IDs
- surprising behavior worth escalation
- coverage gaps and reasons

@tech-lead owns `plan/leaf-ownership.md` updates based on verification reports.
