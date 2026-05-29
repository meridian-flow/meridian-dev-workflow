---
name: tech-docs
type: reference
description: >
  Load when writing design documents — architecture specs, behavioral specs,
  component docs, feasibility reports. Covers structure and clarity for the
  artifacts under design/ in the work directory.
detail: Design document writing conventions — specs, component docs, feasibility.
model-invocable: false
---

# Tech Docs

Methodology for writing design documents — the artifacts under `design/` that
architects produce and coders build from. Use `/dev-artifacts` for file
placement, this skill for how to write them.

Load `/llm-writing` if it isn't already loaded.

## One Concept Per Document

One component, one interface, one interaction pattern, one decision per file.
When a doc starts covering two things, split it. Name files by what they
describe (`token-validation.md`), not when they were written
(`auth-redesign-notes.md`).

## Hierarchical Structure

Depth matches complexity. A simple topic gets a flat file. A complex subsystem
gets nested directories.

```
overview.md
<area>/
  overview.md
  <topic>.md
  <sub-area>/
    <topic>.md
```

The overview at each level orients the reader: what exists here, how pieces
relate, where to go deeper.

## Linked Web

Link to related docs using relative paths — components link to what they
interact with, interfaces they implement, tradeoffs that shaped them. Links
make dependency relationships explicit and navigable. Maintain them as you
would imports.

## Style

Write docs that work when read in isolation — self-contained, with enough
inline context that a reader doesn't need three other docs first. Be concrete:
file paths, function names, type signatures. State invariants explicitly.

Prefer mermaid diagrams for component relationships, data flows, state
machines. Tables for comparisons. Capture what code can't easily tell you:
dependency directions, data flows, and why the system is shaped this way.
Reference source locations rather than pasting code.

## Verification

After writing or restructuring linked docs, run `meridian kg check` for broken
links and `meridian mermaid check` for diagram validity.
