# Handover: the `project` plugin (2026-09-21)

Written for a Claude account with no memory of this work. Everything needed to continue is in this file and in the files it points to. Read it fully before doing anything.

## 1. Where we are

Jacob (Clinical Microbiomics A/S, "Cmbio"; AI governance, tooling rollout, commercial strategy) has built a personal Claude plugin called `project`, v0.1. It carries a project from first conversation to done with ten skills, two modes (lite and full), and a stage-gate model. All ten skills exist and are installed in Claude Code (symlink) and the Claude desktop app (zip upload). One real project is running on it: Cmbio's company finance reporting automation, full mode, six stages, S1 in focus, Brief 01 handed to Claude Code.

The plugin is personal for now. After a few runs a generic version is meant to go to colleagues, so nothing Jacob-specific lives inside the skills.

## 2. Locations

| What | Where |
|---|---|
| Plugin source (edit here) | `/Users/jacobbakholm/AI-Drives/Claude-Drive/claude-plugins/plugins/project` |
| Marketplace repo (source of truth, pushed) | GitHub `jabaho9523/claude-plugins`, marketplace name `jacob-plugins`, catalog at `.claude-plugin/marketplace.json` |
| Claude Code loads it from | `~/.claude/skills/project` (symlink to the source; changes are live next session) |
| Desktop app loads it from | zip uploaded via Customize › Plugins; rebuilt as `~/Desktop/project.zip` after every change |
| Jacob's own playbook | Obsidian vault › `20 - 📘 Cmbio/AI Resources/AI Instructions/My Way of Working.md` (companions: Code Brief Template.md, Handover Template.md, CLAUDE.md Starter.md) |
| First live project | `/Users/jacobbakholm/Library/CloudStorage/OneDrive-ClinicalMicrobiomics/CM - private_personal_folders/AI Drive (Cmbio)/Corporate/Finance Reporting/Company finance reports/company-finance-reporting` (planning/ at its root) |
| One-pager | `project-plugin-onepager.html`, delivered in chat; not yet in the repo |
| Reference for the roadmap map style | `/Users/jacobbakholm/Documents/Documents - JBHolm MacBook Pro/Claude/Artifacts/atlas-maturity-map/index.html` |

Filesystem access from chat: the Filesystem connector (MCP) can read and write `~/AI-Drives/Claude-Drive`, `~/Desktop`, `~/Downloads`, the Obsidian vault, and `~/Documents/Documents - JBHolm MacBook Pro/Claude`. The OneDrive path above also worked. The connector hangs for four minutes several times a day; see Gotchas.

## 3. What the plugin is

### Modes

- **Lite**: one task, one or two sessions ("build a new page"). Files: `project.md`, `SCOPE.md`, `roadmap.yaml` with a flat `tasks` list, `PROGRESS.md`. No gates, no briefs.
- **Full**: multi-session build ("build a new site"). Adds stages with gates, `HANDOVER.md`, `briefs/`, `decisions/`, a `CLAUDE.md` router in the repo, and an HTML roadmap map.
- The switch is `mode:` in `planning/project.md`. `scope` sets it with one sizing question at the end; `setup` never asks.

### Skills (`skills/<name>/SKILL.md`)

| Skill | Does |
|---|---|
| `setup NAME` | Creates `NAME/planning/` with four files from templates. Asks nothing. Two-line reply. |
| `scope NAME` | One dump, one proposal with `[assumed]` tags, at most six questions, one sizing question. Full adds appetite, decider, glossary, pre-mortem, candidate stages with gates. Writes `SCOPE.md`, sets `mode` and `goal`. |
| `plan NAME` | `SCOPE.md` to `roadmap.yaml`. Lite: 3 to 10 tasks with `verify`. Full: 3 to 7 stages, each a bounded context with `gate`, `gate_kind`, optional `rubric` and `sign_off`; invariants; focus order; offers parked ideas for promotion. Gates are written here, before the stage starts. Renders via `roadmap`. |
| `brief NAME` | One shippable change toward the current gate. Nine sections from `templates/brief.md`, ends with the paste-ready Code prompt. Full only. |
| `review NAME` | Checks Code's returned work against the brief: acceptance item by item, invariants, both verify runs, merge result, scope creep. Verdict accepted / accepted with follow-ups / rework. Logs it; never edits roadmap or scope, but writes `Finding:` rows and sends the user to `plan` or `scope`. |
| `gate NAME` | Checks the current stage against its gate as written. Executable: verify command plus baseline. Judgment: fresh-context reader scores the rubric. Green needs `evidence` with source and date, then sign-off confirmation, then `current_stage` advances. Never rewrites a gate; a wrong gate goes back to `plan` as a `Finding:`. |
| `log NAME text` | One row into `PROGRESS.md`, the single tracker. `Decision:` and `Finding:` prefixes. |
| `roadmap NAME` | Reconciles `roadmap.yaml` from `PROGRESS.md` and baseline, renders `ROADMAP.md` and (full) `roadmap.html`, answers with counts and what is next. `roadmap NAME add text` parks an idea. |
| `handover NAME` | Writes `HANDOVER.md`; "resume" reads it back and waits for a priority. "Decided but not in docs" items also become `PROGRESS.md` rows. |
| `closeout NAME` | Log, roadmap, handover, open loops, in that order. Loops over several projects in one session. |

