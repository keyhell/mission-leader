# Staffing

Assign a model and an effort level to each role. Use the defaults below unless the environment or the mission calls for something else, and state the reason for any change.

## Roles and defaults

| Role | Modes | Default model | Default effort | Why |
|---|---|---|---|---|
| Mission leader (you) | 1, 2, 3 | Mid-level model, such as the one the person launched the task with | Medium | Judges the task and buys capability where it is needed. |
| Worker | 2 | You, or the strongest model when the work is above your level | Medium to high | One strong worker plus a review was nearly as good as a full loop in the pilot. |
| Planner | 3 | Strongest available | High | The plan decides everything downstream. |
| Executor | 3 | Strong model | Medium | Does most of the useful work. |
| Senior reviewer (sees the whole log) | 2, 3 | Strongest available, from a different model family when possible | High | Caught both major issues in the pilot. |
| Specialist reviewer (sees only the deliverable) | 3 | Mid-size model | Low to medium | Cheap checks of formulas, units, code, and sources. Skip on routine steps if the person set a tight limit. |
| Adversary (tries to break the result) | 3 | Different model family from the author | Medium to high | Most likely to catch blind spots the author's model family shares. |
| Writer | 3 | Strong model | Medium | Assembles the final deliverable from accepted results only. |
| Arbiter | 2, 3 | A fixed rule applied by you, or a decision model such as Jev | n/a | The same verdicts must always give the same decision. |

## Model diversity

Reviewers from the same model family as the author share its knowledge and blind spots. In order of preference:

1. a reviewer from another model family, called through the recipe in the settings;
2. a reviewer from the same provider but a different model;
3. the same model in a fresh context that sees only the artifacts.

Checks that use no language model at all (unit tests, reruns, unit and dimension checks, citation lookups in a bibliographic database) are independent of every model. Use them wherever possible.

## Setting it up

**Claude Code and similar tools with subagents.** Spawn each role as a subagent and set its model and effort in the call or in a subagent definition file. Give each subagent only the files its brief names. Run reviewers of the same item in parallel.

**Models from another family.** An agent can use another family's model only through something it can call: a command-line tool, an API key, or an MCP tool. Record how as a call recipe in the settings, for example:

- command-line tool: `<tool> run --model <provider/model> -f {files} "{brief}"`
- API key: a request to the provider's endpoint with the key read from an environment variable or secret, the model name, and the brief and files in the body
- MCP tool: the tool's name and its model argument

Apps (desktop, phone, or web chat) cannot be called by an agent. A person can paste a brief into an app and the verdict back, but offer that only for the final review, never mid-run, because it makes the mission wait.

The same settings serve every environment, but tools and keys usually exist only where they were installed. Before each mission, test the recipe with a one-line call; if it fails (common in cloud sessions), fall back to option 2 or 3 above and say so in the report.

**Scripted runs.** Some tools can run a fixed multi-agent script deterministically. Offer this for mode 3 when the person wants it; some tools require the person to request it explicitly.

**No subagents (a plain chat).** Do the work yourself in stages. Run each review as a separate pass that rereads only the written artifacts against the review checklist. Tell the person the review was not independent.

## Decision models

A decision model returns a choice from fixed options with a probability instead of text. It suits the arbiter role and routing decisions (review depth, next subtask). Give it only structured verdicts (pass/fail per check, severity), never the author's prose, so the author cannot talk it into accepting. Do not use it to judge calculations: such models are weak at arithmetic and multi-step reasoning.
