# project

A Claude plugin that carries a project from first conversation to done. Every skill reads one file, `planning/project.md`, so the skills stay generic and the project-specific parts live in the project.

## Two modes

| | lite | full |
|---|---|---|
| For | A single task that fits in one or two sessions ("build a new page for the site") | A build with several components and more than a few sessions ("build a new site") |
| Docs | `planning/project.md`, `SCOPE.md`, `roadmap.yaml`, `PROGRESS.md` | lite set plus `HANDOVER.md`, `briefs/`, `decisions/`, a `CLAUDE.md` router in the repo |
| Stage gates | none, flat task list | stages with checkable exit gates; a stage advances only when its gate passes |
| Roadmap view | `ROADMAP.md` | `ROADMAP.md` plus an HTML maturity map |

The switch is a field in `planning/project.md`. Run `/project:setup` again to change it.

## Skills

| Skill | Status | Does |
|---|---|---|
| `setup` | v0.1 | Writes `planning/project.md`, stands up the doc set |
| `scope` | v0.1 | Discussion that ends in `planning/SCOPE.md` |
| `plan` | planned | `SCOPE.md` to `roadmap.yaml` with stages and gates |
| `brief` | planned | Next code brief plus paste-ready prompt (full) |
| `gate` | planned | Verify current stage against its exit criteria, iterate, report |
| `review` | planned | Check a landed change against its brief |
| `log` | planned | Append to `PROGRESS.md` |
| `roadmap` | planned | Reconcile `roadmap.yaml`, render `ROADMAP.md` and the HTML map |
| `idea` | planned | Add an idea or feature request to the roadmap backlog |
| `handover` | planned | Write or resume from `HANDOVER.md` |
| `closeout` | planned | End-of-session ritual: log, roadmap, handover, open loops |

## Install

Claude Code, personal scope (auto-loads in every project, no marketplace needed):

```bash
ln -s "/Users/jacobbakholm/AI-Drives/Claude-Drive/project-workflow" ~/.claude/skills/project
```

Or for one session only:

```bash
claude --plugin-dir "/Users/jacobbakholm/AI-Drives/Claude-Drive/project-workflow"
```

Cowork: verify how local plugins load there before relying on it; until then, the `skills/*/SKILL.md` files can be shared with a Cowork project as plain instructions.

## Layout

```
project-workflow/
  .claude-plugin/plugin.json
  skills/
    setup/SKILL.md
    setup/references/connectors.md
    scope/SKILL.md
  templates/          copied into a project by setup
    project.md
    SCOPE.md
    roadmap.yaml
    PROGRESS.md
    HANDOVER.md
    CLAUDE.md
  defaults/
    way-of-working.md   generic playbook, overridden by a path in project.md
```

## Sharing later

Nothing personal lives in the skills. Vault paths, personal playbooks, and context files are referenced from `planning/project.md`, which `setup` writes per project. A colleague installs the same plugin and runs `setup` against their own files.
