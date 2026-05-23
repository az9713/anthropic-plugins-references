# Git Worktrees: A Practical Guide for New Users

## What Problem Does This Solve?

You are working on `main`. A new task arrives — rename a file, fix a bug, try an experiment — but you do not want to disturb the work already in progress on `main`. Normally you would stash your changes, switch branches, do the work, switch back, and pop the stash. That is four steps of friction every time, and stashes are easy to lose or forget.

**Git worktrees give you a second working folder for the same repository, on a different branch, at the same time.** No stashing. No branch switching. Both folders are live simultaneously.

---

## How This Project Uses Worktrees

This project has two active locations on disk right now:

| Folder | Branch | Purpose |
|---|---|---|
| `C:/Users/simon/Downloads/anthropic_plugins` | `main` | Primary development |
| `C:/Users/simon/Downloads/anthropic_plugins/.claude/worktrees/remove_readme_txt` | `worktree-remove_readme_txt` | Renamed README.txt → sources.md |

Both folders share the same `.git` database. A commit in one is immediately visible to the other. No duplication of history.

---

## Core Concepts in Plain English

**Repository** — the `.git` folder that stores all history. One repository, one `.git` folder.

**Branch** — a named pointer to a line of commits. `main` is the stable line. Feature branches are parallel lines that diverge and later merge back.

**Working tree** — the folder of files you actually edit. Normally there is one working tree per repository. With `git worktree`, you can have several.

**Worktree** — an extra working folder linked to the same repository, checked out on its own branch. Changes in one worktree do not interfere with another.

---

## Setting Up a Worktree (How This One Was Created)

```bash
# From the main repository folder:
git worktree add .claude/worktrees/remove_readme_txt -b worktree-remove_readme_txt
```

Breaking that down:

| Part | Meaning |
|---|---|
| `git worktree add` | Create a new linked working folder |
| `.claude/worktrees/remove_readme_txt` | Path for the new folder (relative to repo root) |
| `-b worktree-remove_readme_txt` | Create and check out a new branch with this name |

You can place the worktree folder anywhere. Inside the repo (as done here) or completely outside it — both work fine.

---

## Working Inside a Worktree

Once inside the worktree folder, Git behaves exactly as normal. You edit files, stage them, commit them — all standard commands work:

```bash
git status
git add sources.md
git commit -m "Rename README.txt to sources.md in markdown format"
```

The commit lands on `worktree-remove_readme_txt`. The `main` branch is completely untouched.

---

## Listing Your Active Worktrees

Run this from anywhere inside the repository:

```bash
git worktree list
```

Output for this project:

```
C:/Users/simon/Downloads/anthropic_plugins                                     df867c8 [main]
C:/Users/simon/Downloads/anthropic_plugins/.claude/worktrees/remove_readme_txt 254af6b [worktree-remove_readme_txt]
```

This tells you the folder path, the current commit hash, and the branch name for each worktree.

---

## Pushing a Worktree Branch to GitHub

Pushing the worktree branch creates it on the remote without affecting `main`:

```bash
# Run from inside the worktree folder:
git push -u origin worktree-remove_readme_txt
```

`-u` sets the upstream so future `git push` calls from this folder need no arguments.

---

## Merging the Worktree Work Back Into Main

When the work in the worktree is finished and ready to integrate, you have three paths.

### Option A — Direct Merge (simple, preserves full branch history)

```bash
# Step 1: Go to the main working folder (NOT the worktree folder)
cd C:/Users/simon/Downloads/anthropic_plugins

# Step 2: Make sure main is up to date
git pull origin main

# Step 3: Merge the worktree branch into main
git merge worktree-remove_readme_txt

# Step 4: Push main to GitHub
git push
```

This creates a merge commit on `main` that records the two lines of history joining together.

### Option B — Squash Merge (collapses all worktree commits into one tidy commit on main)

```bash
cd C:/Users/simon/Downloads/anthropic_plugins
git pull origin main
git merge --squash worktree-remove_readme_txt
git commit -m "Rename README.txt to sources.md in markdown format"
git push
```

Use this when the worktree branch has many small or messy commits and you want `main`'s history to stay clean.

### Option C — Pull Request on GitHub (recommended for teams or audit trail)

```bash
# 1. Push the worktree branch first (if not done already)
git push -u origin worktree-remove_readme_txt

# 2. Open a PR on GitHub
gh pr create --title "Rename README.txt to sources.md" --base main --head worktree-remove_readme_txt
```

Or open it in the browser at `https://github.com/az9713/anthropic-plugins-references/compare/worktree-remove_readme_txt`. GitHub will show you the diff, let you add a description, and record the merge permanently. This is the safest option when others may review the change.

---

## Cleaning Up After Merging

Once the branch is merged and no longer needed, remove the worktree and delete the branch:

```bash
# From the main repository folder:
git worktree remove .claude/worktrees/remove_readme_txt

# Delete the branch locally
git branch -d worktree-remove_readme_txt

# Delete the branch on GitHub (optional)
git push origin --delete worktree-remove_readme_txt
```

`git worktree remove` will refuse if the worktree has uncommitted changes, which protects you from accidental data loss.

---

## What Can Go Wrong (and How to Recover)

**"fatal: 'worktree-remove_readme_txt' is already checked out"**
Each branch can only be checked out in one worktree at a time. Use `git worktree list` to find where it is.

**Worktree folder deleted manually without running `git worktree remove`**
Git still thinks the worktree exists. Clean up the stale record:
```bash
git worktree prune
```

**Merge conflict during `git merge`**
Git pauses and marks the conflicting lines in the file. Open the file, resolve the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`), save, then:
```bash
git add <the-file>
git merge --continue
```

---

## Quick Reference Card

```bash
# Create a worktree on a new branch
git worktree add <folder-path> -b <branch-name>

# Create a worktree on an existing branch
git worktree add <folder-path> <existing-branch-name>

# List all worktrees
git worktree list

# Remove a worktree (branch stays intact)
git worktree remove <folder-path>

# Clean up stale worktree records
git worktree prune

# Merge worktree branch into main
git checkout main
git merge <branch-name>

# Delete branch after merge
git branch -d <branch-name>
```

---

## When to Reach for a Worktree

| Situation | Use a worktree? |
|---|---|
| Quick unrelated fix while mid-feature | Yes |
| Long-running experiment that may be abandoned | Yes |
| Hotfix needed on `main` while you are mid-refactor | Yes |
| Just switching between two sequential tasks | No — a normal branch switch is fine |
| Reviewing someone else's PR locally | Yes — check it out in a worktree without disturbing your work |

The core rule: **if switching branches would interrupt something you are in the middle of, use a worktree instead.**
