# Brief NN: [short title] (Stage S#)

## 1. Goal
[One sentence, in the user's terms, using the glossary. What "done" looks like.]

## 2. Context
[Why this, now, from the roadmap and the stage gate. Just enough for Code to act.]

## 3. Where
- `path/to/file.ext`: [function or area, and why it is involved]
- `path/to/other.ext`: [...]
(Lines are hints; re-check the structure before editing.)

## 4. Spec
**Logic / data**
- [...]

**Presentation / UI**
- [...] (or "none")

## 5. Must-not-break (invariants)
- [roadmap invariants that apply]
- [stage-specific: anything that, if it moves, means stop and flag]

## 6. How to verify
[The test, tie-out, or diff that proves correctness. Exact command, e.g. `python3 -m pytest -q`, and what a pass looks like. Add a test if none exists.]

## 7. Acceptance (Code self-checks before declaring done)
- [ ] [specific, testable]
- [ ] [...]
- [ ] Verify command green on [main_branch] after the merge
- [ ] `PROGRESS.md` row appended with refs (S#, Brief NN)

## 8. Build / run split (only if relevant)
- **Code builds offline:** [logic, tests against fixtures or synthetic data]
- **Runs on the live machine:** [anything touching a database, network, secrets]

## 9. Open questions
[What Code should confirm before building, or "none".]

---

## Paste-ready Code prompt

```
You're working in this repo as the builder. Implement Brief NN at
planning/briefs/[filename].md.

Before you start:
- Read planning/project.md, planning/SCOPE.md and planning/ROADMAP.md.
- If the working tree has uncommitted changes under planning/, commit them on
  [main_branch] as "planning: <today>". Uncommitted changes anywhere else: stop
  and report, do not proceed.
- Create a short-lived branch off [main_branch] named brief-NN-[slug].

Build:
- For a multi-file or architectural change, use Plan Mode first: read the
  referenced files, propose a plan, make no edits, and wait for approval.
- Implement one focused scope. Keep the invariants in section 5.
- Verify per section 6: [verify command] must pass.
- Commit with descriptive messages. No AI attribution trailers.

Close the loop:
- git switch [main_branch] && git merge --ff-only brief-NN-[slug]
- Run [verify command] again on [main_branch]; it must pass.
- git branch -d brief-NN-[slug]
- If --ff-only refuses or the verify fails on [main_branch], stop and report
  the exact error. Do not rebase, force, or push.
- Append a dated row to planning/PROGRESS.md (refs: S#, Brief NN) and commit it.

Return: files added or changed and why, verification output from both runs,
the merge result, and any open questions or blockers.
```
