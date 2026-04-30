# Maintainers

Agents that keep the project healthy — extracting decisions before they're lost, updating documentation to match reality, surfacing deferred issues before they compound. These don't build new features or review code; they maintain the connective tissue that lets future agents (and humans) work effectively.

## Documentation

@kb-writer — writes and updates the project's knowledge base (resolve via `meridian context kb`). The KB is the project's persistent wiki — decisions, domain knowledge, architecture, synthesized research, retrospective learnings. Not a code mirror; a compounding knowledge base that agents and humans browse to orient. Run after work items ship, after research produces durable findings, or when conversation context contains decisions that will be lost to compaction. Mines transcripts for decisions that don't make it into code. Maintains `index.md` (page catalog) and domain docs with decision rationale. Base agent from meridian-base; spawn with `--from` for session context and pass `/session-mining` for transcript navigation, `@explorer` for bulk codebase reading.

@kb-maintainer — structural health of the knowledge base. Treats KB docs like code: enforces single responsibility (split oversized docs, merge fragments), creates folder structure as domains grow, fixes broken cross-references (`meridian kg check`), validates diagrams (`meridian mermaid check`), flags contradictions and stale content. Resolves merge conflicts when multiple writers collide, using `> [!FLAG]` inline markers for issues needing human review. Run after bursts of kb-writer activity, at work-item boundaries, or when KB navigability degrades. Base agent from meridian-base; spawn `@explorer` for comparing KB claims against current code.

@tech-writer — maintains user-facing documentation: getting started guides, CLI reference, API contracts, tutorials, and integration docs. Adapts to audience level from non-technical overview to developer reference. Run when features ship, APIs change, or users report confusion. Verifies docs against current code — stale user docs destroy trust.

## Issue Mining

@investigator — surfaces problems before they compound. Two modes:

- **Proactive backlog sweeps** — mines conversations, code, and spawn reports for deferred items, latent bugs, TODO debt, and unresolved questions at phase boundaries. These items are invisible in the code and lost from conversations after compaction — if nobody extracts them at phase boundaries, they accumulate silently.
- **Reactive investigation** — digs into root cause when a test fails unexpectedly, a @reviewer flags something ambiguous, or a spawn produces surprising results. Either quick-fixes the issue, files it for tracking, or closes it as a non-issue with reasoning.
