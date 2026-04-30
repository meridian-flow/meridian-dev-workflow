# Artifact Ownership and Documentation Layers

Writer/reader rules for work-dir artifacts, and the placement rules for docs that live outside the work dir.

## Ownership

| Artifact | Writer | Readers |
|---|---|---|
| `requirements.md` | @product-manager | @architect-lead, @tech-lead, @planner |
| `design/` | @architect-lead | @planner, @tech-lead, @product-manager |
| `plan/` | @planner (spawned by @product-manager) | @tech-lead, @product-manager |
| `plan/preservation-hint.md` | @product-manager | @planner, @tech-lead |
| kb (`meridian context kb`) | @kb-writer, @kb-maintainer | all agents |

## Redesign And Rejections

- Redesign brief lives in @tech-lead terminal report.
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
