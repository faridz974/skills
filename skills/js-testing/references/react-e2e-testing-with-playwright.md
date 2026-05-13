# React End-to-End Testing with Playwright

## 01.fundamentals / install-and-configure

- Use `defineConfig` and named browser projects.
- Keep Playwright config explicit about test directory, browser devices, retries, workers, and reporters.

## 01.fundamentals / running-the-app

- Configure `webServer` so Playwright owns starting the app for tests.
- Set `use.baseURL` so tests can navigate with relative URLs.
- Assert first visible page state after navigation.

## 01.fundamentals / custom-fixtures

- Extend Playwright `test` with domain helpers such as typed navigation.
- Export the extended `test` and `expect` from a shared test module.

## 02.authentication / basic

- Drive authentication through user-visible form controls.
- Assert the post-login user-facing state, not only cookies or internal session records.

## 02.authentication / 2fa

- Keep multi-step auth tests explicit: credentials, second-factor prompt, code submission, authenticated state.
- Query security forms by labels and roles.

## 02.authentication / passkeys

- Use browser-supported passkey/WebAuthn helpers or controlled mocks only at the platform boundary.
- Keep assertions on user-observable auth state.

## 02.authentication / protected-logic

- Test protected route behavior through navigation and redirects.
- Assert both denied and allowed states when auth logic controls access.

## 03.guides / mock-database

- Use isolated test databases or fixtures instead of sharing persistent state.
- Clean up created records automatically after the test.

## 03.guides / test-data

- Create only the data needed for the scenario.
- Use disposable data helpers with `await using` where available.
- Assert ordered lists and detail pages through accessible locators.

## 03.guides / recording-tests

- Use code generation/recording to discover interactions, then edit the output into intention-focused tests.
- Replace brittle selectors with roles, labels, and names before keeping the test.

## 03.guides / api-mocking

- Mock third-party APIs at the boundary when testing app-owned auth/callback logic.
- Use domain-specific custom matchers for responses, redirects, sessions, and toasts when they improve failure messages.

## 04.debugging / ui-mode

- Use Playwright UI mode to inspect test execution, selectors, and steps while authoring.
- Keep final tests deterministic after debugging.

## 04.debugging / trace-viewer

- Capture traces on first retry for CI debugging.
- Use trace viewer to inspect actions, screenshots, console output, and network behavior after failures.
