# Testing Principles Reference

## Intention First

- Tests validate the intention behind the system, not the mere implementation.
- A useful test fails when the intended behavior breaks and stays green across refactors that preserve the behavior.
- Before writing assertions, answer: "When should this fail?"

## Anatomy of a Test

- Setup: create the state and dependencies needed for the scenario.
- Action: perform the behavior under test.
- Assertion: verify the resulting state, value, output, or visible user outcome.
- Keep this structure readable even when the test framework does not enforce it.

## Golden Rule of Assertions

- A test must fail if, and only if, the intention behind the system is not met.
- Assertions against implementation details usually fail for the wrong reason.
- If an assertion would fail after a behavior-preserving refactor, reconsider it.

## Boundaries and Mocking

- A test boundary is the extent of the real system executed by the test.
- Mocking is a boundary-setting tool.
- Mock dependencies that are outside the responsibility being tested or create irrelevant failure modes.
- Avoid over-mocking; if every meaningful collaborator is fake, the test may only validate the mock setup.
- For networked UI tests, mock the network in setup but assert the UI outcome.

## UI Assertions

- Use user-facing assertions for user-facing behavior.
- Prefer visibility/accessibility-oriented assertions for expected UI presence.
- Use DOM-presence assertions for absence or low-level DOM-specific checks.
- In Vitest Browser Mode, prefer retryable element assertions for asynchronous UI changes.

## Browser Environment

- Use real browser tests for browser APIs, layout-sensitive behavior, accessibility interactions, and component behavior.
- Keep pure logic in fast Node tests.
- Treat JSDOM and similar environments as simulations that can hide browser differences.

## Test Setup and Cleanup

- Prefer flat tests over deeply nested `describe`, `beforeEach`, and `afterEach` when nesting hides prerequisites.
- Make scenario setup explicit in the test case.
- Use helpers to name domain concepts, not to bury important state.
- Use disposable objects for resources with cleanup contracts:
  - `using` for synchronous disposal.
  - `await using` for asynchronous disposal.
- For mocks:
  - Clear mocks to remove call history while keeping implementation.
  - Reset mocks to remove call history and custom implementation.
  - Restore mocks to return spies/replaced functions to original behavior.

## Negative and Inverse Assertions

- Immediate negative assertions can pass before a delayed side effect happens.
- For async absence checks, identify the period or signal after which the unwanted behavior would have occurred.
- Use polling/waiting utilities around the inverse condition instead of fixed sleeps.

## Flaky Tests

Use the S.M.A.R.T. framing:

- Skip: disable only as a visible temporary risk decision.
- Mitigate: reduce blast radius while root cause work continues.
- Assess: reproduce in the same environment and classify likely causes.
- Rewrite: replace brittle implementation or timing assertions with intention-focused checks.
- Throw away: delete tests whose signal no longer justifies maintenance.

Common causes:

- Shared mutable state.
- Async races.
- Environment mismatch between local, CI, Node, and browser.
- Real external dependencies inside tests that do not own those dependencies.
- Overly broad assertions or implementation-detail assertions.

## Code Coverage

- Coverage reports show which code ran, not whether behavior was validated.
- Use coverage to find blind spots and risk areas.
- Do not treat a percentage as proof of confidence.
- Avoid writing tests only to execute lines.

## Vitest Defaults to Preserve

- Prefer explicit imports from `vitest` unless the project intentionally enables globals.
- Reuse Vite configuration for app-like module transformation when applicable.
- Keep test files isolated by default.
- Let parallel test files improve speed while avoiding shared state.
- Use concurrent cases only when each case is self-contained.
