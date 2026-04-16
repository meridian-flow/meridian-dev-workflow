# Changelog

Caveman style. Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning: [SemVer](https://semver.org/). Versions before 0.0.14 in git history only.

## [Unreleased]

## [0.0.26] - 2026-04-16

### Added
- `@impl-orchestrator` gains explicit **Explore** phase before Plan. Verifies design against code reality — every structural claim in `design/refactors.md` or the architecture tree gets a file:line pointer confirming current code supports it; falsified claims trigger a Redesign Brief *before* any planning burn. Root-caused from R06 workspace-config-design cycle where design narrative ("move composition into factory") didn't match code shape (factory input DTO already carried pre-resolved outputs — the "move" was structurally impossible). Explore is a gate: `plan/pre-planning-notes.md` must exist + be populated before `@planner` spawns. Required fields: verified claims, falsified claims, latent risks not in design, probe gaps, leaf-distribution hypothesis. Previous prompt treated pre-planning notes as a preamble, which made skipping them invisible.
- `@impl-orchestrator` Redesign Brief section lists explore-falsified as the cheapest trigger — costs only the explore phase, no planning or coding wasted. Plan and build triggers cost more. Explore exists to maximize the first and minimize the rest.
- `agent-staffing` skill: new **@reviewer as Architectural Drift Gate** section under "When Reviewers Apply". Names the anti-pattern of rg-count / grep-count CI invariants for architectural enforcement — gameable via rename-and-shim (coder optimizing to green CI can stub the "correct" site and keep real composition at the old site, count stays clean, ship drift). Prescribes `@reviewer` with a declared-invariant prompt (lives at `.meridian/invariants/<surface>-invariant.md` or similar), CI spawns reviewer on PRs touching protected surface, blocks merge on `fail`. Pair with deterministic behavioral tests as backstop — reviewer is probabilistic, tests pin down specific invariants. Distinct @reviewer use from design review and final implementation review.

### Changed
- `agent-staffing` skill: new **Terminology: Fan-Out vs Parallel Lanes** section near the top. Names the distinction between same-prompt-different-models (fan-out, reserved for critical decisions) and different-prompts-different-focus-areas (parallel lanes, default review posture). Previous language conflated them in several places — "testers fan out in parallel" read as same-prompt-different-models to careful readers, but the intent was parallel lanes. Clarified the "testers fan out in parallel" and "final review loop fan-out" language in the Parallelism section to use the right terms.

## [0.0.25] - 2026-04-16

### Changed
- `@impl-orchestrator` full prompt rewrite. Old prompt framed delegation as a constraint ("never write code") and scattered spawn triggers across 6 sections. New prompt mirrors `@design-orchestrator` structure: positive role identity ("you drive it to shipped code"), inline WHY for the delegation rule, explicit "`meridian spawn` is a shell command you invoke through the Bash tool" teaching with a concrete `Bash("meridian spawn -a coder ...")` example, and a centralized Delegation Strategy section listing every agent's spawn trigger. Root-caused from p1900 where opus used Edit 6× and Write 12× despite `disallowed-tools: [Edit, Write, NotebookEdit]` — prompt-steering, not YAML enforcement, is the primary guardrail.
- `@impl-orchestrator`: `effort: medium` → `effort: high`. Multi-hour autonomous orchestration runs need the thinking budget for instruction compliance under pressure.
- `@docs-orchestrator`: `effort: medium` → `effort: high`. Same autonomous-run reasoning. Opening rewritten for positive framing. Removed defensive "don't work around this through Bash file writes" hedge — hedges plant the workaround idea before forbidding it. Added `meridian spawn` Bash-tool teaching with concrete example.
- `@dev-orchestrator`: added `meridian spawn` Bash-tool teaching with concrete example and `/meridian-cli` skill reference.
- `@design-orchestrator`: merged duplicate `disallowed-tools` keys into one. YAML last-key-wins meant the destructive-git restrictions were silently dropped — only `[Agent]` was enforced.
- All four orchestrators: `tools: [Bash]` → `tools: [Bash, Bash(meridian spawn *)]`. Generic `Bash` remains for reading and verification commands; the explicit `Bash(meridian spawn *)` entry signals to readers and future stricter allowlist enforcement that `meridian spawn` is the primary action tool.

## [0.0.22] - 2026-04-14

### Changed
- `shared-workspace` skill scope tightened. Removed from agents that don't mutate repo state: `@explorer`, `@reviewer`, `@refactor-reviewer`, `@architect`, `@planner`, `@frontend-designer`, `@internet-researcher`. Orientation (`meridian work` / `git status` at session start) and the safety rules (no destructive git, no `git add -A`, don't delete unfamiliar untracked files) don't apply to read-only agents or agents that only write to `$MERIDIAN_WORK_DIR/` — their caller already holds repo orientation. Retained on all four orchestrators, code-editing agents (`@coder`, `@frontend-coder`, `@verifier`, `@investigator`, `@code-documenter`, `@tech-writer`, `@unit-tester`), and broad-permission testers (`@browser-tester`, `@smoke-tester`) where the safety rules are real guardrails.
- `@dev-orchestrator`: `effort: medium` → `effort: high`. User-facing orchestrator handling intent gathering, design review, and redesign routing across design/impl orchestrators was under-resourced at medium.
- `dev-principles` skill: new "Depend Deliberately" section — the pair to "Delete Often". A well-maintained library is a pre-validated abstraction; it has already survived the Rule of Three in the wild. Dependency earns its place when it deletes more code than it adds and collapses subsystems rather than swapping primitives. Simplicity measured by total ownership (code + cognitive load + failure modes + test matrix), not import count. Rejects the reflexive "stdlib-only is cleaner" frame.
- `dev-principles` skill: new "Probe Your Options Before You Commit" section — deduction from reading code is cheap but wrong often enough to cost rework cycles. Match probe investment to decision reversibility: cheap experiments for one-way-door decisions, skip for reversible ones. Treat "we'll find out during implementation" as a risk flag, not a plan. When you catch yourself deducing instead of probing ("this looks like phantom complexity"), stop and design the probe.
- `dev-principles` skill: new "Name the Constraint Before Deleting" subsection under integration-boundary probing — reading code tells you what it does, not why it exists. `git log -S <symbol>`, targeted tests, decision logs before removal. "Looks excessive to a fresh reader" is not the same as "is excessive" — code defending an invariant under concurrent load or preserving interactive fidelity will always look excessive on a calm read.
- `dev-principles` skill: new structural-health signal — platform-specific imports (`fcntl`, `msvcrt`, `termios`, `winreg`, `pwd`, `select.kqueue`) or OS-conditional branches appearing in more than one module. Mechanism is leaking into policy; collapse to one adapter.

