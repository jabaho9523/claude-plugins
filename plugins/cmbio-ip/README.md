# cmbio-ip

Cmbio's invention disclosure and IP screening plugin.

## Skills

| Command | Who runs it | What it does |
|---|---|---|
| `/cmbio-ip:disclose` | Any employee | Interview, fills the IDF, gets inventor sign-off, produces the file and a cover note for the IP owner |
| `/cmbio-ip:screen` | IP owner | Logs the IDF, checks completeness, screens disclosure and ownership risk, runs a first-pass patentability screen (via `/ip-legal:invention-intake` when installed), fills the scorecard, drafts the route decision for the panel |

## How it is meant to be used

Manager to employee: "Great idea. Start by running `/cmbio-ip:disclose` in Claude." Twenty-five minutes later the employee has a confirmed IDF and sends it to the IP owner. The IP owner runs `/cmbio-ip:screen` the same week. The panel decides monthly. The firm sees a batch twice a year.

## Setup

1. Edit `references/cmbio-ip-config.md`: IP owner, contacts, where IDFs are logged, patent firm. Then copy it into `skills/disclose/references/` and `skills/screen/references/` (each skill carries its own copy so it works wherever the skill is installed). Values still marked TBD are handled gracefully; the skills run in trial mode and tell the inventor to send files to their manager.
2. Optionally install the upstream IP plugin, which `screen` will use for its first pass:
   ```
   /plugin marketplace add anthropics/claude-for-legal
   /plugin install ip-legal@claude-for-legal
   /ip-legal:cold-start-interview
   ```

## Reference files

Master copies live in `references/`. Each skill bundles the subset it needs under its own `references/` folder. When you change a master copy, re-copy it into the skills.

| File | Used by |
|---|---|
| `cmbio-ip-config.md` | both |
| `idf-template.md` | disclose |
| `process-summary.md` | both |
| `disclosure-checklist.md` | disclose |
| `scorecard.md` | screen |
| `route-decision.md` | screen |

The full process document, the trade secret register and the annual tax pack index are not bundled; they live with the IP owner.

## Not legal advice

Output is structured preparation for a decision by the review panel and the patent firm. It is not a patentability opinion and never says "file" or "safe to publish".
