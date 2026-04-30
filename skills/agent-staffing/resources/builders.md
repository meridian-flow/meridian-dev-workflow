# Builders

Agents that produce artifacts — code, design docs, research findings, and captured decisions.

## Coders

@coder — production code writer across the full stack: backend, frontend logic, CLI, infrastructure, data flow, build systems. Implements tasks from phase blueprints. One @coder per phase — multiple @coders on the same files create merge conflicts and duplicated work. If a phase feels too big for one @coder, splitting the plan is better than parallelizing coders. The exception is when phases touch cleanly disjoint file sets, determined at plan time — then @coders can run in parallel across phases.

@frontend-coder — production frontend code writer for visual/UI work where design fidelity and aesthetics are the primary concern. Same staffing rules as @coder. Pick this variant when the work is about what users see and feel — matching a design spec, implementing visual polish — not just because the file is frontend code. Pass mockups or screenshots when available.

@mockup-gen — fast throwaway mockups using the project's real components and design system. Not production code — exists for rapid visual iteration with the user. One per iteration round. Spawned by @ux-lead, not by tech-lead.

@imagegen — native image generation. UI concept mockups, visual explorations, icons, reference imagery. Usually spawned on explicit user request — image generation is expensive. Spawned by @ux-lead.

## Design Exploration

Scale design staffing to uncertainty, not implementation volume.

@architect — structural design, boundaries, interfaces, and tradeoff evaluation. One @architect is usually enough when the problem is constrained and the tradeoffs are obvious. Staff multiple @architects when there are materially different viable approaches, high-cost mistakes, or conflicting non-functional goals.

@web-researcher — reads the internet so design decisions reflect what the ecosystem already knows: library behavior in production, known failure modes, upstream issue trackers, real-world usage patterns. The single most-forgotten delegation in the design loop — agents default to guessing from training data instead of verifying what's actually out there, and designs built on unverified assumptions are expensive to unwind. Staff liberally whenever the design depends on how something behaves upstream. Research is high-throughput information gathering, not deep reasoning — use fast, cheap models and spawn multiple in parallel if needed.

@browser — general-purpose browser interaction via `playwright-cli`. Scrape CSS/HTML, extract design tokens, navigate web apps, take screenshots, fill forms, run interactive annotation sessions. The prompt defines the purpose — design research, data extraction, site analysis, whatever needs a live browser. Staff when the task requires interacting with a real website rather than reading documentation.

@explorer — the internal counterpart to @web-researcher. Reads the current codebase so design decisions reflect real code paths and integration points instead of assumptions. Pair the two when the design crosses the boundary between "what we have" and "what the world has." Cheap and high-throughput.
