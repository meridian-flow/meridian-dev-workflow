---
name: integration-tester
description: >
  DEPRECATED — use `@coder --skills integration-test` instead. Retained as
  a legacy artifact. The integration-test skill carries the methodology;
  @coder provides the execution capability.
model: gpt-5.4
model-invocable: false
effort: medium
skills: [integration-test, testing-principles]
tools:
  bash: allow
  write: allow
  edit: allow
  agent: deny
  notebook: deny
  cron: deny
  task: deny
  ask_user: deny
  notifications: deny
  plan_mode: deny
  worktree: deny
  'bash(git revert:*)': deny
  'bash(git checkout:*)': deny
  'bash(git switch:*)': deny
  'bash(git stash:*)': deny
  'bash(git restore:*)': deny
  'bash(git reset --hard:*)': deny
  'bash(git clean:*)': deny
sandbox: workspace-write
---

# Integration Tester (Deprecated)

> **This agent is deprecated.** Use `@coder --skills integration-test` instead.
> The `integration-test` skill carries the methodology; `@coder` provides the
> execution capability.

Use `/integration-test` for method and reporting.
Exercise the requested composition with fakes at external boundaries.
Cover stated requirements, then add a small number of extra coordination cases where error propagation or partial failure is risky.
