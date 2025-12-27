# Git Notes
---

## 1. Introduction to Version Control

### What is Git?

Git is a **distributed version control system (VCS)** used to:

- Track changes in source code
- Maintain complete history of files
- Collaborate with multiple developers
- Roll back to previous versions when needed

Without Git, if a file is deleted from the file system, it **cannot be recovered** easily.
With Git, you can **restore deleted files and revert changes**.

---

## 2. Setting Up Git and GitHub

### Install Git

Download Git from the official source:
 https://github.com/git-guides/install-git

Check installation:

```bash
git --version
```

### Git Platforms

- **GitHub**
- **GitLab**
- **Bitbucket**

> Git is the tool. GitHub/GitLab/Bitbucket are platforms that host Git repositories.

---

## 3. Basic Git Concepts

### Initialize a Git Repository

```bash
git init
# Setup
git config --global user.name "Name"
git config --global user.email "email"
```

Creates an empty Git repository with a `.git` directory.

---

## 4. Git File States (Very Important)

Git has **three stages**:

1. **Untracked** – File exists in file system only
2. **Staged** – File added to Git index
3. **Tracked (Committed)** – File saved in Git history

### Example

```bash
git status
```

Output:

```
On branch main
No commits yet
Untracked files:
  test.py
```

### Add File to Staging Area

```bash
git add test.py
```

- File turns **green** (staged)

### Remove from Staging

```bash
git rm --cached test.py
```

- File turns **red** (unstaged)

---

## 5. Committing Changes

```bash
git commit -m "add test file"
```

Example output:

```
[main (root-commit)] add test file
1 file changed, 1 insertion(+)
create mode 100644 test.py
```

### Restore Deleted File

If a file is deleted accidentally:

```bash
git restore test.py
```

---

## 6. Local to Remote (GitHub)

### Add Remote Repository

```bash
git remote add origin <repo-url>
```

### Rename Branch to Main

```bash
git branch -M main
```

### Push Code to GitHub

```bash
git push origin main
```

### Update Remote URL with Token

```bash
git remote set-url origin https://<token>@github.com/username/repo.git
```

---

## 7. Clone vs Fork

### Clone

- GitHub → Local system
- Exact replica of repository

```bash
git clone <repo-url>
```

### Fork

- GitHub → GitHub (your own copy)
- Used for testing and contributions

**Fork is always done through GitHub, GitLab, or Bitbucket UI.**

**There is no `git fork` command in Git CLI.**

## 8. Fetch vs Pull

### Fetch

```bash
git fetch
```

- Downloads all remote branches
- Does NOT merge

### Pull

```bash

git pull              # fetch + merge
git pull --rebase     # fetch + rebase (cleaner)
```

- Downloads + merges

---

## 9. Branches in Git

### What is a Branch?

A branch is a **separate copy of code**.

- `main` → Production
- `staging` → Pre-production
- `dev` → Development
- `feature/*` → New features
- `hotfix/*` → A short-lived Git branch used to quickly fix critical bugs or security issues in a live production env

> Latest commit in a branch is called **HEAD**.

### Recover deleted branch?

```bash
git reflog              # Find commit hash
git switch -c branch-name <hash>
```

### Create and Switch Branch

```bash
git checkout -b staging
git switch staging
git switch main

```

### Compare branches?

```bash
gitdiff main..feature
git log main..feature
```

### Delete branch?

```bash
git branch -d branch-name    # Local delete (safe)
git branch -D branch-name    # Force delete
git push origin --delete branch-name  # Remote delete
```

---

## 11. git switch and git checkout

**Main Difference**
            git switch = One job - only switch branches (clear and safe)
            git checkout = Many jobs - branches, files, and many other things (can be confusing)

## **git switch** - Only for branches

This command is  **only for switching branches** :

```bash
git switch main           # Go to main branch
git switch -c new-branch  # Create new branch and switch to it
```

## **git checkout** - Does many things

This command can do  **many different tasks** :

* Switch branches
* Restore files
* And much more

```bash
git checkout main              # Switch branch
git checkout -- file.txt       # Restore file to previous state
```

## 12. a. Merge using Git CLI (Local Merge)

### Push Branch to GitHub

```bash
git push origin staging
```

### Step 1: Switch to main branch

```bash
git switch main
```

### Step 2: Pull latest changes (safe practice)

```bash
git pull origin main
```

### Step 3: Merge staging into main

```bash
git merge staging
```

### Step 4: Push changes to GitHub

