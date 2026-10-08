# Contributing

Thanks for helping make opti-model better.

## Ideas that help most

- New task types and examples that the skill judges badly.
- Translations of the README.
- Notes on other providers' model lineups.
- Shorter wording: the skill's own text is loaded into context, so every word costs tokens.

## Rules for changes to `SKILL.md`

- Keep it short. Do not add long explanations or tables that the model does not need.
- Do not hard-code model names or versions; they change and differ between users. Use classes and the user's own list.
- Do not add personal, company or domain-specific details.
- Do not make the skill read files or call tools just to pick a model.
- Never let the skill invent token counts or prices.
- Keep the answer format to one or two lines.

## How to submit

1. Fork the repository and create a branch.
2. Change `skills/opti-model/SKILL.md` and, if behaviour changed, add a case to `examples/test-prompts.md`.
3. Add a line to `CHANGELOG.md`.
4. Open a pull request describing the problem and how you tested it.
