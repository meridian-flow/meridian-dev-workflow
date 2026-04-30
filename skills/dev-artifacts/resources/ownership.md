# Artifact Ownership and Documentation Layers

Writer/reader rules for work-dir artifacts, and the placement rules for docs that live outside the work dir.

## Ownership

| Artifact | Writer | Readers |
|---|---|---|
| `requirements.md` | @dev-orchestrator | @design-orchestrator, @impl-orchestrator, @planner |
| `design/` | @design-orchestrator | @planner, @impl-orchestrator, @dev-orchestrator |
| `plan/` | @planner (spawned by @dev-orchestrator) | @impl-orchestrator, @dev-orchestrator |
| `plan/preservation-hint.md` | @dev-orchestrator | @planner, @impl-orchestrator |
| kb (`meridian context kb`) | @kb-writer, @kb-maintainer | all agents |

## Redesign And Rejections

- Redesign brief lives in @impl-orchestrator terminal report.
- Keep only approved state at `design/` and `plan/`.
- Replace rejected drafts atomically. Use git history for prior versions.

## Documentation Layers

- **kb** (`meridian context kb`) — persistent knowledge base. Decisions, domain knowledge, architecture, synthesized research.
- **`docs/`** — user-facing docs.
- **work dir** (`meridian work current`) — work-scoped artifacts and research.

Rules:

- Work-scoped research stays in the work directory during the work item.
- Durable findings are synthesized into KB via @kb-writer when work completes.
- See `/kb-conventions` for what belongs in KB vs work dir.
