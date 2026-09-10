---
name: roadmap
description: 🧭 Show and update a project's roadmap. Use when the user says "roadmap <name>", "where are we", "what's next", "status", "update the roadmap", or "add idea: ..." / "feature request: ...". Reads PROGRESS.md and the verify baseline, reconciles planning/roadmap.yaml, renders ROADMAP.md (plus roadmap.html in full mode), and answers with what is next.
---

# Roadmap

`roadmap.yaml` is the single source. This skill keeps it honest and renders it. A status moves to green only with `evidence` naming its source and date. The map records verified state, never intent.

## 0. Which project

Argument or ask "Which project?". Read `project.md`, `roadmap.yaml`, `PROGRESS.md`. First line of every reply: `Project: <name>`.

## 1. Intake (only when asked)

`/project:roadmap <name> add <text>`, or a message like "add idea ..." or "feature request ...": append to `ideas` with today's date, `source` (the user, or the thread or person named), `status: parked`. Ideas never become tasks or stages here; `plan` promotes them. Reply `Parked: <text>` and stop, unless a status update was also asked for.

## 2. Reconcile

For each task or stage, gather evidence:

- `PROGRESS.md` rows since `updated:` that mention its id or refs.
- The baseline file named in `project.md` `repo.baseline`, if any, for executable gates and verify lines.
- Git log only on the code surface. Never run git from Cowork; there, `PROGRESS.md` is the record.

Propose status changes as one short list, for example `S2 red to yellow: 3 of 4 gate checks pass, PROGRESS 2026-09-12`. Rules:

- **Green needs `evidence`** with source and date. No evidence, no green; say what evidence would do it.
- **Yellow needs at least one `caveats` line.**
- **Never move backwards silently.** A green whose evidence no longer holds goes to yellow with a caveat and a note.

Ask for one confirmation of the whole batch, apply it, bump `version`, set `updated`.

## 3. Render

- `ROADMAP.md` from `${CLAUDE_PLUGIN_ROOT}/templates/ROADMAP.md`: Now (focus items), Next, Later, Done, Ideas, Invariants. Full mode adds the stages table.
- Full mode: `roadmap.html` from `${CLAUDE_PLUGIN_ROOT}/templates/roadmap.html`. First copy any existing file to `planning/roadmap-versions/roadmap-YYYY-MM-DD-v<N>.html`. Fill every `{{placeholder}}`; one stage box per stage, in `focus` order, with status class, chip text, gate, evidence or caveats, refs. Footer lists the sources used in step 2 and appends one changelog line for this version.
- Status is shown with an icon and a word as well as a colour, never colour alone.

## 4. Reply

Three lines at most:

```
Roadmap v<N>: <g> done, <y> with caveats, <r> focus, <i> parked ideas.
Next: <first red stage | first open task>: <gate | verify>. Refs: <...>.
```

Add a third line only when a status change is waiting on evidence the user has to produce.

## Rules

- Figures are copied from evidence sources, never computed here.
- Never run git from the control surface.
- Keep it short; the map is the detail.
- Tone and format follow the playbook named in `project.md`.
