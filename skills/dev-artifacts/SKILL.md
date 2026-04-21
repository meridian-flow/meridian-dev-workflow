---
name: dev-artifacts
description: Trigger when creating, reading, or deciding where to put artifacts in the work directory — design packages, plans, decision logs, status files, preservation hints.
---

# Dev Artifacts

- Durable workflow state lives in the work directory.
- If it is not on disk, it is not durable.

## Discover Paths

```bash
meridian work current     # absolute path to work directory
meridian context kb       # absolute path to knowledge base
```

- Query paths once at session start. Use the literal paths returned.

## Layout

```text
<work_dir>/
  requirements.md
  decisions.md
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
