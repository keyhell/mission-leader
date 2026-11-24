# mission-leader

[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
![Agent Skill](https://img.shields.io/badge/agent-skill-red)
![Works with](https://img.shields.io/badge/works%20with-Claude%20Code%20%C2%B7%20opencode%20%C2%B7%20Codex%20%C2%B7%20Cursor%20%C2%B7%2075%2B%20more-yellow)
[![Install](https://img.shields.io/badge/npx-skills%20add%20keyhell%2Fmission--leader-blue)](#install)

A skill that lets an AI agent decide how to run a task and hold the result to a checked standard. For each task it picks one of three modes:

1. **Answer directly.** For short, low-stakes tasks that can be checked at a glance. The agent does the work itself, with no review.
2. **Work and review.** The default for most research and analysis. The agent does the work itself or gives it to a stronger model, and an independent reviewer checks the result.
3. **Mission loop.** For long, multi-stage, or high-stakes work. The agent plans the mission, runs subtasks in order of their impact on the result with a worker for each, has every step reviewed, and reopens earlier results when later work shows they were wrong.

```mermaid
flowchart LR
  subgraph M1["Mode 1 · Answer directly"]
    direction TB
    P1(["Person<br/>task"]) --> L1["Leader<br/>does the work"] --> R1(["Result"])
  end
  subgraph M2["Mode 2 · Work and review"]
    direction TB
    P2(["Person<br/>task"]) --> L2["Leader<br/>decides, staffs"] --> D2["Doer<br/>leader or stronger model"] --> V2["Reviewer<br/>fresh context"] --> R2(["Result"])
    V2 -. redo .-> D2
    O2["Reviewer<br/>other model family"] -.- V2
  end
  subgraph M3["Mode 3 · Mission loop"]
    direction TB
    P3(["Person<br/>task + start questions"]) --> L3["Leader<br/>routes, applies rules"] --> PL["Planner<br/>subtasks ranked by impact<br/>on the result"] --> VP["Reviewer<br/>plan"]
    VP --> EX["Executors<br/>one per subtask"] --> VE["Reviewers<br/>every step"] --> LOG[("Log<br/>results, verdicts, dependencies")]
    VE -. redo .-> EX
    LOG -. reopen .-> EX
    A3["Adversary<br/>key steps"] -.- VE
    LOG --> W["Writer<br/>final report"] --> VF["Reviewers + adversary<br/>final report"] --> R3(["Result"]) --> S3(["Person<br/>sign-off"])
    L3 -. escalate .-> P3
  end
  M1 ~~~ M2 ~~~ M3
  classDef leader fill:#1d4ed8,stroke:#1e3a8a,color:#ffffff
  classDef doer fill:#16a34a,stroke:#14532d,color:#ffffff
  classDef leaderdoer fill:#16a34a,stroke:#1d4ed8,stroke-width:4px,color:#ffffff
  classDef reviewer fill:#f59e0b,stroke:#b45309,color:#111111
  classDef adversary fill:#991b1b,stroke:#450a0a,color:#ffffff
  classDef person fill:#7c3aed,stroke:#4c1d95,color:#ffffff
  classDef infra fill:#1f2937,stroke:#9ca3af,stroke-width:2px,color:#ffffff
  classDef optional fill:#f59e0b,stroke:#6b7280,stroke-width:2px,stroke-dasharray:5 4,color:#111111
  classDef optadv fill:#991b1b,stroke:#6b7280,stroke-width:2px,stroke-dasharray:5 4,color:#ffffff
  class L2,L3 leader
  class L1 leaderdoer
  class D2,PL,EX,W doer
  class V2,VP,VE,VF reviewer
  class P1,P2,P3,S3 person
  class R1,R2,R3,LOG infra
  class O2 optional
  class A3 optadv
```

Blue: leader, which decides and routes. Green: doers. Orange: reviewers. Red: adversary. Purple: the person. Dark: results and the shared log. Dashed boxes are optional; dashed arrows send work back.

## Install

1. Install the skill:

   - **With Node.js** 22.20 or later, run in your project:

     ```
     npx skills add keyhell/mission-leader
     ```

     Add `-g` to install it for all your projects, and `-a <agent>` to choose the tool (for example `-a claude-code`, `-a opencode`, `-a codex`, `-a cursor`). The installer is the open [skills CLI](https://github.com/vercel-labs/skills).

   - **Without Node.js**, copy this repository into your skills folder as `mission-leader`. For Claude Code:

     - one project: `<project>/.claude/skills/mission-leader/`
     - all your projects: `~/.claude/skills/mission-leader/`

     For example: `git clone https://github.com/keyhell/mission-leader ~/.claude/skills/mission-leader`

2. Add one line to your `CLAUDE.md` or `AGENTS.md`, so the skill loads whenever it matters:

   ```
   For research, simulation or quantitative analysis whose result someone will rely on, use the mission-leader skill.
   ```

3. Start a task. The first time the skill runs, it asks a few set-up questions (which models to use and how to reach models from other providers) and, with your OK, saves the answers in that same file.

## Credit and contact

You can use mission-leader for anything, commercial or private, under the [MIT license](LICENSE). If you do, please mention this repository and its author, Alexei 'keyhell' Zhurba ([github.com/keyhell/mission-leader](https://github.com/keyhell/mission-leader)).

If you need help, or want to share how you use it, [open an issue](https://github.com/keyhell/mission-leader/issues). I'd like to hear about your case.
