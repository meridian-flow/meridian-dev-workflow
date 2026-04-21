# Artifact Ownership and Documentation Layers

Writer/reader rules for work-dir artifacts, and the placement rules for docs that live outside the work dir.

## Ownership

| Artifact | Writer | Readers |
|---|---|---|
| `requirements.md` | @dev-orchestrator | @design-orchestrator, @impl-orchestrator, @planner |
| `design/` | @design-orchestrator | @planner, @impl-orchestrator, @dev-orchestrator |
| `plan/` | @planner (spawned by @dev-orchestrator) | @impl-orchestrator, @dev-orchestrator |
| `plan/preservation-hint.md` | @dev-orchestrator | @planner, @impl-orchestrator |
| `decisions.md` | @design-orchestrator, @impl-orchestrator | all downstream |
| `.meridian/fs/` | @code-documenter | all agents |

## Redesign And Rejections

- Redesign brief lives in @impl-orchestrator terminal report.
- Keep only approved state at `design/` and `plan/`.
- Replace rejected drafts atomically. Use git history for prior versions.

## Documentation Layers

- `.meridian/fs/` — durable agent-facing architecture docs.
- `docs/` — user-facing docs.
- `.meridian/work/<work_id>/` — work-scoped artifacts and research.

Rules:

- Do not create `.meridian/fs/research/`.
- Work research stays in work dir.
- Durable findings are synthesized into `fs/` domain docs.
