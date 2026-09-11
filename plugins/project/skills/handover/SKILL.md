---
name: handover
description: 🧭 Write a session handover to planning/HANDOVER.md, or resume from one. Use when the user says "handover [name]", "wrap up", "we're running out of context", "write the handover", or to resume: "resume [name]", "pick up where we left off", "read the handover". Retire a session before it degrades and start the next one from the file.
---

# Handover

Sessions degrade as they fill: earlier decisions get contradicted, rejected patterns come back. Write the handover before that point and start fresh from it.

## Write (default)

1. Resolve the project per `${CLAUDE_PLUGIN_ROOT}/shared/resolve-project.md`. Read `project.md`, `roadmap.yaml`, `PROGRESS.md`, the current `HANDOVER.md`, and `planning/briefs/`.
2. Write `planning/HANDOVER.md` from `${CLAUDE_PLUGIN_ROOT}/templates/HANDOVER.md`:
   - **Where we are**: one paragraph, current stage and its gate status.
   - **Shipped this session**: from today's `PROGRESS.md` rows and the chat.
   - **Loose ends**, priority order.
   - **Next 3 tasks**, concrete.
   - **Decided but not yet in the docs**: anything settled in chat that is not in `SCOPE.md`, `roadmap.yaml`, or a decision record. For each, also append a `Decision:` or `Finding:` row to `PROGRESS.md` now, so `roadmap` and the next session can see it without opening the handover.
   - **Gotchas**: environment quirks, things that bit us.
3. Overwrite the rolling `HANDOVER.md`. If the user wants a milestone copy, also write `HANDOFF-YYYY-MM-DD.md`.
4. Reply: the path and the three next tasks. Suggest a name for the next session, `<project>-<stage>-YYYY-MM`.

## Resume

Triggered by "resume", "pick up", "read the handover".

1. Resolve the project, then read `HANDOVER.md`, `project.md`, `ROADMAP.md`, and the last ten `PROGRESS.md` rows.
2. Reply in five lines at most: project, current stage and gate status, last thing shipped, top loose end, proposed first task.
3. If "Decided but not yet in the docs" is not empty, offer to write those into the docs first.
4. Ask what to prioritise. Do not start work until the user confirms.

## Rules

- Concise. The handover is read cold by a session with no memory of this one.
- Never run git.
- Follow the playbook named in `project.md` for tone.
