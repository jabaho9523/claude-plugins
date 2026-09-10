---
name: scope
description: Discuss a project with the user, gather everything needed, and write planning/SCOPE.md before planning starts. Use whenever the user describes something they want to build, change, or achieve and no agreed scope exists yet; when they say "let's scope this", "define the project", "what should this include", "help me think this through before we plan", "I want to build X"; or when a plan is requested and planning/SCOPE.md is missing or still a draft. Works in lite mode (a short scope for a single task, six questions at most) and full mode (goal, non-goals, appetite, decider, glossary, pre-mortem, candidate stages with gates).
---

# Scope

Purpose: close the gap between what the user knows and what you know, then write it down so planning starts from a shared, written understanding rather than from memory of a chat. The output is `planning/SCOPE.md`. Everything else in this skill is about getting there without exhausting the user.

## 0. Read the config

Read `planning/project.md`. If it is missing, offer to run `/project:setup` first and stop; scope needs the mode and the connector list.

The mode sets the budget:

| | lite | full |
|---|---|---|
| Question budget | 6, one round | 15, at most two rounds |
| Fields | Goal, Done when, Non-goals, Constraints, What exists, Assumptions, Open questions | lite fields plus Appetite, Decider and stakeholders, Success measures, Domain glossary, Risks, Candidate stages |
| Pre-mortem | skip | yes |
| Candidate stages | skip | yes |

The budget is a hard cap. When it is spent, every unanswered field becomes a line under Assumptions with your best guess, marked so the user can correct it later. An assumption on paper beats a question the user never answers.

If `SCOPE.md` already has content with `status: agreed`, offer to revise it (show what would change) rather than starting over. If it is `status: draft`, continue from where it stopped.

## 1. Dump

Ask the user to dump everything they have, unordered: what they want, why now, what already exists, who cares about the result, hard constraints, deadline, anything that has been tried. Say explicitly that shorthand and half-sentences are fine and that you will structure it.

Offer to read sources they point at, using only the connectors listed in `project.md`: a Slack thread, an Obsidian note, a Monday item, a GitHub issue, a file. Read what they name, not the whole channel or vault. Summarise each source in two lines back to them so they can see what you took from it.

Wait for the dump before asking anything else. Questions asked before the dump are almost always answered by the dump.

## 2. Propose, do not interrogate

After the dump, write a proposed scope with every field filled. Where you are guessing, write the guess and tag it `[assumed]`. Reacting to a wrong guess is faster for the user than answering an open question, and it shows them what you did not understand.

Then ask numbered questions only for fields where a wrong guess would be costly: the decider, the appetite, hard constraints, things that must not change. Invite shorthand answers. This is the round that spends the budget.

Push back where it belongs. If the goal and the done-when criteria do not match, say so. If the appetite is too small for the goal, say so and offer two ways out (shrink the goal or grow the appetite). The user asked for a collaborator, not a form.

## 3. Pre-mortem (full mode)

Ask: "It is [appetite] from now and this failed. Why?" Ask for three reasons and add two of your own. Each reason becomes a row under Risks with either a mitigation or an explicit "accept". Three sentences per row at most.

## 4. Candidate stages (full mode)

Propose three to six stages. For each, write a one-line **gate**: the check that has to pass before the next stage starts. Prefer gates you can execute: a test suite passes, a tie-out matches to the cent, a benchmark clears a threshold, a schema validates against real records. Where only judgment works (design quality, stakeholder acceptance), write a short rubric and tag the gate `[judgment]` so `plan` knows it needs a fresh-context grader instead of a script.

These are candidates. `plan` finalises them and may split or merge. Say that to the user so they do not over-invest in stage boundaries here.

Domain-driven design lives in the stages too: each stage should be describable in the terms from the glossary, and if a stage needs a term the glossary lacks, add it now.

## 5. Domain glossary (full mode)

Collect the nouns the user used and define each in one line, in their words. When two terms seem to mean the same thing, ask which one wins. This becomes the ubiquitous language for briefs, tests, and the roadmap, so pick names that will still make sense in code.

## 6. Exit gate

Scope is done when:

- every field is filled or marked `[assumed]`,
- no open question blocks planning (open questions that can be answered during a stage stay open, listed with an owner),
- you could ask the user about trade-offs without needing basics explained.

Play the scope back as five lines and ask once: "Anything wrong or missing?" Apply corrections. Do not ask again.

## 7. Write

1. Write `planning/SCOPE.md` from `${CLAUDE_PLUGIN_ROOT}/templates/SCOPE.md`. Remove the full-only sections in lite mode. Set `status: agreed`, `version: 1`, today's date.
2. Append a dated line to `planning/PROGRESS.md`: "Scope agreed, v1."
3. Tell the user the scope is written and where. Next step is `/project:plan`.

Nothing in the scope is computed. If a number matters (record counts, budget, traffic), write where it will be verified, not a guess at its value.

## Working rules

- Follow the playbook referenced in `project.md` for tone and formatting. If none is referenced, `${CLAUDE_PLUGIN_ROOT}/defaults/way-of-working.md` applies.
- Keep your turns short. The user's dump should be the longest message in this conversation.
- Never run git. Scope only writes markdown into `planning/`.
- If the user wants to skip a step, skip it and record what was skipped under Assumptions.
