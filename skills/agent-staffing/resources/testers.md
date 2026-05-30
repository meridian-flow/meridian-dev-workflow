# Testers

Different testers cover different failure modes. Coders self-verify with the narrowest useful evidence for the change — often focused checks or manual smoke, plus automated checks where they buy confidence. Beyond that, match testing to what the phase touches.

Testers are not limited to the @coder's described checks. Generate independent edge cases, failure modes, and boundary-condition probes based on the change itself.

When tier choice is unclear, improve the manual smoke instructions first: add
the scenario, command, expected result, and edge/failure cases an agent should
run against the real system. Add automated unit or integration coverage only
when the risk is hard to verify manually and protects a named contract cheaply.

## Default Lanes

@probe — default for any phase touching process boundaries, state initialization, CLI behavior, IPC, or fresh-state workflows. Unit tests and type checks verify internal correctness, but orchestration bugs — process lifecycle, file locking, IPC, fresh-state initialization — only surface when real processes run in real environments. These are exactly the bugs that ship to users because "all tests passed." Smoke-tester runs manual runtime checks in `$MERIDIAN_TASK_DIR` where the change lives — the work item's bound task dir, which may be the project root, a sibling worktree, or another sibling checkout. The caller owns workspace placement; the tester does not create or switch worktrees. Reserve temp repos or throwaway configs only for destructive probes or clean-baseline comparisons that would contaminate the workspace. Do not stop at the happy path — manually exercise realistic edge cases and failure paths as a default lane. **For integration phases** (code that talks to external binaries, APIs, or wire protocols), manual smoke verification is a mandatory behavioral lane. Pair it with other lanes when contract, structural, or design risks remain. Run against the real external system and test every supported target, not just one. Explicitly report which targets were tested manually and which were not.

@browser-probe — default for any phase touching frontend behavior. The frontend equivalent of @probe — verifies that UI changes actually work in a real browser, not just that component tests pass. Uses `playwright-cli` for browser interaction — snapshot-driven navigation, form filling, screenshotting, console/network inspection. Visual correctness, user flows, form interactions, console errors, and accessibility. When the honest test requires fresh state, a stub API, or a temp config, build it rather than testing against whatever happens to be running. Use `playwright-cli show --annotate` when the user should see and interact with the browser directly.

`@coder` with `/testing` `resources/unit-patterns.md` — for tricky logic, edge cases, and regression guards. Isolated, fast, exhaustive on boundary conditions. Not every phase needs new unit tests — but when there's complex pure logic that's hard to verify end-to-end, targeted unit tests pin down the behavior. Generate edge cases independently; don't only codify the requirements handed over by the caller.

`@coder` with `/testing` `resources/integration-patterns.md` — the middle tier between unit and manual. Tests that internal components compose correctly — module boundaries, coordination logic, contracts between collaborators — with fakes at external system boundaries. Use when a phase introduces new interfaces, protocols, or state machines where the composition matters more than the isolated logic. Use the unit-test lane for pure logic and `@probe` for real runtime behavior against live systems.

**Test retention during implementation:** Disposable probes and test harnesses
(scratch scripts, one-off verification fixtures) are removed after verification
passes. Durable unit and integration tests that protect key behavior or
interfaces stay — tech-lead judges which tests earn their place in the
permanent suite. @ audits the permanent test suite after implementation
converges and before shipping. Uncertain coverage becomes a better manual
smoke guide before it becomes a permanent automated test.

## Situational
