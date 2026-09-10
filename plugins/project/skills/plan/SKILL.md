---
name: plan
description: 🧭 Turn an agreed SCOPE.md into planning/roadmap.yaml: an ordered task list in lite mode, stages with checkable gates in full mode. Use when the user says "plan <name>", "let's plan", "break this down", "what are the stages", or when scope is agreed and no roadmap exists yet. Re-run to re-plan after a gate fails or to promote parked ideas.
---

# Plan

Gates are written here, before any stage starts, by the planner rather than by whoever implements. That separation is what makes a gate a control instead of a formality.

## 0. Which project

Argument (`/project:plan coloring-site`) or ask "Which project?". Read `<name>/planning/project.md`, `SCOPE.md`, `roadmap.yaml`. If `SCOPE.md` is not `status: agreed`, stop and point to `/project:scope <name>`. Start every reply with `Project: <name>`.

## 1. Lite mode

From Done-when and the scope, propose 3 to 10 tasks in order. Each task: `name`, `verify` (one line: how we know it is done), `refs`. Tag guesses `[assumed]`. Ask at most 4 questions, one round. Write `tasks` in `roadmap.yaml`, set `focus` to the first task id. Go to step 3.

## 2. Full mode

Start from the Candidate stages in `SCOPE.md` and finalise:

- **3 to 7 stages**, each a bounded context: one part of the domain, named in glossary terms, deliverable on its own.
- **Per stage:** `gate` (the checkable exit criterion), `gate_kind` (`executable` or `judgment`), `refs`. A judgment gate also gets a `rubric` of 3 lines that a fresh reader could score.
- **`invariants`:** what must never break across stages, from Constraints and Done-when.
- **`focus`:** stage order.
- **Repo:** if the work touches code and `repo.present` is false in `project.md`, ask for the path and the verify command and write them in. This is the only setup-type question plan asks.
- **Ideas:** offer each parked idea once: promote into a stage (`status: promoted`, `note: S3`) or leave parked.

Propose first, then ask at most 8 questions in one round. Unanswered items become `[assumed]`.

Write `stages` in `roadmap.yaml`, every stage `status: red` unless it is already done and has evidence. Set `gates.current_stage` in `project.md` to the first stage.

## 3. Sanity checks

- Every task or stage has a `verify` or `gate`. One without either is rewritten or dropped.
- Full: the stages fit the appetite. If not, say so and offer to cut stages or grow the appetite.
- Nothing from Non-goals has crept in.

## 4. Write and reply

1. `roadmap.yaml`: `updated` today, `version` plus one.
2. Render: follow step 3 of `${CLAUDE_PLUGIN_ROOT}/skills/roadmap/SKILL.md` to write `ROADMAP.md` (and `roadmap.html` in full mode).
3. Append to `PROGRESS.md`: "Plan agreed, roadmap v<N>."
4. Reply in two lines:

   ```
   Roadmap written: <name>/planning/ROADMAP.md (<N> tasks | <N> stages, first gate: <gate>).
   Next: start <T1|S1>. Log each landed change with /project:log <name>; check status with /project:roadmap <name>.
   ```

## Rules

- Propose, then ask. Reacting is faster than answering open questions.
- Never run git. Plan writes only into `planning/` and `project.md`.
- Numbers are not computed here; write where they will be verified.
- Tone and format follow the playbook named in `project.md`.
