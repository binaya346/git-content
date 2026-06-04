# Chapter 6: Advanced Git — Merge, Rebase & Power Tools

This chapter covers the tools that separate beginners from experienced developers. You will use these in every real-world project.

---

## 1. Merge vs. Rebase — Two Ways to Combine Branches

When you want to integrate changes from one branch into another, you have two strategies. Understanding the difference is one of the most important skills in Git.

**The starting scenario:** Two developers started from the same commit. Developer A kept working on `main`. Developer B created a `feature` branch.

```
Before:
main    ──●──●──●
                 \
feature           ●──●──●
```

Now you want to bring the feature branch changes into main.

---

### A. Git Merge — The Honest Historian

Merging combines both branches and creates a new **Merge Commit** that ties them together. It preserves the exact history of how things happened.

```
After merge:
main    ──●──●──●──────────────────●  (merge commit)
                 \                /
feature           ●──●──●──●──●──
```

<!-- IMAGE: git merge diagram -->

**Command:**
```bash
git checkout main
git merge feature-login
```

**Characteristics:**
- Creates a "diamond" shape in the history graph
- Preserves the complete, unmodified history
- Makes it easy to trace when branches split and rejoined
- With many developers, the graph becomes complex ("spaghetti")

**When to use it:**
- When you want a truthful record of parallel development
- For merging long-lived branches (`feature` → `main`)
- When using Pull Requests on GitHub (GitHub defaults to merge commits)

---

### B. Git Rebase — The Clean Storyteller

Rebasing takes your entire feature branch and **replays** all of its commits on top of the latest commit on `main`. It rewrites history to look like you built the feature directly on top of the current main.

```
Before rebase:          After rebase:
main    ──●──●──●       main    ──●──●──●
                 \                        \
feature           ●──●  feature            ●──●  (replayed commits)
```

<!-- IMAGE: git rebase diagram -->

**Command (run from inside the feature branch):**
```bash
git checkout feature-login
git rebase main
```

**Characteristics:**
- Produces a clean, linear, straight-line history
- No extra merge commits
- Looks like all changes were made sequentially
- **Rewrites commit history** — commit hashes change

**When to use it:**
- To keep your feature branch up to date with `main` before opening a PR
- When you want a readable, linear history

---

### Merge vs. Rebase Comparison

| Feature | Git Merge | Git Rebase |
|---|---|---|
| **History** | Preserves original history | Rewrites history to be linear |
| **Commit Logs** | Adds a "Merge commit" | No extra merge commits |
| **Complexity** | Simple and safe | Can be tricky (conflict resolution) |
| **Traceability** | Easy to see when branches diverged | Harder to trace branch creation time |
| **Safe to use on shared branches?** | Yes | No — see Golden Rule below |

---

### The Golden Rule of Rebasing

> **Never rebase a branch that has already been pushed to a shared repository.**

Rebasing rewrites commit history. If you rebase commits that your teammates have already pulled, their local history will diverge from the remote — this causes severe confusion and data loss.

**Safe rule:** Rebase only on **private** branches that you have not yet pushed, or that only you are using.

⚠️ **Common Mistake**
Running `git rebase main` on the `main` branch itself, or rebasing a branch that multiple people are working on. Always rebase from your own feature branch, not the shared base.

---

## 2. Hands-on Lab: "The Conflict Race"

This exercise shows you exactly what a merge conflict looks like and how to resolve it.

### Setup
```bash
mkdir conflict-lab && cd conflict-lab
git init
echo 'print("Start")' > script.py
git add .
git commit -m "Initial commit"
git checkout -b feature-a
```

### Create the Divergence
```bash
# On feature-a branch — change the print statement
echo 'print("Feature A version")' > script.py
git add . && git commit -m "Feature A changes"

# Switch back to main and make a different change to the same line
git checkout main
echo 'print("Main branch version")' > script.py
git add . && git commit -m "Main branch changes"
```

Both branches now have different versions of the same line. They have diverged.

### The Merge Challenge
```bash
git merge feature-a
```

Git will output:
```
CONFLICT (content): Merge conflict in script.py
Automatic merge failed; fix conflicts and then commit the result.
```

Open `script.py`. You will see:
```
<<<<<<< HEAD
print("Main branch version")
=======
print("Feature A version")
>>>>>>> feature-a
```

**Resolve it:**
1. Delete the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
2. Keep the line you want (or write a new combined version)
3. Save the file

