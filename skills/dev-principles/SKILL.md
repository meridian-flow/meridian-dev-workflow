---
name: dev-principles
type: principle
description: Core engineering values for all code decisions — simplicity, separation of concerns, structural judgment.
model-invocable: true
---

# Core Beliefs

1. **Code is cheap; bad code is expensive.** Writing code costs almost nothing now. The real cost is code that's hard to change — inconsistent, bloated, entangled. Optimize for the cost that's real.
2. **Consistency beats cleverness.** Consistent patterns lower the reasoning cost for every human and agent that follows. Match what's there before inventing something new.
3. **Code is fluid.** Optimize for change: clear seams, good boundaries, minimal coupling. The next change should be easy to make.

These shape every principle below.

# Get It Right the First Time

The default agent failure is producing something plausible and moving on — half-finished, approximate, "works for now." The cost of wrong isn't one more generation; it's the mess every later agent inherits. Read the code before changing it. Follow the existing pattern before inventing one. Handle the edge cases now. Investigate when something's unclear instead of guessing. One correct pass costs less than three rounds of fixing a sloppy one.

# Simplicity

Good software is easy to change. Every boundary, type, and layer is a cost that must earn its place by making future changes smaller and safer. The default failure mode is over-engineering.

Before adding structure, ask: is this a separate concern, or one thing wearing two names? Things that always change together are one thing. Independence justifies a split — partitioning alone doesn't. Justify the split, not the merge.

Treat code growth as a cost: meaningful LOC growth should buy clearer behavior, a real boundary, or reasoning cost removed elsewhere.

**Deep modules over shallow.** A deep module hides substantial complexity behind a simple interface. A shallow one has a complex interface hiding little — many exports, thin implementation. If an exported function wraps three lines, keep it in its caller. When 3+ shallow modules in a directory touch the same concept, bundle them into one deep module with a few well-named exports.

# Separation of Concerns

Group by concern; draw boundaries where things change independently. Smaller focused files also cost less to read — an agent consumes the whole file, so a 500-line module with one relevant function wastes attention on the other 480.

When you see duplication across boundaries, suspect the boundaries before patching with extraction: is this really two modules, or one split for the wrong reason? Rethink the structure rather than bridging it.

# Make Changes Easy

Refactoring is disentangling. Ask: what would you need to understand to make the next change here? Reduce that. Refactor early, while context is fresh — a refactor that grows the codebase is suspect: disentangling, or adding ceremony?

Before extracting, ask if it's real shared behavior or surface similarity — two cases alike by coincidence, three reveal the pattern. When an abstraction grows flags and branches, it captured the wrong concept: inline and re-form rather than patch.

# Deletion and Structural Cleanup

LLMs default to preserving code. Fight that.

- **Dead code: delete it** in the same change — unused functions, unreachable branches, commented-out blocks, stale imports, orphaned files. It's entanglement surface, not "keeping options open."
- **Obvious duplication: collapse it.** Every copy drifts independently; "clean up later" never comes.
- **Structural problems are immediate debt.** Circular dependencies, god modules, leaky abstractions, misplaced responsibility — fix on sight. Rot compounds at agent speed, in hours not months.
- **Escalate deep rot.** When a fix risks breaking unrelated behavior or needs rethinking module boundaries, name the problem and propose a path rather than silently working around it.

# Testing

Verify by running the program and the project's checks. Automated tests earn their place by protecting a durable boundary, contract, or risk that's hard to verify by running the system — default to restraint, not coverage targets. See `/testing` for tier judgment and when automated tests are justified. In reports, keep manual runtime evidence separate from automated checks.

# Consistency

Read surrounding code first. Does the project already solve this? Prefer its patterns over introducing new ones. A good dependency deletes more code than it adds.
