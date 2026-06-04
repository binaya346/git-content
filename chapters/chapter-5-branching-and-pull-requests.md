# Chapter 5: Branching & Pull Requests

Branching is what makes team collaboration possible. Without branches, every developer would be pushing directly to `main` and constantly breaking each other's work.

---

## Why Branches?

Think of the `main` branch as the **published version** of your product. It must always be clean, working, and ready to deploy.

When you want to build a new feature or fix a bug, you create a **branch** — an independent, isolated copy of the codebase where you can work freely without affecting `main`.

```
main  ──●──────────────────────────────────────●── (production-ready)
         \                                     /
          ●──●──●  feature/login-page  ──●──●
```

When the feature is complete and reviewed, the branch is merged back into `main`.

<!-- IMAGE: Git branch diagram -->

---

## Branching Strategy

In a professional team, branches are organized into levels:

| Branch | Purpose |
|---|---|
| `main` | Production-ready code only. This is what is deployed to users. |
| `develop` | Integration branch. Completed features are merged here first, before going to `main`. |
| `feature/xyz` | Where individual tasks are done. One branch per feature or bug fix. |

**Examples of good branch names:**
- `feature/user-authentication`
- `feature/dashboard-redesign`
- `bugfix/login-null-pointer`
- `hotfix/payment-crash`

---

## Branch Commands

### Create a new branch
```bash
git branch feature-login
```

### Switch to that branch
```bash
git checkout feature-login
```

### Create and switch in one step (recommended)
```bash
git checkout -b feature-login
```

### See all branches
```bash
git branch
```
The branch with the `*` is the one you are currently on.

### Delete a branch (after it is merged)
```bash
git branch -d feature-login
```

---

## The Feature Branch Workflow

This is the exact workflow used in professional teams. Follow this process for every task:

### Step 1 — Start from the latest main
```bash
git checkout main
git pull origin main
```
Always start your new branch from the latest version of `main`.

### Step 2 — Create your feature branch
```bash
git checkout -b feature/add-login-page
```

### Step 3 — Do your work
Write code, edit files, build the feature.

### Step 4 — Stage and commit your changes
```bash
git add .
git commit -m "Add login form with email and password fields"
```
Commit often with clear messages. Do not wait until the feature is 100% done.

### Step 5 — Push your branch to GitHub
```bash
git push origin feature/add-login-page
```

### Step 6 — Create a Pull Request (PR)
Now your branch is on GitHub. The next step is a **Pull Request**.

---

## What is a Pull Request?

A **Pull Request** (PR) is a formal request to merge your feature branch into the target branch (usually `main` or `develop`).

It is not a Git command — it is a feature of GitHub. A PR creates a space for:
- Your team to **review your code** before it is merged
- Leaving **comments** on specific lines
- Running **automated tests** (CI/CD pipelines)
- Ensuring quality and catching bugs before they reach production

### How to Create a Pull Request on GitHub

1. Go to your repository on GitHub
2. GitHub will usually show a yellow banner: **"feature/add-login-page had recent pushes"** → click **"Compare & pull request"**
3. Set the **base branch** (where you want to merge *into*, e.g., `main`)
4. Set the **compare branch** (your feature branch)
5. Write a clear **title** and **description** — explain what you built and why
6. Add **reviewers** (your teammates)
7. Click **Create Pull Request**

### What Happens After Creating a PR?
- Reviewers get notified and will read your code
- They can **approve**, **request changes**, or leave comments
- You can push more commits to the same branch — the PR updates automatically
- Once approved, click **Merge pull request**

💡 **Pro Tip**
Write a good PR description. Explain what the feature does, how you tested it, and anything the reviewer should pay special attention to. A good PR saves your team time.

---

## Keeping Your Branch Up to Date

While you are working on your feature branch, your teammates might merge other work into `main`. Your branch can fall behind.

To update your feature branch with the latest `main`:
```bash
git checkout feature/add-login-page
git rebase origin/main
```

Or using merge:
```bash
git merge main
```

We will cover the difference between rebase and merge in detail in Chapter 6.

---

## Fast-Forward Merge

When merging a branch locally (not via PR), the simplest case is a **fast-forward merge**. This happens when your feature branch is directly ahead of `main` with no conflicting changes:

```bash
git checkout main
git merge feature/add-login-page
```

Git simply moves the `main` pointer forward to include your commits — no merge commit needed.

---

## Handling Merge Conflicts

A **merge conflict** happens when two branches have made different changes to the same line in the same file. Git cannot automatically decide which version is correct, so it asks you to resolve it manually.

**How to recognize a conflict:**
When you merge or pull, Git marks the conflicting file like this:

```
<<<<<<< HEAD (main branch version)
print("Main branch version")
=======
print("Feature A version")
>>>>>>> feature-a
```

**How to resolve it:**
1. Open the file in your editor
2. Decide which version to keep (or write a combined version)
3. Delete all the `<<<<<<<`, `=======`, and `>>>>>>>` markers
4. Save the file
5. Stage and commit the resolution:
```bash
git add script.py
git commit -m "Resolve merge conflict in script.py"
```

⚠️ **Common Mistake**
Do not panic when you see a conflict. It is a normal part of team development. Read both versions carefully, choose the correct one, clean up the markers, and commit. Conflicts get easier to handle with practice.

---

## Summary: Branch Commands

| Command | What it does |
|---|---|
| `git branch <name>` | Create a new branch |
| `git checkout <name>` | Switch to a branch |
| `git checkout -b <name>` | Create and switch in one step |
| `git branch` | List all local branches |
| `git push origin <name>` | Push the branch to GitHub |
| `git merge <name>` | Merge a branch into the current branch |
| `git branch -d <name>` | Delete a local branch |

---

✅ **Best Practice**
The rule in every professional team: **never commit directly to `main`**. Always work on a feature branch and merge via a Pull Request. This ensures every change is reviewed by at least one other person before it reaches production.
