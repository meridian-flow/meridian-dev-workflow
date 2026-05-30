---
name: prototype
type: mode-shift
description: Throwaway code that answers a question — logic prototypes or UI exploration on test routes.
model-invocable: true
user-invocable: true
argument-hint: "What question should the prototype answer?"
---

# Prototype

Build throwaway code to answer a specific question. Prototypes are
disposable — they exist to learn, not to ship.

## Two branches

### Logic prototype
"Does this approach work?" Build a minimal version that proves or disproves
the idea. Terminal output, no UI needed. Focus on the core mechanic.

### UI exploration
"What should this look like?" Build variations on test routes within the
current system. Real components, real data shapes, throwaway routes.

## Discipline

- State the question the prototype answers before writing code
- Build the minimum that answers the question — no polish, no error handling,
  no tests
- Isolate from production code — test routes, scratch files, temp directories
- Report what you learned, not what you built
- Delete or clearly mark prototype code when done — it must not drift into
  production

## Use something else when

- The question is answerable by reading code or docs → read
- The question is about design → design artifacts
- You already know the implementation → just implement
