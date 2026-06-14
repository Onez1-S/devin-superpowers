---
name: superpowers
description: "Master skill that activates all 10 development workflow skills at once. Use at the start of any development session."
triggers:
  - user
  - model
---

# Superpowers โ€” Master Development Workflow

This skill activates all 10 superpowers workflow guidelines simultaneously. The agent MUST apply the relevant sub-skill based on the current task context.

## Always-Active Rules

### 1. Brainstorming (Before creative work)
Do NOT code before design approval. Ask one question at a time. Propose 2-3 approaches.

### 2. Test-Driven Development (Before implementation)
NO production code without a failing test first. Red โ’ Green โ’ Refactor.

### 3. Systematic Debugging (Before any fix)
NO fixes without root cause investigation. 4 phases: Root Cause โ’ Pattern โ’ Hypothesis โ’ Implementation.

### 4. Subagent-Driven Development (Multi-task plans)
Fresh subagent per task. Two-stage review: spec compliance โ’ code quality.

### 5. Parallel Agents (2+ independent problems)
One agent per domain. Run concurrently. Review and integrate.

### 6. Request Code Review (After major work)
After each task, before merge. Fix critical immediately.

### 7. Receive Code Review (When getting feedback)
Verify before implementing. No performative agreement.

### 8. Execute Plans (Following written plans)
Review first. Execute step-by-step. Don't skip verifications.

### 9. Finish Branch (Work complete)
Verify tests โ’ present 4 options โ’ execute โ’ cleanup.

### 10. Git Worktrees (Starting feature work)
Detect isolation first. Prefer native tools. Verify clean baseline.

## Auto-Detection Guide

| Task Type | Active Sub-Skill |
|-----------|------------------|
| Designing a feature | Brainstorming |
| Writing new code | TDD |
| Fixing a bug | Systematic Debugging + TDD |
| Multiple failing tests | Parallel Agents |
| Following a plan | Executing Plans |
| Multi-task implementation | Subagent-Driven Development |
| Ready to merge | Requesting Code Review โ’ Finishing Branch |
| Getting feedback | Receiving Code Review |
| Starting new feature | Git Worktrees |
