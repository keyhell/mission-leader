# Templates

## Settings section (in the project's `AGENTS.md` or `CLAUDE.md`, or the organization's instructions)

```
## mission-leader settings
- Model ranking: strongest = <model> (high effort); mid = <model> (medium); cheap = <model>
- Other family: <call recipe with {brief} and {files}> | none
- Plan approval in mode 3: no | yes
- Lessons file: <path, default mission-lessons.md at the project root>
```

## First-run questions

```
mission-leader: first-time set-up. I checked this environment:
- Models I can use: <list>. Other families: <found / none found>.

1. Ranking, strongest to cheapest: <proposal>. OK? (yes / your order)
2. Can I call a model from another family? a) command-line tool; b) API key; c) MCP tool; d) no, or only an app.
   For a, b, or c: the command, endpoint, or tool, and the model name.
3. Approve mission plans before work starts? (no, recommended: missions proceed and show you the plan / yes)
4. Keep mission lessons in <project>/mission-lessons.md and save these answers to <project AGENTS.md or CLAUDE.md>? (yes / other path outside the work folder / no, ask each time)
```

## Mission file (`mission.md`)

```
# Mission: <short name>

Goal: <one or two sentences>
Inputs: <files, data, sources>
Constraints: <tools, things not to do, any limits the person set on time, tokens, or money>
Done criteria:
1. <checkable criterion>
2. ...
Mode: <1 | 2 | 3> because <one line>
Plan approval: not required | <name>
Sign-off: not required | pending | <name>, <date>
Open questions: none | <question, recommendation> (when no person could answer)
```

## Lessons file (`mission-lessons.md`)

Append-only. One entry line at delivery; one outcome line later, when the result proves right or wrong.

```
<date> | <mission> | <task type> | mode <1|2|3> | tokens <n | unknown> | reviewers caught: <...> | hindsight: pending
<date> | <mission> | outcome | mode was <right | wrong>: <why, with evidence>
```

## Plan (`plan.md`, mode 3)

```
# Plan

## Subtasks
S1. <goal>. Inputs: <...>. Output: <files>. Done when: <criterion>.
S2. ...

## Hypotheses
H1. <statement>. Supported if <...>; refuted if <...>.

## Dependencies
S2 <- S1
S3 <- S1, S2

## Key results and pivots
S2 (key result), S3 (pivot)

## Risks
<risk> -> guarded by <subtask>
```

## Log (`log.md`)

Optional. When used, the log is one file per mission, append-only, one line per event. You write a `cost` line for every call; agents may append any type to pass information to each other. Never edit earlier lines. Write `unknown` for a value the tool does not report.

```
<date time> | <agent> | <item> | <type> | <text> | <files>
2026-11-27 14:02 | executor-1 | S2 | result | P_LH computed for 3 cases | items/S2-result.md
2026-11-27 14:09 | reviewer-senior | S2 | verdict | FAIL: 1 major (injected vs loss power) | items/S2.review-senior.md
2026-11-27 14:09 | reviewer-senior | S2 | cost | model <model>, effort high, tokens unknown, 412 s |
```

Types: `brief`, `result`, `verdict`, `decision`, `question`, `cost`.

## Work folder layout

```
<work folder>/
  mission.md
  plan.md            (mode 3)
  log.md             (optional)
  items/             briefs, restatements, results, code, and verdicts, one file per item: S1-brief.md, S1-restatement.md, S1-result.md, S1.review-senior.md, ...
  deliverable/       report.html and the script that reproduces its numbers
```
