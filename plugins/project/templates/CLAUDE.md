# CLAUDE.md: [project name]

## How to work here
Read `planning/project.md` first; it points to the playbook and context files for this project.
Import the playbook here if the path has no spaces (Claude Code ignores `@` imports whose path contains spaces):
@[path/to/way-of-working.md]

## This repo
- Stack / runtime: []
- Run / test: [verify_command]
- Standing invariants (must not break): see `planning/roadmap.yaml` `invariants`
- A change's source of truth is its brief in `planning/briefs/`; if code and brief disagree, stop and flag.

## Git
You own the branch lifecycle. Before branching, commit pending `planning/` edits on `[main_branch]` as `planning: <date>`; stop if anything else is uncommitted. Work on `brief-NN-slug`, verify, commit, then `git merge --ff-only` into `[main_branch]`, re-run the verify command there, delete the branch. If `--ff-only` refuses: stop and report. Never rebase, force, rewrite history, or push without an explicit instruction.

## Autonomy
- Auto: read files, run tests, write to `planning/`, commit on a brief branch, ff-only merge after green verify, delete the merged branch
- Confirm: edit existing source files, add modules
- Always confirm: push, rebase, force, change config / auth / secrets

## Records
- `planning/PROGRESS.md` is the only tracker. Append a dated row when a change lands, and commit it.
- No AI attribution trailers in commits or files unless asked.
