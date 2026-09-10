---
name: setup
description: Initialise or reconfigure a project for the project plugin. Use whenever the user starts a new project or build with Claude, says "set up the project", "new project", "let's start a project", "bootstrap this", wants to switch a project between lite and full mode, wants to record which connectors a project uses, or when any other project skill finds no planning/project.md. Writes planning/project.md (the config every other project skill reads) and stands up the planning doc set.
---

# Setup

Every skill in this plugin starts by reading `planning/project.md`. Setup writes that file. Run it once per project, and again whenever a value in it changes (mode, connectors, test command, playbook path). Keep it short: the user wants to get to scoping, and the config can be edited later.

## 1. Check for an existing config

Look for `planning/project.md` in the project folder. If it exists:

1. Read it and show the current values as a short table.
2. Ask what to change.
3. Apply the change, update `updated:` in the frontmatter, and stop.

Do not re-run the interview for an existing project. Switching lite to full is the common case: add the full-mode files from step 4 that are missing, leave everything else alone.

## 2. Detect where you are running

The plugin can run from the desktop app (Cowork), from Claude Code in a terminal, or from a chat with no project folder. Detect it, since it decides what you can write and whether git is yours to touch:

- **code**: you have a shell inside the project working tree and `git status` succeeds.
- **cowork**: you read and write project files through a mounted folder or a filesystem tool, but have no shell in the working tree.
- **chat**: neither. Create the files as outputs and tell the user where to put them.

Record the result for this run only. The two-plane split (who controls, who executes) is asked in the interview, because it is a decision about the project, not an observation about this session.

## 3. Interview, one round

Ask the questions below as a single numbered list and invite shorthand answers ("1: site-relaunch, 3: full, 6: slack, github"). Propose a default for every question so the user can answer "defaults" to most of it. One round only; if an answer is contradictory, ask about that one thing, otherwise move on.

1. **Name** (a slug, used for folder and session names).
2. **Goal** in one line. Scope will refine it; this is just for the header.
3. **Mode**: lite or full. Propose one and say why. Lite fits in one or two sessions and has a single verifiable outcome. Full has several components, spans many sessions, or is anything the user would want to stop and check partway through.
4. **Repo**: is there a code repository? Path, main branch, and the command that proves things work (`pytest -q`, `npm test`, a tie-out script). No repo is a valid answer.
5. **Planes**: which surface controls (plans, briefs, keeps the docs) and which executes (edits code, runs git)? Default: Cowork controls, Claude Code executes. For lite projects done in one place, both can be the same.
6. **Connectors**: list the connectors available in this session (check your own tool list) and ask which this project uses and for what, one phrase each. Read `references/connectors.md` for what each connector is typically for and what it must not be used for.
7. **Playbook**: use the bundled default (`defaults/way-of-working.md`) or a path to the user's own way-of-working file? Plus any context files to read (company, product, about the user).
8. **Autonomy**: propose the default three tiers and accept edits.

Full mode adds:

9. **Gate sign-off**: who confirms that a stage has passed its gate? Default: the user.
10. **Briefs folder**: default `planning/briefs/`.

Then show the filled config and ask for one confirmation.

## 4. Write the doc set

Copy from `${CLAUDE_PLUGIN_ROOT}/templates/` and fill every placeholder. Nothing in `[brackets]` may remain.

Lite writes:

- `planning/project.md` (from `templates/project.md`)
- `planning/SCOPE.md` (from `templates/SCOPE.md`, sections marked full-only removed)
- `planning/roadmap.yaml` (from `templates/roadmap.yaml`, `mode: lite`, empty `tasks`)
- `planning/PROGRESS.md` with a first dated entry: project initialised, mode, by whom

Full adds:

- `planning/SCOPE.md` with all sections kept
- `planning/roadmap.yaml` with `mode: full`, empty `stages`
- `planning/HANDOVER.md` (from `templates/HANDOVER.md`)
- `planning/briefs/` and `planning/decisions/` (empty folders with a one-line README each)
- If there is a repo: a `CLAUDE.md` router at the repo root (from `templates/CLAUDE.md`). This touches the repo root, so show the draft and confirm before writing. If a `CLAUDE.md` already exists, propose an edit rather than a replacement.

Rules that hold in every mode:

- **Never run git from setup.** If the user wants the folder under version control, say so and let the executing surface or the user run `git init`. On the code surface, run it only if the user asks in this session.
- **Anything the user is meant to paste goes in one fenced code block.**
- **No AI attribution** in files, commits, or docs unless the user asks for it.
- Existing files are never overwritten silently. Show a diff or a proposal first.

## 5. Hand off

Print the tree of what was written, one line per file. Then point to the next step:

- Lite: "Next: `/project:scope`. It will be short, six questions at most."
- Full: "Next: `/project:scope`."

Do not start scoping inside setup. Separate sessions or at least separate turns keep each skill's context clean.
