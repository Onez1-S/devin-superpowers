---
name: finishing-a-development-branch
description: Use when implementation is complete, all tests pass, and you need to decide how to integrate the work - guides completion of development work by presenting structured options for merge, PR, or cleanup
---

# Finishing a Development Branch

## Overview

Guide completion of development work by presenting clear options and handling chosen workflow.

**Core principle:** Verify tests โ’ Detect environment โ’ Present options โ’ Execute choice โ’ Clean up.

**Announce at start:** "I'm using the finishing-a-development-branch skill to complete this work."

## The Process

### Step 1: Verify Tests

Run the project's test suite before presenting options.

**If tests fail:** Show failures and stop. Do not proceed to Step 2 until tests pass.

### Step 2: Detect Environment

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
```

| State | Menu | Cleanup |
|-------|------|---------|
| `GIT_DIR == GIT_COMMON` (normal repo) | Standard 4 options | No worktree to clean up |
| `GIT_DIR != GIT_COMMON`, named branch | Standard 4 options | Provenance-based |
| `GIT_DIR != GIT_COMMON`, detached HEAD | Reduced 3 options (no merge) | No cleanup (externally managed) |

### Step 3: Determine Base Branch

Try `git merge-base HEAD main` or `git merge-base HEAD master`, or ask.

### Step 4: Present Options

**Normal repo and named-branch worktree โ€” exactly 4 options:**

```
Implementation complete. What would you like to do?

1. Merge back to <base> locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

**Detached HEAD โ€” exactly 3 options:**

```
Implementation complete. You're on a detached HEAD (externally managed workspace).

1. Push as new branch and create a Pull Request
2. Keep as-is (I'll handle it later)
3. Discard this work
```

### Step 5: Execute Choice

#### Option 1: Merge Locally
```bash
git checkout <base>
git pull
git merge <feature-branch>
# Verify tests on merged result
# Only after merge succeeds: cleanup worktree, then delete branch
git branch -d <feature-branch>
```

#### Option 2: Push and Create PR
```bash
git push -u origin <feature-branch>
gh pr create --title "<title>" --body "..."
```
**Do NOT clean up worktree** โ€” user needs it alive to iterate on PR feedback.

#### Option 3: Keep As-Is
Report: "Keeping branch `<name>`. Worktree preserved at `<path>`."

#### Option 4: Discard
**Confirm first** โ€” require typed "discard" confirmation before proceeding.

### Step 6: Cleanup Workspace (Options 1 and 4 only)

Only clean up worktrees under `.worktrees/`, `worktrees/`, or `~/.config/superpowers/worktrees/`.

```bash
cd "$MAIN_ROOT"
git worktree remove "$WORKTREE_PATH"
git worktree prune
```

**Never clean up worktrees you didn't create.**

## Quick Reference

| Option | Merge | Push | Keep Worktree | Cleanup Branch |
|--------|-------|------|---------------|----------------|
| 1. Merge locally | yes | - | - | yes |
| 2. Create PR | - | yes | yes | - |
| 3. Keep as-is | - | - | yes | - |
| 4. Discard | - | - | - | yes (force) |

## Red Flags

**Never:**
- Proceed with failing tests
- Merge without verifying tests on result
- Delete work without confirmation
- Force-push without explicit request
- Remove a worktree before confirming merge success
- Clean up worktrees you didn't create
- Run `git worktree remove` from inside the worktree

**Always:**
- Verify tests before offering options
- Detect environment before presenting menu
- Present exactly 4 options (or 3 for detached HEAD)
- Get typed confirmation for Option 4
- `cd` to main repo root before worktree removal
