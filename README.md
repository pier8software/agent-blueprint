# agent-blueprint

A Claude Code plugin providing multi-agent orchestration for software projects — planning, code review, security audits, and structured execution workflows.

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
| **planner** | Strategic Planner | opus | Analyzes requirements, researches the codebase, produces detailed implementation plans in `.blueprint/plans/`. Never writes code. |
| **researcher** | External Researcher | sonnet | Searches documentation, APIs, and web sources. Read-only. Cites all sources. |
| **reviewer** | Code Reviewer | opus | Reviews completed work and plans. Approval-biased — rejects only for true blockers. Read-only. |
| **auditor** | Security Auditor | opus | Audits for security vulnerabilities and spec compliance (OAuth, JWT, CORS, etc.). Skeptical-biased. Read-only. |

## Skills

| Skill | Invocation | Description |
|-------|-----------|-------------|
| **plan** | `/agent-blueprint:plan` | Orchestrates the full Plan-Review-Execute workflow: delegates to Planner, runs Reviewer/Auditor review, then hands off to start-work. |
| **start-work** | `/agent-blueprint:start-work` | Executes a plan from `.blueprint/plans/` by working through tasks sequentially, tracking progress via checkboxes. |
| **review** | `/agent-blueprint:review` | Runs Reviewer and Auditor agents against recent changes for code quality and security review. |

## Workflow

The intended workflow follows a Plan-Review-Execute cycle:

1. **Plan**: Run `/agent-blueprint:plan` to kick off the full orchestrated workflow, or invoke the `planner` agent directly for just the planning step.
2. **Review**: The plan skill automatically runs Reviewer/Auditor review. You can also run `/agent-blueprint:review` independently at any time.
3. **Execute**: Run `/agent-blueprint:start-work` to work through the plan task-by-task.
4. **Post-Review**: After execution, run `/agent-blueprint:review` again to audit the completed work.

## Structure

```
.claude-plugin/plugin.json   # Plugin manifest
agents/                      # Sub-agent definitions (.md files)
  planner.md                 # Strategic planner
  researcher.md              # External researcher
  reviewer.md                # Code reviewer
  auditor.md                 # Security auditor
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
