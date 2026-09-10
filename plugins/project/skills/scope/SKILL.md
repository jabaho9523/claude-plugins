---
name: scope
description: 🧭 Discuss a project and write planning/SCOPE.md. Use when the user says "scope <name>", "let's scope this", "define the project", "I want to build X", or asks for a plan when no agreed scope exists. Short by default (one round of questions); goes deeper only when the project turns out to be a multi-session build.
---

# Scope

## 0. Which project

Name from the argument (`/project:scope coloring-site`) or one question: "Which project?" Resolve `<name>/planning/project.md` under the current folder. If missing, offer `/project:setup <name>` and stop. Start every reply with `Project: <name>` so two projects in one session never blur.

## 1. Dump

One ask: "Tell me everything: what, why now, what already exists, who cares, constraints, deadline. Half sentences are fine. Point me at files, notes, or threads and I'll read them." Read only what is named. Then wait.

## 2. Propose

Write the scope with every field filled, guesses tagged `[assumed]`. Fields: Goal, Done when, Non-goals, Constraints, What exists, Assumptions, Open questions. Then at most six numbered questions, only where a wrong guess would be costly. Shorthand answers welcome.

Push back if the goal and the done-when criteria disagree. An honest objection beats a form.

## 3. Size it

One question: "Is this one or two sessions with a single verifiable outcome, or a longer build you'd want to check partway through?"

- **Lite:** done, go to step 4.
- **Full:** add Appetite, Decider and stakeholders, Success measures, Domain glossary, Risks (pre-mortem: "it is [appetite] from now and this failed, why?", three reasons from the user, two from you), and three to six Candidate stages, each with a one-line gate. Prefer gates that can be executed (tests pass, tie-out matches, benchmark clears). Tag judgment-only gates `[judgment]`. Budget: nine more questions, one round. Unanswered fields become `[assumed]` lines.

## 4. Write

Play the scope back in five lines and ask once: "Anything wrong or missing?" Apply corrections, then:

1. Write `<name>/planning/SCOPE.md` from `${CLAUDE_PLUGIN_ROOT}/templates/SCOPE.md`, `status: agreed`. Drop the full-only sections in lite mode.
2. Set `mode` and `goal` in `planning/project.md`.
3. Append to `planning/PROGRESS.md`: "Scope agreed, v1."
4. Full mode only: create `HANDOVER.md`, `briefs/`, `decisions/` from templates.
5. Reply with two lines:

   ```
   Scope written: <name>/planning/SCOPE.md (mode: lite|full).
   Next: /project:plan <name>
   ```

## Rules

- Keep turns short. The user's dump should be the longest message in the conversation.
- An assumption on paper beats a question asked twice.
- Numbers are not computed here. Write where a figure will be verified, not a guess at its value.
- Never run git. Scope only writes markdown into `planning/`.
- Tone and format follow the playbook named in `project.md`.