### Shared pieces

- `shared/resolve-project.md`: step 0 of every skill. NAME is the first word after the skill name. Order: `./planning/project.md` if NAME is empty or matches its name; else `./NAME/planning/project.md`; else list candidates or ask. Each call resolves fresh; nothing carries between calls. Every reply opens with `Project: <name>`. Never write in the home directory.
- `defaults/way-of-working.md`: the bundled generic playbook. Overridden per project by `playbook:` in `project.md`. Contains the two-plane rule, hard rules, hand-off format, the loop, stage gates, sessions.
- `templates/`: `project.md` (YAML frontmatter config), `SCOPE.md` (full-only sections fenced in HTML comments), `roadmap.yaml` (tasks, stages, focus, ideas, invariants), `ROADMAP.md`, `roadmap.html` (Atlas-style map with `{{placeholders}}`), `PROGRESS.md`, `HANDOVER.md`, `brief.md`, `CLAUDE.md`.
- `skills/setup/references/connectors.md`: what each connector is for and what it must not become (no second tracker).

### Key rules encoded

- Cowork (control plane) never runs git, not even status. It reads the repo and writes `planning/`.
- Claude Code (execution plane) owns the branch lifecycle: commit pending `planning/` edits on main as `planning: <date>`, branch `brief-NN-slug`, build, verify, commit, `git merge --ff-only`, verify again on main, delete the branch. Any refusal means stop and report. Push, rebase, force, history rewrites, config/auth/secrets are always confirmed by the human.
- Every hand-off to the user (anything to run in Code or a terminal) is numbered steps, one copy-ready block per step, expected result stated. He pastes, he does not type.
- Green needs evidence with source and date. Yellow needs a caveat. Nothing moves backwards silently. A ref to a file that does not exist yet is intent, not evidence.
- Whoever builds a stage does not certify it alone. Sign-off is per stage (`sign_off` on the stage in `roadmap.yaml`, falling back to `gates.sign_off` in `project.md`).
- No AI attribution trailers in commits or files. Real numbers are computed in committed code, never in chat.

## 4. What shipped, in order

1. Design discussion: named approaches (Shape Up appetite and no-gos, PR-FAQ, design-doc non-goals, impact mapping, pre-mortem, spec-driven workflows). Decided: skills stay generic, the project-specific part is one config file per project.
2. v0.1 skeleton with `setup` (interview) and `scope`. Then simplified on Jacob's feedback: `setup` takes only the name, `scope` gathers everything and sets the mode. "Least text and complexity."
3. `plan`, `roadmap`, `log` plus `ROADMAP.md` and `roadmap.html` templates, modelled on the Atlas maturity map (evidence-backed status boxes, settlement section, footer with sources and changelog).
4. `brief`, `review`, `gate`, `handover`, `closeout`. Per-stage `sign_off`. Short Now/Next lists in `ROADMAP.md`.
5. Git policy change (2026-09-11): Code owns the branch lifecycle; hand-off format added to the playbook and to `brief`, `gate`, `review`, `closeout`. Delivered as `project-update.zip` because the connector hung.
6. `shared/resolve-project.md` and all ten skills pointing at it. Delivered as `project-update-2.zip`. Confirm it was applied (see Open loops).
7. One-pager HTML.
8. Jacob updated his own playbook §2 and Code Brief Template prompt to the new git policy, so the finance project (which uses his playbook, not the bundled one) follows it too.