### Added
- `disallowed-tools` entries for destructive git commands (`git revert:*`, `git checkout --:*`, `git restore:*`, `git reset --hard:*`, `git clean:*`) on agents with `Bash` tool access. Hard guard alongside the `shared-workspace` safety rules — prevents accidental destruction of other actors' uncommitted work even if the skill guidance is ignored.

## [0.0.19] - 2026-04-11

### Changed
- Spec-driven workflow restructure. Design package splits into `design/spec/` (EARS-notation behavioral contract with stable IDs) + `design/architecture/` (technical realization) + `design/refactors.md` (rearrangement agenda) + `design/feasibility.md` (probe evidence). Plan gains `plan/leaf-ownership.md` (exclusive EARS-statement-to-phase ownership), `plan/pre-planning-notes.md` (planning impl-orch runtime observations), `plan/preservation-hint.md` (dev-orch carry-over on redesign cycles). Replaces the `scenarios/` folder convention — verification keys on claimed EARS statement IDs, not scenario files.
- `@impl-orchestrator` two-role contract: planning role (consumes design, calls `@planner`, terminates plan-ready) and execution role (consumes plan from disk, drives phase loops, terminates converged). Each role runs in its own spawn. Planning caps `K_fail=3`, `K_probe=2`. Execution adds preserved-phase re-verification and spec-drift escape hatch. All escape hatches emit a `Redesign Brief` section in the terminal report.
- `@planner` three terminal shapes: `plan-ready`, `probe-request`, `structural-blocking`. Distinguishes knowledge gaps from structural properties — structural-blocking is a redesign signal, not a retry case. Ownership at EARS-statement granularity. Parallelism-first; refactor agenda mandatory.
- `@design-orchestrator` spec-first ordering, active gap-finding (probe real systems while designing), four-lens convergence (behavioral correctness, structural soundness, spec/arch alignment, refactor impact).
- `@dev-orchestrator` v3 routing with plan acceptance criteria keyed to EARS ownership and refactor accounting. Autonomous redesign loop: `design-problem` → design-orch + preservation hint + counter advance; `scope-problem` → fresh planning impl-orch. `K=2` design-problem cap. Trivial path names `@frontend-coder` / `@code-documenter` / `@tech-writer` alongside `@coder`.
- `@docs-orchestrator` tightened: drops inline spawn examples (→ `/meridian-spawn`), adds `decisions.md` and `design/spec/` + `design/architecture/` as mining/authority sources, adds explicit "never write docs directly" standing principle, collapses prescriptive phase steps into behavioral write/review/fix description.
- `@coder` loads `dev-principles`, bound to claimed EARS statement IDs as verification contract.
- `@reviewer` loads `dev-principles`, validates EARS alignment. Principle violations are ordinary findings.
- `planning` skill rewritten for v3: required files, overview.md contract, leaf-ownership.md contract, staffing as mandatory output, terminal shapes.
- `dev-artifacts` skill rewritten as canonical v3 layout reference.
- `smoke-test`, `unit-test`, `verification` skills: scenarios/ acceptance → claimed-EARS-statement acceptance. Load `/ears-parsing` for per-pattern parse and per-ID reporting (`verified` / `falsified` / `unparseable` / `blocked`).
- 29 agent + skill descriptions rewritten from WHAT-lead to WHEN-lead ("Use when ___" / "Spawn when ___" / "Spawned by ___"). Fixes widespread WHAT-in-frontmatter pattern — descriptions serve callers deciding whether to spawn/load, not runners. Worst offenders fixed: `dev-principles` now has a real trigger, `mermaid` is no longer a fragment. Load-bearing sibling discriminators preserved.

