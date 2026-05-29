---
name: issues
type: reference
description: Use when you find bugs, unexpected behavior, deferred decisions, or backlog items worth tracking beyond the current work item. Also use when asked to file an issue or when investigation determines a GH issue is needed rather than a quick fix.
---

# Issue Tracking

File GitHub issues proactively. File an issue when follow-up needs durable tracking beyond the current report.

GitHub Issues are a visibility layer on top of spawn reports and work-scoped
detail: GitHub issue filing conventions for bugs, decisions, and backlog.
notes. If `gh` is unavailable, report that issue filing was skipped and log the draft locally.

## When to File

- Bug in adjacent code you can't fix without derailing current work
- Unexpected behavior that isn't broken enough to stop for
- Backlog items discovered during implementation (cleanup, missing tests,
  hardcoded values, dead code)
- Deferred design decisions that need broader context
- Review findings marked non-blocking but worth addressing later
- At phase boundaries, mine conversation history and touched files for
  deferred items, TODOs, and debt markers

Quick rule: fix it if < 5 minutes and you're in the area. File an issue for
everything else.

## Mechanics

Check `gh` availability before creating:

```bash
gh auth status 2>/dev/null && gh repo view --json name 2>/dev/null
```

Use consistent labels so the team can filter by category. Tag every issue with
the discovering work item (`work:<slug>`). Every issue body answers: where was
this found, what is it, what should be done.

See [`resources/gh-commands.md`](resources/gh-commands.md) for the label
taxonomy, issue body template, and `gh` CLI commands.
