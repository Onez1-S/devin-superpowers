---
name: subagent-driven-development
description: Use when executing implementation plans with independent tasks in the current session
---

# Subagent-Driven Development

Execute plan by dispatching fresh subagent per task, with two-stage review after each: spec compliance review first, then code quality review.

**Core principle:** Fresh subagent per task + two-stage review (spec then quality) = high quality, fast iteration

**Continuous execution:** Do not pause to check in between tasks. Execute all tasks from the plan without stopping. The only reasons to stop are: BLOCKED status you cannot resolve, ambiguity that genuinely prevents progress, or all tasks complete.

## When to Use

**vs. Executing Plans (parallel session):**
- Same session (no context switch)
- Fresh subagent per task (no context pollution)
- Two-stage review after each task
- Faster iteration (no human-in-loop between tasks)

**Use when:**
- You have an implementation plan
- Tasks are mostly independent
- You want to stay in the current session

## The Process

### Per Task Loop:

1. **Dispatch implementer subagent** with full task text + context
2. **If subagent asks questions:** Answer questions, provide context, re-dispatch
3. **Implementer implements, tests, commits, self-reviews**
4. **Dispatch spec reviewer subagent** โ€” confirms code matches spec
5. **If spec gaps:** Implementer fixes, re-review
6. **Dispatch code quality reviewer subagent** โ€” approves or rejects
7. **If quality issues:** Implementer fixes, re-review
8. **Mark task complete in TodoWrite**

### After All Tasks:

1. Dispatch final code reviewer subagent for entire implementation
2. Use `finishing-a-development-branch` skill

## Model Selection

Use the least powerful model that can handle each role:

- **Mechanical implementation** (isolated functions, clear specs, 1-2 files): cheap/fast model
- **Integration tasks** (multi-file, pattern matching, debugging): standard model
- **Architecture, design, review**: most capable model

## Handling Implementer Status

**DONE:** Proceed to spec compliance review.

**DONE_WITH_CONCERNS:** Read the concerns. If about correctness/scope, address before review. If observations only, proceed to review.

**NEEDS_CONTEXT:** Provide missing context and re-dispatch.

**BLOCKED:** Assess the blocker:
1. Context problem โ’ provide more context, re-dispatch same model
2. Needs more reasoning โ’ re-dispatch with more capable model
3. Task too large โ’ break into smaller pieces
4. Plan is wrong โ’ escalate to human

**Never** ignore an escalation or force the same model to retry without changes.

## Advantages vs. Manual Execution

- Subagents follow TDD naturally
- Fresh context per task (no confusion)
- Parallel-safe (subagents don't interfere)
- Subagent can ask questions before AND during work
- Review checkpoints automatic
- No "should I continue?" prompts wasting time
