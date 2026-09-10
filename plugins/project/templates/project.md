---
name: [slug]
goal: "[one line]"
mode: lite            # lite | full
created: [YYYY-MM-DD]
updated: [YYYY-MM-DD]
planes:
  control: cowork      # who plans, briefs, keeps the docs: cowork | code | chat
  execution: code      # who edits code and runs git: code | user | same
repo:
  present: false
  path: null
  main_branch: main
  verify_command: null    # the command that proves the build is right, e.g. "pytest -q"
  baseline: null          # optional file the gate and roadmap read as evidence, e.g. tests/baseline.json
docs:
  scope: planning/SCOPE.md
  roadmap: planning/roadmap.yaml
  roadmap_md: planning/ROADMAP.md
  roadmap_html: planning/roadmap.html    # full only
  progress: planning/PROGRESS.md
  handover: planning/HANDOVER.md         # full only
  briefs: planning/briefs/               # full only
  decisions: planning/decisions/         # full only
playbook: default       # "default" uses the plugin's defaults/way-of-working.md, or a path to your own
context_files: []       # files every skill should read for background, e.g. company or product descriptions
connectors: []          # - name: slack
                        #   use: "idea intake from #project-x"
autonomy:
  auto: [read files, run tests, write to planning/]
  confirm: [edit existing source files, add modules]
  always_confirm: [push, merge, delete, change config or auth or secrets]
gates:                  # full only
  sign_off: user        # who confirms a stage has passed: user | <name>
  current_stage: null   # id from roadmap.yaml, advanced only on confirmed pass
---

# [name]

[goal]

Every project skill reads this file first. Edit it directly or run `/project:setup` to change a value. Keep it small; anything about *what* the project is belongs in SCOPE.md, anything about *what is next* belongs in roadmap.yaml.
