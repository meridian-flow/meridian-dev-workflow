# meridian-dev-workflow

An opinionated multi-agent dev team for structured software development,
built on [Meridian](https://github.com/haowjy/meridian-channel)'s coordination
primitives. Install this and your orchestrator gets a full squad — architects,
coders, reviewers, testers, researchers, and documenter — plus workflow skills
that teach it how to run a structured development lifecycle.

Built on [meridian-base](https://github.com/haowjy/meridian-base). Both must
be installed.

## Split Orchestration → 3-Orchestrator Model

The dev lifecycle is split across three orchestrators:

**dev-orchestrator** (interactive) owns the user relationship — understanding
intent, clarifying requirements, reviewing designs with the user, and approving
implementation plans.

**design-orchestrator** (autonomous) owns design exploration — architecture,
tradeoffs, review cycles, and implementation planning until the design package
is ready for approval.

**impl-orchestrator** (autonomous) owns execution — implementing approved phases
through code, test, review, and fix loops until complete.

```bash
# dev-orchestrator handles requirements + design review
meridian spawn -a dev-orchestrator -p 'Build JWT token validation'

# dev-orchestrator spawns design-orchestrator for autonomous design
# dev-orchestrator reviews the design with the user, then spawns impl-orchestrator
```

## Agents

**Orchestrators:**

| Agent | Model | Role |
|---|---|---|
| `dev-orchestrator` | (harness default) | User relationship — understands intent, reviews designs, approves plans |
| `design-orchestrator` | opus | Autonomous design — architecture, tradeoffs, review cycles, implementation planning |
| `impl-orchestrator` | claude-opus-4-6 | Autonomous implementation — executes all phases through code/test/review/fix loops |

**Design & Planning:**

| Agent | Model | Role |
|---|---|---|
| `architect` | opus | Explores tradeoffs and produces design docs that implementation agents build from |
| `planner` | opus | Decomposes designs into independently executable phases with dependency mapping |
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
| `smoke-tester` | sonnet | End-to-end testing from the user's perspective — CLI flows, HTTP requests, race probes |
| `browser-tester` | opus | Browser-based QA via Playwright — visual verification, user flows, console errors |

**Review & Analysis:**

| Agent | Model | Role |
|---|---|---|
| `reviewer` | gpt | Deep code review — specify a focus area (security, SOLID, correctness) or leave broad |
| `investigator` | gpt | Brief triage of flagged issues — quick-fixes trivial items, files GH issues for the rest |

**Research & Documentation:**

| Agent | Model | Role |
|---|---|---|
| `researcher` | codex | Best practices, library comparisons, and architecture patterns via web search |
| `explorer` | gpt-5.3-codex-spark | Fast, cheap codebase explorer — reads files, searches code, mines past sessions |
| `documenter` | opus | Technical document synthesis — writes clear, linked docs humans and agents can navigate |

## Skills

**Workflow orchestration:**

| Skill | What it teaches |
|---|---|
| `decision-log` | Decision capture — reasoning, alternatives, constraints |
| `dev-artifacts` | Shared artifact convention between orchestrators |
| `context-handoffs` | Context scoping for agent spawns |
| `architecture` | Problem framing, tradeoff analysis, visual communication with Mermaid diagrams |
| `planning` | Decomposing designs into phases — focused blueprints, dependency mapping, execution order |
| `review-orchestration` | Directing reviewers — choosing focus areas, model diversity, synthesizing findings |
| `agent-staffing` | Team composition — which agents to spawn, how many, what runs in parallel |

**Agent methodology:**

| Skill | What it teaches |
|---|---|
| `review` | Adversarial code review — severity thinking, structured reporting |
| `issues` | GitHub Issues integration — labels, work-item linking, `gh` CLI patterns |
| `browser-test` | Browser QA methodology — visual verification, accessibility, console errors |
| `smoke-test` | End-to-end testing — CLI, HTTP, race probes, interruption recovery |
| `unit-test` | Focused test writing — edge cases, regression guards, tricky logic |
| `verification` | Build verification — getting tests, types, and lint green |
| `tech-docs` | Technical writing craft — hierarchical docs, linking strategy, and progressive disclosure |
| `frontend-design` | Distinctive, production-grade frontend interfaces — anti-generic-AI aesthetics |
| `mermaid` | Mermaid diagram syntax rules and validation |

## Cross-Source Dependencies

Several agents load skills from both this repo and `meridian-base`:

- `__meridian-spawn` (base) — how to spawn and coordinate agents
- `__meridian-session-context` (base) — how to mine past sessions for context
- `__meridian-work-coordination` (base) — how to manage work items

The install engine warns about cross-source deps but doesn't fail — these
resolve from the base source. Both sources must be installed.

## Install

```bash
mars add haowjy/meridian-base
mars add haowjy/meridian-dev-workflow
```

## Layout

```
agents/*.md              # Agent profiles (YAML frontmatter + markdown)
skills/*/SKILL.md        # Skills (with optional resources/ subdirectory)
```

## See Also

- [meridian-channel](https://github.com/haowjy/meridian-channel) — the Meridian coordination engine
- [meridian-base](https://github.com/haowjy/meridian-base) — core coordination layer this builds on
