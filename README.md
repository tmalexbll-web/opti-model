# opti-model

A small Claude skill that helps you spend fewer tokens. At the start of a task it quickly judges how big and how hard the work is, then recommends the **cheapest model, version and effort level** that will still do the job well.

It costs almost nothing to run: the judgement uses only the text of your request, with no file reads and no tool calls.

## Why

Most people leave one model selected for everything. Renaming files and designing a system do not need the same model. Smaller classes and older versions cost less per token; larger and newer ones are stronger. opti-model nudges you to pick the lowest rung that is enough and to climb only when needed.

## What it does

- Reads your request and judges size and difficulty (number of files, new logic or calculation, clarity of requirements, cost of a mistake, available templates).
- Builds a cost ladder from the models **you** have available (it asks once for your `/model` list; it never assumes names).
- Recommends one rung in one or two lines, or says nothing if your current model is fine.
- Suggests changing the **effort level** instead of the model when that is cheaper.
- Suggests escalating by one step after two failed attempts, not straight to the most expensive model.
- Adds short token-hygiene hints only when they apply: switch models at the start of a task, `/clear` between unrelated tasks, `/compact` for long ones, avoid very large context variants without need, check real spending with `/cost`.

Example output:

> Recommend Sonnet (older version, rung 2): routine edit from an existing template. Switch with `/model`. Continue on the current one?

## What it does not do

- It cannot switch your model by itself; you switch with `/model`.
- It cannot see real token counts and will not invent them. Use `/cost`.
- It does not call any external service and does not write any file unless you ask it to keep a log.
- It never suggests models that need extra paid credits unless you opt in.

## Install

Pick one.

**With the skills CLI**

```bash
npx skills add https://github.com/tmalexbll-web/opti-model --skill opti-model
```

**Manually, for all your projects (Claude Code)**

```bash
mkdir -p ~/.claude/skills/opti-model
cp skills/opti-model/SKILL.md ~/.claude/skills/opti-model/SKILL.md
```

**Manually, for one project**

```bash
mkdir -p .claude/skills/opti-model
cp skills/opti-model/SKILL.md .claude/skills/opti-model/SKILL.md
```

**In the Claude app**

Zip the `skills/opti-model` folder and upload it as a custom skill in your skill settings.

Optional safety net: add this line to your `CLAUDE.md` if the skill sometimes does not trigger:

```
At the start of a task, apply the opti-model skill.
```

## Use

Just start a task. The skill runs on its own at the start of a new task and when the task type changes. You can also say "apply opti-model" at any time.

Tell it your standing preferences once, for example:

- "Always prefer the cheapest model that works."
- "Never suggest model X."
- "I do not have access to Y."

The current message always overrides a standing preference.

## Optional log

Off by default. Ask "track model choices in `model-log.md`" and it will append one line per event (date, task, model and rung, outcome, cost if you pasted it from `/cost`). Ask "model statistics" to get a short summary.

## Test it

See [examples/test-prompts.md](examples/test-prompts.md) for prompts and the behaviour to expect.

## Contributing

Issues and pull requests are welcome, especially for new task types, other languages and other providers' model lineups. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT, see [LICENSE](LICENSE).

---

## Кратко по-русски

opti-model помогает экономить токены. В начале задачи он быстро оценивает объём и сложность и рекомендует самую дешёвую модель, версию и уровень effort, которых хватит для хорошего результата. Список доступных моделей он берёт у вас (один раз просит вывод `/model`), сам модель не переключает и реальные токены не оценивает: для этого есть `/cost`. Установка: скопируйте `skills/opti-model/SKILL.md` в `~/.claude/skills/opti-model/SKILL.md`. Отвечает на языке пользователя.
