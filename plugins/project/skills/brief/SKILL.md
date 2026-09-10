---
name: brief
description: 🧭 Write the next code brief for a project: a self-contained work order Claude Code can implement without re-reading the codebase or the chat, ending in a paste-ready prompt. Use when the user says "brief [name]", "write the brief", "next brief", "work order for S2", or when a stage is in focus and no brief covers its next change. Full mode only.
---

# Brief

One brief is one shippable change toward the current stage's gate. If it needs two acceptance lists, it is two briefs.

## 0. Read

Which project (argument or ask). Then `project.md` (repo, verify command, main branch), `SCOPE.md` (glossary, constraints, non-goals), `roadmap.yaml` (current stage, its gate, invariants), `PROGRESS.md` (what has landed, review findings), and `planning/briefs/` (next number, what is already covered). Lite project: stop and say briefs are for full mode; describe the task inline instead.

## 1. Pick the change

Name the change that moves the current stage closest to its gate. If the stage needs several, list them in order, one line each, and write the first unless the user picks another. If this brief follows a review verdict of rework or follow-ups, say what changed from the previous brief.

## 2. Write

`planning/briefs/YYYY-MM-DD-NN-short-slug.md` from `${CLAUDE_PLUGIN_ROOT}/templates/brief.md`. Date is the target review date, NN the order within that batch. All nine sections:

1. **Goal**: one sentence, in the user's terms, using glossary words.
2. **Context**: why this, now, from the roadmap. Enough to act, no more.
3. **Where**: files and functions involved. Lines are hints; the structure is the contract, so tell Code to re-check before editing.
4. **Spec**: logic and data separate from presentation.
5. **Must-not-break**: the roadmap invariants that apply plus stage-specific ones. Anything that, if it moves, means stop and flag.
6. **How to verify**: the exact command and what a pass looks like. Mandatory; a change with no proof is not ready to brief.
7. **Acceptance**: a checklist Code self-checks before declaring done.
8. **Build / run split**: only when something touches a database, network, secrets, or the live machine.
9. **Open questions**: what Code confirms before building, or "none".

End with the paste-ready Code prompt in one fenced block: read the planning docs first, Plan Mode before edits for multi-file work, branch off `main_branch`, one scope, keep the invariants, run the verify command, descriptive commits with no AI attribution trailers, append a `PROGRESS.md` row with refs (stage id, brief NN) when it lands, and return a summary plus verification output.

Never put a computed figure in a brief. If a number matters, tell Code to print it with its source. Reference confidential files by path; never paste their contents.

## 3. Check

Does the brief move the stage toward its gate as written? Can Code verify it alone? If either is no, fix the brief before writing it.

## 4. Log and reply

Append to `PROGRESS.md`: "Brief NN written (S#: title)". Reply with the file path, one line, followed by the paste-ready prompt block so the user can copy it straight from chat.

## Rules

- Never run git. Brief writes only into `planning/briefs/` and `PROGRESS.md`.
- Follow the playbook named in `project.md` for tone.
