---
name: using-git-worktrees
description: Use when starting feature work that needs isolation from current workspace or before executing implementation plans - ensures an isolated workspace exists via native tools or git worktree fallback
---

# Using Git Worktrees

## Overview

Ensure work happens in an isolated workspace. Prefer your platform's native worktree tools. Fall back to manual git worktrees only when no native tool is available.

**Core principle:** Detect existing isolation first. Then use native tools. Then fall back to git. Never fight the harness.

**Announce at start:** "I'm using the using-git-worktrees skill to set up an isolated workspace."

## Step 0: Detect Existing Isolation

**Before creating anything, check if you are already in an isolated workspace.**

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
```

**Submodule guard:** Check if inside a submodule (not a worktree):
```bash
git rev-parse --show-superproject-working-tree 2>/dev/null
```

**If `GIT_DIR != GIT_COMMON` (and not a submodule):** Already in a linked worktree. Skip to Step 3.

**If `GIT_DIR == GIT_COMMON`:** Normal repo. Ask for consent before creating a worktree.

## Step 1: Create Isolated Workspace

### 1a. Native Worktree Tools (preferred)

If you have a native tool (like `EnterWorktree`, `WorktreeCreate`, `/worktree` command), use it and skip to Step 3.

**Never use `git worktree add` when you have a native tool.**

### 1b. Git Worktree Fallback

Only use if Step 1a does not apply.

**Directory Selection (priority order):**
1. Explicit user preference in instructions
2. Existing `.worktrees` directory (preferred)
3. Existing `worktrees` directory
4. Existing global `~/.config/superpowers/worktrees/<project>`
5. Default to `.worktrees/` at project root

**Safety Verification (project-local directories only):**
```bash
git check-ignore -q .worktrees 2>/dev/null || git check-ignore -q worktrees 2>/dev/null
```
If NOT ignored: Add to .gitignore, commit, then proceed.

**Create the Worktree:**
```bash
git worktree add "$path" -b "$BRANCH_NAME"
cd "$path"
```

## Step 3: Project Setup

Auto-detect and run appropriate setup:
```bash
# Node.js
if [ -f package.json ]; then npm install; fi
# Rust
if [ -f Cargo.toml ]; then cargo build; fi
# Python
if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
# Go
if [ -f go.mod ]; then go mod download; fi
```

## Step 4: Verify Clean Baseline

Run tests to ensure workspace starts clean.

**If tests fail:** Report failures, ask whether to proceed or investigate.
**If tests pass:** Report ready.

## Quick Reference

| Situation | Action |
|-----------|--------|
| Already in linked worktree | Skip creation (Step 0) |
| In a submodule | Treat as normal repo |
| Native worktree tool available | Use it (Step 1a) |
| No native tool | Git worktree fallback (Step 1b) |
| `.worktrees/` exists | Use it (verify ignored) |
| Neither exists | Default `.worktrees/` |
| Directory not ignored | Add to .gitignore + commit |
| Tests fail during baseline | Report failures + ask |

## Common Mistakes

- **Fighting the harness:** Using `git worktree add` when platform provides isolation
- **Skipping detection:** Creating nested worktree inside existing one
- **Skipping ignore verification:** Worktree contents get tracked in git
- **Proceeding with failing tests:** Can't distinguish new bugs from pre-existing issues

## Red Flags

**Never:**
- Create a worktree when Step 0 detects existing isolation
- Use `git worktree add` when you have a native worktree tool
- Skip Step 1a by jumping straight to Step 1b
- Create worktree without verifying it's ignored (project-local)
- Skip baseline test verification

**Always:**
- Run Step 0 detection first
- Prefer native tools over git fallback
- Verify directory is ignored for project-local
- Auto-detect and run project setup
- Verify clean test baseline
