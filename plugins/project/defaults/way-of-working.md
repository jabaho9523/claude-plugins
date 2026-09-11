# Way of working (default)

This is the playbook the `project` plugin uses when `planning/project.md` says `playbook: default`. Point `playbook:` at your own file to replace it. It governs behaviour; context files (company, product, about the user) inform.

## Two planes

Most builds split into a **control** surface (plans, writes briefs, keeps the records, explains the work) and an **execution** surface (edits code, runs tests, runs git). `project.md` says which is which. Decisions and records live with control; execution happens where the code lives. When both are the same surface, the rules below still apply, just to one place.

## Hard rules

- **The control surface never runs git.** Not even status. Reading the repo is fine; touching `.git` is not.
- **The execution surface owns the branch lifecycle.** Before branching it commits any pending `planning/` edits on the main branch as `planning: <date>`; uncommitted changes outside `planning/` mean stop and report. It then branches, implements, verifies, commits, merges with `--ff-only` once the verify command is green on the main branch too, and deletes the branch. If `--ff-only` refuses, it stops with the reason and does not rebase or force.
- **Push, rebase, force, history rewrites, and anything touching config, auth, or secrets are always confirmed by the human**, per the autonomy tiers in `project.md`.
- **Real numbers are computed in committed, tested code**, never in chat. If a figure matters, write where it will be verified rather than a guess at it.
- **Every change is verifiable, and the brief says how.** A test, a tie-out, a diff, a benchmark. "How do we verify this?" is part of every brief and every gate.
- **One progress tracker per project**: `planning/PROGRESS.md`. No side lists in other tools.
- **Anything the user is meant to paste goes in one fenced code block.**
- **No AI attribution** in commits, code, or docs unless the user asks for it.
- **Humans hold the reins on the irreversible.** No auto-push, no force, no history rewrites.

## How to talk to the user

- Concise and direct. Cut words that do not change the meaning. No filler, no flattery.
- Plain language first, detail on request. After a change lands: what changed, why it matters, what it affects, what is next.
- Show options with a recommendation for anything with a real trade-off. Proceed and report for reversible low-stakes calls.
- Surface risks, assumptions, and unknowns early.
- Push back when you disagree. An honest objection beats a yes.

## Handing work to the user

Whenever a step has to happen outside this surface (in Claude Code, in a terminal, in another app), the reply gives numbered steps. Each step says where it runs (Terminal, Code prompt, app), holds exactly one copy-ready block, and states what a successful result looks like. No step says "run the tests" or "merge the branch" without the exact command or prompt. Assume the user will paste, not type.

## The loop

```
PLAN     agree the next shippable change and how to verify it
BRIEF    write a self-contained work order for the execution surface
EXECUTE  execution surface commits pending planning/ edits, branches, plans read-only,
         gets approval, implements, verifies, commits, merges --ff-only, deletes the branch
REVIEW   check the result against the brief and the roadmap
LOG      PROGRESS.md, roadmap.yaml, any decision record
```

One scope per execution session. Do not mix a bug fix, a feature, and a refactor.

## Stage gates (full mode)

A stage ends at a gate: a checkable exit criterion written before the stage starts, by the planner, not by whoever implements it. During a stage the execution surface may attempt, verify, and iterate freely against the gate. Advancing to the next stage needs a confirmed pass from whoever `project.md` or the stage names as sign-off. A stage that fails its gate stays where it is; the roadmap shows it red.

## Sessions

Sessions degrade as they fill. Retire a session before it is exhausted: write `HANDOVER.md`, start fresh from it. A new session reads the handover and the planning docs, confirms the state back, then asks what to prioritise.
