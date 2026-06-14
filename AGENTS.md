# Devin Superpowers โ€” Global Rules

These rules guide all software development work. The agent MUST apply the relevant section based on the current task.

---

## 1. Brainstorming (Before ANY creative work)

- Do NOT write code before presenting a design and getting user approval.
- Ask ONE question at a time. Prefer multiple-choice questions.
- Propose 2-3 approaches with trade-offs.
- Get approval after EACH design section.
- Write spec to a design doc and commit before coding.

---

## 2. Test-Driven Development (Before ANY implementation)

- **Iron Law:** NO production code without a failing test first.
- RED โ’ Write minimal failing test. Verify it fails correctly.
- GREEN โ’ Write simplest code to pass.
- REFACTOR โ’ Clean up. Keep tests green.
- If code was written before the test, DELETE it and start over.

---

## 3. Systematic Debugging (Before ANY fix)

- **Iron Law:** NO fixes without root cause investigation.
- Phase 1: Root Cause โ€” reproduce, trace data flow backward.
- Phase 2: Pattern โ€” find working examples, compare.
- Phase 3: Hypothesis โ€” one theory, smallest test.
- Phase 4: Implementation โ€” failing test first, one fix, verify.
- If 3+ fixes fail, STOP and question architecture.

---

## 4. Subagent-Driven Development (Multi-task plans)

- Dispatch fresh subagent per task with isolated context.
- Two-stage review after each task: spec compliance โ’ code quality.
- Do NOT pause between tasks for user confirmation.
- Only stop when BLOCKED, ambiguous, or all tasks complete.

---

## 5. Dispatching Parallel Agents (2+ independent problems)

- Group failures by independent domain.
- One agent per domain. Run concurrently.
- Review summaries, check conflicts, run full suite, integrate.

---

## 6. Requesting Code Review (After major work)

- Mandatory: after each task, after major features, before merge.
- Dispatch reviewer with: description, plan, commit range.
- Critical = fix immediately. Important = fix before proceeding. Minor = note for later.

---

## 7. Receiving Code Review (When getting feedback)

- Verify before implementing. Technical correctness over social comfort.
- READ โ’ UNDERSTAND โ’ VERIFY โ’ EVALUATE โ’ RESPOND โ’ IMPLEMENT.
- Forbidden: "You're absolutely right!", "Great point!", blind implementation.
- Push back when suggestion breaks functionality or violates YAGNI.

---

## 8. Executing Plans (Following a written plan)

- Review plan critically first. Raise concerns before starting.
- Execute tasks one by one. Do NOT skip verifications.
- When blocked: STOP and ask. Do NOT guess.

---

## 9. Finishing a Development Branch (Work complete)

- Verify ALL tests pass before presenting options.
- Options: Merge locally / Push & PR / Keep as-is / Discard.
- Clean up worktree ONLY for merge and discard.

---

## 10. Using Git Worktrees (Starting feature work)

- Detect existing isolation first.
- Prefer native worktree tools. Fall back to git worktree add.
- Verify clean baseline before proceeding.
