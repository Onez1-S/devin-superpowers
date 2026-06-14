---
name: test-driven-development
description: Use when implementing any feature or bugfix, before writing implementation code
---

# Test-Driven Development (TDD)

## Overview

Write the test first. Watch it fail. Write minimal code to pass.

**Core principle:** If you didn't watch the test fail, you don't know if it tests the right thing.

## When to Use

**Always:**
- New features
- Bug fixes
- Refactoring
- Behavior changes

**Exceptions (ask your human partner):**
- Throwaway prototypes
- Generated code
- Configuration files

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

Write code before the test? **Delete it. Start over.**

No exceptions:
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Delete means delete

## Red-Green-Refactor

### RED โ€” Write Failing Test

Write one minimal test showing what should happen.

```typescript
test('retries failed operations 3 times', async () => {
  let attempts = 0;
  const operation = () => {
    attempts++;
    if (attempts < 3) throw new Error('fail');
    return 'success';
  };
  const result = await retryOperation(operation);
  expect(result).toBe('success');
  expect(attempts).toBe(3);
});
```

Requirements:
- One behavior
- Clear name
- Real code (no mocks unless unavoidable)

### Verify RED โ€” Watch It Fail

**MANDATORY. Never skip.**

Confirm:
- Test fails (not errors)
- Failure message is expected
- Fails because feature missing (not typos)

**Test passes?** You're testing existing behavior. Fix test.

### GREEN โ€” Minimal Code

Write simplest code to pass the test. Just enough to pass. Don't add features, refactor, or "improve" beyond the test.

### Verify GREEN โ€” Watch It Pass

**MANDATORY.**

Confirm:
- Test passes
- Other tests still pass
- Output pristine (no errors, warnings)

**Test fails?** Fix code, not test.

### REFACTOR โ€” Clean Up

After green only:
- Remove duplication
- Improve names
- Extract helpers
- Keep tests green. Don't add behavior.

## Why Order Matters

**"I'll write tests after to verify it works"**
Tests written after code pass immediately. Passing immediately proves nothing โ€” might test wrong thing, might miss edge cases.

**"I already manually tested all the edge cases"**
Manual testing is ad-hoc. No record, can't re-run, easy to forget cases.

**"Deleting X hours of work is wasteful"**
Sunk cost fallacy. Working code without real tests is technical debt.

**"TDD is dogmatic, being pragmatic means adapting"**
TDD IS pragmatic: finds bugs before commit, prevents regressions, documents behavior, enables refactoring.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Already manually tested" | Ad-hoc โ  systematic. No record, can't re-run. |
| "Deleting X hours is wasteful" | Sunk cost. Keeping unverified code is tech debt. |
| "Need to explore first" | Fine. Throw away exploration, start with TDD. |

## Red Flags โ€” STOP and Start Over

- Code before test
- Test after implementation
- Test passes immediately
- Can't explain why test failed
- Tests added "later"
- "I already manually tested it"
- "TDD is dogmatic, I'm being pragmatic"
- "This is different because..."

**All of these mean: Delete code. Start over with TDD.**

## Example: Bug Fix

**Bug:** Empty email accepted

**RED:**
```typescript
test('rejects empty email', async () => {
  const result = await submitForm({ email: '' });
  expect(result.error).toBe('Email required');
});
```

**Verify RED:** `FAIL: expected 'Email required', got undefined`

**GREEN:**
```typescript
function submitForm(data) {
  if (!data.email?.trim()) {
    return { error: 'Email required' };
  }
  // ...
}
```

**Verify GREEN:** `PASS`

## Verification Checklist

Before marking work complete:
- [ ] Every new function/method has a test
- [ ] Watched each test fail before implementing
- [ ] Each test failed for expected reason
- [ ] Wrote minimal code to pass each test
- [ ] All tests pass
- [ ] Edge cases and errors covered

## Final Rule

```
Production code โ’ test exists and failed first
Otherwise โ’ not TDD
```
