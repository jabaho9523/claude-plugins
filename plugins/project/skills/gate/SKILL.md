---
name: gate
description: 🧭 Check whether a project's current stage has passed its gate, record the evidence, and advance the stage on confirmed sign-off. Use when the user says "gate [name]", "check the gate", "are we through S1", "can we move to the next stage", or when a review accepts the last brief of a stage. Executable gates run the verify command and read the baseline; judgment gates score a rubric with a fresh reader.
---

# Gate

The gate was written by `plan` before the stage started. This skill checks the work against the gate as written and never rewrites the gate to fit the work. If the gate turns out to be wrong, say so and send the user to `/project:plan`.

## 0. Read

Which project (argument or ask). `project.md` (`gates.current_stage`, `gates.sign_off`, repo, verify command, baseline), `roadmap.yaml` (the stage: gate, gate_kind, rubric, sign_off, invariants), `PROGRESS.md` rows for the stage.

## 1. Executable gate

On the code surface: run the verify command, read the baseline file if one is named, run nothing else. On the control surface: ask the user to paste the output, or read a results file the gate names; never run git or compute a result yourself.

Split the gate into clauses. For each clause and each invariant: **pass**, **fail**, or **no evidence**, with the output line or file that decides it.

## 2. Judgment gate

Gather the artefacts the gate names (docs, files, outputs). Score each rubric line with a fresh-context reader: a subagent given only the artefacts and the rubric, no chat history, when subagents are available. Without subagents, score it yourself and say plainly that you have context bias, then rely on the human sign-off. Per rubric line: **met**, **not met**, or **can't tell**, with the passage or file that shows it.

## 3. Iterate (on request)

When the user asks the execution surface to work the gate, the builder may attempt fixes and re-check up to 3 times (the user can raise the number). Log each attempt in `PROGRESS.md`. The gate text does not change between attempts.

## 4. Outcome

- **All pass.** Write `evidence` on the stage: `<what passed>, <source>, <date>`. Ask the named sign-off (the stage's `sign_off`, else `gates.sign_off`) for a one-line confirmation. If the sign-off is the user and they confirm: set `status: green`, move `gates.current_stage` to the next id in `focus`, append `PROGRESS.md` "Gate S# passed: <evidence>", then render (step 3 of `${CLAUDE_PLUGIN_ROOT}/skills/roadmap/SKILL.md`). If the sign-off is someone else: write the confirmation request as a paste-ready message, record "Gate S# awaiting sign-off from <name>" in `PROGRESS.md`, and stop. Nothing advances until the confirmation is pasted back.
- **Partial.** `status: yellow`, `caveats` lists the failed or unproven clauses, stage stays current, `PROGRESS.md` row.
- **Fail.** `status: red`, `PROGRESS.md` row naming what failed, and one line on what the next brief should target.
- **Gate is wrong.** A clause cannot be checked, or a finding since `plan` contradicts it. Do not pass it. `PROGRESS.md` row `Finding: gate S# needs re-plan: <why>`, and tell the user to run `/project:plan`.

## Reply

A short table, one row per clause or rubric line with its result and source. Then the outcome line and what is next.

## Rules

- Never green without an evidence line that names a source and a date.
- Whoever built the stage does not certify it alone; that is what the fresh reader and the sign-off are for.
- Never run git from the control surface. Never compute a figure; copy it from the output.
- Follow the playbook named in `project.md` for tone.
