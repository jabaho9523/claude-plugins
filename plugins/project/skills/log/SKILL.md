---
name: log
description: 🧭 Append a dated entry to a project's planning/PROGRESS.md. Use when the user says "log <name> ...", "log this", "note that we shipped X", "progress: ...", "decision: ...", or when a change lands in a project that uses this plugin. PROGRESS.md is the single tracker per project; never create another.
---

# Log

## Steps

1. **Which project.** Argument or ask. First line of the reply: `Project: <name>`.
2. **One row.** `| YYYY-MM-DD | what, one line | refs | by |`
   - `what`: from the argument or the user's last message. Decisions are prefixed `Decision:`.
   - `refs`: task or stage ids (T2, S1), issue or PR numbers, ADR ids. Always include the task or stage id when the entry belongs to one, so `roadmap` can pick it up.
   - `by`: `cowork`, `code`, or `user`.
3. **Append** to `<name>/planning/PROGRESS.md`. Reply with the row and nothing else.

## Rules

- Append only. Never rewrite or reorder earlier rows.
- If the entry claims a gate or verify passed, ask for the evidence source (test run, tie-out, file) and put it in the row. `roadmap` will not turn anything green without it.
- Never run git.
