# Resolving the project (step 0 of every skill)

NAME is the first word after the skill name and may be empty. Any text after it is the skill's payload (a log row, an idea), not part of the name. The current folder is the mounted folder in Cowork or the working directory in Claude Code.

1. If `./planning/project.md` exists in the current folder:
   - NAME is empty, or NAME equals its `name`: use it.
   - NAME is different: continue to 2.
2. If `./NAME/planning/project.md` exists: use it.
3. NAME is empty and 1 found nothing: list folders directly under the current folder that contain `planning/project.md`. Exactly one: use it. Several: ask "Which project?" and name them. None: ask "Which project?" and mention `/project:setup NAME`.
4. NAME is given and nothing matched: stop with `No project "NAME" here. Run /project:setup NAME to create it.`

Once resolved: the project root is the folder that holds `planning/`. Read `planning/project.md`, resolve every path under `docs:` against the root, write nowhere outside it, and never write in the home directory. Open every reply with `Project: <name>`.

Nothing carries over between calls. Each call resolves again from NAME, so switching projects in one session is just a different NAME.

`setup` is the exception to step 4: it creates `./NAME/planning/` when nothing matched, and stops if step 1 or 2 already found a project with that name.
