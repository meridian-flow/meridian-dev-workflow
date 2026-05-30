---
name: dev-workflow
type: reference
description: Commit discipline during implementation — commit after each passing step, changelog at commit time.
model-invocable: true
---

# Dev Workflow

Commit discipline during implementation.

## Commit Discipline

Commit after each step that passes checks. Don't accumulate changes across multiple steps.

1. Implement the change
2. Verify (lint, type-check, tests)
3. Commit with a descriptive message
4. Move to next step

Keep `CHANGELOG.md` current under `## [Unreleased]` — write entries at commit time, not retroactively.

## Do Not

- Push directly to main for feature work (use PRs)
- Create or push `v*` tags manually (CI owns tagging)
- Use `--no-verify` on push without explicit user permission
- Delete untracked files without asking (may be someone else's work)
- Edit `__version__` or `Cargo.toml` version for stable releases (CI-owned)
