# Templates

## Settings section (in the project's `AGENTS.md` or `CLAUDE.md`, or the organization's instructions)

```
## mission-leader settings
- Model ranking: strongest = <model> (high effort); mid = <model> (medium); cheap = <model>
- Other family: <call recipe with {brief} and {files}> | none
- Plan approval in mode 3: no | yes
```

## First-run questions

```
mission-leader: first-time set-up. I checked this environment:
- Models I can use: <list>. Other families: <found / none found>.

1. Ranking, strongest to cheapest: <proposal>. OK? (yes / your order)
2. Can I call a model from another family? a) command-line tool; b) API key; c) MCP tool; d) no, or only an app.
   For a, b, or c: the command, endpoint, or tool, and the model name.
3. Approve mission plans before work starts? (no, recommended: missions proceed and show you the plan / yes)
4. Save these answers to <project AGENTS.md or CLAUDE.md>? (yes / no, ask each time)
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
Approver: <person who approves the plan, escalations, and the final result>

## Lessons
<one line per finished mission: mode used, right in hindsight?, cost, what reviewers caught>
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

## Risks
<risk> -> guarded by <subtask>
```

## Cost ledger (`ledger.tsv`)

```
step	role	model	effort	tokens	seconds	outcome
S1	executor	<model>	medium	181481	279	accepted
S1	review-senior	<model>	high	121993	293	pass, 0 major / 5 minor
```

## Work folder layout

```
<work folder>/
  mission.md
  plan.md            (mode 3)
  ledger.tsv
  log/               results, code, and verdicts, one file per item: S1-result.md, S1.review-senior.md, ...
  deliverable/       the final result and the script that reproduces its numbers
```
