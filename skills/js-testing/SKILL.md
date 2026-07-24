---
name: js-testing
description: Write, review, debug, or refactor automated tests for code. Use when user needs to add test coverage, choose test boundaries, improve flaky or brittle tests, test React or browser UI behavior, decide between unit/integration/end-to-end/component tests, work with Vitest/Jest/Testing Library/Playwright/MSW mocks, clean up test state, or review whether tests assert user or domain intent instead of implementation details.
---

# Testing Code

## Core Workflow

1. Identify the intention behind the code before choosing assertions.
   - State the behavior the system exists to provide.
   - Make every important assertion fail if, and only if, that intention is not met.

2. Choose the test boundary deliberately.
   - Keep inside the boundary the code whose collaboration matters for the behavior.
   - Mock only dependencies that add irrelevant failure modes or belong to another responsibility.
   - Do not mock so much that the test no longer executes meaningful behavior.

3. Match the environment to the behavior.
   - Use Node tests for pure logic.
   - Use a real browser runner for browser APIs, layout, user interaction, accessibility, and component behavior.
   - Treat JSDOM-style environments as simulations; verify browser-sensitive behavior in a browser.

4. Structure tests as setup, action, assertion.
   - Keep essential setup visible in the test case.
   - Prefer flat tests over nested hooks when hooks hide prerequisites.
   - Use helpers to express domain setup, not to obscure the scenario.

5. Assert outcomes, not mechanics.
   - In UI tests, assert visible user-facing state rather than API calls.
   - Put request details in mock setup only when they define the scenario.
   - Prefer accessible queries and visibility assertions for UI presence.

6. Isolate and clean up state.
   - Avoid shared mutable mocks, timers, servers, DOM, files, database rows, and module state.
   - Use framework cleanup, mock clearing/resetting/restoring, and disposable resources as appropriate.
   - Validate that a test can fail for the intended reason before trusting a new green test.

## Decision Points

- If a test asserts a request happened, ask what user or domain outcome the request enables.
- If a negative assertion checks absence immediately after an action, wait for the relevant side-effect window or completion signal.
- If a test is flaky, isolate reproduction first, then classify the cause: async race, shared state, environment mismatch, external dependency, timing, or unclear assertion.
- If coverage is low, use it as a map to inspect risk. Do not write tests only to raise the percentage.

## Reference

Read [testing-principles.md](references/testing-principles.md) when the task involves nontrivial test design, flaky tests, mocks, browser/component tests, coverage policy, or review feedback that needs testing rationale.

Read [better-test-setup-with-disposable-objects.md](references/better-test-setup-with-disposable-objects.md) when tests need explicit per-test setup, reliable cleanup, flat setup without nested hooks, TypeScript `using` / `await using`, `Symbol.dispose`, `Symbol.asyncDispose`, or resource helpers for servers, databases, files, browser contexts, timers, subscriptions, or mock infrastructure.

Read [react-component-testing-with-vitest.md](references/react-component-testing-with-vitest.md) when working with React component tests, Vitest Browser Mode, `vitest-browser-react`, browser locators, MSW in component tests, or JSDOM migration.

Read [mocking-and-advanced-vitest-patterns.md](references/mocking-and-advanced-vitest-patterns.md) when working with Vitest mocks, timers, globals, environment variables, MSW in Node tests, module mocking, fixtures, custom matchers, retryable assertions, coverage, concurrency, isolation, or sharding.

Read [react-e2e-testing-with-playwright.md](references/react-e2e-testing-with-playwright.md) when working with React end-to-end tests, Playwright config, web servers, authentication flows, E2E fixtures, test data, API mocking, UI mode, or trace viewer.