## 5. Decisions and why

- **Config file, not flags.** `planning/project.md` holds mode, planes, repo, verify command, playbook path, context files, connectors, autonomy tiers, gate sign-off, current stage. Skills read it; the user never passes options.
- **Folder name is the project name; no registry.** Several projects can sit in one mounted folder. Jacob asked for exactly this over a `projects.yaml` index.
- **`roadmap.yaml` is the single source; `ROADMAP.md` and `roadmap.html` are renders.** Ideas are an intake block promoted only by `plan`.
- **Gates are authored in `plan`, before the stage, never by the implementer, never rewritten to fit the work.** Executable gates preferred; judgment gates get a three-line rubric and a fresh-context reader.
- **Plugin name `project`** (commands `/project:setup` etc.), `displayName` "🧭 Project Workflow". The compass emoji marks every skill description so they are easy to find. Jacob considered a shorter unique prefix; not done.
- **Angle brackets are banned in skill descriptions.** The app validator reads `<name>` as XML. Use `[name]`.
- **App install is by zip upload, not marketplace.** Jacob cannot add marketplaces in the app (managed deployment). The fallback for sharing is the Cmbio marketplace `Clinical-Microbiomics/claude-plugins`, which Jacob controls, with `defaultEnabled: false`.
- **`version` in `plugin.json`** is still `0.1.0`. Consider removing it while iterating (updates then follow commits) and restoring it before sharing.

## 6. Open loops (priority order)

