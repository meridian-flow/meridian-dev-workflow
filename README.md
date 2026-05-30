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

**ux-lead** (interactive) — visual design and UX entry point. Gathers visual
requirements, establishes shared design vocabulary, routes to implementation
specialists. Gates on `visual-requirements.md`.

**design-lead** (autonomous) — owns the technical design. Explores structural
options, produces high-level structure, key interfaces, boundaries, patterns,
and tradeoffs.

**tech-lead** (autonomous) — owns implementation end-to-end. Decomposes work,
coordinates specialists, verifies functionality, owns targeted boundary tests,
safely restructures, and runs a final structural review before shipping.

**** (autonomous) — designs the permanent test suite from the design
package and shared understanding. Also handles structural test-suite work when
needed.

**kb-lead** (autonomous, conditional) — coordinates knowledge capture across
.context/, KB, and docs/ layers. Spawned when implementation produces
understanding worth preserving; timing depends on the workflow.

```bash
# Default lifecycle:
# product-lead -> design-lead -> tech-lead
# ux-lead -> frontend-coder (for visual work)
# kb-lead runs when knowledge capture is needed
meridian spawn -a product-lead -p 'Build JWT token validation'
```

## Agents

**Leads:**

| Agent | Model | Role |
|---|---|---|
| `product-lead` | opus | Primary developer — requirements gathering, routing, design approval |
| `ux-lead` | opus | Visual design entry point — visual requirements, design vocabulary, frontend routing |
| `design-lead` | claude-opus-4-6 | Technical design — structural options, interfaces, boundaries, tradeoffs |
| `tech-lead` | gpt55 | Implementation owner — decomposition, coordination, verification, structural review |

**Design:**

| Agent | Model | Role |
|---|---|---|
| `architect` | gpt-5.4 | Explores tradeoffs and produces hierarchical design docs with spec/architecture trees |
| `design-writer` | deepseek | Lightweight design doc writer — post-review updates, scope adjustments, settled design edits |

**Implementation:**

| Agent | Model | Role |
|---|---|---|
| `coder` | composer | Production code writer — implements scoped tasks and behavior-preserving refactors |
| `frontend-coder` | opus47 | Production frontend code with visual self-verification via playwright-cli |

**Testing & Verification:**

| Agent | Model | Role |
|---|---|---|
| `probe` | gpt-5.4 | Runtime verification — CLI flows, HTTP requests, race probes, integration boundaries |
| `browser-probe` | gpt55 | Browser-based QA via Playwright — visual verification, user flows, console errors |

**Review & Analysis:**

| Agent | Model | Role |
|---|---|---|
| `reviewer` | gpt-5.4 | Deep code review — specify a focus area (security, structural health, correctness) or leave broad |
| `alignment-reviewer` | gpt | Coverage verification — does one artifact deliver what another promised? |
| `simplify-reviewer` | gpt | Structural friction audit — shallow modules, fragmentation, deletion targets |
| `investigator` | gpt-5.4 | Brief triage of flagged issues — quick-fixes trivial items, files GH issues for the rest |

**Research & Documentation:**

| Agent | Model | Role |
|---|---|---|
| `web-researcher` | gpt-5.4-mini | External evidence — library docs, upstream issues, architecture patterns via web search |
| `explorer` | gpt-5.4-mini | Fast, cheap codebase explorer — reads files, searches code, mines past sessions |
| `kb-lead` | sonnet | Coordinates knowledge capture — routes to @code-mirror, @kb-writer, @tech-writer |
| `code-mirror` | deepseek | Writes .context/CONTEXT.md and AGENTS.md at module boundaries |
| `tech-writer` | deepseek | User-facing docs — getting started guides, API reference, CLI usage, tutorials |

**Visual:**

| Agent | Model | Role |
|---|---|---|
| `browser` | gpt55 | General-purpose browser interaction — scraping, navigation, screenshots |
| `imagegen` | gpt55 | Image generation — UI concept mockups, visual explorations, icons |

**Deprecated** (retained as legacy artifacts):

| Agent | Model | Status |
|---|---|---|
| `integration-tester` | gpt-5.4 | Use `@coder` with `/testing` `resources/integration-patterns.md` instead |

## Skills

**Workflow orchestration:**

| Skill | What it teaches |
|---|---|
| `decision-log` | Decision capture — reasoning, alternatives, constraints |
| `dev-artifacts` | Shared artifact convention between leads — requirements, design, plan/status |
| `session-mining` | Session-mining workflow patterns — recover parent-session decisions and delegate bulk transcript reads |
| `architecture` | Problem framing, tradeoff analysis, approach evaluation |
| `planning` | Plan execution — phases, subphases, verification levels, probe/diagnosis lanes |
| `agent-staffing` | Team composition — which agents to spawn, how many, what runs in parallel |
| `dev-principles` | Simplicity, separation of concerns, structural judgment — the operating lens for code decisions |
| `testing` | Restraint-first testing discipline — tier selection, when NOT to write tests, functional core patterns. Resources cover unit, integration, manual, and browser testing. |

**Agent methodology:**

| Skill | What it teaches |
|---|---|
| `review` | Adversarial code review — severity thinking, structured reporting |
| `improve-codebase-architecture` | Structural improvement — shallow modules, fragmentation, deletion targets, deep-module opportunities, code-judo moves. |
| `issues` | GitHub Issues integration — labels, work-item linking, `gh` CLI patterns |
| `react-architecture` | React-specific structural lens — tokens, state, composition, imports, component API consistency |
| `tech-docs` | Technical writing craft — hierarchical docs, linking strategy, and progressive disclosure |
| `frontend-design` | Distinctive, production-grade frontend interfaces — anti-generic-AI aesthetics |
| `thermo-nuclear-review` | Extremely strict maintainability review — abstraction quality, code judo moves |
| `post-impl-capture` | Post-implementation knowledge capture workflow for @kb-lead |

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
