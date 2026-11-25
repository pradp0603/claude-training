---
tags: [skill-creator]
max_turns: 20
timeout_seconds: 600
allowed_tools: [Read, Glob, Grep, Skill, Write, Edit, Bash]
---

can you write up an ADR for how we persist tasks? we save them to localStorage under the training-todos key on every render and fall back to the sample tasks if the data is missing or broken. we did it this way so there's no backend to run during the workshop
