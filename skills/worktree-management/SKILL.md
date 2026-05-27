---
name: worktree-management
type: reference
description: >
  Trigger when creating, inspecting, rebinding, or cleaning up managed
  worktrees. Covers the command surface, layout conventions, cross-repo
  targeting, and recovery.
model-invocable: false
---

# Managed Worktrees

Meridian manages git worktrees for work items. Managed worktrees live at
`<repo>.worktrees/<worktree-name>`; the default worktree name is the work
item slug.

## Provisioning

Create a work item with its worktree in one step:

```bash
meridian work start --worktree "<descriptive name>"
```

Ensure or recover an existing managed worktree:

```bash
# Active work item:
meridian work worktree --ensure

# Explicit work item (no session re-attachment):
meridian work worktree <work-id> --ensure
```

`--ensure` is idempotent — it reuses or recovers the recorded worktree.

Inspect the current worktree assignment (without creating or recovering):

```bash
meridian work worktree
meridian work worktree <work-id>
```

`--ensure` does not relocate the calling session. After ensure, the session
stays in its launch directory.

## Spawning into a Worktree

```bash
meridian spawn -a <agent> --work <work-id> --worktree \
  --prompt-file handoff.md -f design/ -f requirements.md
```

`spawn --worktree` re-ensures the managed worktree before launch.
Requires a selected work item.

## Cross-Repo Targeting

When implementation belongs in a different repository, pass `--repo`:

```bash
meridian work worktree <work-id> --repo <path-or-alias> --ensure
meridian spawn -a <agent> --work <work-id> --worktree \
  --repo <path-or-alias> --prompt-file handoff.md
```

The target repo determines the worktree path
(`<target-repo>.worktrees/<worktree-name>`). Work item artifacts stay
under the authority project. Pass `--repo` on every subsequent
`--worktree` spawn.

## Metadata is Authoritative

The recorded worktree path is the source of truth. `--ensure` reuses it.

## Rebinding

Reassign a work item to a different existing directory:

```bash
meridian work set-worktree <work-id> <path>
```

`set-worktree` updates the recorded path. The target directory must
already exist.

## Cleanup

Remove a work item's worktree assignment (the directory stays on disk):

```bash
meridian work clear-worktree <work-id>
```

## Temporary Worktrees (No Work Item)

`meridian work worktree --ensure` without an active work item creates a
managed temporary worktree for the caller's own use. `spawn --worktree`
targets work-item worktrees, not temporary ones.
