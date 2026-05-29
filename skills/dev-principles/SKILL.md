---
name: dev-principles
type: principle
description: >
  Load when implementing, reviewing, refactoring, or designing. Core
  engineering values — simplicity, separation of concerns, structural
  judgment. The operating lens for all code decisions.
detail: Core engineering values — simplicity, separation of concerns, structural judgment.
model-invocable: true
---

# Simplicity

Good software is software that is easy to change. Every boundary, type, and
layer is a cost — it should earn its place by making future changes smaller
and safer.

The default failure mode is over-engineering, not under-engineering. Every
boundary, type, and layer is a cost.

Before adding structure, ask: is this actually a separate concern, or one
thing wearing two names? If these pieces always change together, they are
one thing. Partitioning does not make code simpler — independence does.

Justify the split, not the merge.

## Deep Modules Over Shallow Modules

A deep module hides substantial complexity behind a simple interface. The
caller gets a lot of functionality for little interface cost.

A shallow module has a complex interface but hides little — many exports,
small implementation. If a module has one exported function that wraps three
lines, it's shallow — keep that function in the file that calls it.

Prefer one deep module over three shallow ones. If a module has one exported
function that wraps three lines, it's shallow — keep that function in the
file that calls it. When 3+ shallow modules in the same directory touch the
same concept, bundle them into one deep module with a few well-named exports.

# Separation of Concerns

Group by concern. Draw boundaries where things actually change
independently. Smaller, focused files also cost less to read — LLM agents
consume the whole file, so a 500-line module with one relevant function
wastes attention on the other 480 lines.

When you see duplication across boundaries: are the boundaries wrong? Is
this really two modules, or one module that got split for the wrong reason?
Step back and rethink the structure — don't patch it with extraction.

# Make Changes Easy

Refactoring is disentangling. The question is: what would you need to
understand to make the next change here? Reduce that.

Refactor early while context is fresh. A refactor that grows the codebase
is suspect — are you disentangling, or adding ceremony?

# Abstraction Judgment

Before extracting: is this real shared behavior, or surface similarity?
Two cases look alike by coincidence. Three reveal the pattern.

When an abstraction grows flags and branches: did it capture the wrong
concept? Inline and re-form rather than patch.

# Deletion and Structural Cleanup

LLMs default to preserving code. Fight that instinct.

**Dead code: delete it.** Unused functions, unreachable branches, commented-out
blocks, stale imports, orphaned files. Dead code is not "keeping options open"
— it's entanglement surface that misleads readers and accumulates coupling.
If it's dead, remove it in the same change.

**Obvious duplication: collapse it.** When the same logic appears in multiple
places, unify it. Don't leave "will clean up later" TODOs — later never
comes, and every copy drifts independently.

**Structural problems are immediate tech debt.** Circular dependencies,
god modules, leaky abstractions, misplaced responsibilities — fix them when
you find them, not in a future cleanup pass. Structural rot compounds:
every change built on a broken foundation makes the next fix harder.
Tech debt left to fester at agent speed compounds in hours, not months.

**Escalate deep rot.** When structural problems are large enough that fixing
them risks breaking unrelated behavior or requires rethinking module
boundaries, escalate to the user rather than silently working around it.
Name the problem, explain the risk, and propose a path.

# Testing

Verify changes by running the program and the project's existing checks.

Add tests when they protect a durable boundary, contract, or risk that is hard
to verify manually. Prefer manual smoke verification and focused integration
tests when they answer the question with lower maintenance cost. Unit tests fit
best where narrow logic, parsing edges, or similar cases give stronger signal
than higher-tier checks.

When tier choice is unclear, improve the manual smoke instructions first:
capture the scenario, command, expected result, and edge/failure cases an agent
should run against the real system.

In final reports, separate manual smoke checks from automated checks/tests.
Pytest, unit tests, integration tests, lint, and type checks are automated
verification; they are not smoke testing.

Test at module interfaces. Interface-focused tests make simplification safe:
refactor freely, keep behavior stable, and let tests confirm the contract
still holds.

Treat code growth as a cost. Notice meaningful LOC growth and make sure it
earns its place through clearer behavior, a real boundary, or a
simplification that removes reasoning cost elsewhere. When the diff grows a
lot, re-examine it for unnecessary files, shallow wrappers, duplicated logic,
or boundaries that did not earn their cost.

When a test-suite audit is needed, qa-lead reshapes the permanent suite
toward high-leverage boundary coverage and removes tests that do not protect
real behavior.

# Consistency

Read surrounding code first. Does the project already solve this? Prefer
its patterns over introducing new ones. A good dependency deletes more code
than it adds.
