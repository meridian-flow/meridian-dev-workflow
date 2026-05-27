---
name: dev-artifacts
type: reference
description: Trigger when creating, reading, or deciding where to put artifacts in the work directory — design packages, plans, decision logs, status files, preservation hints.
model-invocable: false
---

# Dev Artifacts

- Durable workflow state lives in the work directory.
- If it is not on disk, it is not durable.

## Layout

```text
<work_dir>/
  requirements.md
  visual-requirements.md     # ux-lead visual problem statement
  vocab.md                   # shared terminology (when needed)
  design/
    spec/
    architecture/
    refactors.md
    feasibility.md
  plan/
    status.md
```

See `resources/ownership.md` for writer/reader rules and doc-layer placement.

## Convention Is Swappable

- This skill defines convention. Workflows can replace it without code changes.
