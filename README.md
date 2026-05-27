# meridian-dev-workflow

An opinionated multi-agent dev team for structured software development,
built on [Meridian](https://github.com/haowjy/meridian-cli)'s coordination
primitives. Install this and your orchestrator gets a full squad — architects,
coders, reviewers, testers, web-researchers, and documenters — plus workflow
skills that teach it how to run a structured development lifecycle.

Built on [meridian-base](https://github.com/haowjy/meridian-base). Both must
be installed.


## Orchestrator Topology

The dev lifecycle splits across leads with distinct ownership:

**product-lead** (interactive) — the primary developer. Translates between
user and technical teams. Requirements gathering, scope sizing, design
approval, implementation routing. Spawns everything downstream.

**design-lead** (autonomous) — owns the technical design. Explores structural
options, produces high-level structure, key interfaces, boundaries, patterns,
and tradeoffs.

**tech-lead** (autonomous) — owns implementation end-to-end. Decomposes work,
coordinates specialists, verifies functionality, owns targeted boundary tests,
safely restructures, and runs a final structural review before shipping.

**qa-lead** (autonomous) — designs the permanent test suite from the design
package and shared understanding. Also handles structural test-suite work when
needed.

**kb-lead** (autonomous, conditional) — coordinates knowledge capture across
.context/, KB, and docs/ layers. Spawned when implementation produces
understanding worth preserving; timing depends on the workflow.

```bash
# Default lifecycle:
# product-lead → design-lead → tech-lead
# kb-lead runs when knowledge capture is needed (pre-merge, post-ship, or on request)
meridian spawn -a product-lead -p 'Build JWT token validation'
```

## Agents

**Leads:**

| Agent | Model | Role |
|---|---|---|
| `product-lead` | (harness default) | Primary developer — requirements gathering, routing, design approval |
| `design-lead` | opus | Technical design — structural options, interfaces, boundaries, tradeoffs |
| `tech-lead` | opus | Implementation owner — decomposition, coordination, verification, structural review |
| `qa-lead` | sonnet | Test suite designer — risk-based strategy, tier design, structural test work |

**Design:**

| Agent | Model | Role |
|---|---|---|
| `architect` | gpt | Explores tradeoffs and produces hierarchical design docs with spec/architecture trees |
| `design-writer` | deepseek | Lightweight design doc writer — post-review updates, scope adjustments, settled design edits |
**Implementation:**

| Agent | Model | Role |
|---|---|---|
| `coder` | codex | Production code writer — implements scoped tasks and behavior-preserving refactors |
| `frontend-coder` | opus | Production frontend code with distinctive design quality via the frontend-design skill |

**Testing & Verification:**

| Agent | Model | Role |
|---|---|---|
| `smoke-tester` | gpt-5.4 | End-to-end testing from the user's perspective — CLI flows, HTTP requests, race probes |
| `browser-tester` | opus | Browser-based QA via Playwright — visual verification, user flows, console errors |

**Review & Analysis:**

| Agent | Model | Role |
|---|---|---|
| `reviewer` | gpt | Deep code review — specify a focus area (security, structural health, correctness) or leave broad |
| `investigator` | gpt | Brief triage of flagged issues — quick-fixes trivial items, files GH issues for the rest |

**Research & Documentation:**

| Agent | Model | Role |
|---|---|---|
| `web-researcher` | codex | Best practices, library comparisons, and architecture patterns via web search — the external counterpart to `explorer` |
| `explorer` | gpt-5.4-mini | Fast, cheap codebase explorer — reads files, searches code, mines past sessions |
| `kb-lead` | sonnet | Coordinates knowledge capture — routes to @code-mirror, @kb-writer, @tech-writer |
| `kb-writer` | sonnet | Writes and updates the project's knowledge base — decisions, domain knowledge, architecture, synthesized research |
| `kb-maintainer` | gpt | Structural health of the KB — splits, merges, cross-references, staleness, conflict resolution |
| `tech-writer` | deepseek | Writes and maintains user-facing docs — getting started guides, API reference, CLI usage, and tutorials |

## Skills

**Workflow orchestration:**

| Skill | What it teaches |
|---|---|
| `decision-log` | Decision capture — reasoning, alternatives, constraints |
| `dev-artifacts` | Shared artifact convention between leads — v3 layout with spec/architecture trees |
| `session-mining` | Session-mining workflow patterns — recover parent-session decisions and delegate bulk transcript reads |
| `architecture` | Problem framing, tradeoff analysis, approach evaluation |
| `planning` | Plan execution — phases, subphases, verification levels, probe/diagnosis lanes |
| `agent-staffing` | Team composition — which agents to spawn, how many, what runs in parallel |
| `dev-principles` | Simplicity, separation of concerns, structural judgment — the operating lens for code decisions |
| `testing-principles` | Test tier selection, risk-based coverage, functional core / imperative shell |

**Agent methodology:**

| Skill | What it teaches |
|---|---|
| `review` | Adversarial code review — severity thinking, structured reporting |
| `issues` | GitHub Issues integration — labels, work-item linking, `gh` CLI patterns |
| `browser-test` | Browser QA methodology — visual verification, accessibility, console errors |
| `smoke-test` | End-to-end testing — CLI, HTTP, race probes, interruption recovery |
| `unit-test` | Focused test writing — edge cases, regression guards, tricky logic |
| `integration-test` | Composition testing — module boundaries, fakes at external systems |
| `react-architecture` | React-specific structural lens — tokens, state, composition, imports, component API consistency |
| `tech-docs` | Technical writing craft — hierarchical docs, linking strategy, and progressive disclosure |
| `frontend-design` | Distinctive, production-grade frontend interfaces — anti-generic-AI aesthetics |

## Cross-Source Dependencies

Several agents load skills from both this repo and `meridian-base`:

- `meridian-spawn` (base) — how to spawn and coordinate agents
- `meridian-work-coordination` (base) — how to manage work items
- `session-mining` (base) — workflow patterns for mining decisions from session history
- `kb-conventions` (base) — KB structure, navigation, writing standards, flag protocol

The install engine warns about cross-source deps but doesn't fail — these
resolve from the base source. Both sources must be installed.

## Install

```bash
meridian mars add meridian-flow/meridian-base
meridian mars add meridian-flow/meridian-dev-workflow
meridian config set primary.agent product-lead
```

## Layout

```
agents/*.md              # Agent profiles (YAML frontmatter + markdown)
skills/*/SKILL.md        # Skills (with optional resources/ subdirectory)
```

## See Also

- [meridian-cli](https://github.com/meridian-flow/meridian-cli) — the Meridian coordination engine
- [meridian-base](https://github.com/meridian-flow/meridian-base) — core coordination layer this builds on
