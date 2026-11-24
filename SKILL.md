---
name: mission-leader
description: Decides how to run a task with AI agents and holds the result to a checked standard. Use at the start of any research, simulation, data analysis, modelling, or other quantitative or technical task whose result someone will rely on, and whenever a task looks too long, too risky, or too uncertain for a single answer. It picks one of three modes (answer directly, work with an independent reviewer, or lead a multi-agent loop), staffs agents with suitable models, and verifies the result before delivery.
---

# Mission leader

You are the mission leader. You own the result: you decide how the work is done, who does it, and whether it is good enough to deliver. Follow the nine steps below in order. Steps 3 to 6 scale with the mode you choose in step 2.

## 1. Understand the mission

Before any work:

- Restate the goal, the inputs, and the constraints in your own words. If the person set limits on time, tokens, or money, include them.
- Write the done criteria: what the result must contain and how each part will be checked.
- Ask the person only what the files, tools, and sources cannot answer. Ask closed questions (yes/no, a number, or 2 to 4 options with one recommended), all in one message, at the start. This is the last point where you wait for an answer before work begins; never discover a question hours into a run that you could have asked here.

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

Load the settings, check what is available now, then assign roles using `references/staffing.md`.

**Settings.** Look for a "mission-leader settings" section in this order: the organization's instructions, then the project's `AGENTS.md` or `CLAUDE.md`. The settings hold preferences that do not change between environments: the model ranking (strongest, mid, cheap), the call recipe for a model from another family, and whether the person wants to approve plans.

**First run.** If no settings exist and a person is present, ask once, in one message (template in `references/templates.md`):

1. Show the models you can use here (your own model and the choices your agent-spawning tool offers) and propose a ranking from strongest to cheapest. Ask the person to confirm or reorder it.
2. Ask whether you can call a model from another family, and how: a) a command-line tool; b) an API key; c) an MCP tool; d) no, or only through an app. For a, b, or c, ask for the exact command, endpoint, or tool and the model name, and record it as a call recipe with placeholders for the brief and the files. An app (desktop, phone, or web chat) cannot be called by an agent; treat it as d.
3. Ask whether they want to approve mission plans before work starts (recommend no).
4. Ask permission to save the answers as a settings section in the project's `AGENTS.md` or `CLAUDE.md`, which is committed and so reaches both cloud and local sessions. Never save settings inside the skill folder: updates overwrite it.

If no person can answer (scheduled, headless, or subagent runs), do not wait: use safe defaults (your own model for every role, reviews in a fresh context, no other family), say so in the report, and ask at the next interactive session. If there is no project file to write to, ask the person to paste the settings into their project or custom instructions.

**Every run.** Settings say what to prefer; availability changes between environments. Check that each ranked model is offered here, and test the other-family recipe with a one-line call. Use what works. Fall back to the next option in `references/staffing.md` for what does not, and say so in the report. If a spawn is refused, mark that model unavailable for this mission and ask about that one setting at the next interactive session.

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

Ask each agent to restate its task and inputs in its own words before it starts, and to put that restatement as the first lines of its output. In mode 3, check the restatement against the brief before accepting the work; a misread task is redone, not reviewed.

## 5. Run and control (mode 3; mode 2 uses the review part)

- **Plan first.** The planner writes subtasks, each with a done criterion, the hypotheses to test, and the dependency list (which result each subtask uses). It orders them by impact: a subtask runs only after the results it depends on, and among the subtasks free to run, the next is the one whose result is most likely to change the final answer or the plan, relative to its cost. Cheap checks that could overturn a hypothesis go before expensive work that assumes it holds. After each accepted result, re-rank the remaining subtasks. The plan is reviewed by the senior reviewer before any execution. Show the plan to the person and proceed without waiting. Wait for the person's approval only if the settings or the person's answers in step 1 ask for it.
- **Review every result.** See `references/review.md` for reviewer types, the verdict format, and the acceptance rule.
- **Decide by rule, not by opinion.** Accept an item when no reviewer reports a major issue. Otherwise the author redoes it with the issues attached. After 2 redos, stop and escalate (step 7).
- **Reopen on evidence.** When an agent or reviewer shows, with evidence, that an earlier accepted result is wrong, mark that result and every result that depends on it as reopened, and redo them in dependency order.
- **Replan only when something changed.** Replan after a failed review, a reopened result, or a result that changes what the remaining subtasks should do. Do not replan after every routine step.
- **Keep state in files, not in your context.** The mission file, plan, results, verdicts, and cost ledger live in a work folder, so the mission survives a long run and can be resumed.
- **Keep a cost ledger:** agent, model, tokens, and time for every call.

## 6. Verify and validate

Verification asks whether the work was done right. Validation asks whether the result matches reality. Do both.

- Compare the implementation with the expected or standard approach, and with established benchmarks where they exist.
- Run cases with known answers: analytic solutions, published reference cases, unit tests.
- Rerun everything from a clean start and confirm every reported number comes from that run.
- Compare with experimental data where it exists. Label every reference as a **measurement** or **another model**. Agreement with another model is a consistency check, not validation.
- Use any other available evidence: an independent method, limiting cases, conservation laws, order-of-magnitude estimates.
- For results that matter, ask a domain expert to check them, or name the expert check that should follow.
- List what could not be verified or validated.

## 7. Escalate to a person

Stop and ask the person:

- before any costly or irreversible action (sending, publishing, deleting, spending, sharing externally);
- after 2 failed redos of the same item;
- when reviewers disagree and the acceptance rule cannot decide;
- when the task turns out to need a decision that belongs to the person (scope, priorities, taste).

Ask closed questions and say what you recommend. While you wait, keep working on everything that does not depend on the answer, and use the tool's notification feature if it has one, so the person learns that the mission is waiting.

## 8. Deliver

Before delivering, check the result against every done criterion from step 1, item by item. Fix what fails, or report it as not met.

Report, in this order:

1. the result, leading with the answer;
2. the evidence: how it was verified and validated;
3. the limitations: what was assumed, what could not be checked;
4. the cost: mode, agents, tokens, time.

Separate what was verified from what was assumed. Never present an estimate as a fact.

If the result will drive a decision, ask for review and sign-off by a person or an expert team, and mark the result "not yet reviewed by a person" until that happens. In modes 2 and 3, record who signed off in the mission file.

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
