---
name: review
description: 🧭 Evaluate what Claude Code returned against the brief it was given. Use when the user pastes Code's output or a git log and says "review", "check this against the brief", "did Code do it right", "evaluate brief 02", or after any change lands in a project that uses briefs. Logs the verdict and flags anything that should change the roadmap or the scope.
---

# Review

Review checks the work against the brief as written. It does not re-do the work, re-derive numbers, or edit the roadmap.

## 0. Read

Which project and which brief (argument, or infer from the pasted output; if unclear, ask which brief). Read the brief, `roadmap.yaml` (stage, gate, invariants), `project.md`.

## 1. Evidence

What the user pasted (Code's summary, test output from both verify runs, merge result) and the files the brief says should now exist. On the control surface, read files and never run git. On the code surface you may run the verify command and `git log --oneline -n 20`, read-only.

"Code says the tests passed" without the output is not evidence. Mark it "can't tell" and hand the user the exact command to get it, as a numbered step with one block.

## 2. Check, in this order

- **Acceptance**, item by item: met, not met, or can't tell, each with the file or output line that shows it.
- **Invariants**: any violated, any unverifiable from the evidence.
- **Verify step**: was the exact command from the brief run on the branch and again on the main branch after the merge, and did both pass, per the pasted output.
- **Merge**: did Code report a clean `--ff-only` merge and branch deletion? A stop-and-report from Code is a valid outcome, not a failure; it becomes the first rework item.
- **Scope creep**: anything done the brief did not ask for.
- **Deviations** Code reported, and whether each is acceptable.

## 3. Verdict

One of: **accepted**, **accepted with follow-ups**, **rework**. Rework lists the exact items to fix and ends with the Code prompt for the rework, paste-ready. Follow-ups become a note for the next brief.

## 4. Explain

Four lines for the user, plain language: what changed, why it matters, what it affects, what is next.

## 5. Record

- `PROGRESS.md` row: "Review Brief NN: <verdict>" with refs (stage id, brief NN).
- Anything the work revealed that changes a gate, a stage, or a scope item goes in as its own row prefixed `Finding:` or `Decision:`, and the reply tells the user to run `/project:plan` (gate or stage) or `/project:scope` (goal or done-when) before the next brief.
- Review never edits `roadmap.yaml` or `SCOPE.md`.

## Reply

Verdict line, the four-line explanation, what is next. Rework items as a short list plus the Code prompt when there are any.

## Rules

- Do not accept what you cannot see.
- Never run git from the control surface.
- Anything the user must do in Code or a terminal follows the hand-off format in the playbook.
- Follow the playbook named in `project.md` for tone.
