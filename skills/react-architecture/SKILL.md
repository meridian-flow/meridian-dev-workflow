---
name: react-architecture
type: reference
description: >
  Load when building or reviewing React frontend code for maintainability
  and visual consistency. React-specific structural lens — token discipline,
  state architecture, component composition, import boundaries, component
  API consistency. Complements dev-principles with what generic structural
  skills don't cover about React codebases.
detail: React component patterns, layout composition, and visual consistency.
model-invocable: false
---

# React Architecture

How to structure React code for maintainability and consistent feel. Generic
structural principles (deep modules, separation of concerns, deletion) live in
`/dev-principles` and `/simplify` — this skill applies those principles to
React code and adds what only frontend codebases need.

## Token Discipline

The design system's tokens are the source of visual consistency. Every ad hoc
value is a drift vector.

**Hunt for:**
- Raw hex/rgb colors instead of semantic token references
- Hardcoded spacing values (px, rem) instead of spacing scale tokens
- One-off font sizes, weights, or line heights outside the type scale
- Inline border-radius, shadow, or z-index values that should be tokens
- Component-level color/spacing overrides that fight the theme

**What good looks like:**
- Semantic CSS variables define the visual system (`--background`,
  `--foreground`, `--primary`, not `#1a1a2e`)
- Components consume tokens; tokens define the feel
- One source of truth for spacing, type, radius, shadow, color, breakpoints
- Runtime theming (dark mode, density, brand) works through variable swaps,
  not class rewrites

## State Architecture

Server state and client state are different concerns with different lifecycles.
Mixing them creates state that's hard to reason about and hard to refactor.

**Hunt for:**
- Server/remote data copied into Zustand or global stores — this duplicates
  cache, invalidation, and sync logic that TanStack Query already handles
- Context used as a global store — context is for low-velocity cross-tree data
  (theme, current account, routing metadata), not interactive state
- State lifted higher than necessary — data flows should be explicit and
  local; lifting "just in case" creates invisible dependencies
- Duplicated or contradictory state — two sources of truth for the same value
- Missing URL state — filters, tabs, pagination, and selected entities that
  should survive reloads and be shareable

**What good looks like:**
- Local component state for form fields, toggles, transient UI
- Context for stable app-wide metadata
- TanStack Query (or equivalent) for remote/server state
- Zustand for client-global interactive state that genuinely needs it
- URL state for navigation-meaningful selections

## Component Composition

React components scale through composition, not through growing prop lists.

**Hunt for:**
- Prop drilling through 3+ levels where composition (children/slots) or
  context would be cleaner
- Components with 10+ props — usually a sign the component does too many
  things or its API isn't compositional
- Nested component definitions inside render — creates new component
  identities on every render, destroys state, breaks memoization
- Premature extraction — a "reusable" component with one caller and no
  evidence a second is coming
- Monolith components that mix orchestration, rendering, and data fetching
  in one file

**What good looks like:**
- Compound component pattern for related UI pieces (`Tabs` / `TabsList` /
  `TabsTrigger` / `TabsContent`)
- Children and slots over deep prop threading
- Each component does one thing: render, interact, or orchestrate
- Extract when the third use case appears, not the first

## Import Boundaries

How modules import each other determines how hard the codebase is to change.

**Hunt for:**
- Cross-feature imports — `features/billing/` importing from
  `features/auth/` internals
- Barrel file sprawl in app code — `index.ts` re-exports that create
  circular dependencies, slow dev startup, and hide import costs
- No clear boundary between shared primitives and feature-specific code —
  everything dumped in `components/`
- Import chains that force reading 4+ files to understand one component

**What good looks like:**
- Unidirectional: `shared/` → `features/` → `app/`
- Features don't import from other features' internals
- Direct imports in app code; barrel files only for intentional public API
  surfaces (package entrypoints, small library boundaries)
- Feature internals (hooks, stores, components, styles) colocated with the
  feature they serve

## Component API Consistency

When component APIs follow different patterns, every new component is a
learning curve. Consistency lets agents and humans reuse patterns instead of
reinventing them.

**Hunt for:**
- Inconsistent variant prop naming — `size="sm"` vs `size="small"` vs
  `isSmall={true}` across similar components
- Mixed composition patterns — some components use children, others use
  render props, others use slot props for the same kind of content
- Inconsistent naming conventions — `UserCard` vs `cardUser` vs
  `user-card` across the same codebase
- Missing or inconsistent TypeScript prop types

**What good looks like:**
- Semantic variant props with consistent naming (`size`, `variant`)
- Consistent slot/composition structure across component families
- Stable naming convention across all primitives
- Component docs (Storybook autodocs) as living documentation that agents
  and humans use to discover and reuse patterns

## How to Apply

1. **Orient first.** Read the project's design tokens, component library,
   and existing patterns. Build toward existing good patterns, not new ones.
2. **Prioritize by leverage.** A token violation in a shared primitive
   affects every page; one in a feature-specific component affects one view.
   High-leverage issues first.
3. **Report or fix, depending on your role.** If reviewing, report with
   file paths and severity. If implementing, apply these patterns as you
   build — don't leave cleanup TODOs.
