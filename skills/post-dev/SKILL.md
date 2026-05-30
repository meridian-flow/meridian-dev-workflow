---
name: post-dev
type: checkpoint
description: Ship readiness — PR template, changelog, release label, cleanup. Run when implementation is done.
model-invocable: true
user-invocable: false
---

# Post-Dev Checkpoint

Run this when implementation is complete, before creating the PR.

## Checks

### PR readiness
- Read `.github/pull_request_template.md` — fill every section
- Set a `release:*` label (default: `release:patch`)
- PR title under 70 characters, descriptive

### Changelog
- `CHANGELOG.md` has entries under `## [Unreleased]` for this work
- Entries written in caveman style — terse, behavioral, filler-free
- Focus on what downstream users notice, not which lines moved

### Review
- Has structural review passed? If not, spawn a reviewer first.
- Any review findings addressed or explicitly accepted?

### Cleanup
- No stale files, dead code, or debug artifacts left behind
- No TODO comments added without corresponding issue

### After merge
- Prune worktrees: `scripts/prune-worktrees.sh`
- Verify CI passed on main
