---
name: setup
description: 🧭 Create the planning files for a new project. Use when the user says "new project", "set up [name]", "start a project called X", or when another project skill finds no planning/project.md for the named project. Takes the project name as its only input and asks nothing else.
---

# Setup

One input, the project name. No interview. Everything about the project is gathered in `scope`. Settings you rarely change are defaults in `planning/project.md`; edit them by hand when needed.

## Steps

1. **Name.** From the argument (`/project:setup coloring-site`). If missing, ask "Project name?" and nothing else.
2. **Folder.** Follow `${CLAUDE_PLUGIN_ROOT}/shared/resolve-project.md`. If a project with this name already exists, say so and stop. Otherwise the root is `./<name>/`. Never create files in the home directory.
3. **Create** from `${CLAUDE_PLUGIN_ROOT}/templates/`, filling `name` and today's date, everything else at default:
   - `<name>/planning/project.md`
   - `<name>/planning/SCOPE.md` (status: draft)
   - `<name>/planning/roadmap.yaml`
   - `<name>/planning/PROGRESS.md` (one entry: project created)
4. **Reply** with exactly two lines:

   ```
   Created <name>/planning/ (4 files).
   Next: /project:scope <name>
   ```

## Rules

- Do not ask about mode, repo, connectors, or playbook. `scope` sets the mode and goal, `plan` asks about the repo when it matters, the rest stay at their defaults until the user edits `project.md`.
- Full-mode files (`HANDOVER.md`, `briefs/`, `decisions/`, the repo `CLAUDE.md`) are added by `scope` when it sets `mode: full`, not here.
- Never run git. No AI attribution in files.
