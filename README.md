# agent-blueprint

A Claude Code plugin providing multi-agent orchestration for software projects — planning, code review, security audits, and structured execution workflows. Inspired by [Weave](https://github.com/opencode-ai/weave).

## Installation

```bash
# Install to your project
claude plugin install agent-blueprint

# Or load during development
claude --plugin-dir /path/to/agent-blueprint
```

## Agents

| Agent | Role | Model | Description |
|-------|------|-------|-------------|
| **pattern** | Strategic Planner | opus | Analyzes requirements, researches the codebase, produces detailed implementation plans in `.weave/plans/`. Never writes code. |
| **spindle** | External Researcher | sonnet | Searches documentation, APIs, and web sources. Read-only. Cites all sources. |
| **weft** | Code Reviewer | opus | Reviews completed work and plans. Approval-biased — rejects only for true blockers. Read-only. |
| **warp** | Security Auditor | opus | Audits for security vulnerabilities and spec compliance (OAuth, JWT, CORS, etc.). Skeptical-biased. Read-only. |

## Skills

| Skill | Invocation | Description |
|-------|-----------|-------------|
| **plan** | `/agent-blueprint:plan` | Orchestrates the full Plan-Review-Execute workflow: delegates to Pattern, runs Weft/Warp review, then hands off to start-work. |
| **start-work** | `/agent-blueprint:start-work` | Executes a plan from `.weave/plans/` by working through tasks sequentially, tracking progress via checkboxes. |
| **review** | `/agent-blueprint:review` | Runs Weft and Warp agents against recent changes for code quality and security review. |

## Workflow

The intended workflow mirrors Weave's Plan-Review-Execute cycle:

1. **Plan**: Run `/agent-blueprint:plan` to kick off the full orchestrated workflow, or invoke the `pattern` agent directly for just the planning step.
2. **Review**: The plan skill automatically runs Weft/Warp review. You can also run `/agent-blueprint:review` independently at any time.
3. **Execute**: Run `/agent-blueprint:start-work` to work through the plan task-by-task.
4. **Post-Review**: After execution, run `/agent-blueprint:review` again to audit the completed work.

## Structure

```
.claude-plugin/plugin.json   # Plugin manifest
agents/                      # Sub-agent definitions (.md files)
  pattern.md                 # Strategic planner
  spindle.md                 # External researcher
  weft.md                    # Code reviewer
  warp.md                    # Security auditor
skills/                      # Skill definitions (directories with SKILL.md)
  plan/SKILL.md              # Full Plan-Review-Execute orchestration
  start-work/SKILL.md        # Plan execution workflow
  review/SKILL.md            # Review orchestration
scripts/                     # TypeScript hook/utility scripts
```

## Adding Custom Agents

Create a new `.md` file in `agents/`:

```markdown
---
name: my-agent
description: When Claude should use this agent.
model: sonnet
maxTurns: 20
---

System prompt for the agent goes here.
```

## Adding Custom Skills

Create a new directory in `skills/` with a `SKILL.md`:

```markdown
---
name: my-skill
description: When and why Claude should use this skill.
---

Instructions for the skill go here.
```

Skills are invoked with `/agent-blueprint:<skill-name>`.
