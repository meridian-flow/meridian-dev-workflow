---
name: shared-workspace
description: Shared-repo safety for multi-agent dev environments — orientation on active work, rules for not destroying other actors' uncommitted changes, and staging discipline.
---

# Shared Workspace

You are one of several agents working in this repository concurrently. Other agents — and humans — may have uncommitted changes, untracked files, or in-progress work that you cannot distinguish from stale artifacts by inspection alone.

## Orientation

At the start of your session, run `meridian work` to see what work items are active and who else may be operating in the repo. Check `git status` for uncommitted changes that may belong to another actor. This context prevents you from accidentally interfering with in-progress work.

## Safety Rules

- **Never revert changes you did not create.** `git checkout .`, `git restore .`, `git reset --hard`, and `git clean` destroy other actors' uncommitted work. If you need a clean state for your own files, stage and restore surgically by path.
- **Never delete untracked files without confirming ownership.** Untracked files may be another agent's or human's in-progress work. If a file looks unfamiliar, leave it alone.
- **Stage only files you changed.** Use specific paths with `git add`, not `git add -A` or `git add .`. When committing spawn output, use `meridian spawn files <id>` to get the exact file list your spawn modified, then stage only those.
- **Unfamiliar code is not evidence of error.** If you encounter code, patterns, or files you don't recognize, do not assume they are bugs or leftover artifacts. Confirm intent before modifying or removing them.
- **Escalate conflicts instead of resolving them.** If your work overlaps another actor's uncommitted edits, report the conflict to your caller rather than force-merging or overwriting.