### Added
- `ears-parsing` skill: mechanical EARS verification contract for testers. Pattern table (u/s/e/w/c → trigger/fixture/assertion), per-ID reporting format, escape valve for non-parseable statements. Loaded by `smoke-test`, `unit-test`, `verification`.

### Removed
- `scenarios/` folder convention. Replaced by `design/spec/` EARS leaves + `plan/leaf-ownership.md` ledger.

## [0.0.18] - 2026-04-10

### Added
- `dev-principles` skill: new "Keep Docs Current" section. Two bullets — update docs in the same change as the behavior they describe (drift compounds silently), and treat reference material for external tools as a snapshot that must be re-verified when versions change. Companion lesson to the v0.0.17 "Configuration is integration too" bullet: v0.0.17 covered the consumer side (don't trust docs blindly), v0.0.18 covers the producer side (don't let your own docs drift).

## [0.0.17] - 2026-04-10

### Changed
- `dev-principles` skill: extend "Probe Before You Build at Integration Boundaries" with a new bullet — "Configuration is integration too. When you add a field to an external tool's config based on docs, verify the installed tool honors it end-to-end — parser acceptance isn't behavior, and docs often describe features the installed version doesn't implement." Lesson from the `v0.0.16` caveman exclude misadventure.

### Removed
- `exclude = ["skill:caveman-commit", "skill:caveman-review"]` from the `caveman` dep block in `mars.toml`. Dead code — `FilterConfig::to_mode()` in mars makes filter modes mutually exclusive (`skills.is_some()` fires before `exclude.is_some()`), so the field was silently dropped on every read. Shipped in `v0.0.16` based on a misread of `mars-toml-reference.md` without verifying the installed mars version honored it. The dead-weight `caveman-commit` and `caveman-review` skills in `.agents/skills/` are caused by a separate bug — transitive filter drop in mars `0.0.9`, already fixed upstream in mars-agents commit `b540032` (included in mars `0.0.13`). They will clean up automatically once meridian's bundled mars is upgraded.

