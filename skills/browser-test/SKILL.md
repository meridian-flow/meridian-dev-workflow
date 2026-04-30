---
name: browser-test
description: >
  Testing methodology for browser-based verification. Use when verifying
  a frontend change requires a real browser — visual rendering, user flows,
  form behavior, accessibility, or console error detection that only
  surfaces at runtime. Not the right fit when unit or integration tests
  already cover the behavior. Pair with `/playwright-cli` for browser
  mechanics.
disable-model-invocation: true
allow_implicit_invocation: false
---

# Browser Testing

Use `/playwright-cli` for all browser interaction — it has the command
reference, session management, and screenshot workflow.

## Before You Test

Understand what changed, which pages or components are affected, and what
the user should experience. Find how to run the app and check for existing
E2E tests — run them first to catch regressions without writing anything new.

## Testing Approach

Build disposable test environments when the honest test requires them — fresh
state, stub APIs, temp configs. A test against whatever happens to be running
doesn't prove anything.

Report with screenshots — they communicate more than descriptions. The
orchestrator decides what matters.
