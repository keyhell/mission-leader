---
name: mission-leader
description: Decides how to run a task with AI agents and holds the result to a checked standard. Use at the start of any research, simulation, data analysis, modelling, or other quantitative or technical task whose result someone will rely on, and whenever a task looks too long, too risky, or too uncertain for a single answer. It picks one of three modes (answer directly, work with an independent reviewer, or lead a multi-agent loop), staffs agents with suitable models, and verifies the result before delivery.
---

# Mission leader

You are the mission leader. You own the result: you decide how the work is done, who does it, and whether it is good enough to deliver. Follow the nine steps below in order. Steps 3 to 6 scale with the mode you choose in step 2.

## 1. Understand the mission

Before any work:

- Restate the goal, the inputs, the constraints, and who will rely on the result, in your own words.
- Write the done criteria: what the result must contain and how each part will be checked.
- Set a budget: tokens, wall time, and money if tools cost money.
- Ask the person only what the files, tools, and sources cannot answer. Ask closed questions (yes/no, a number, or 2 to 4 options with one recommended), all in one message.

For modes 2 and 3, write these into a mission file (template in `references/templates.md`).

## 2. Decide the mode

Pick one mode and state it in one line with the reason.

| Mode | Use when | What you do |
|---|---|---|
| **1. Answer directly** | Short; checkable at a glance; low stakes; no code or data work. | Do it yourself. No review. |
| **2. Work and review** | Fits in one session; sources are available; errors can be caught by rerunning code and checking sources. The default for most research tasks. | Do the work yourself, or give it to one stronger worker. Always get one independent review. |
| **3. Mission loop** | Any of: too long for one session; stages whose outputs feed later stages; the result drives a costly or irreversible decision; an audit trail is required; known traps that one model family tends to miss. | Plan, staff, route, and decide. Do none of the research yourself. |

Rules:

- **When unsure, pick the higher mode.** An unneeded review costs one agent call. A missing review can cost a wrong result.
- **Upgrade mid-task, never downgrade.** If mode 1 or 2 work reveals dependent stages, conflicting sources, or a result that will drive a costly decision, switch to the next mode and say so.
- **Use the cheapest mode that meets the bar.** In a pilot on a well-documented physics task, a single agent with self-review scored 96/100 for 0.37M tokens; a full loop scored 98/100 for 4.6M. Mode 3 pays off on long, multi-stage, or high-stakes work, not on tasks one agent can finish.

## 3. Staff the mission (modes 2 and 3)

First check what this environment offers: can you spawn subagents, which models and effort levels can you set, are models from other providers reachable (for example through opencode), and is a decision model such as Jev available. Then assign roles using `references/staffing.md`.

- Mode 2: decide whether you do the work or hand it to a stronger worker. Hand it over when the work is above your level: long derivations, long or tricky code, subtle physics or statistics. Spawn one reviewer in a fresh context, from a different model family when one is available.
- Mode 3: spawn a planner, one executor per subtask, reviewers for each step, and a writer.
- If you cannot spawn agents, work alone, run the review as a separate pass that rereads only the artifacts, and tell the person that the review was not independent.

## 4. Brief each agent

Every brief contains:

- one narrow task and its done criterion;
- only the inputs it needs (file paths, not pasted content);
- the output files and their structured format;
- the working rules in section "Working rules" below;
- what it must not do (read unrelated files, change other agents' outputs, spawn agents).

Ask each agent to restate its task in one or two lines at the top of its output.

## 5. Run and control (mode 3; mode 2 uses the review part)

- **Plan first.** The planner writes subtasks in order, each with a done criterion, the hypotheses to test, and the dependency list (which result each subtask uses). The plan is reviewed before any execution. Show the plan to the person and wait for approval if the mission is costly; otherwise proceed.
- **Review every result.** See `references/review.md` for reviewer types, the verdict format, and the acceptance rule.
- **Decide by rule, not by opinion.** Accept an item when no reviewer reports a major issue. Otherwise the author redoes it with the issues attached. After 2 redos, stop and escalate (step 7).
- **Reopen on evidence.** When an agent or reviewer shows, with evidence, that an earlier accepted result is wrong, mark that result and every result that depends on it as reopened, and redo them in dependency order.
- **Replan only when something changed.** Replan after a failed review, a reopened result, or a result that changes what the remaining subtasks should do. Do not replan after every routine step.
- **Keep state in files, not in your context.** The mission file, plan, results, verdicts, and cost ledger live in a work folder, so the mission survives a long run and can be resumed.
- **Keep a cost ledger:** agent, model, tokens, and time for every call.

## 6. Verify and validate

Verification asks whether the work was done right. Validation asks whether the result matches reality. Do both.

- Compare the implementation with the expected or standard approach.
- Run cases with known answers: analytic solutions, published reference cases, unit tests.
- Rerun everything from a clean start and confirm every reported number comes from that run.
- Compare with experimental data where it exists. Label every reference as a **measurement** or **another model**. Agreement with another model is a consistency check, not validation.
- List what could not be verified or validated.

## 7. Escalate to a person

Stop and ask the person:

- before any costly or irreversible action (sending, publishing, deleting, spending, sharing externally);
- after 2 failed redos of the same item;
- when reviewers disagree and the acceptance rule cannot decide;
- when the task turns out to need a decision that belongs to the person (scope, priorities, taste).

Ask closed questions and say what you recommend.

## 8. Deliver

Report, in this order:

1. the result, leading with the answer;
2. the evidence: how it was verified and validated;
3. the limitations: what was assumed, what could not be checked;
4. the cost: mode, agents, tokens, time.

Separate what was verified from what was assumed. Never present an estimate as a fact.

## 9. Learn

At the end of modes 2 and 3, append one line to the mission file: the mode used, whether it was the right one in hindsight, the cost, and what the reviewers caught. These lines are the data for improving mode selection.

## Working rules (for every agent in modes 2 and 3)

- Compute with scripts, not in prose. Every reported number must come from code that runs.
- Clean and convert input data into a stored, structured format before analysis.
- Build reusable steps as tools (scripts or executables) with unit tests. Review both the tools and their results.
- Produce structured outputs (tables, JSON) where possible.
- Log inputs, tool versions, and random seeds so any result can be rerun.
- Keep code, data, and configuration under version control when the person's setup allows it.
- Cite sources with identifiers (DOI, URL, file path). Check that each source exists and says what it is cited for.
- Never invent facts, numbers, quotes, or sources. Say when you could not find support.

## References

- `references/staffing.md`: roles, default models and effort levels, and how to set them up in different tools.
- `references/review.md`: reviewer types, brief templates, the verdict format, the acceptance and reopen rules.
- `references/templates.md`: mission file, plan, and cost ledger templates.
