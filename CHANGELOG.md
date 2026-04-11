# Changelog

Caveman style. Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning: [SemVer](https://semver.org/). Versions before 0.0.14 in git history only.

## [Unreleased]

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
