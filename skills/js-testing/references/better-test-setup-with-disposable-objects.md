# Better Test Setup with Disposable Objects

Use this when setup needs cleanup but `beforeEach` / `afterEach` hooks would hide the scenario.

## Rules

- Prefer flat tests: keep the setup that matters to a test inside that test.
- Use folders, file names, and test names to organize scenarios before adding nested `describe` blocks.
- Avoid clever shared setup in tests; tests should optimize for local readability.
- If a helper creates something that must be cleaned up, make cleanup part of the returned object's contract.
- Use disposable objects for resources whose lifecycle is tied to the test case.
- Use `using` for synchronous cleanup via `Symbol.dispose`.
- Use `await using` for asynchronous cleanup via `Symbol.asyncDispose`.
- If the setup helper itself is async, use `await using resource = await createResource()`.
- Keep cleanup idempotent where possible so failed setup or partial teardown does not cascade.
- Prefer disposable helpers for databases, servers, temp files, browser contexts/pages, event subscriptions, fake timers, mock workers, and seeded records.

## Pattern

```ts
test('uses a resource', async () => {
  await using resource = await createTestResource()

  await resource.arrangeScenario()

  await expect(runBehavior()).resolves.toEqual('expected result')
})
```

## Helper Shape

```ts
async function createTestResource() {
  const resource = await startResource()

  return {
    resource,
    async [Symbol.asyncDispose]() {
      await resource.stop()
    },
  }
}
```

## When To Prefer This Over Hooks

- The setup differs per test case.
- The reader needs to see prerequisites without chasing `beforeEach` closures.
- The cleanup belongs to the object being created, not to the entire file.
- Multiple resources are composed in one test and should clean up in reverse declaration order.

## When Hooks Are Still Fine

- The setup is truly global for every test in the file.
- The setup is test framework infrastructure, not scenario-specific state.
- The hook does not hide behavior-critical prerequisites.

## Review Checklist

- Can a reader understand the scenario from the test body alone?
- Does every created external resource have an owner responsible for cleanup?
- Would moving the setup into a shared hook make a future test accidentally inherit hidden state?
- Does cleanup run if the assertion fails?
- Is the helper named after the domain resource instead of the cleanup mechanism?
