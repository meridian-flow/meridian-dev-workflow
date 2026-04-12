# meridian-dev-workflow

An opinionated multi-agent dev team for structured software development,
built on [Meridian](https://github.com/meridian-flow/meridian-cli)'s coordination
primitives. Install this and your orchestrator gets a full squad — architects,
coders, reviewers, testers, internet-researchers, and documenters — plus workflow
skills that teach it how to run a structured development lifecycle.

Built on [meridian-base](https://github.com/meridian-flow/meridian-base). Both must
be installed.

## Orchestrator Topology

The dev lifecycle splits across orchestrators with distinct ownership:

**dev-orchestrator** (interactive) owns the user relationship — intent capture,
scope sizing, design approval, plan review, and redesign routing.

**design-orchestrator** (autonomous) owns the design package — behavioral spec
(EARS statements), technical architecture, refactor agenda, and feasibility probes.

**impl-orchestrator** (autonomous, two roles per spawn) owns planning and execution.
Planning role consumes the design package and calls `@planner` to produce the plan.
Execution role consumes the approved plan and drives phase loops (code/test/fix)
through to a final review loop. Each role runs in its own spawn.

**docs-orchestrator** (autonomous) owns post-implementation documentation —
codebase mirror and user-facing docs through write/review/fix loops.

```bash
# dev-orchestrator handles requirements + design review
meridian spawn -a dev-orchestrator -p 'Build JWT token validation'

# dev-orch spawns design-orch → reviews design with user → spawns impl-orch (planning role)
# → reviews plan with user → spawns impl-orch (execution role) → spawns docs-orch
```

## Agents

**Orchestrators:**

| Agent | Model | Role |
|---|---|---|
| `dev-orchestrator` | (harness default) | User relationship — intent capture, scope sizing, design/plan approval, redesign routing |
| `design-orchestrator` | opus | Autonomous design — spec-first behavioral contract, architecture, feasibility probes, review convergence |
| `impl-orchestrator` | opus | Two roles: planning (calls @planner, produces plan) and execution (drives phase loops, final review) |
| `docs-orchestrator` | opus | Post-implementation documentation — write/review/fix loops for codebase mirror and user-facing docs |

**Design & Planning:**

| Agent | Model | Role |
|---|---|---|
| `architect` | gpt | Explores tradeoffs and produces hierarchical design docs with spec/architecture trees |
| `planner` | gpt-5.4 | Decomposes design packages into executable phases with EARS-statement ownership and parallelism posture |
| `frontend-designer` | opus | UI/UX design specs — layout, hierarchy, motion, aesthetic direction for frontend-coder |

**Implementation:**

| Agent | Model | Role |
|---|---|---|
| `coder` | codex | Production code writer — implements scoped tasks from phase blueprints |
| `frontend-coder` | opus | Production frontend code with distinctive design quality via the frontend-design skill |
| `refactor-reviewer` | gpt | Reduces codebase entropy — structural cleanup, SOLID fixes, dependency untangling |

**Testing & Verification:**

| Agent | Model | Role |
|---|---|---|
| `verifier` | gpt | Runs tests, type checks, and linters — fixes mechanical breakage, reports real issues |
| `unit-tester` | gpt | Writes and runs targeted unit tests for edge cases and regression guards |
| `smoke-tester` | gpt-5.4 | End-to-end testing from the user's perspective — CLI flows, HTTP requests, race probes |
| `browser-tester` | opus | Browser-based QA via Playwright — visual verification, user flows, console errors |

**Review & Analysis:**

| Agent | Model | Role |
|---|---|---|
| `reviewer` | gpt | Deep code review — specify a focus area (security, SOLID, correctness) or leave broad |
| `investigator` | gpt | Brief triage of flagged issues — quick-fixes trivial items, files GH issues for the rest |

**Research & Documentation:**

| Agent | Model | Role |
|---|---|---|
| `internet-researcher` | codex | Best practices, library comparisons, and architecture patterns via web search — the external counterpart to `explorer` |
| `explorer` | gpt-5.4-mini | Fast, cheap codebase explorer — reads files, searches code, mines past sessions |
| `code-documenter` | sonnet | Maintains the codebase mirror in `$MERIDIAN_FS_DIR`, keeps code comments accurate, and captures design rationale from sessions |
| `tech-writer` | sonnet | Writes and maintains user-facing docs — getting started guides, API reference, CLI usage, and tutorials |

## Skills

**Workflow orchestration:**

| Skill | What it teaches |
|---|---|
| `decision-log` | Decision capture — reasoning, alternatives, constraints |
| `dev-artifacts` | Shared artifact convention between orchestrators |
| `context-handoffs` | Context scoping for agent spawns |
| `session-mining` | Session-mining workflow patterns — recover parent-session decisions and delegate bulk transcript reads |
| `architecture` | Problem framing, tradeoff analysis, approach evaluation |
| `planning` | Decomposing design packages into executable phases — EARS ownership, parallelism posture, staffing |
| `agent-staffing` | Team composition — which agents to spawn, how many, what runs in parallel |
| `dev-principles` | Engineering principles LLM agents systematically violate — refactoring, abstraction, deletion, edge cases |
| `dev-artifacts` | Shared artifact convention between orchestrators — v3 layout with spec/architecture trees |

**Agent methodology:**

| Skill | What it teaches |
|---|---|
| `review` | Adversarial code review — severity thinking, structured reporting |
| `issues` | GitHub Issues integration — labels, work-item linking, `gh` CLI patterns |
| `browser-test` | Browser QA methodology — visual verification, accessibility, console errors |
| `smoke-test` | End-to-end testing — CLI, HTTP, race probes, interruption recovery |
| `unit-test` | Focused test writing — edge cases, regression guards, tricky logic |
| `verification` | Build verification — getting tests, types, and lint green |
| `ears-parsing` | Mechanical EARS verification contract for testers — per-pattern parse and per-ID reporting |
| `tech-docs` | Technical writing craft — hierarchical docs, linking strategy, and progressive disclosure |
| `frontend-design` | Distinctive, production-grade frontend interfaces — anti-generic-AI aesthetics |
| `mermaid` | Mermaid diagram syntax rules and validation |

## Cross-Source Dependencies

Several agents load skills from both this repo and `meridian-base`:

- `meridian-spawn` (base) — how to spawn and coordinate agents
- `meridian-cli` (base) — meridian and mars CLI mental model, sessions, and diagnostics
- `meridian-work-coordination` (base) — how to manage work items
- `session-mining` (dev-workflow) — workflow patterns for mining decisions from session history

The install engine warns about cross-source deps but doesn't fail — these
resolve from the base source. Both sources must be installed.

## Install

```bash
meridian mars add meridian-flow/meridian-base
meridian mars add meridian-flow/meridian-dev-workflow
```

## Layout

```
agents/*.md              # Agent profiles (YAML frontmatter + markdown)
skills/*/SKILL.md        # Skills (with optional resources/ subdirectory)
```

## See Also

- [meridian-cli](https://github.com/meridian-flow/meridian-cli) — the Meridian coordination engine
- [meridian-base](https://github.com/meridian-flow/meridian-base) — core coordination layer this builds on
