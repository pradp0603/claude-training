---
name: write-adr
description: Write an architecture decision record (ADR) in docs/adr/ for this repo, numbered and formatted like the existing ones. Use this whenever the user asks to write, add, draft or record an ADR or "decision record", wants to document why an architectural choice was made, or is making a significant architectural change (new dependency, new file or layer, storage change, test strategy change, tooling change) that CLAUDE.md says needs an ADR — even if they don't say "ADR". Also use it to supersede or update the status of an existing ADR.
---

# Write an ADR

ADRs in this repo live in `docs/adr/` and explain *why* a significant architectural choice was made, so that a future reader (often an attendee of the training session, or Claude in a later session) can understand the trade-off without digging through git history. Keep them short and concrete: one decision per record.

## Steps

1. **Look at what exists.** List `docs/adr/` and read at least the most recent ADR and any that touch the same area. This tells you the next number, the house style, and whether the new decision supersedes or conflicts with an earlier one.
2. **Pick the number and file name.** Next number = highest existing `ADR-NNN` + 1, zero-padded to three digits. File name is `ADR-NNN-short-kebab-title.md` (3–6 words, lowercase, describes the decision, e.g. `ADR-002-store-tasks-in-indexeddb.md`). Never reuse or renumber an existing ADR.
3. **Gather the facts.** The decision usually comes from the conversation or the change being made. If the reason for the decision isn't known, ask the user rather than inventing one — a made-up rationale is worse than none, because readers trust ADRs. Read the relevant code (`todos.js`, `app.js`, `package.json`, `tests/`) if it helps you describe consequences accurately.
4. **Write the file** using the template below.
5. **Handle superseded ADRs.** If the new decision replaces an earlier one (in whole or in part), say so in the new ADR's status line, e.g. `Proposed (supersedes ADR-MMM once accepted)` or `Accepted (supersedes ADR-MMM)`. Leave the old ADR untouched until the user explicitly confirms the supersession in this conversation — the old record still describes how the project works until then, and editing it early makes the docs claim a change nobody has agreed to. Once they confirm, change only the old ADR's status line to `Superseded by ADR-NNN` (or `Partly superseded by ADR-NNN`), set the new one to `Accepted`, and don't otherwise rewrite the old ADR — it's a historical record. In your reply, tell the user this confirmation step is pending.
6. **Report** the file path and a one-line summary of the decision. Don't commit unless asked.

## Template

Match ADR-001 exactly in structure — the bold status/date list, the three `##` sections, and nothing else:

```markdown
# ADR-NNN: Title in sentence case

- **Status:** Accepted
- **Date:** YYYY-MM-DD

## Context

What situation or force prompted the decision. Mention the constraints that matter here
(e.g. one-hour training session, attendees on Windows/macOS/Linux, no toolchain).

## Decision

What was decided, stated plainly in the present tense ("The app stores tasks in ...").
Bullets for the concrete specifics: which files, functions, commands.

## Consequences

- What becomes easier or possible.
- What becomes harder, what is given up, and any follow-up work it creates.
```

Guidance for each part:

- **Title**: names the decision, not the problem ("Store tasks in IndexedDB", not "Storage issues").
- **Status**: `Proposed` if the user is still deciding or asks for a draft/proposal; `Accepted` if the change is being made or already made. Other values: `Deprecated`, `Superseded by ADR-NNN`.
- **Date**: today's date in ISO format.
- **Context**: 2–5 sentences. Explain the pressure, not the solution.
- **Decision**: concrete enough that someone could check the code against it.
- **Consequences**: include the downsides honestly. An ADR with only upsides reads like marketing and isn't useful when someone later wonders whether to reverse it.
- Wrap prose lines at roughly 100–110 characters, like ADR-001.

## Respect existing decisions

ADR-001 says the app is plain HTML/CSS/JS with no build step, no framework and no npm dependencies. If the decision being recorded conflicts with it (for example adding a bundler or an npm package), point that out to the user at the start of your reply. Write the new ADR as `Proposed`, noting in its status that it supersedes ADR-001 (or the relevant part) once accepted, and have its Context say why the old constraint no longer holds. Only after the user confirms do you mark it `Accepted` and update ADR-001's status line (see step 5).
