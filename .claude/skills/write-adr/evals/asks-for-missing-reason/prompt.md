---
tags: [skill-creator]
max_turns: 20
timeout_seconds: 600
allowed_tools: [Read, Glob, Grep, Skill, Write, Edit, Bash]
---

write an ADR: we're moving the current filter (all/active/done) into the URL hash, like #active, instead of only keeping it in memory in app.js

Input files: docs/adr/ADR-002-persist-tasks-in-localstorage.md
