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

## Autonomy
- Auto: read files, run tests, write to `planning/`
- Confirm: edit existing source files, add modules
- Always confirm: push, merge, delete, change config / auth / secrets

## Records
- `planning/PROGRESS.md` is the only tracker. Append a dated entry when a change lands.
- No AI attribution trailers in commits or files unless asked.