```bash
git push origin main
```

### 12. b. Merge Using GitHub Pull Request (Best Practice)

### Push staging branch to GitHub

```bash
git push origin staging
```

1. Go to GitHub
2. Create **Pull Request** > Click New Pull Request
3. Base: `main`
4. Compare: `staging`
5. Review → Merge

## 12.c Git Merge and Git Rebase

## Main Difference

**git merge** = Combines branches with history preserved (safe, realistic)
**git rebase** = Rewrites history for clean linear timeline (powerful, risky)

## **git merge** - Safe combination

```bash
git switch main
git merge feature-branch
```

---

## **git rebase** - Clean history

```bash
git switch feature-branch
git rebase main
```

## When to use what?

**Merge** = Public/shared branches, preserve history
**Rebase** = Local branches only, clean history

---

## 13. Git Logs

```bash
git log                              # Detailed history
git log --oneline                    # Every commit in online
git log --graph                      # Visual graph
git log --author="Name"              # Filter by author
git log --since="2 weeks ago"          # Filter by date
git log -n 5                          # Only last 5 commits
git reflog                            # All history actions
git blame file.txt                    # show the user history who change the code 

```

## Short & Visual

```bash
git log --oneline --graph --all --decorate
```

---

## 14. GIT Stashing (save your work in local)

```bash
git stash                    # Save Changes temporarily 
git stash pop               # Return Stashed changes 
git stash list              # All stashes show
git stash drop              # If you want to delete Stash 
```

## 15. Tagging(For Release versions)

```bash
git tag v1.0.0              # Create tag Version 
git tag -a v1.0.0 -m "msg"  # Annotated tag
git push origin v1.0.0      # Push Tag
git push --tags             # All tags push 
git tag -a v1.0.0 -m "Release version 1.0"

```

## 16. Diff & Status (Show the changes)

```bash
git status                  # Check Current state 
git diff                    # Unstaged changes
git diff --staged           # Staged changes
git diff main..feature      # Branch comparison
```

## 17. Reset & Revert (To fix your bugs)

* **reset** = Deletes history (dangerous)
* **revert** = Creates new commit to undo (safe)

```bash
git reset --soft HEAD~1     # Last commit undo (changes still remains)
git reset --hard HEAD~1     # Last commit undo (changes delete)
git revert commit-hash      # Safe undo (new commit create)
git clean -fd               # Untracked files delete
HEAD       # Current commit
HEAD~1     # Previous commit
HEAD~2     # 2 commits back
```

## 18.Cherry-pick (Apply specific commit from one branch to another.)

```bash

git cherry-pick commit-hash # one specific commit apply 
```

## 19. Git Hooks

Hooks are scripts that run **automatically** during Git commit actions.

Location:

```
.git/hooks/
```

Common Hooks:

- `pre-commit`
- `pre-push`
- `commit-msg`

---

## Pre-Commit Hook Example (Code Quality)

### Flake8 Code Check

```bash
files=$(git diff --cached --name-only --diff-filter=ACM)
flake8 $files
```

If code quality fails → **commit is blocked**.

---

## Prevent Secrets in Code (Security Hook)

```bash
if git grep -q "PASS_DB\|aws_secret_key\|API_KEY" $(git diff --cached --name-only); then
  echo "Do not commit secrets"
  exit 1
fi
```

---

## Pull Requests & Code Reviews on Github

- Review code before merging
- Catch bugs early
- Enforce coding standards
- Mandatory for production branches

---

## **Production Deployment Workflow Example**

# 1. Latest code pull

```bash
git fetch origin
git pull origin main
```

# 2. create new branch

```bash
git switch -c hotfix-123
```

# 3. Do Changes & create commit

```bash
git add .
git commit -m "Fix: critical bug"
```

# 4. Merge in Main branch

```bash
git switch main
git merge hotfix-123
```

# 5. Push on Production

```bash
git push origin main
```

# 6. you can use Tag (optional)

```bash
git tag v1.0.1
git push origin v1.0.1
```

---

## **Quick Commands Cheatsheet**


# Setup

git config --global user.name "Name"
git config --global user.email "email"

# Daily workflow

git status
git add .
git commit -m "message"
git push origin main
git pull origin main

# Branching

git switch -c feature
git merge feature
git branch -d feature

# Undo

git reset --soft HEAD~1
git revert HEAD
git stash

# History

git log --oneline --graph --all
git reflog
git blame file.txt

# Remote

git remote -v
git fetch
git push --tags
