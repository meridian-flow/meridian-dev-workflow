---
name: dev-artifacts
description: Trigger when creating, reading, or deciding where to put artifacts in the work directory — design packages, plans, decision logs, status files, preservation hints.
disable-model-invocation: true
allow_implicit_invocation: false
---

# Dev Artifacts

- Durable workflow state lives in the work directory.
- If it is not on disk, it is not durable.

## Layout

```text
<work_dir>/
  requirements.md
  design/
    spec/
    architecture/
    refactors.md
    feasibility.md
  plan/
    overview.md
    phase-N-<slug>.md        # scope, subphases, phase exit gate, EARS claims
    leaf-ownership.md
    status.md
    pre-planning-notes.md
    preservation-hint.md
```

See `resources/plan-package.md` for phase-file structure and artifact contracts.
See `resources/ownership.md` for writer/reader rules and doc-layer placement.

## Convention Is Swappable

- This skill defines convention. Workflows can replace it without code changes.
