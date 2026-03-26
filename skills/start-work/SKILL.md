---
name: start-work
description: Execute a plan from .weave/plans/ by working through tasks sequentially, tracking progress via checkboxes.
---

You are now in **execution mode**. Your job is to work through a plan file task-by-task.

## Startup

1. **Find the plan**: Look in `.weave/plans/` for the most recent `.md` file (or the one specified by the user).
2. **Read the plan**: Understand the full scope before starting.
3. **Find your place**: Locate the first unchecked `- [ ]` task. If resuming, skip already-checked `- [x]` tasks.
4. **Record start point**: Run `git rev-parse HEAD` and note the SHA for later review.

## Execution Loop

For each unchecked `- [ ]` task:

1. **Read** the task description, files, and acceptance criteria
2. **Execute** the work (write code, run commands, create files)
3. **Verify** before marking complete:
   - Read every file you modified — confirm correctness
   - Re-read the task's acceptance criteria from the plan
   - Verify EACH criterion is met — exactly, not approximately
   - If any criterion is unmet: fix it, then re-verify
4. **Mark complete**: Edit the plan file to change `- [ ]` to `- [x]`
5. **Report**: "Completed task N/M: [title]"
6. **Continue** to the next unchecked task

NEVER stop mid-plan unless explicitly told to or completely blocked.

## When Blocked

- Document the reason clearly
- Move to the next unblocked task if possible
- Report the blocker to the user

## Post-Execution Review

After ALL plan tasks are checked off:

1. **Identify all changed files**: Run `git diff --name-only <start-sha>..HEAD`
2. **Self-review**: Read every changed file. Check for stubs, TODOs, placeholders, hardcoded values.
3. **Report summary**: List all completed tasks, changed files, and any issues found.
4. **Suggest review**: Tell the user they can run the `weft` or `warp` agents for formal review.

## Style

- Terse status updates only
- No meta-commentary
- Dense > verbose
- Work through tasks top to bottom
- Report completion with evidence (test output, file paths, commands run)
