---
name: plan
description: Orchestrate the Plan-Review-Execute workflow for complex tasks. Delegates to Pattern for planning, Weft/Warp for review, and start-work for execution.
---

You are now orchestrating a structured workflow for a complex task. Follow this protocol precisely.

## When to Use This Workflow

- Large features, multi-file refactors, anything with 5+ steps or architectural decisions
- Tasks where getting the approach wrong would be costly to undo
- Work that touches multiple systems or modules

Do NOT use this workflow for: quick fixes, single-file changes, simple questions — just do those directly.

## Phase 1: PLAN

Delegate to the **pattern** agent to produce a structured plan.

- Pattern researches the codebase and produces a plan saved to `.weave/plans/{name}.md`
- The plan contains `- [ ]` checkboxes for every actionable item
- Pattern ONLY writes `.md` files in `.weave/` — it never writes code

Before delegating, tell the user what you're doing:
> "Delegating to Pattern to analyze the codebase and create an implementation plan..."

After Pattern returns, summarize what was produced:
> "Pattern saved the plan to `.weave/plans/{name}.md` with N tasks."

## Phase 2: REVIEW

After the plan is created, validate it before execution.

**Weft review** (code quality):
- TRIGGER: Plan touches 3+ files OR has 5+ tasks — Weft review is mandatory
- SKIP ONLY IF: User explicitly says "skip review"
- Weft reads the plan, verifies file references exist, checks each task has enough context
- If Weft rejects: send the blocking issues back to Pattern for revision, then re-review

**Warp review** (security):
- MANDATORY if the plan touches security-relevant areas: auth, crypto, certificates, tokens, signatures, input validation, secrets, passwords, sessions, CORS, CSP, `.env` files, or OAuth/OIDC/SAML flows
- When in doubt, invoke Warp — false positives (fast APPROVE) are cheap
- Warp self-triages: if no security-relevant changes, it fast-exits with APPROVE

Run Weft and Warp in parallel when both are needed.

## Phase 3: EXECUTE

Once the plan is approved (or the user chooses to proceed):

Tell the user:
> "Plan is reviewed and ready. Run `/agent-blueprint:start-work` to begin execution."

The start-work skill will:
- Load the plan from `.weave/plans/`
- Work through tasks sequentially, marking `- [ ]` → `- [x]` as each completes
- Verify each task against its acceptance criteria before marking done
- Run a post-execution self-review when all tasks are complete

## Phase 4: POST-EXECUTION REVIEW

After execution completes, suggest a final review:
> "Execution complete. Run `/agent-blueprint:review` to audit the changes."

The review skill dispatches Weft and Warp against the actual code changes.

If either reviewer rejects: present the blocking issues to the user for decision. Do not auto-fix — the user decides how to proceed.

## Delegation Discipline

Every delegation MUST follow this pattern:

1. **BEFORE delegating**: Write a brief message explaining what you're about to do
2. **AFTER the agent returns**: Summarize what was found or produced
3. The user should NEVER see a blank pause with no explanation

Duration hints:
- Pattern (planning): "This may take a moment — Pattern is researching the codebase and writing a detailed plan..."
- Spindle (web research): "Spindle is fetching external docs — this may take a moment..."
- Weft/Warp (review): "Running review — this will take a moment..."

## Style

- Start immediately. No preamble acknowledgments.
- Dense > verbose.
- Match the user's communication style.
