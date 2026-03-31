---
description: Use when merging branches, finishing feature work, or when user mentions "feature branching" - guides the merge-back workflow for git feature branches
---

# Git Feature Branching

A merge-based branching strategy. No rebase, no force push.

## Branch Naming

Pick the prefix that fits:

| Prefix | Use for |
|--------|---------|
| `feature/` | New functionality |
| `bugfix/` | Bug fixes |
| `hotfix/` | Urgent production fixes |
| `issue/` | Work tied to a specific GitHub issue |
| `task/` | Minor or miscellaneous work |

Example: `feature/add-user-auth`, `issue/42-fix-login`

## Workflow

### 1. Start

```bash
git checkout <TRUNK>        # e.g. main, develop, or another branch
git pull
git checkout -b <prefix>/description
```

TRUNK is whatever branch you're branching from — usually `main` or `develop`, but can be any branch.

### 2. Develop

Commit freely to your branch. Push to remote as needed.

### 3. Finish

1. **Merge TRUNK into your branch** — always do this first to pick up any new changes.
   ```bash
   git checkout <your-branch>
   git merge <TRUNK>
   ```
2. **Fix merge conflicts** if any, then commit.
3. **Check if TRUNK is protected:**
   - **Protected** (`main`, `develop`): Open a PR from your branch towards TRUNK.
   - **Not protected**: Merge your branch into TRUNK directly:
     ```bash
     git checkout <TRUNK>
     git merge <your-branch>
     git push
     ```
4. **If push fails** because TRUNK moved while you were merging — do NOT force push. Go back to step 1 and merge TRUNK into your branch again.

## Rules

- **This workflow uses merge only** — no rebase.
- **Never force push.** If TRUNK has moved, merge it in again.
- **Always merge TRUNK into your branch first** before merging back or opening a PR. This keeps conflict resolution on your branch, not on TRUNK.
