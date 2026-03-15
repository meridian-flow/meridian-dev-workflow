# meridian-dev-workflow

Opinionated software development lifecycle methodology for Meridian. A complete dev team — coder, reviewers, testers, investigator, researcher, documenter — plus structured workflow skills for design → plan → implement → review → test → document.

Requires [meridian-base](https://github.com/haowjy/meridian-base) to be installed (the `dev-orchestrator` agent references base skills via cross-source dependencies).

## Layout

- `agents/*.md` — installable agent profiles
- `skills/*/SKILL.md` — installable skills (with optional `resources/`)

## Contents

### Agents (14)

| Agent | Model | Purpose |
|---|---|---|
| `dev-orchestrator` | opus | Full SDLC orchestrator (loads base + dev skills) |
| `coder` | codex | Production code writer — implements scoped tasks |
| `reviewer` | gpt | General code reviewer — broad quality dimensions |
| `reviewer-planning` | opus | Architecture alignment reviewer |
| `reviewer-security` | gpt | Security analysis — attack surface, vulnerabilities |
| `reviewer-solid` | gpt | SOLID principles — structural quality, design consistency |
| `reviewer-concurrency` | gpt | Concurrency correctness — race conditions, deadlocks |
| `browser-tester` | sonnet | Browser-based QA — visual verification, user flows |
| `smoke-tester` | sonnet | External QA — end-to-end testing from user perspective |
| `unit-tester` | gpt | Focused unit test writer |
| `verifier` | sonnet | Runs tests, type checks, linters — fixes mechanical failures |
| `investigator` | gpt | Bug investigation — brief triage, quick-fix or file GH issue |
| `researcher` | gpt | Read-only codebase explorer — answers questions with evidence |
| `documenter` | opus | Documentation maintainer — keeps docs in sync with code |

### Skills (6)

| Skill | Purpose |
|---|---|
| `dev-workflow` | Development lifecycle orchestration — phase loop, agent staffing, complexity routing |
| `design` | Interactive architecture design — collaborative problem-solving, design artifacts |
| `plan-implementation` | Breaking designs into executable phases — dependency mapping, agent headcount |
| `reviewing` | Adversarial code review methodology — review lenses, severity framework |
| `issue-tracking` | GitHub Issues integration — severity labels, work-item linking, `gh` CLI patterns |
| `documenting` | Documentation synchronization — two-pass discovery + writing pattern |

## Cross-Source Dependencies

The `dev-orchestrator` agent references skills from `meridian-base`:

- `__meridian-orchestrate`
- `__meridian-spawn-agent`
- `__meridian-work-coordination`

The install engine's dependency resolver warns about cross-source deps but does not fail — these skills resolve from the separately-installed base source. Both sources must be installed for `dev-orchestrator` to work.

## Install

```bash
# Install everything
meridian install @haowjy/meridian-dev-workflow

# Or selectively
meridian install @haowjy/meridian-dev-workflow --agents coder,reviewer
```

Requires `meridian-base` to be installed first:

```bash
meridian install @haowjy/meridian-base
meridian install @haowjy/meridian-dev-workflow
```

## The Lifecycle

```
designing → reviewing → planning → implementing → done
```

Each phase has associated agents and artifacts. The `dev-workflow` skill orchestrates the full loop; phase-specific skills (`design`, `plan-implementation`, `reviewing`) teach the craft for each phase.

## See Also

- [meridian-channel](https://github.com/haowjy/meridian-channel) — the Meridian coordination engine
- [meridian-base](https://github.com/haowjy/meridian-base) — core coordination layer this builds on
