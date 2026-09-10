# Connectors: what each is for in a project

Setup records, per project, which connectors are in use and for what. Other skills read that list from `planning/project.md` and use only what is listed. Two rules override everything below:

1. **One progress tracker per project, and it is `planning/PROGRESS.md`.** No connector becomes a second tracker. Tasks in Things, items in Monday, or issues in GitHub can be *referenced* from the roadmap, never mirrored into it as a parallel list.
2. **Read what the user points at, not everything you can reach.** A connector being available is not permission to trawl it.

| Connector | Typical use in a project | Which skills | Not for |
|---|---|---|---|
| Obsidian | Read reference notes, playbooks, and context files the user names. Optionally mirror `SCOPE.md` and `ROADMAP.md` into the vault as read-only copies. | setup, scope, roadmap | Tracking progress; the vault is a reader, the repo is the record |
| GitHub | Issue and PR numbers as `refs` on roadmap items. Read a PR or issue the user points at. Execution surface runs git; control surface never does. | scope, brief, review, roadmap, gate | Running git from the control surface |
| Slack | Idea and feature-request intake from a thread the user names. Post a status line or the "what's next" summary when asked. | scope, idea, closeout | Auto-posting without being asked |
| Monday | Intake of tickets or requests into the ideas backlog. Reference item ids. | idea, scope | A second task tracker |
| HubSpot | Business context for BD-related projects (which client, which deal). Read-only. | scope | Anything written back |
| Things | Personal reminders the user sets for themselves. | none by default | A project tracker, per rule 1 |
| Filesystem | Read and write the project folder from Cowork. | all | Running git |

When a connector is listed in `project.md` with a use the table does not cover, follow the user's stated use. When a skill needs data from a connector not listed, ask before using it and offer to add it to `project.md`.
