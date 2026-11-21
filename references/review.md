# Review

## Reviewer types

| Type | Sees | Checks |
|---|---|---|
| Senior reviewer | The item and the whole work log: mission file, plan, earlier results, earlier verdicts. | Whether the item meets its done criterion; consistency with earlier results; whether it exposes an error in an earlier accepted result. |
| Specialist reviewer | The item only, plus the definition of its subtask. | Formulas, units, definitions, numbers (by rerunning the code and recomputing at least one number independently), and whether each cited source exists and says what is claimed. |
| Adversary | The item and the mission file. | Tries to break the result: other explanations, counterexamples, edge cases, a quick test run. Reports only what it could break, with evidence. |

Mode 2 uses one reviewer: the senior type, which also does the specialist checks. Mode 3 uses the senior and specialist reviewers on every result, and the adversary on key results, pivots, and the final deliverable. Plans and replans get the senior reviewer only.

No reviewer sees the author's reasoning beyond what is written in the item.

## Reviewer brief

```
You are the <TYPE> REVIEWER of item <ID> in a mission. You did not write it and you see only what is written.

Read: <mission file>, <review rules>, <item files>[, the whole work log].
Check: <type-specific checks from the table above>. Run any code and confirm the item's numbers.
Write your verdict to <path> in the verdict format.
Tag [major] only what would make the final answer wrong or incomplete.
Rules: read only the files named here; do not edit any other file; do not spawn agents.
Reply with only the verdict line, the counts of major and minor issues, and whether you raised a challenge.
```

## Verdict format

```
VERDICT: PASS | FAIL
ISSUES:
1. [major|minor] <what is wrong>. Evidence: <how you know>. Fix: <what would resolve it>.
CHALLENGE: none | <earlier item ID>: <what is wrong>, <evidence>
```

## Acceptance rule

- Accept the item when no reviewer reports a [major] issue.
- Otherwise the author redoes the item with all verdicts attached, and the same reviewers check it again.
- After 2 redos, stop and escalate to the person with the open issues.
- Forward [minor] issues to the next steps and to the writer, who must address them or list them as limitations.

## Reopen rule

A challenge with evidence goes to the senior reviewer. If it holds, reopen the challenged result and every result that depends on it, using the plan's dependency list. Redo them in dependency order. If the same result is reopened more than twice, record it as unresolved and report it as a limitation.