1. **Confirm update 2 landed**: `shared/resolve-project.md` present, all ten skills reference it, committed, pushed, app zip rebuilt and re-uploaded. If Jacob did not say "done" after update 2, assume not.
2. **First real `review`**: Code was implementing Brief 01 (archive inventory and workbook map) in the finance project. When it returns, `/project:review company-finance-reporting` is the first live test of the new skills. Expect tuning.
3. **Re-plan S2 and S3** in the finance project: S1 found a different tie-out target than those gates assume (recorded in that project's `HANDOVER.md`). Run `/project:plan` after Brief 01 lands, before Brief 02.
4. **Gate S1** is a judgment gate with Morten (Cmbio finance colleague) as the reviewer; `gate` should produce the paste-ready confirmation request and wait.
5. **One-pager into the repo**: add as `plugins/project/docs/onepager.html`, generalise the two personal paths, note in README. Also `setup NAME`: "or omit it to be asked".
6. **README** still lists a `project-workflow` install path in one place if update edits did not all apply; check.
7. **Not built, not yet needed**: `adr` skill (decisions/ exists, written by hand so far), `walkthrough` skill (code-walkthrough.md at milestones), SessionStart hook for auto-resume in Code, subagent definition for the fresh-context gate reader, personal marketplace loading in the app (blocked), a distinctive short namespace instead of `project`.
8. **Sharing version** for colleagues: generic playbook only, no Jacob paths, `version` set, published through the Cmbio marketplace.

## 7. Next 3 tasks

1. Verify update 2 is applied and pushed (Open loop 1). If not, re-apply from the zip or re-create `shared/resolve-project.md` and the step-0 edits.
2. Run `/project:review company-finance-reporting` on Code's Brief 01 output; fix whatever in `review` felt wrong.
3. Run `/project:plan company-finance-reporting` to reword S2 and S3 gates against the S1 finding, then `/project:brief` for Brief 02.

## 8. Gotchas

- **Filesystem connector hangs** (four-minute timeout, "check the Desktop app for a pending approval prompt"). It comes back after a while. When it hangs mid-task, deliver changed files as a zip in chat with an `unzip -o` command into the plugin folder rather than waiting.
- **`zip -r` onto an existing archive appends.** Always `rm -f` the old zip first. Verify with `unzip -l ... | grep -c plugin.json` (must be 1).
- **Skill descriptions cannot contain `<...>`.** The app validator fails with "description cannot contain XML tags".
- **Skills-directory plugins load on the next Code session** and do not appear in the inline `/plugin list`; use `claude plugin list` in a terminal or the `/plugin` interface.
- **Commands typed inside a Claude Code session run in its sandbox** and can be blocked from network (`claude plugin validate` failed that way). Use a plain terminal, or prefix with `!`.
- **`~/Desktop` versus "Desktop - JBHolm MacBook Pro"**: iCloud shows a second Desktop folder in Finder; the shell writes to `~/Desktop`. Use Cmd+Shift+G with the full path in file dialogs.
- **GitHub handle is `jabaho9523`**, not `jacobbakholm`.
- **The sandbox shell here is `/bin/sh`**: no brace expansion in `mkdir -p a/{b,c}`.
- **Cmbio managed deployment** (POL-003): managed settings can block marketplace sources; Jacob owns that policy.
- **Jacob's own playbook says Cowork never runs git** because of two real incidents (zero-byte `.git/index.lock`; a OneDrive-synced checkout switching branches mid-session). Do not relax it.

## 9. How Jacob wants to work

- Hands-on and iterative. He runs commands himself; give him exact, copy-ready blocks, one per step, and say what a success looks like.
- He wants Claude to edit files directly via the Filesystem connector when it works.
- Least text, least complexity. He rejected the interview-style `setup` as "a lot of information". Two-line replies from skills where possible.
- Actionable outputs over explanations. Tables and short lists are fine; long prose is not.
- Never use em dashes or en dashes in prose. Replace with a comma or a new sentence.
- His saved user preferences also forbid: "That's not X. That's Y." constructions; two-fragment punchlines; metaphor pairs without advice; self-applause ("and that matters"); "It's the X of Y" comparisons; warm-up openers ("Here's the thing"); always-three lists; ranges instead of a number; summary endings; naming Claude as co-author. Ask him to copy his user preferences into the new account.
- He thinks in stage gates and domain-driven design; he built the Atlas maturity map and wants roadmaps that look and behave like it (verified status only).
- Danish and English both fine; this work was in English.

## 10. Start-of-session prompt for the new account

```
I'm resuming work on my Claude plugin called "project" with a new account.
Read HANDOVER-project-plugin-2026-09-21.md fully, then read the plugin source at
/Users/jacobbakholm/AI-Drives/Claude-Drive/claude-plugins/plugins/project
(README.md, shared/resolve-project.md, defaults/way-of-working.md, and the ten
skills/*/SKILL.md). Confirm the current state back to me in five lines, list the
open loops you see, and ask which one to take first. Do not change any file
until I confirm. Rules that always apply: Cowork never runs git; anything I must
run in Code or a terminal comes as numbered steps with one copy-ready block each;
no em or en dashes.
```

## 11. Appendix: file tree of the plugin

```
plugins/project/
  .claude-plugin/plugin.json          name project, displayName 🧭 Project Workflow, version 0.1.0
  README.md                           modes, skill table, install, layout, sharing notes
  shared/resolve-project.md           step 0 of every skill
  defaults/way-of-working.md          bundled generic playbook
  skills/
    setup/SKILL.md, setup/references/connectors.md
    scope/  plan/  brief/  review/  gate/  log/  roadmap/  handover/  closeout/   (SKILL.md each)
  templates/
    project.md  SCOPE.md  roadmap.yaml  ROADMAP.md  roadmap.html
    PROGRESS.md  HANDOVER.md  brief.md  CLAUDE.md
```

Per-project files it creates:

```
<project>/
  planning/
    project.md      config (frontmatter): name, goal, mode, planes, repo, docs, playbook,
                    context_files, connectors, autonomy, gates.sign_off, gates.current_stage
    SCOPE.md        goal, done-when, non-goals, constraints, what exists; full adds appetite,
                    decider, success measures, glossary, risks, candidate stages
    roadmap.yaml    tasks (lite) or stages (full), invariants, focus, ideas
    ROADMAP.md      rendered
    roadmap.html    rendered, full only, previous versions in roadmap-versions/
    PROGRESS.md     append-only table: date, what, refs, by
    HANDOVER.md     full only
    briefs/         full only, YYYY-MM-DD-NN-slug.md
    decisions/      full only, ADR-NNNN-*.md
  CLAUDE.md         router for Claude Code, only when there is a repo
```

Status vocabulary in `roadmap.yaml`: `green` done with evidence, `yellow` works with caveats, `red` focus, `planned` not started. Ideas: `parked`, `promoted`, `rejected`.
