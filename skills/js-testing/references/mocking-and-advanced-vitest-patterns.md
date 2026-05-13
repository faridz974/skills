# Mocking Techniques in Vitest and Advanced Vitest Patterns

Use these as rules

## Mocking Techniques in Vitest

### 01.boundaries / boundaries

- Decide the test boundary before picking mocks.
- Mock dependencies outside the responsibility under test, not collaborators whose behavior is the point of the test.

### 02.functions / mock-functions

- Use `vi.fn()` for callbacks/listeners when the call itself is the observable behavior.
- Type mock functions when their call signature matters.

### 02.functions / spies

- Use `vi.spyOn()` when you need to observe or replace an existing object method.
- Restore spies after the test so the original implementation returns.

### 02.functions / mock-implementation

- Use `mockReturnValue`, `mockResolvedValue`, or `mockImplementation` to model the branch the test needs.
- Keep mocked behavior minimal and specific to the scenario.

### 03.date-and-time / date-time

- Use `vi.useFakeTimers()` with `vi.setSystemTime()` for code that reads the current date.
- Always return to real timers after the test group.

### 03.date-and-time / timers

- Use fake timers plus `vi.advanceTimersByTime()` or related APIs instead of sleeping.
- Advance mocked time only as far as the behavior contract requires.

### 03.date-and-time / ticks-and-tasks

- Include `queueMicrotask` in `vi.useFakeTimers({ toFake: [...] })` when microtasks are part of the behavior.
- Use `vi.runAllTicks()` to flush queued microtasks.

### 04.globals / global-methods

- Spy on global methods such as `console.log` when the global call is the boundary.
- Use reset/restore cleanup so global spies do not leak.

### 04.globals / global-values

- Use `vi.stubGlobal()` for mutable global values such as `location`.
- Call `vi.unstubAllGlobals()` in cleanup.

### 04.globals / environment-variables

- Use `vi.stubEnv()` to model environment-specific behavior.
- Call `vi.unstubAllEnvs()` in cleanup.

### 05.network / setup-msw

- Use MSW `setupServer` for Node-side request interception.
- Start the server before tests, reset handlers after each test, and close after all tests.
- Begin with an unhandled-request strategy that exposes unexpected network calls.

### 05.network / mock-responses

- Define explicit request handlers such as `http.post(url, resolver)`.
- Return Fetch `Response`/`HttpResponse` values to model the server response.

### 05.network / error-responses

- Override handlers per test with `server.use()`.
- Remember that `server.use()` prepends higher-priority handlers until reset.

### 05.network / network-errors

- Model transport failure separately from HTTP error status.
- Use `Response.error()` or equivalent MSW behavior for true network failures.

### 05.network / response-delay

- Use MSW `delay()` to model slow or hanging responses.
- Combine delayed responses with fake timers when testing request timeouts.

### 06.modules / dependency-injection

- Prefer dependency injection when the production design naturally accepts collaborators.
- Inject dependencies to avoid module mocking when the boundary can be explicit.

### 06.modules / private-side-effects

- Use `vi.mock()` for module dependencies that must be replaced before the subject imports.
- Use `vi.hoisted()` for mock values referenced by a hoisted `vi.mock()` factory.
- Use `vi.doMock()` only when unhoisted, per-test module mocking is required and imports are controlled.

## Advanced Vitest Patterns

### 01.setup / vscode-extension

- Use editor integrations to run focused Vitest tests quickly while developing.
- Keep CLI scripts as the source of truth for CI.

### 01.setup / multiple-workspaces

- Use Vitest projects for code that needs different environments, include patterns, setup files, or globals.
- Run focused workspaces with the project flag when debugging or splitting CI.

### 01.setup / code-coverage

- Add `@vitest/coverage-v8` for V8 coverage.
- Use coverage to find weak spots, not as a merge gate by itself.

### 02.context / custom-fixtures

- Use `test.extend<Fixtures>()` for reusable setup that belongs to the test context.
- Keep fixture APIs domain-oriented, such as `createMockCart`, rather than leaking setup mechanics.

### 02.context / automatic-fixtures

- Use automatic fixtures for setup that every test in a project needs.
- Use `onTestFinished` for robust cleanup and for preserving useful artifacts when a test fails.
- Create resource names from the test file and task id to avoid collisions.

### 03.assertions / custom-matchers

- Use `expect.extend()` for repeated domain assertions that deserve better failure messages.
- Add TypeScript matcher augmentation when using custom matchers in TS.

### 03.assertions / asymmetric-matchers

- Prefer asymmetric matchers for partial object/array matching and schema-like expectations.
- Expose custom asymmetric matchers through the same matcher definitions when supported.

### 03.assertions / custom-equality-testers

- Use `expect.addEqualityTesters()` when `.toEqual()` needs domain-specific equality.
- Return `undefined` from equality testers for values the tester does not own.

### 03.assertions / retryable-assertions

- Prefer `expect.poll()` for eventual non-DOM assertions.
- Prefer `expect.element()` in Vitest Browser Mode; it is built for retryable element assertions.
- Do not chain `.resolves` or `.rejects` after `expect.poll()`.

### 03.assertions / soft-assertions

- Use `expect.soft()` when one scenario should report multiple independent assertion failures.
- Avoid soft assertions when a later assertion depends on an earlier one passing.

### 04.performance / profiling-slow-tests

- Profile before changing test architecture for speed.
- Identify slow files/cases before adding concurrency, disabling isolation, or sharding.

### 04.performance / concurrency

- Use `test.concurrent` only for isolated test cases.
- Start with slow, self-contained unit tests and expand incrementally.
- Do not make tests concurrent if they share files, databases, mocks, globals, timers, or network state.

### 04.performance / test-isolation

- Keep Vitest isolation enabled by default.
- Disable isolation only for a measured subset through a workspace/project and only when tests are proven isolated by design.

### 04.performance / sharding

- Use Vitest `--shard=x/y` to split large suites across CI jobs.
- Use the blob reporter for sharded runs and merge reports after the jobs complete.
