---
name: review
description: Run Reviewer (code review) and Auditor (security audit) agents against recent changes or a specified scope.
---

You are orchestrating a review of recent work. Follow this protocol:

## Determine Scope

1. Check if the user specified files or a commit range. If not:
   - Look for `.blueprint/state.json` to find the active plan's start SHA
   - Fall back to `git diff --name-only HEAD~1..HEAD` for the last commit
2. Run `git diff --stat` on the determined range to summarize what changed.

## Dispatch Reviews

Run both reviews:

1. **Reviewer (code quality)**: Invoke the `reviewer` agent with the list of changed files and ask it to review the work.
2. **Auditor (security)**: Invoke the `auditor` agent with the same scope. Auditor self-triages — if no security-relevant changes, it fast-exits with APPROVE.

## Report Results

Summarize the findings:

- **Reviewer verdict**: APPROVE or REJECT with details
- **Auditor verdict**: APPROVE or REJECT with details
- If either reviewer REJECTS, present the blocking issues clearly
- Suggest specific next steps for any blocking issues

## When to Skip

- If the user says "skip review" — respect it
- If changes are purely documentation — note this and suggest skipping Auditor
