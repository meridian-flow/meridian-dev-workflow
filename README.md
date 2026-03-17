# meridian-dev-workflow

An opinionated dev team for [Meridian](https://github.com/haowjy/meridian-channel).
Install this and your orchestrator gets a full squad — coder, reviewers,
testers, investigator, researcher, documenter — plus workflow skills that
teach it how to run a structured software development lifecycle.

Built on [meridian-base](https://github.com/haowjy/meridian-base). Both must
be installed.

## What You Get

The orchestrator can now run a full dev lifecycle autonomously:

```bash
# Research phase — explore the codebase on a fast model
meridian spawn -a researcher -p "Map the auth module — all token handling and session management"

# Design phase — spawn a designer to flesh out the architecture
meridian spawn -a designer --from p1 -p "Design JWT token validation — explore tradeoffs between symmetric and asymmetric signing"

# Implementation — spawn coders in parallel, passing prior context
meridian spawn -a coder --from p1 -p "Phase 1: JWT token validation" -f plan/phase-1.md
meridian spawn -a coder --from p1 -p "Phase 2: Middleware integration" -f plan/phase-2.md

# Review — fan out reviewers on a different model family
meridian spawn -a reviewer --from p2 -p "Review phase 1 — focus on security"
meridian spawn -a reviewer --from p3 -p "Review phase 2 — focus on error handling"

# Test — verify the build is clean
meridian spawn -a verification-tester -p "Run tests, type checks, and lint"

# Investigate — triage an unexpected finding without derailing the main work
meridian spawn -a investigator -p "The reviewer found a race condition in refresh.py — quick-fix or file an issue"
```

Each agent has a model, sandbox tier, and skill set tuned to its role.
The orchestrator picks the right agent for each task.

## Agents

**Orchestration:**

| Agent | Model | Role |
|---|---|---|
| `dev-orchestrator` | (configured default) | Plans, delegates, and drives work through design → plan → implement → review → test |

**Implementation:**

| Agent | Model | Role |
|---|---|---|
| `designer` | opus | Explores tradeoffs, creates design docs, and captures decisions with rationale |
| `coder` | codex | Implements scoped tasks from phase specs and design docs |
| `researcher` | codex | Explores best practices, alternatives, and architecture patterns via web search |
| `explorer` | gpt-5.3-codex-spark | Fast, cheap codebase explorer — reads files, searches code, mines past sessions |
| `documenter` | opus | Synthesizes technical documentation from codebase into `$MERIDIAN_FS_DIR` |

**Review:**

| Agent | Model | Role |
|---|---|---|
| `reviewer` | gpt | Broad code review across quality dimensions — the orchestrator sets the focus via prompt |
| `investigator` | gpt | Brief triage of flagged issues — quick-fixes trivial items, files GH issues for the rest |

**Testing:**

| Agent | Model | Role |
|---|---|---|
| `verification-tester` | gpt | Runs tests, type checks, and linters — fixes mechanical breakage, reports real issues |
| `unit-tester` | gpt | Writes and runs targeted unit tests for specific edge cases and regression guards |
| `smoke-tester` | codex | End-to-end testing from the user's perspective — CLI flows, HTTP requests, race probes |
| `browser-tester` | opus | Browser-based QA — visual verification, user flows, form testing, console errors |

## Skills

**Workflow orchestration:**

| Skill | What it teaches |
|---|---|
| `dev-orchestration` | Phase sequencing, agent staffing, scaling ceremony to task complexity |
| `architecture-design` | Collaborative design with the user — problem framing, tradeoff analysis, Mermaid diagrams |
| `plan-implementation` | Decomposing designs into phases — focused blueprints, dependency mapping, execution order |
| `review-orchestration` | Directing reviewers — choosing focus areas, model diversity, synthesizing findings |

**Agent methodology:**

| Skill | What it teaches |
|---|---|
| `review` | Adversarial code review — severity thinking, structured reporting |
| `issue-tracking` | GitHub Issues integration — labels, work-item linking, `gh` CLI patterns |
| `browser-testing` | Browser QA methodology — visual verification, accessibility, console errors |
| `smoke-testing` | End-to-end testing methodology — CLI, HTTP, race probes, interruption recovery |
| `unit-testing` | Focused test writing — edge cases, regression guards, tricky logic |
| `verification-testing` | Build verification — getting tests, types, and lint green |
| `tech-docs` | Technical documentation — compressed codebase mirror with architecture and decision rationale |
| `mermaid` | Mermaid diagram syntax rules and validation |

## Cross-Source Dependencies

The `dev-orchestrator` agent loads skills from both this repo and `meridian-base`:

- `__meridian-spawn-agent` (base) — how to spawn and coordinate agents
- `__meridian-session-context` (base) — how to mine past sessions for context
- `__meridian-work-coordination` (base) — how to manage work items

The install engine warns about cross-source deps but doesn't fail — these
resolve from the base source. Both sources must be installed.

## Install

```bash
meridian sources install @haowjy/meridian-base
meridian sources install @haowjy/meridian-dev-workflow
```

## Layout

```
agents/*.md              # Agent profiles (YAML frontmatter + markdown)
skills/*/SKILL.md        # Skills (with optional resources/ subdirectory)
```

## See Also

- [meridian-channel](https://github.com/haowjy/meridian-channel) — the Meridian coordination engine
- [meridian-base](https://github.com/haowjy/meridian-base) — core coordination layer this builds on
