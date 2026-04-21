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
| kb (`meridian context kb`) | @code-documenter | all agents |

## Redesign And Rejections

- Redesign brief lives in @impl-orchestrator terminal report.
- Keep only approved state at `design/` and `plan/`.
- Replace rejected drafts atomically. Use git history for prior versions.

## Documentation Layers

- **kb** (`meridian context kb`) — durable agent-facing architecture docs.
- **`docs/`** — user-facing docs.
- **work dir** (`meridian work current`) — work-scoped artifacts and research.

Rules:

- Do not create a `research/` directory in the kb.
- Work research stays in the work directory.
- Durable findings are synthesized into kb domain docs.
