# Chapter 4: Working with GitHub (Remote Repositories)

In Chapter 3 you learned how to save code on your own machine. Now we take that code to the cloud so it can be backed up, shared, and collaborated on.

---

## Local vs. Remote Repository

| | Local Repository | Remote Repository |
|---|---|---|
| **Where it lives** | Your laptop (`~/.git` folder) | GitHub / GitLab / Bitbucket server |
| **Who can access it** | Only you | Your whole team |
| **What it is for** | Day-to-day commits while you work | Sharing, backup, and collaboration |

You are always working in your local repository. When you are ready to share your work, you **push** it to the remote. When you want to get your teammate's latest work, you **pull** from the remote.

<!-- IMAGE: Git Workflow diagram showing local ↔ remote flow -->

---

## Scenario A: Starting from Scratch (New Project)

If you have a new project on your laptop that does not exist on GitHub yet:

### Step 1 — Create the repository on GitHub
1. Go to GitHub and click the **+** icon → **New repository**
2. Give it a name (e.g., `my-web-app`)
3. Leave it **empty** — do not add a README yet
4. Click **Create repository**

GitHub will show you a set of commands. Use these:

### Step 2 — Link and push from your terminal
```bash
cd my-project                                        # Navigate to your project folder
git init                                             # Initialize local repo (if not done)
git add .                                            # Stage all files
git commit -m "Initial commit"                       # First commit
git remote add origin git@github.com:username/my-web-app.git  # Link to GitHub
git branch -M main                                   # Rename branch to main
git push -u origin main                              # Push to GitHub (first time)
```

The `-u` flag in `git push -u origin main` sets the **upstream** — it means from now on, you can just type `git push` without specifying the branch name every time.

---

## Scenario B: Joining an Existing Project (Clone)

If the project already exists on GitHub (e.g., your team's repo or your instructor shared a link):

```bash
cd ~/Desktop
git clone git@github.com:username/repo-name.git
```

`git clone` does three things at once:
1. Downloads the entire repository (all files and history)
2. Sets up the `origin` remote automatically
3. Checks out the default branch (usually `main`)

You are now ready to start working immediately.

📝 **Note**
`git clone` is always done **once** — when you first join a project. After that, you use `git pull` to get updates.

---

## Scenario C: Getting Your Teammate's Updates (Pull)

Your teammates have been pushing new code while you were working. Before you push your own changes, you must first get their latest updates:

```bash
git pull origin main
```

`git pull` does two things:
1. **Fetch** — downloads new commits from the remote
2. **Merge** — integrates them into your current local branch

💡 **Pro Tip**
Make it a habit to run `git pull` every morning before you start coding. This ensures you are always working on top of the latest code and reduces merge conflicts.

---

## The Complete Day-to-Day Workflow

Here is the full cycle every developer follows every single day:

```
Morning
  └── git pull origin main          ← Get latest code from teammates

During the day
  └── Write / edit code

When you have a meaningful change
  └── git status                    ← Check what changed
  └── git add .                     ← Stage changes
  └── git commit -m "message"       ← Save snapshot locally

End of task / end of day
  └── git push origin main          ← Upload your work to GitHub
```

---

## Viewing Remote Configuration

To see which remote your local repository is connected to:
```bash
git remote -v
```

Output:
```
origin  git@github.com:username/repo-name.git (fetch)
origin  git@github.com:username/repo-name.git (push)
```

`origin` is just the conventional name for the primary remote. You can rename it, but never do — everyone uses `origin`.

---

## Pushing Subsequent Changes

After the first push with `-u`, every future push is simply:
```bash
git push
```

Or explicitly:
```bash
git push origin main
```

---

## What Happens If You Push and Someone Else Has Already Pushed?

Git will reject your push to protect the remote from losing their work:

```
! [rejected] main -> main (fetch first)
error: failed to push some refs
hint: Updates were rejected because the remote contains work that you do not have locally.
```

**The fix:**
```bash
git pull origin main    # Get their changes first
# Resolve any conflicts if needed
git push origin main    # Now push your combined work
```

⚠️ **Common Mistake**
Never use `git push --force` on a shared branch. It overwrites the remote with your local version and destroys your teammates' work. Force push is only acceptable on private feature branches you own completely.

---

## Summary: Remote Commands

| Command | What it does |
|---|---|
| `git remote add origin <url>` | Link your local repo to a GitHub remote |
| `git remote -v` | View the remote connection |
| `git clone <url>` | Download a remote repository to your machine |
| `git push origin main` | Upload your local commits to GitHub |
| `git push -u origin main` | Push for the first time (sets upstream) |
| `git pull origin main` | Download and merge latest changes from GitHub |

---

✅ **Best Practice**
The rule is: **pull before you push**. Always run `git pull` before `git push` to avoid rejections and reduce the chance of conflicts.
