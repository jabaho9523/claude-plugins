---
name: closeout
description: 🧭 End-of-session ritual for a project: log today's work, reconcile the roadmap, refresh the handover, and flag open loops. Use when the user says "closeout [name]", "close out", "end of day", "let's stop here", "wrap up for today". Runs the other skills in order so nothing decided in chat is left unrecorded.
---

# Closeout

## Steps

0. **Which project.** Argument or ask. If several projects were touched this session, run the steps for each in turn, naming which one you are on.
1. **Log.** Anything shipped, decided, or found today that is not yet in `PROGRESS.md` goes in as rows, following `${CLAUDE_PLUGIN_ROOT}/skills/log/SKILL.md`. Decisions prefixed `Decision:`, discoveries `Finding:`. If nothing shipped, write nothing and say so.
2. **Roadmap.** Steps 2 to 4 of `${CLAUDE_PLUGIN_ROOT}/skills/roadmap/SKILL.md`: reconcile, render, state what is next. A ref to a file that does not exist yet is intent, not evidence.
3. **Handover.** Write mode of `${CLAUDE_PLUGIN_ROOT}/skills/handover/SKILL.md`.
4. **Open loops.** One line each: uncommitted Code work, verify steps not yet run, briefs written but not yet executed, gates awaiting sign-off and from whom, questions with owners still open.

## Reply

Eight lines at most: files updated, current stage and gate status, what is next, the open loops.

## Rules

- Never invent progress. The log records what happened, not what was intended.
- Never run git.
- Follow the playbook named in `project.md` for tone.
