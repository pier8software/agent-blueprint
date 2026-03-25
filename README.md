# agent-blueprint

A Claude Code plugin providing a collection of reusable agents, skills, and workflows for software projects.

## Installation

```bash
# Install to your project
claude plugin install agent-blueprint

# Or load during development
claude --plugin-dir /path/to/agent-blueprint
```

## Structure

```
.claude-plugin/plugin.json   # Plugin manifest
agents/                      # Sub-agent definitions (.md files)
skills/                      # Skill definitions (directories with SKILL.md)
scripts/                     # TypeScript hook/utility scripts
```

## Adding an Agent

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

## Adding a Skill

Create a new directory in `skills/` with a `SKILL.md`:

```markdown
---
name: my-skill
description: When and why Claude should use this skill.
---

Instructions for the skill go here.
```

Skills are invoked with `/agent-blueprint:<skill-name>`.

## Development

```bash
npm install          # Install TypeScript tooling
```

Hook scripts go in `scripts/` and are written in TypeScript.
