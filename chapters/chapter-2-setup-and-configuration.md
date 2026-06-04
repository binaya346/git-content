# Chapter 2: Setup & Configuration

Before you can use Git, you need two things:
1. Git installed on your machine
2. A GitHub account connected via SSH

This chapter covers the one-time setup you do before starting any project.

---

## Step 1: Install Git

### On macOS
Open Terminal and run:
```bash
brew install git
```
If you do not have Homebrew, install it from `https://brew.sh` first, or download Git directly from `https://git-scm.com/`.

### On Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install git
```

### On Windows
Download and install Git from `https://git-scm.com/`. During installation, select **"Git Bash"** as your terminal — use Git Bash for all commands in this course.

### Verify Installation
After installing, open your terminal and run:
```bash
git -v
```
You should see an output like `git version 2.43.0`. If you see an error, Git is not installed correctly.

---

## Step 2: Configure Git (Tell Git Who You Are)

Git attaches your name and email to every commit you make. This is how the team knows who wrote what. You only need to do this once per machine.

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

The `--global` flag means these settings apply to every repository on your machine.

To verify your settings:
```bash
git config --list
```

You should see output like:
```
user.name=John Doe
user.email=john@example.com
```

📝 **Note**
Use the same email you used to create your GitHub account. This links your commits to your GitHub profile correctly.

---

## Step 3: Create a GitHub Account

1. Go to `https://github.com`
2. Click **Sign up** and create your account
3. Choose a professional username — this is visible to employers and teammates
4. Verify your email address

---

## Step 4: Set Up SSH Key Authentication

### Why SSH?
When you push or pull code, GitHub needs to verify your identity. You have two options:
- **HTTPS** — You type your username and password (or token) every time
- **SSH** — You set up a key pair once, and GitHub trusts your machine automatically

SSH is the professional standard. Set it up once and forget about authentication forever.

### How SSH Key Authentication Works
SSH works with a **key pair**:
- **Private key** — Stays on your machine. Never share this.
- **Public key** — You give this to GitHub. It is safe to share.

When you connect to GitHub, your machine proves it has the private key that matches the public key GitHub has on file. No password needed.

### Generate an SSH Key

Open your terminal and run:
```bash
ssh-keygen -t ed25519 -C "your@email.com"
```

- When asked `Enter file in which to save the key`, press **Enter** to accept the default location (`~/.ssh/id_ed25519`)
- When asked for a passphrase, you can press **Enter** to skip (for a shared/class environment) or set a passphrase for extra security

This creates two files:
- `~/.ssh/id_ed25519` — your **private key** (never share this)
- `~/.ssh/id_ed25519.pub` — your **public key** (this goes to GitHub)

### Find Your Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

You will see a long string starting with `ssh-ed25519`. Copy the entire output.

### Add the Public Key to GitHub

1. Go to GitHub → click your **profile icon** (top right) → **Settings**
2. In the left sidebar, click **SSH and GPG keys**
3. Click **New SSH key**
4. Give it a descriptive name (e.g., `My Laptop`, `Work Machine`)
5. Paste your public key into the **Key** field
6. Click **Add SSH key**

### Test the Connection

```bash
ssh -T git@github.com
```

If successful, you will see:
```
Hi yourusername! You've successfully authenticated, but GitHub does not provide shell access.
```

⚠️ **Common Mistake**
Copying the private key instead of the public key. The public key file ends in `.pub`. Make sure you are copying `id_ed25519.pub`, not `id_ed25519`.

---

## Step 5: Clone a Repository Using SSH

Once your SSH key is set up, you can clone a repository using the SSH URL.

1. Go to the repository on GitHub
2. Click the green **Code** button
3. Select **SSH** (not HTTPS)
4. Copy the URL — it looks like `git@github.com:username/repo-name.git`
5. In your terminal, navigate to where you want the project to live:

```bash
cd ~/Desktop
git clone git@github.com:username/repo-name.git
```

This downloads the entire repository, including all history, to your machine.

---

## Summary: One-Time Setup Checklist

| Step | Command / Action | Done? |
|---|---|---|
| Install Git | `git -v` to verify | ☐ |
| Configure name | `git config --global user.name "..."` | ☐ |
| Configure email | `git config --global user.email "..."` | ☐ |
| Create GitHub account | `github.com` | ☐ |
| Generate SSH key | `ssh-keygen -t ed25519 -C "..."` | ☐ |
| Add public key to GitHub | GitHub → Settings → SSH Keys | ☐ |
| Test SSH connection | `ssh -T git@github.com` | ☐ |

✅ **Best Practice**
Once you complete this setup, you will not need to repeat it. Treat your SSH private key like a password — never put it in a repository or share it with anyone.
