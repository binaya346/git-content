## Setup
1. Install git in your computer | server. `https://git-scm.com/`
2. Verify: `git -v`  
3. Create a profile in `github`.  
4. Configure in local
    `git config --global user.name "Your Name"`
    `git config --global user.email "your@email.com"`
    `git config --list`  # Verify settings
5. Add ssh key into the github access
   1. Go to => profile icon on top right corner => settings => SSH and GPG keys => Add your Public key in SSH Keys section. 

1. Clone the repo: `git clone <url>`
2. Create new branch => `git checkout -b feature-1`
3. Make changes, 
4. Stage the change: `git add .`
5. Commit the change: `git commit -m 'message here..'`
    Commit => Groups the changes | Create a new version for changes we can track that version using commit id | allow us to provide message for the changes.  | Use V5 for message
6. Push the changes to remote repository: `git push origin feature-1`
7. Create PR
8. Rebase your feature branch with main branch: `git rebase origin/main`
9.  Merge the changes, (Make sure we are doing fast forward merge.): `git checkout main && git merge feature-1`