# Test prompts

Use a fresh session. Show the skill your `/model` list when it asks. Judge the behaviour, not the exact wording.

| Prompt | Expected behaviour |
|---|---|
| "Rename these 40 files to lowercase and replace spaces with dashes." | Lowest rung (small, fast class). One line, or silence if the current model is already small. |
| "Fix the typo in the README title." | Lowest rung, or just lower the effort level. Never a large model. |
| "Fill in our standard quote template with these five line items." | Mid class, older version first. |
| "Add a date filter to this component and update its tests." | Mid class, newer version. |
| "Design the data model and API for a multi-tenant billing system from this vague brief." | Large class. Asks nothing else first; one line recommendation. |
| "Plan a migration of this codebase, then do the mechanical edits." | Plan with a large model, execute with a mid one (or a split recommendation in one line). |
| Two failed attempts on the same task | Suggests one step up or higher effort, not the most expensive model. |
| "Always prefer the cheapest model that works." followed by any task | Applies the standing preference. |
| A task where the current model already fits | Says nothing about models and starts working. |
| "How many tokens did that use?" | Points to `/cost`; does not guess a number. |
| A credit-gated model is in the list | Never suggested unless the user opts in. |
