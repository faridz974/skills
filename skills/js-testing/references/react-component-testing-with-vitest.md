# React Component Testing with Vitest

## 01.sunsetting-jsdom / break-jsdom

- Prefer a real browser when component behavior depends on browser events, layout, accessibility, or platform APIs.
- Treat JSDOM failures and workarounds as a signal that the test environment is not representative enough.

## 02.vitest-browser-mode / installation-and-setup

- Add `@vitest/browser`, Playwright, and framework bindings such as `vitest-browser-react` for browser component tests.
- Configure `test.browser.enabled`, `provider: "playwright"`, and a concrete browser instance such as Chromium.
- Add browser matcher/provider types to the test tsconfig used by browser tests.

## 02.vitest-browser-mode / migrate-the-test

- Replace React Testing Library rendering with `render` from `vitest-browser-react`.
- Replace `screen` queries with `page` locators from `@vitest/browser/context`.
- Await `expect.element(locator)` for retryable browser assertions.

## 02.vitest-browser-mode / playwright

- Use Playwright as the Vitest Browser Mode provider when the component test needs real browser execution.
- Keep component tests in Vitest when the tested scope is a component, even though Playwright provides the browser.

## 02.vitest-browser-mode / shared-assets

- Put browser setup that must run for all component tests in a browser setup file.
- Keep shared mocks, styles, and test bootstrapping close to the browser project config.

## 02.vitest-browser-mode / multiple-workspaces

- Use Vitest projects/workspaces to separate Node unit tests from browser component tests.
- Give each workspace a name, include/exclude pattern, environment, and browser config where relevant.
- Keep `.browser.test.tsx` files in the browser workspace and ordinary `.test.ts` files in the unit workspace.

## 03.best-practices / queries

- Query like a user: prefer roles, labels, names, and visible text over test IDs or DOM structure.
- Scope queries when repeated elements exist.

## 03.best-practices / user-events

- Interact with the UI through locators: fill inputs, click buttons, and assert the visible outcome.
- Do not call component props or internal handlers directly when testing user behavior.

## 03.best-practices / network-mocking

- Use MSW in browser component tests to keep the network boundary realistic.
- Wrap the MSW browser worker in a Vitest fixture so each test can override handlers and cleanup consistently.
- Set `onUnhandledRequest: "error"` to catch missing mocks.

## 03.best-practices / element-presence

- Use `toBeVisible()` for expected user-visible elements.
- Use `.not.toBeInTheDocument()` for removal/absence when the element should leave the DOM.
- Keep the locator before the removal action when asserting that the same element disappears.

## 03.best-practices / page-navigation

- Assert navigation through URL, route-visible headings, or user-facing destination state.
- Prefer behavior-level navigation checks over implementation details of router calls.

## 04.debugging / dom-snapshots

- Use DOM snapshots to understand what the browser can query when a locator fails.
- Let the accessible DOM guide better roles, names, and labels.

## 04.debugging / debugger

- Debug browser component tests in the browser when the failure depends on rendered state.
- Keep the failing component visible and inspect the actual DOM rather than inferring from test code.

## 04.debugging / breakpoints

- Use browser debugging and targeted locators to resolve ambiguous elements.
- When a role matches many elements, scope or refine by accessible name before asserting.

## 05.extras / vitest-4.0

- Check Vitest major-version migration notes before updating browser mode, workspace, or assertion APIs.
- Prefer config that follows current Vitest names and provider type packages.
