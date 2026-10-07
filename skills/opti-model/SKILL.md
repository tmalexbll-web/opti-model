---
name: opti-model
description: At the start of each new task or when the task type changes, quickly judge size and difficulty and recommend the cheapest model, version and effort level that will do the job well, to save tokens. Works for any user and any domain.
---

# opti-model

Goal: every task runs on the cheapest model tier, version and effort level that still gives a good result, while spending almost no tokens on the choice itself. Reply in the user's language.

## When to run
- First message of a new task in a session or project.
- The kind or size of the task clearly changed (for example from fixing a typo to designing a system).
- Do not run again inside the same task unless something changed.

## Know the available models (never assume)
- Use the model list the user has shown (for example the output or menu of `/model`). If it is unknown, ask once for it, then reuse it for the session.
- Skip any model the user says is unavailable or that is marked as needing extra paid credits, unless the user explicitly opts in.
- Skip models the user calls outdated, and old generations in general unless there is a specific reason.
- Version names change over time, so rely on the user's list, not on names written in this file.

## Cost ladder
Build the ladder from the user's list, cheapest first. As a general rule, a smaller model class and an older version within a class cost less per token and are less capable; confirm with the provider's pricing page if the user asks for numbers.
1. Smallest, fastest class (Haiku-class).
2. Mid class (Sonnet-class): start with the oldest version that is still good enough, move to newer versions only if quality is lacking.
3. Largest class (Opus-class): same rule, oldest sufficient version first.
4. Top tier or credit-gated models: only if the user opts in, and only for very large or very high-stakes work.

Always pick the lowest rung that is enough, and climb only when needed.

## How to judge (silently, from the request text, without reading files or running tools)
Signals: number of files or documents, whether new logic or calculation is needed, how clear the requirements are, cost of a mistake, size of the expected output, whether a ready template or existing example is available.

Typical mapping, in any domain:
- Purely mechanical work with no judgment and no wording to write (rename, reformat, extract fields, fix typos, repeat the same edit many times): lowest rung. Anything that needs written text, accuracy of numbers or customer-facing wording is not mechanical.
- Routine work from a template or clear pattern (filling standard documents or quotes, translations, short summaries, small code or site edits, simple data cleanup): mid class, older version first. Never the lowest rung for documents others will read.
- Normal development or analysis (any change to code logic plus tests, multi-file changes, analysing supplied documents or drawings, structured reports): mid class, newest version.
- Work from scratch with unclear or conflicting requirements, new designs or calculations, hard bugs, architecture, high cost of error: large class.
- Huge or critical jobs: top rung, rarely.

If only the planning is hard and the execution is routine, suggest a plan-with-large, execute-with-mid mode when the product offers one (in Claude Code that is `opusplan`).

## Effort lever
If the current model is fine but the task is simple, suggest lowering the effort level instead of switching models; for a hard task suggest raising it. This is cheaper than a model switch and does not reset the prompt cache.

## Escalation
If the chosen rung failed twice in a row or the work had to be redone, suggest one step up (or higher effort), not straight to the most expensive model. Suggest stepping down only on a new task.

## Answer format (one or two lines, before starting work)
- Current model is suitable: say nothing and start working.
- Another model is better: "Recommend <model and version> (rung N): <reason in 5-10 words>. Switch with `/model`. Continue on the current one?"
- The task can be split: "Prep on <small model>, design on <large model>" (one line).

The model cannot switch itself in the main session; the user decides. Start subagents on another model only if the user asks.

## Token hygiene (suggest only when it clearly applies, one line)
- Switching models in the middle of a long session resets the cache, so switch at the start of a task or in a new session.
- Between unrelated tasks suggest `/clear`; for a long ongoing task with a lot of old context suggest `/compact`.
- Avoid very large context variants (for example `[1m]`) unless the work really needs it.
- Do not re-read files that are already in context and do not paste long outputs back.
- `/cost` shows real spending; never estimate or invent token numbers, because the model cannot see them.

## Rules
- Follow the user's standing preferences when stated (for example "always cheapest", "always best quality", "never suggest model X"). The current message overrides them.
- Do not write long reasoning about the choice and do not recite this ladder.
- Do not read files or run tools only to choose a model.
- Do not suggest a more expensive rung when the current one will do.
- If the user declines a recommendation, continue without repeating it for that task.
- No log is written by default. If the user asks to track results, append one line per event (date, task in a few words, model and rung, outcome: ok / redo / escalated, cost from `/cost` if the user pasted it) to a file the user names, and summarise it on request.
