# Chapter 1: What is Version Control & Why Git?

## The Problem: Life Without Version Control. Changes from branch 1

Imagine you are part of a 3-person team building a web application. Each developer is working on a different page.

Here is what can go wrong:

**Scenario 1 — Data Loss**
Developer 1 finishes building Page 1 after 3 days of hard work. On the delivery day, their laptop crashes. All the code is gone.

**Scenario 2 — Code Sharing Nightmare**
Developer 1 builds a reusable function. Developer 2 needs the same function for their page. How does Developer 2 get it? Via WhatsApp? Email? A USB drive?

**Scenario 3 — New Team Member**
A 4th developer joins the team. The project is already 2 months old. Who gives them the complete, latest version of the code? What if 3 different developers each have slightly different versions on their laptops?

**Scenario 4 — A Bug in Production**
The website is down. A bug was introduced in yesterday's update. Which developer wrote the broken code? Which exact change caused the crash? Without any history, you have no idea.

**Scenario 5 — Rollback**
The new feature is broken and the deadline is in 1 hour. You need to go back to the version from last week. How?

All five of these problems are solved by **Version Control**.

---

## The Solution: Version Control

Version control is a system that tracks every change made to your files over time.

Think of it like this:
- **Version 1** → You write the initial login page and save it.
- **Version 2** → You add a "Forgot Password" link.
- **Version 3** → You fix a bug in the login form.

Every version is stored permanently. You can go back to any version at any time.

```
Your Code  →  Push  →  [Version 1]
More Code  →  Push  →  [Version 2]
Fix Bug    →  Push  →  [Version 3]
                            ↑
                    Can revert to any point
```

The repository (repo) is the storage unit — one project, one repository. It lives in the cloud so it is safe, shareable, and accessible to the whole team.

---

## Centralized vs. Distributed VCS

There are two types of version control systems.

### Centralized VCS (CVCS)
- One central server holds all the history.
- Every developer connects to that one server to save or retrieve code.
- **Example:** SVN (Subversion)
- **Problem:** If the server goes down, nobody on the team can work. The single server is also a single point of failure — if it is lost, all history is lost.

### Distributed VCS (DVCS)
- **Every developer** has a full copy of the entire repository and its complete history on their own machine.
- You can work completely offline.
- Even if the central server goes down, the full history exists on every developer's laptop.
- **Example:** Git

**Git is a Distributed VCS.** This is one of the key reasons it became the industry standard.

---

## Git vs. GitHub — Not the Same Thing

This is one of the most common points of confusion for beginners.

| | Git | GitHub |
|---|---|---|
| **What it is** | A tool that runs on your computer | A website/cloud platform |
| **What it does** | Tracks changes in your files locally | Hosts your Git repository online |
| **Works offline?** | Yes | No |
| **Who made it** | Linus Torvalds (2005) | Microsoft (acquired 2018) |

**Simple analogy:**
- **Git** is the engine of a car.
- **GitHub** is the parking lot where you store your car so others can access it.

You can use Git without GitHub (purely local tracking). But to collaborate with a team, you need both.

### Other Cloud Providers
GitHub is the most popular, but there are alternatives that work the same way:
- **GitLab** — popular in enterprises, has built-in CI/CD
- **Bitbucket** — popular in teams that use Atlassian tools (Jira, Confluence)

All three use Git under the hood. The commands you learn in this course work with all of them.

---

## What Git Actually Does

When you use Git, it does three things for you:

1. **Backup** — Your code is stored safely in the cloud. A crashed laptop does not mean lost work.
2. **Track Changes** — Every change is recorded with who made it and when. You can always see the full history.
3. **Collaborate** — Multiple developers can work on the same project simultaneously without overwriting each other's work.

---

## Key Terms You Will Use Throughout This Course

| Term | Meaning |
|---|---|
| **Repository (Repo)** | The storage container for your project and its entire history |
| **Commit** | A saved snapshot of your code at a specific point in time |
| **Branch** | An independent line of development, like a parallel copy of the project |
| **Clone** | Downloading a remote repository to your local machine |
| **Push** | Uploading your local commits to the remote repository |
| **Pull** | Downloading the latest commits from the remote to your local machine |
| **Remote** | A repository hosted on a server (e.g., on GitHub), as opposed to your local machine |

---

💡 **Pro Tip**
Git does not just track code — it is used to version control infrastructure files, configuration files, Kubernetes manifests, Terraform scripts, and anything else that is text-based. In DevOps, Git is the foundation of everything. If you change infrastructure, that change should be in Git.

📝 **Note**
In this course, we will use **GitHub** as our cloud provider. All the concepts apply equally to GitLab and Bitbucket.
