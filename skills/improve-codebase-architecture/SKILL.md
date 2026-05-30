---
name: improve-codebase-architecture
type: reference
description: Structural improvement — shallow modules, fragmentation, deletion targets, deep-module opportunities, code-judo moves.
model-invocable: true
---

# Improve Codebase Architecture

The goal is making the codebase easier to change. Every boundary, file, and
abstraction is a cost — it should earn its place by hiding substantial
complexity behind a simple interface. When it doesn't, it's friction.

## What to Hunt

### Shallow Modules

A file with a high interface-to-implementation ratio: lots of exports, little
implementation behind them. The reader pays the interface cost but gets little
functionality in return.

Ask: does this module hide substantial complexity behind a simple interface?
If someone can understand the implementation by reading one small function,
the module boundary adds cost without benefit.

### Fragmented Concerns

Things that always change together but live in separate files. Check co-commit
history: if two files are consistently modified together, they are one concern
wearing two names. Unify them.

Too many small files scattered across directories means an agent must read
each one separately to understand the system. One well-structured module
beats eight 40-line fragments.

### Deletion Targets

- **Dead code**: unreachable functions, unused imports, commented-out blocks,
  orphaned files. Not "keeping options open" — it's entanglement surface.
- **Speculative structure**: abstractions built for futures that never arrived.
  If it has one caller and no evidence of a second arriving, inline it.
- **Stale duplication**: the same logic drifted apart in two places. Collapse
  to the canonical version.

### Inline Targets

- **Single-caller abstractions**: a function, class, or module used exactly once.
- **Pass-through wrappers**: functions that add zero logic beyond calling another.
- **Over-split helpers**: one-liners that name an operation already clear from
  the call site.

## Deep Module Opportunities

When you find 3+ shallow modules in the same directory that touch the same
concept, consider bundling them into one deep module. The new deep module
exposes a few well-named interfaces; the implementation consolidates what was
scattered.

A deep module:
- Hides substantial complexity (the implementation is non-trivial)
- Exposes a simple interface (few exports, clear contracts)
- Is testable at the interface (tests don't need to know internals)

## How to Report

For each finding:

- **What**: the specific file(s), class(es), or abstraction(s) — with paths
- **Why it matters**: the concrete cost — what change is harder because of this
  structure? How does it compound?
- **Recommended move**: one concrete action — "merge A into B", "delete C",
  "inline D into E", "bundle F, G, H into module I with interface J"
- **Leverage**: does this one move untangle dependencies across the codebase,
  or is it a local cleanup?

Prioritize by leverage. One deletion that removes three cross-module imports
matters more than ten cosmetic renames.

## Resources

The structural-health catalog under `/review` provides detailed smell families
and refactoring move patterns. Load `review/resources/structural-health/overview.md`
for the catalog when you need specific pattern guidance. The smell families
(bloaters, change-preventers, couplers, dispensables) and move families
(composing, moving, organizing, simplifying, dealing-with-generalization)
are available there.

## Boundary

You report structural friction and simplification moves. You do not execute
them — that's the coder's job. Your value is the structural lens: finding
what makes the next change harder than it needs to be and recommending the
simplest path to unfreeze it.
