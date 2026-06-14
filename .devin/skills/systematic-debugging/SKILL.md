---
name: systematic-debugging
description: Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes
---

# Systematic Debugging

## Overview

Random fixes waste time and create new bugs. Quick patches mask underlying issues.

**Core principle:** ALWAYS find root cause before attempting fixes. Symptom fixes are failure.

## The Iron Law

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

If you haven't completed Phase 1, you cannot propose fixes.

## When to Use

Use for ANY technical issue:
- Test failures
- Bugs in production
- Unexpected behavior
- Performance problems
- Build failures
- Integration issues

**Use this ESPECIALLY when:**
- Under time pressure (emergencies make guessing tempting)
- "Just one quick fix" seems obvious
- You've already tried multiple fixes
- Previous fix didn't work

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Root Cause Investigation

**BEFORE attempting ANY fix:**

1. **Read Error Messages Carefully** โ€” Don't skip past errors or warnings. Read stack traces completely.

2. **Reproduce Consistently** โ€” Can you trigger it reliably? What are the exact steps?

3. **Check Recent Changes** โ€” What changed that could cause this? Git diff, recent commits.

4. **Gather Evidence in Multi-Component Systems** โ€” Add diagnostic instrumentation at each component boundary. Log what data enters and exits each component. Run once to gather evidence, THEN analyze.

5. **Trace Data Flow** โ€” Where does bad value originate? What called this with bad value? Keep tracing up until you find the source. Fix at source, not at symptom.

### Phase 2: Pattern Analysis

1. **Find Working Examples** โ€” Locate similar working code in same codebase
2. **Compare Against References** โ€” Read reference implementation COMPLETELY, don't skim
3. **Identify Differences** โ€” List every difference, however small
4. **Understand Dependencies** โ€” What other components does this need?

### Phase 3: Hypothesis and Testing

1. **Form Single Hypothesis** โ€” State clearly: "I think X is the root cause because Y"
2. **Test Minimally** โ€” Make the SMALLEST possible change to test hypothesis
3. **Verify Before Continuing** โ€” Did it work? Yes โ’ Phase 4. No โ’ Form NEW hypothesis
4. **When You Don't Know** โ€” Say "I don't understand X". Don't pretend to know.

### Phase 4: Implementation

1. **Create Failing Test Case** โ€” Simplest possible reproduction. MUST have before fixing.
2. **Implement Single Fix** โ€” Address the root cause identified. ONE change at a time.
3. **Verify Fix** โ€” Test passes? No other tests broken?
4. **If Fix Doesn't Work** โ€” STOP. If < 3 tries: Return to Phase 1. **If โฅ 3 tries: Question the architecture.**

### If 3+ Fixes Failed: Question Architecture

Pattern indicating architectural problem:
- Each fix reveals new shared state/coupling in a different place
- Fixes require "massive refactoring"
- Each fix creates new symptoms elsewhere

**Stop and discuss with your human partner before attempting more fixes.**

## Red Flags โ€” STOP and Follow Process

If you catch yourself thinking:
- "Quick fix for now, investigate later"
- "Just try changing X and see if it works"
- "Add multiple changes, run tests"
- "It's probably X, let me fix that"
- "I don't fully understand but this might work"
- "One more fix attempt" (when already tried 2+)

**ALL of these mean: STOP. Return to Phase 1.**

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Issue is simple, don't need process" | Simple issues have root causes too. |
| "Emergency, no time for process" | Systematic is FASTER than thrashing. |
| "Just try this first, then investigate" | Do it right from the start. |
| "Multiple fixes at once saves time" | Can't isolate what worked. Causes new bugs. |
| "One more fix attempt" (after 2+) | 3+ failures = architectural problem. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Root Cause** | Read errors, reproduce, check changes, gather evidence | Understand WHAT and WHY |
| **2. Pattern** | Find working examples, compare | Identify differences |
| **3. Hypothesis** | Form theory, test minimally | Confirmed or new hypothesis |
| **4. Implementation** | Create test, fix, verify | Bug resolved, tests pass |

## Real-World Impact

- Systematic approach: 15-30 minutes to fix
- Random fixes approach: 2-3 hours of thrashing
- First-time fix rate: 95% vs 40%
- New bugs introduced: Near zero vs common
