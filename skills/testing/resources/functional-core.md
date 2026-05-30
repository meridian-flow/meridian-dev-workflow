# Functional Core, Imperative Shell

Gary Bernhardt's pattern: decisions live in pure functions, I/O lives in a thin shell that wraps them.

## The Shape

```
[ Imperative Shell ]  — reads input, writes output, performs I/O
        │
        ▼
[ Functional Core ]   — pure functions: inputs → decisions → outputs
        │
        ▼
[ Imperative Shell ]  — applies the decisions
```

The core has no side effects. The shell has no logic to speak of — it orchestrates.

## Why It Matters for Testing

- **The core is trivially testable.** Pure functions take inputs and return outputs. No fixtures, no setup, no teardown, no mocks. You can run thousands of cases per second.
- **The shell is barely worth unit testing.** It has no branches, no decisions. You test it at a higher tier (integration or smoke) where real I/O is the point.
- **Mocks disappear.** When decisions are pure and I/O is isolated, there is nothing left to mock except the I/O boundary itself.

## How to Recognize the Pattern

Functional core:
- takes data, returns data
- no network, no disk, no clock, no randomness (or injected)
- same inputs always produce the same outputs
- can run in a tight loop without consequences

Imperative shell:
- reads from the world (stdin, files, HTTP, DB)
- writes to the world (stdout, files, HTTP, DB)
- holds the decisions the core produced and applies them
- thin — mostly just plumbing

## When It Breaks Down

Not every problem factors this cleanly. Streaming systems, highly concurrent systems, and systems where timing is the behavior resist strict separation. Approximate the pattern where possible; reach for integration or smoke tests where it isn't.

## What This Changes About Testing

If your unit tests require heavy mocking, the code is probably mixing decisions with I/O. The fix is usually refactoring the code, not writing more elaborate mocks. Push the decision into a pure function. Test that. The shell becomes uninteresting, which is the goal.