## [0.0.16] - 2026-04-10

### Fixed
- `@internet-researcher` frontmatter: bare colon in description ("Reads the internet, not the codebase: library docs...") broke YAML parse — `mars check` flagged `mapping values are not allowed here`. Converted description to folded block style (`description: >`), matching `@architect` convention. Shipped broken in `v0.0.14`; this is the first release where `@internet-researcher` actually loads.
- `caveman` dep filter: `skills = ["caveman"]` did prefix-match, not exact-match, in mars sync — pulled `caveman-commit` and `caveman-review` into `.agents/skills/` alongside `caveman`. Added `exclude = ["skill:caveman-commit", "skill:caveman-review"]` so next sync drops them as orphans. No agent referenced them, so effect is cleanup only.

### Changed
- `CHANGELOG.md` rewritten in caveman style. Prior entries (`0.0.14`, `0.0.15`) ported, substance preserved. Convention documented in `meridian-channel/AGENTS.md`.

## [0.0.15] - 2026-04-10

### Added
- `caveman` skill (dep on [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)) loaded into intermediary orchestrators: `@design-orchestrator`, `@impl-orchestrator`, `@docs-orchestrator`. Compresses coordination chatter (delegation prompts, decision logs, phase status). Each runs `caveman full` mode with agent-specific extension: decision logs, phase status, `scenarios/` seeds still record *why* in caveman style so resumed work rehydrates reasoning. Sub-agent profiles (`@architect`, `@coder`, `@code-documenter`, `@tech-writer`, etc.) stay non-caveman — design docs, code, `fs/` mirrors, user docs untouched.
- `caveman` dep declared in `mars.toml`. Pins to `v0.0.15` pick it up transitively. Pins to `v0.0.14` skip caveman entirely.

## [0.0.14] - 2026-04-10

### Added
- `@investigator`: delegation capability. Spawns `@smoke-tester`, `@explorer`, `@unit-tester`, `@internet-researcher`, or narrower `@investigator` recursively. Sandbox → `danger-full-access` for `gh`/`curl`/web tools.
- `@impl-orchestrator`: "External knowledge gaps" escalation paragraph. `@coder` stuck on lib/API behavior? Spawn `@internet-researcher`, don't burn `@coder` cycles guessing from training-data assumptions.
- `CHANGELOG.md` introduced. Keep a Changelog format (later caveman-ified, see `[Unreleased]`).

### Changed
- `@researcher` → `@internet-researcher`. New name advertises external-knowledge role, pairs with `@explorer` (internal counterpart). Body refreshed: "internet vs codebase" split explicit, no overlap with `@explorer`.
- `@investigator` refocused around diagnose → triage. Three outcomes: scoped fix, filed GH issue, documented non-issue. Dropped dual-primary reactive/backlog-sweep split. GH issue filing = first-class, not fallback.
- `@architect` "External research" section rewritten. Frames external research as grounding design in ecosystem knowledge, not training-data guessing. Calls out `@internet-researcher` vs `@explorer`.
- `@design-orchestrator` "Research what you don't know": names `@internet-researcher` as "single most-forgotten delegation in design loop." Urges heavy use.
- `agent-staffing` `builders.md`: `@internet-researcher` headlines with same framing. `@explorer` reframed as "internal counterpart."
- `README` prose + agent table updated for rename.

### Removed
- `@researcher` profile. Replaced by `@internet-researcher`.
