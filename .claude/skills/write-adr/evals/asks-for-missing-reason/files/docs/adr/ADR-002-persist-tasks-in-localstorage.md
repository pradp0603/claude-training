# ADR-002: Persist tasks in localStorage

- **Status:** Accepted
- **Date:** 2026-11-25

## Context

Tasks need to survive a page reload so attendees can see their changes stick while they work through the
labs. The app is used in a one-hour training session and is served as static files with
`python -m http.server 8000` (ADR-001), so there is no server-side code to store data. Adding a backend
would mean one more process for every attendee to install and run during the workshop.

## Decision

The app persists tasks in the browser's `localStorage`, with no backend.

- `app.js` stores the whole task array as JSON under the key `training-todos` (`STORAGE_KEY`).
- `render()` calls `save()` at the end of every render, so every change made through `update(next)`, and
  every filter change, writes the full list.
- On page load, `load()` parses the saved value. If it is missing, is not valid JSON, or is not an array,
  `load()` falls back to `sampleTasks()` from `todos.js`.
- `save()` and `load()` swallow storage errors (for example when `localStorage` is unavailable) instead of
  breaking the page.
- Persistence stays in `app.js`; `todos.js` remains pure and knows nothing about storage.

## Consequences

- There is nothing extra to run: the static server is enough, and the app works offline.
- A fresh or corrupted browser always shows the five sample tasks, so every attendee starts from the same
  state. The "Reset sample tasks" button gives the same result on demand.
- Tasks live in one browser on one machine. They don't sync across browsers or devices, and clearing site
  data deletes them.
- `load()` only checks that the saved value is an array; it doesn't validate each task's shape, so a
  hand-edited entry with missing fields is loaded as-is. A saved empty array is kept, not replaced by the
  sample tasks.
- Save failures are silent, so if storage is full or blocked the user won't know their changes weren't
  kept.
- Rewriting the whole list on every render is fine for a handful of tasks but would not scale to large
  lists.
- Persistence lives in the DOM layer, which has no automated tests; changes to it have to be checked by
  hand in the browser.