```bash
git add script.py
git commit -m "Merge feature-a into main"
```

### The Rebase Challenge
Now try the same scenario using rebase:

```bash
git checkout feature-a
git rebase main
```

With rebase, Git replays each commit from `feature-a` one at a time on top of `main`. If there are conflicts, you resolve them **commit by commit**, then continue:

```bash
# After resolving the conflict in the file:
git add script.py
git rebase --continue
```

Once finished, inspect the history:
```bash
git log --oneline --graph --all
```

Notice the difference: merge creates a diamond, rebase creates a straight line.

---

## 3. Git Stash — Temporarily Set Aside Work

You are halfway through a feature when your manager asks you to urgently fix a bug on `main`. You are not ready to commit your unfinished work. This is where `git stash` saves you.

Stash temporarily shelves all your uncommitted changes, giving you a clean working directory.

```bash
git stash                 # Stash all uncommitted changes
git checkout main         # Switch to another branch safely
# ... fix the bug, commit, push ...
git checkout feature-x    # Come back to your feature branch
git stash pop             # Restore your stashed changes
```

You can have multiple stashes:
```bash
git stash list            # See all stashes
git stash pop             # Apply and remove the most recent stash
git stash drop            # Discard the most recent stash without applying
```

---

## 4. Cherry Pick — Grab One Specific Commit

Imagine your colleague fixed a critical bug on their feature branch. That branch is not ready to merge yet, but you urgently need that specific bug fix on `main`.

Cherry pick lets you copy a **single commit** from anywhere in history and apply it to your current branch.

```bash
# First, find the commit hash
git log --oneline                          # Look at your colleague's branch

# Copy just that one commit to main
git checkout main
git cherry-pick a3f9c12                    # Use the actual commit hash
```

**When to use it:**
- Applying a bug fix from one branch to another without merging the whole branch
- Backporting a fix to an older release

⚠️ **Common Mistake**
Cherry-picking too many commits. If you find yourself cherry-picking 5+ commits, consider merging the entire branch instead. Cherry-pick is for surgical, one-off operations.

---

## 5. Squash — Clean Up Messy History

During feature development, you might accumulate lots of small, messy commits:
```
a1b2c3  fix typo
d4e5f6  fix typo again
g7h8i9  oops wrong variable
j0k1l2  Add login feature (finally)
```

Before merging to `main`, you can **squash** all of these into one clean commit:
```
m3n4o5  Add user login feature
```

Squash is usually done during a Pull Request on GitHub. When merging a PR, select **"Squash and merge"** to combine all commits into one.

---

## 6. Tagging — Mark a Release

Tags are permanent labels attached to a specific commit. They are used to mark release versions.

```bash
git tag -a v1.0 -m "Release version 1.0"   # Create annotated tag
git push origin v1.0                         # Push the tag to GitHub
git tag                                      # List all tags
```

On GitHub, tags automatically appear under the **Releases** section of your repository.

---

## 7. Inspecting Your Repository

### Viewing a Specific Commit
```bash
git show a3f9c12               # Show changes made in a specific commit
```

### Seeing What Changed
```bash
git diff                       # Changes in working directory (not yet staged)
git diff --staged              # Changes in staging area (not yet committed)
```

### Finding Who Changed a Line
```bash
git blame login.py             # Shows which commit and author last modified each line
```

`git blame` is invaluable when debugging — it tells you exactly who introduced a specific line and in which commit.

---

## Summary: Advanced Commands

| Command | What it does |
|---|---|
| `git merge <branch>` | Merge a branch into current branch (creates merge commit) |
| `git rebase <branch>` | Replay current branch commits on top of target branch |
| `git rebase --continue` | Continue rebase after resolving a conflict |
| `git stash` | Temporarily shelve uncommitted changes |
| `git stash pop` | Restore stashed changes |
| `git cherry-pick <hash>` | Copy one specific commit to current branch |
| `git tag -a v1.0 -m "..."` | Create an annotated release tag |
| `git diff` | Show unstaged changes |
| `git blame <file>` | Show who last modified each line of a file |
| `git log --oneline --graph --all` | Visualize the full branch history |

---

💡 **Pro Tip**
In most teams, the workflow is:
1. Work on a feature branch
2. Run `git rebase origin/main` on your branch before opening a PR (to stay up to date)
3. Open a PR and use **"Squash and merge"** on GitHub when merging

This gives you the best of both worlds: clean linear history on `main`, while your feature branch can have as many small commits as you need during development.
