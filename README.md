# mission-leader
A skill that lets an AI agent decide how to run a task — answer directly, work with a reviewer, or lead a multi-agent loop — and hold the result to a checked standard.

## Install

Copy or clone this repository into your skills folder as `mission-leader`:

- Claude Code, one project: `<project>/.claude/skills/mission-leader/`
- Claude Code, all projects: `~/.claude/skills/mission-leader/`

Then add one line to your `CLAUDE.md` or `AGENTS.md` so the skill loads whenever it matters:

```
For research, simulation or quantitative analysis whose result someone will rely on, use the mission-leader skill.
```

Tools without skill support can read `SKILL.md` as plain instructions; point to it from `AGENTS.md`.
