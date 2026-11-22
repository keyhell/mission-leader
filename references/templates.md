# Templates

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
