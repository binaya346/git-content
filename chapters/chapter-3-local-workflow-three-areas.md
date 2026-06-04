# Chapter 3: The Local Workflow — Git's Three Areas

This is the most important chapter in the entire course. If you understand the three areas of Git, everything else will make sense.

---

## The Three Areas of Git

Every file in your project lives in one of three places at any given time:

```
┌─────────────────┐     git add     ┌─────────────────┐     git commit     ┌─────────────────┐
│                 │  ─────────────► │                 │  ────────────────► │                 │
│    Working      │                 │    Staging      │                    │     Local       │
│    Directory    │                 │    Area         │                    │   Repository    │
│                 │  ◄───────────── │                 │                    │                 │
│  (Your files)   │   git restore   │  (Ready to      │                    │  (Saved history)│
│                 │                 │   snapshot)     │                    │                 │
└─────────────────┘                 └─────────────────┘                    └─────────────────┘
```

### 1. Working Directory
This is your project folder — the files you see and edit in your code editor. When you write code, you are working here.

Git watches this folder and notices when any file changes. But a changed file is not saved to Git history yet.

### 2. Staging Area (also called the Index)
Think of this as the **waiting room** or a **draft list** for your next commit.

You manually move files here using `git add`. This step lets you choose *which* changes go into the next snapshot. Not every change needs to be in the same commit.

**Example:** You fixed a bug in `login.py` and also started a new feature in `dashboard.py`. You can stage only `login.py` and commit just the bug fix, keeping the unfinished feature out of that commit.

### 3. Local Repository
This is where Git permanently stores your commits (snapshots). It lives inside a hidden `.git` folder in your project directory.

When you run `git commit`, Git takes everything in the Staging Area and saves it here as a new snapshot with a unique ID.

---

## Starting a New Project

### Initialize a Repository

To start tracking a new project with Git:

```bash
cd my-project        # Navigate into your project folder
git init             # Initialize Git tracking
```

`git init` creates a hidden `.git` folder. This folder is the Local Repository — it contains your entire project history. Never delete it.

📝 **Note**
You only run `git init` when starting a brand new project that does not already exist on GitHub. If you are joining an existing project, you will use `git clone` instead (covered in Chapter 4).

---

## The Daily Workflow: Save Work to Git

### Step 1 — Check Status

Before doing anything, always check the current state of your repository:

```bash
git status
```

This shows you:
- Which files have been **modified** (changed but not staged)
- Which files are **staged** (ready to commit)
- Which files are **untracked** (new files Git has never seen before)

Get comfortable running `git status` constantly. It is your compass.

**Example output:**
```
On branch main
Changes not staged for commit:
  modified:   login.py

Untracked files:
  dashboard.py
```

### Step 2 — Stage Your Changes

Move changed or new files from the Working Directory to the Staging Area:

```bash
git add login.py          # Stage a specific file
git add .                 # Stage ALL changed and new files at once
```

After staging, run `git status` again. You will see the files listed under **"Changes to be committed"**.

### Step 3 — Commit Your Changes

Create a snapshot of everything in the Staging Area and save it permanently to your Local Repository:

```bash
git commit -m "Fix login validation bug"
```

The `-m` flag lets you write the commit message inline. Every commit must have a message.

After committing, `git status` will show a clean working tree — all changes have been saved.

---

## Writing Good Commit Messages

Your commit message tells your team (and future you) **what changed and why**. Bad messages make debugging a nightmare.

| Bad ❌ | Good ✅ |
|---|---|
| `update` | `Add email validation to login form` |
| `fix` | `Fix null pointer error in user profile page` |
| `wip` | `Add password strength indicator to signup` |
| `asdfg` | `Remove hardcoded API key, move to env variable` |

**Rules for a good commit message:**
- Use the imperative tense: "Add feature", "Fix bug", not "Added" or "Fixes"
- Keep the first line under 72 characters
- Be specific enough that someone can understand the change without reading the code

💡 **Pro Tip**
Commit small and often. A commit that says "Add login page" with 500 changed lines is hard to review and hard to revert. Aim for commits that represent one logical change.

---

## Viewing History

To see all past commits:

```bash
git log
```

Each commit shows:
- A unique **commit hash** (ID) like `a3f9c12`
- The author and timestamp
- The commit message

For a cleaner, visual view:

```bash
git log --oneline --graph --all
```

This shows the history as a compact graph — extremely useful once you start branching.

---

## Undoing Things (Safety Net)

### Unstage a file (you ran `git add` by accident)
```bash
git reset login.py
```
This moves the file back to the Working Directory. Your code changes are not lost.

### Undo the last commit but keep the code changes
```bash
git reset --soft HEAD~1
```
The commit is undone, but your changes are still staged and safe.

### Discard all local changes (DANGER — permanently deletes your work)
```bash
git reset --hard HEAD
```

⚠️ **Common Mistake**
`git reset --hard` permanently destroys all uncommitted changes. There is no undo. Only use this when you are absolutely certain you want to throw away your work.

---

## Summary: Commands for the Local Workflow

| Command | What it does |
|---|---|
| `git init` | Initialize a new local repository |
| `git status` | Show the current state of all three areas |
| `git add <file>` | Move a file from Working Directory to Staging Area |
| `git add .` | Stage all changes at once |
| `git commit -m "message"` | Save staged changes as a new snapshot in Local Repository |
| `git log` | Show full commit history |
| `git log --oneline --graph --all` | Show compact visual history |
| `git reset <file>` | Unstage a file |
| `git reset --soft HEAD~1` | Undo last commit, keep changes staged |
| `git diff` | Show what changed in files (not yet staged) |

---

✅ **Best Practice**
The golden sequence you will use every single day:
```bash
git status          # 1. See what changed
git add .           # 2. Stage the changes
git commit -m "..." # 3. Save the snapshot
```
Repeat this cycle often throughout your workday, not just at the end.
