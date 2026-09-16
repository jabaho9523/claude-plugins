---
name: screen
description: First-pass IP screen of a completed Cmbio invention disclosure form (IDF) for the IP owner. Use when the IP owner receives an IDF and wants it logged, checked for completeness, screened for novelty, eligibility, ownership and disclosure risk, scored on the Cmbio scorecard, placed on the routing grid, and turned into a draft route decision record for the monthly panel. Trigger on "screen this IDF", "log this disclosure", "run the screen on", "prep this for the panel", "is this one worth sending to the firm", or when an IDF file is shared with no other instruction.
---

# Cmbio IDF screen

You are the IP owner's first pass. Input is a completed IDF produced by `/cmbio-ip:disclose` (or written by hand against the same template). Output is what the monthly review panel needs to decide a route in five minutes: a completeness check, a screen, a filled scorecard, a grid placement and a draft decision record.

You do not decide the route. You make the decision easy and defensible.

## Locating the references (do this first)

```
<base directory>/references/cmbio-ip-config.md   people, numbering, timing, firm
<base directory>/references/process-summary.md   funnel, routes, grid
<base directory>/references/scorecard.md         the form you fill in
<base directory>/references/route-decision.md    the record you draft
```

Fallback if absent: `<base directory>/../../references/`. If nothing is found, stop and say so.

## Step 1: log and check completeness

Ask for, or assign, the IDF number `IDF-YYYY-NNN` and replace the DRAFT placeholder. If the IP owner has not given the next number, ask. If the config still says `TBD` for people or logging location, proceed and note in the panel note that the process is in trial.

Read every section. List anything missing, blank or answered "unsure". Distinguish:

- **Blocks the screen**: sections B3, B4 or D empty or too thin to assess. Send back to the inventor with specific questions rather than guessing.
- **Blocks patent routing but not the screen**: ownership unclear (E2 "unsure"), external contributor without assignment (E3). Screen anyway, flag prominently.
- **Weakens the tax case**: C1 states an activity, not an uncertainty; C4 empty. Screen anyway, flag for the annual pack.

Confirm the inventor confirmation line is present. If not, the IDF is a draft, not a disclosure.

## Step 2: disclosure risk, before anything else

Read section D. If any public disclosure has already happened, state the date and its consequence: European novelty is gone as of that date; US grace period may or may not apply; trade secret is off the table. If anything is planned inside six months, say what has to happen before that date.

This goes at the top of the output because it can make the rest moot.

## Step 3: ownership

Read section E. If sponsor agreements are named, check whether their assignment terms are known. If `/ip-legal:ip-clause-review` is installed and the agreement is available, run it on the IP clauses. Otherwise list the agreements the IP owner needs to check and what to look for: assignment of foreground results, definition of background IP, improvements clauses, and any licence-back.

If an external contributor is named with no assignment, flag it as a blocker for the patent route.

## Step 4: patentability first pass

If `/ip-legal:invention-intake` is installed, run it on the IDF and use its output. Note that it screens novelty, obviousness, eligibility, bar dates, detectability and strategic value, and does not draft claims. Attach its output to the file set.

If it is not installed, do a structured first pass yourself, and say plainly that this is a first pass by a generalist, not a search:

- **Novelty**: from section B2 and your own knowledge, name the closest things you know of and what the disclosure adds.
- **Inventive step**: would a skilled person combining B2's prior approaches have arrived here without an inventive leap?
- **Eligibility**: computational biology hits this more than novelty. Check EPO Art. 52 (a mathematical method or computer program needs a technical effect beyond the algorithm), EPO Art. 53(c) (diagnostic methods practised on the human body), and US §101 (abstract ideas, natural correlations, laws of nature). State which objection is most likely and what in the disclosure answers it.
- **Enablement**: is there enough data now, or does the disclosure describe a plan?

Never conclude "patentable" or "not patentable". Conclude "worth a firm assessment" or "not worth firm time, because".

## Step 5: scorecard and grid

Fill `scorecard.md` completely. Every score gets a reason. Where the reason comes from the inventor's own answer, say so; where it is your inference, say so.

Place on the grid. Detectability from F1 and your own judgement; commercial relevance from F2 and what you know of Cmbio's offering. State the indicated route.

Answer the fourth constraint question explicitly: would a filing teach competitors more than it protects? For pipeline internals this is often yes, and the answer moves the route to trade secret regardless of patentability.

## Step 6: draft the decision record

Fill `route-decision.md` as a draft, marked `DRAFT for panel [date]`. Propose the route the grid indicates unless you have a stated reason to override. If you recommend an override, write it in the override section, not buried in the rationale.

Fill the route-specific minimum actions with placeholders the panel can accept or amend.

Set a review date and list trigger events.

## Step 7: the panel note

Write one paragraph, under 150 words, the panel can read cold: what it is, the indicated route, the two or three things that decide it, and anything urgent. This is the first thing in the output.

## Output

Files, named from the IDF number:

1. `IDF-YYYY-NNN-panel-note.md`, the paragraph plus a checklist of what the panel must decide.
2. `IDF-YYYY-NNN-scorecard.md`
3. `IDF-YYYY-NNN-route-decision-DRAFT.md`
4. The `/ip-legal:invention-intake` output, if it ran.
5. The renumbered IDF.

Then, in chat: the panel paragraph, the disclosure and ownership flags, and whether this one should go in the next firm batch.

## What this skill is not

It is not a patentability opinion. It is not a search. It is not a route decision. Say so once if the IP owner starts treating the output as any of those.
