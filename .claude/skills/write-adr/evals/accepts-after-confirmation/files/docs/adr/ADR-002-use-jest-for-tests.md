# ADR-002: Use Jest for tests

- **Status:** Proposed (supersedes the test-runner part of ADR-001 once accepted)
- **Date:** 2026-11-25

## Context

The tests in `tests/` use Node's built-in runner (`node:test` with `node:assert`), as ADR-001 chose so that
`npm install` installs nothing. The team wants to move to Jest because more people already know it, which
makes the tests easier to read and extend. This conflicts with ADR-001's rule of no npm dependencies: Jest
is a dev dependency, so setup gains a real `npm install` step on attendees' Windows, macOS and Linux laptops.
The rest of ADR-001 (no framework, no bundler, no build step, plain `<script>` tags) is unaffected.

## Decision

Tests run on Jest, installed as a dev dependency. The app itself stays plain HTML, CSS and JavaScript.

- `jest` is added to `devDependencies` in `package.json`, with `package-lock.json` committed.
- The `test` script in `package.json` becomes `jest`, replacing `node --test tests/`.
- `tests/todos.test.js` and `tests/test.js` drop `require("node:test")` and use Jest's global `test`;
  assertions move from `node:assert` to Jest's `expect`.
- `todos.js` is unchanged: Jest loads it through the existing `module.exports` block (CommonJS), so no
  Babel or transform config is needed.
- The runtime still has no dependencies; Jest is used only to run tests in Node.

## Consequences

- Attendees and contributors can write tests in a familiar style, with Jest's matchers, watch mode and
  clearer failure output.
- `npm test` works the same way on every supported Node version, removing the Node 21+ breakage of
  `node --test tests/` noted in `CLAUDE.md`.
- Setup now depends on `npm install` succeeding. That adds download time and a network dependency at the
  start of a one-hour session, and corporate proxies or offline laptops can block it.
- `node_modules/` is already in `.gitignore`, but `README.md`, `CLAUDE.md` and ADR-001's Consequences
  ("nothing can fail to install") no longer describe the test setup accurately and need updating.
- Jest and its transitive dependencies must be kept up to date, and lab answers written against `node:test`
  must be rewritten.
- There is still no DOM test coverage; this change only swaps the runner.
