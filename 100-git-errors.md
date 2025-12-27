# Top 100 Git Errors and Their Solutions

## Table of Contents

1. [Authentication &amp; Permission Errors (1-20)](#authentication--permission-errors-1-20)
2. [Branch &amp; Merge Errors (21-40)](#branch--merge-errors-21-40)
3. [Commit &amp; Repository Errors (41-60)](#commit--repository-errors-41-60)
4. [Push &amp; Pull Errors (61-80)](#push--pull-errors-61-80)
5. [Advanced Git Errors (81-100)](#advanced-git-errors-81-100)

---

## Authentication & Permission Errors (1-20)

### 1. Permission denied (403)

**Error:** `remote: Permission to user/repo.git denied to username`
### Solution

```bash
git remote set-url origin https://correct_username:token@github.com/user/repo.git
```

### 2. Authentication failed

**Error:** `Authentication failed for 'https://github.com/user/repo.git/'`
 ### Solution

```bash
# Use Personal Access Token
git config --global credential.helper store
git push  # Enter username and PAT as password
```

### 3. Repository not found

**Error:** `remote: Repository not found`
 ### Solution

```bash
# Check repository URL
git remote -v
git remote set-url origin https://github.com/correct_user/correct_repo.git
```

### 4. SSL certificate problem

**Error:** `SSL certificate problem: unable to get local issuer certificate`
  ### Solution

```bash
git config --global http.sslverify false
# Or better: update certificates
git config --global http.sslcainfo /path/to/certificates
```

### 5. Could not resolve host

**Error:** `Could not resolve host: github.com`
 ### Solution

```bash
# Check internet connection
ping github.com
# Check DNS settings
nslookup github.com
```

### 6. Access denied for user

**Error:** `access denied for user 'username'@'hostname'`
 ### Solution

```bash
# Clear cached credentials
git credential reject <<< $'protocol=https\nhost=github.com\n'
```

### 7. Invalid username or password

**Error:** `remote: Invalid username or password`
 ### Solution

```bash
# Use Personal Access Token instead of password
git config credential.helper store
# Re-enter credentials with PAT
```

### 8. Two-factor authentication required

**Error:** `remote: Invalid username or password (2FA enabled)`
 ### Solution

```bash
# Use PAT with 2FA enabled
# Generate token: GitHub → Settings → Developer Settings → PAT
```

### 9. Rate limit exceeded

**Error:** `API rate limit exceeded`
 ### Solution

```bash
# Wait for rate limit reset
# Use authenticated requests
git config --global user.name "username"
```

### 10. Organization access denied

**Error:** `Permission denied to organization/repo`
 ### Solution

```bash
# Check organization membership
# Request access from organization admin
# Verify PAT has correct organization access
```

### 11. SSH key not found

**Error:** `Permission denied (publickey)`
 ### Solution

```bash
ssh-keygen -t ed25519 -C "email@example.com"
ssh-add ~/.ssh/id_ed25519
# Add public key to GitHub
```

### 12. SSH agent not running

**Error:** `Could not open a connection to your authentication agent`
 ### Solution

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### 13. Wrong SSH key passphrase

**Error:** `Enter passphrase for key '/path/.ssh/id_rsa': Bad passphrase`
 ### Solution

```bash
ssh-keygen -p -f ~/.ssh/id_rsa  # Change passphrase
ssh-add -d ~/.ssh/id_rsa        # Remove from agent
ssh-add ~/.ssh/id_rsa           # Re-add with new passphrase
```

### 14. Git credential manager error

**Error:** `fatal: credential-manager-core not found`
 ### Solution

```bash
git config --global credential.helper manager-core
# Or install Git Credential Manager
```

### 15. Proxy authentication required

**Error:** `Proxy Authentication Required`
 ### Solution

```bash
git config --global http.proxy http://username:password@proxy:port
git config --global https.proxy https://username:password@proxy:port
```

### 16. Corporate firewall blocking

**Error:** `Failed to connect to github.com port 443`
 ### Solution

```bash
git config --global http.proxy http://proxy:port
git config --global url."https://".insteadOf git://
```

### 17. GPG signing failed

**Error:** `error: gpg failed to sign the data`
 ### Solution

```bash
git config --global commit.gpgsign false
# Or configure GPG properly
export GPG_TTY=$(tty)
```

### 18. Credential helper not found

**Error:** `git: 'credential-helper' is not a git command`
 ### Solution

```bash
git config --global credential.helper store
# Or for macOS:
git config --global credential.helper osxkeychain
```

### 19. Token expired

**Error:** `remote: Invalid access token`
 ### Solution

```bash
# Generate new Personal Access Token
# Update stored credentials
git credential reject <<< $'protocol=https\nhost=github.com\n'
```

### 20. Multiple SSH keys conflict

**Error:** `Permission denied (publickey)` with multiple keys
 ### Solution

```bash
# Create SSH config file ~/.ssh/config
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_work
```

---

## Branch & Merge Errors (21-40)

### 21. Branch already exists

**Error:** `fatal: A branch named 'branch-name' already exists`
 ### Solution

```bash
git checkout branch-name  # Switch to existing branch
# Or force create new:
git checkout -B branch-name
```

### 22. Cannot switch branch with uncommitted changes

**Error:** `error: Your local changes would be overwritten by checkout`
 ### Solution

```bash
git stash                    # Save changes temporarily
git checkout other-branch
git stash pop               # Restore changes
```

### 23. Merge conflict

**Error:** `CONFLICT (content): Merge conflict in file.txt`
 ### Solution

```bash
# Edit conflicted files manually
git add file.txt
git commit -m "Resolve merge conflict"
```

### 24. Cannot merge unrelated histories

**Error:** `fatal: refusing to merge unrelated histories`
 ### Solution

```bash
git merge --allow-unrelated-histories other-branch
```

### 25. Branch not found on remote

**Error:** `error: pathspec 'branch-name' did not match any file(s) known to git`
 ### Solution

```bash
git fetch origin                           # Fetch all branches
git checkout -b branch-name origin/branch-name  # Create local branch
```

### 26. Cannot delete current branch

**Error:** `error: Cannot delete branch 'main' checked out at`
 ### Solution

```bash
git checkout other-branch  # Switch to different branch first
git branch -d main        # Then delete
```

### 27. Branch contains unpushed commits

**Error:** `error: The branch 'branch-name' is not fully merged`
 ### Solution

```bash
git branch -D branch-name  # Force delete
# Or push first:
git push origin branch-name
```

### 28. Upstream branch not set

**Error:** `fatal: The current branch has no upstream branch`
 ### Solution

```bash
git push -u origin branch-name
```

### 29. Fast-forward merge not possible

**Error:** `hint: You have divergent branches and need to specify how to reconcile them`
 ### Solution

```bash
git config pull.rebase false  # merge
git config pull.rebase true   # rebase
git config pull.ff only       # fast-forward only
```

### 30. Cannot rebase with uncommitted changes

**Error:** `error: cannot rebase: You have unstaged changes`
 ### Solution

```bash
git stash
git rebase main
git stash pop
```

### 31. Rebase conflict

**Error:** `CONFLICT (content): Merge conflict during rebase`
 ### Solution

```bash
# Fix conflicts in files
git add .
git rebase --continue
# Or abort: git rebase --abort
```

### 32. Cherry-pick conflict

**Error:** `error: could not apply commit... conflict`
 ### Solution

```bash
# Resolve conflicts
git add .
git cherry-pick --continue
```

### 33. No common ancestor

**Error:** `fatal: no merge base found`
 ### Solution

```bash
git merge --allow-unrelated-histories branch-name
```

### 34. Branch tracking wrong remote

**Error:** `Your branch is ahead of 'origin/main' by N commits`
 ### Solution

```bash
git branch --set-upstream-to=origin/correct-branch
```

### 35. Cannot create branch from detached HEAD

**Error:** `You are in 'detached HEAD' state`
 ### Solution

```bash
git checkout -b new-branch-name  # Create branch from current state
```

### 36. Merge tool not configured

**Error:** `merge tool candidates: ... No such file or directory`
 ### Solution

```bash
git config --global merge.tool vimdiff
# Or install and configure: meld, kdiff3, etc.
```

### 37. Invalid branch name

**Error:** `fatal: 'branch/name' is not a valid branch name`
 ### Solution

```bash
# Use valid characters only: alphanumeric, dash, underscore
git checkout -b valid-branch-name
```

### 38. Cannot merge into current branch

**Error:** `error: Trying to write non-commit object to branch refs/heads/main`
 ### Solution

```bash
git checkout main
git merge feature-branch  # Merge from main branch
```

### 39. Recursive merge strategy failed

**Error:** `Recursive merge strategy-option failed`
 ### Solution

```bash
git merge -X ours feature-branch    # Prefer current branch
git merge -X theirs feature-branch  # Prefer incoming branch
```

### 40. Branch protection rule violation

**Error:** `remote: error: GH006: Protected branch update failed`
 ### Solution

```bash
# Create pull request instead of direct push
# Or ask admin to temporarily disable protection
```

---

## Commit & Repository Errors (41-60)

### 41. Nothing to commit

**Error:** `nothing to commit, working tree clean`
 ### Solution

```bash
git status              # Check what's staged
git add file.txt        # Stage changes
git commit -m "message"
```

### 42. Empty commit message

**Error:** `Aborting commit due to empty commit message`
 ### Solution

```bash
git commit -m "Add meaningful commit message"
git commit --allow-empty-message  # If really needed
```

### 43. Invalid object name

**Error:** `fatal: invalid object name 'HEAD'`
 ### Solution

```bash
# Usually means no commits exist yet
git add .
git commit -m "Initial commit"
```

### 44. Repository not found locally

**Error:** `fatal: not a git repository`
 ### Solution

```bash
cd correct-directory
# Or initialize new repo:
git init
```

### 45. Corrupted repository

**Error:** `fatal: bad object HEAD`
 ### Solution

```bash
git fsck --full                    # Check for corruption
git gc                            # Garbage collection
git reflog expire --expire=now --all  # If needed
```

### 46. File too large

**Error:** `remote: error: File file.zip is 123.45 MB; this exceeds GitHub's file size limit`
 ### Solution

```bash
git rm --cached large-file.zip
echo "large-file.zip" >> .gitignore
git add .gitignore
git commit -m "Remove large file"
```

### 47. Invalid commit hash

**Error:** `fatal: bad revision 'invalid-hash'`
 ### Solution

```bash
git log --oneline      # Find correct hash
git show correct-hash  # Use valid commit hash
```

### 48. Commit author not set

**Error:** `*** Please tell me who you are`
 ### Solution

```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

### 49. Pre-commit hook failed

**Error:** `pre-commit hook failed`
 ### Solution

```bash
# Fix the issues mentioned by the hook
# Or skip hooks (not recommended):
git commit --no-verify -m "message"
```

### 50. Repository is bare

**Error:** `fatal: this operation must be run in a work tree`
 ### Solution

```bash
# Clone to working directory:
git clone /path/to/bare/repo.git working-copy
```

### 51. Gitignore not working

**Error:** Files still tracked despite .gitignore
 ### Solution

```bash
git rm --cached filename
git add .gitignore
git commit -m "Update gitignore"
```

### 52. Line ending issues

**Error:** `warning: CRLF will be replaced by LF`
 ### Solution

```bash
git config --global core.autocrlf true   # Windows
git config --global core.autocrlf input  # Mac/Linux
```

### 53. File permission changes

**Error:** `old mode 100644 new mode 100755`
 ### Solution

```bash
git config core.filemode false  # Ignore permission changes
```

### 54. Repository too large

**Error:** `remote: error: pack exceeds maximum allowed size`
 ### Solution

```bash
# Use Git LFS for large files
git lfs track "*.zip"
git add .gitattributes
git lfs push origin main
```

### 55. Index lock file exists

**Error:** `fatal: Unable to create index.lock: File exists`
 ### Solution

```bash
rm .git/index.lock
```

### 56. HEAD detached

**Error:** `You are in 'detached HEAD' state`
 ### Solution

```bash
git checkout main              # Return to main branch
# Or create branch from current state:
git checkout -b new-branch
```

### 57. Submodule initialization failed

**Error:** `fatal: could not get a repository handle for submodule`
 ### Solution

```bash
git submodule update --init --recursive
```

### 58. Working tree has modifications

**Error:** `error: Your local changes would be overwritten`
 ### Solution

```bash
git stash                 # Save changes
git pull                  # Update
git stash pop            # Restore changes
```

### 59. Invalid gitconfig

**Error:** `fatal: bad config line N in file .gitconfig`
 ### Solution

```bash
# Edit .gitconfig file and fix syntax error at line N
git config --global --edit
```

### 60. Shallow repository limitation

**Error:** `fatal: attempt to fetch/clone from a shallow repository`
 ### Solution

```bash
git fetch --unshallow origin
```

---

## Push & Pull Errors (61-80)

### 61. Non-fast-forward push rejected

**Error:** `Updates were rejected because the tip of your current branch is behind`
 ### Solution

```bash
git pull origin main      # Pull first
git push origin main      # Then push
# Or force push: git push --force-with-lease
```

### 62. Push rejected due to hook

**Error:** `remote: error: hook declined`
 ### Solution

```bash
# Fix the issue mentioned by the remote hook
# Check with repository admin about hook requirements
```

### 63. No upstream configured

**Error:** `fatal: The current branch has no upstream branch`
 ### Solution

```bash
git push -u origin branch-name
```

### 64. Repository access denied

**Error:** `remote: Permission to user/repo.git denied`
 ### Solution

```bash
# Check repository permissions
# Use correct credentials/PAT
git remote set-url origin https://username:token@github.com/user/repo.git
```

### 65. Push size limit exceeded

**Error:** `remote: error: pack exceeds maximum allowed size`
 ### Solution

```bash
# Split large commit into smaller ones
git rebase -i HEAD~N  # Squash/split commits
# Or use Git LFS for large files
```

### 66. Branch protection prevents push

**Error:** `remote: error: GH006: Protected branch update failed`
 ### Solution

```bash
# Create pull request instead
# Push to different branch first:
git push origin feature-branch
```

### 67. Pull with uncommitted changes

**Error:** `error: Your local changes would be overwritten by merge`
 ### Solution

```bash
git stash        # Save changes
git pull         # Update
git stash pop    # Restore changes
```

### 68. Merge conflict during pull

**Error:** `CONFLICT (content): Merge conflict in file`
 ### Solution

```bash
# Resolve conflicts in files
git add resolved-file
git commit -m "Resolve merge conflicts"
```

### 69. Remote branch deleted

**Error:** `error: unable to delete 'refs/remotes/origin/branch': remote ref does not exist`
 ### Solution

```bash
git remote prune origin        # Clean up deleted remote branches
git branch -d local-branch     # Delete local branch
```

### 70. Push to wrong remote

**Error:** `error: src refspec main does not match any`
 ### Solution

```bash
git remote -v                  # Check remotes
git remote add origin https://github.com/user/repo.git
git push -u origin main
```

### 71. Large file push rejected

**Error:** `remote: error: File file.pdf is 234.56 MB`
 ### Solution

```bash
git rm --cached large-file.pdf
git lfs track "*.pdf"
git add .gitattributes large-file.pdf
git commit -m "Use LFS for large file"
```

### 72. Push hook timeout

**Error:** `remote: fatal: The remote end hung up unexpectedly`
 ### Solution

```bash
git config --global http.postBuffer 524288000  # Increase buffer
git push origin main
```

### 73. Insufficient storage space

**Error:** `remote: error: insufficient disk space`
 ### Solution

```bash
# Contact repository admin
# Or reduce repository size:
git gc --aggressive --prune=now
```

### 74. Network connection issues

**Error:** `fatal: unable to access 'https://github.com/': Could not resolve host`
 ### Solution

```bash
# Check internet connection
ping github.com
# Try SSH instead:
git remote set-url origin git@github.com:user/repo.git
```

### 75. Pull with merge conflicts

**Error:** `Automatic merge failed; fix conflicts and then commit the result`
 ### Solution

```bash
# Edit conflicted files
git add .
git commit -m "Resolve merge conflicts"
```

### 76. Push with missing commits

**Error:** `error: failed to push some refs to remote`
 ### Solution

```bash
git pull --rebase origin main  # Rebase instead of merge
git push origin main
```

### 77. Remote repository not found

**Error:** `fatal: repository 'https://github.com/user/repo.git/' not found`
 ### Solution

```bash
# Check repository URL
git remote set-url origin https://github.com/correct-user/correct-repo.git
```

### 78. Pull request merge conflict

**Error:** `This branch has conflicts that must be resolved`
 ### Solution

```bash
git checkout main
git pull origin main
git checkout feature-branch
git merge main              # Resolve conflicts locally
git push origin feature-branch
```

### 79. Force push protection

**Error:** `remote: error: denying non-fast-forward refs/heads/main`
 ### Solution

```bash
# Use --force-with-lease for safer force push
git push --force-with-lease origin main
# Or create new branch and PR
```

### 80. Multiple remotes confusion

**Error:** `fatal: 'origin' does not appear to be a git repository`
 ### Solution

```bash
git remote -v               # List remotes
git remote add origin https://github.com/user/repo.git
git remote remove old-origin  # Remove incorrect remote
```

---

## Advanced Git Errors (81-100)

### 81. Submodule update failed

**Error:** `fatal: Needed a single revision`
 ### Solution

```bash
git submodule update --init --recursive --force
git submodule sync --recursive
```

### 82. Git LFS pointer file issue

**Error:** `Error downloading object: batch response missing`
 ### Solution

```bash
git lfs fetch --all
git lfs checkout
git lfs pull
```

### 83. Worktree operation failed

**Error:** `fatal: 'path' is already checked out`
 ### Solution

```bash
git worktree list           # Check existing worktrees
git worktree remove path    # Remove if needed
git worktree add path branch
```

### 84. Partial clone fetch error

**Error:** `remote: error: unable to read sha1 file`
 ### Solution

```bash
git fetch --unshallow origin
git fsck --full
```

### 85. Sparse checkout configuration error

**Error:** `error: sparse-checkout file 'info/sparse-checkout' not found`
 ### Solution

```bash
git config core.sparseCheckout true
echo "folder/*" > .git/info/sparse-checkout
git read-tree -m -u HEAD
```

### 86. Git hook execution failed

**Error:** `error: cannot run hooks/pre-push: No such file or directory`
 ### Solution

```bash
chmod +x .git/hooks/pre-push  # Make hook executable
# Or disable hook temporarily:
git push --no-verify
```

### 87. Attributes file syntax error

**Error:** `fatal: bad config line in '.gitattributes'`
 ### Solution

```bash
# Fix syntax in .gitattributes file
# Format: pattern attr1=value1 attr2=value2
```

### 88. Bundle create/verify failed

**Error:** `fatal: bundle header unrecognized`
 ### Solution

```bash
git bundle verify bundle-file
git bundle create fresh-bundle.bundle --all
```

### 89. Filter-branch operation failed

**Error:** `WARNING: filter-branch has a glut of gotchas`
 ### Solution

```bash
# Use git-filter-repo instead (modern alternative)
pip install git-filter-repo
git filter-repo --path wanted-file.txt
```

### 90. Patch apply failed

**Error:** `error: patch failed`
 ### Solution

```bash
git apply --3way patch-file.patch  # Three-way merge
# Or use git am for email patches:
git am < patch-file.patch
```

### 91. Reflog corruption

**Error:** `fatal: bad object in reflog`
 ### Solution

```bash
rm .git/logs/refs/heads/branch-name
git reflog expire --expire=now --all
git gc --prune=now
```

### 92. Bisect operation stuck

**Error:** `error: terminating unsuccessful bisect`
 ### Solution

```bash
git bisect reset          # Reset bisect state
git bisect start HEAD commit-hash  # Restart properly
```

### 93. Archive creation failed

**Error:** `fatal: pathspec 'path' did not match any files`
 ### Solution

```bash
git archive --format=zip HEAD -o archive.zip  # Archive entire HEAD
git archive --format=tar --prefix=project/ HEAD | gzip > archive.tar.gz
```

### 94. Mailmap configuration error

**Error:** `warning: .mailmap entry has no mapping`
 ### Solution

```bash
# Fix .mailmap format:
# Proper Name <proper@email.com> Old Name <old@email.com>
```

### 95. Performance issues with large repos

**Error:** `warning: You appear to be on a large repository`
 ### Solution

```bash
git config core.preloadindex true
git config core.fscache true
git config gc.auto 256
git maintenance start  # Git 2.30+
```

### 96. Unicode filename issues

**Error:** `warning: could not open directory 'unicode-name'`
 ### Solution

```bash
git config core.quotepath false
git config core.precomposeunicode true  # macOS
```

### 97. Case sensitivity issues

**Error:** `fatal: destination path 'File' already exists`
 ### Solution

```bash
git config core.ignorecase false
git mv file.txt temp.txt
git mv temp.txt File.txt
```

### 98. Symbolic link handling

**Error:** `error: unable to create symlink`
 ### Solution

```bash
git config core.symlinks false  # Windows
# Or run as administrator on Windows
```

### 99. Git daemon connection refused

**Error:** `fatal: Unable to look up localhost`
 ### Solution

```bash
# Check git daemon is running:
git daemon --base-path=/path/to/repos --export-all --reuseaddr --informative-errors --verbose
```

### 100. Memory allocation error

**Error:** `fatal: Out of memory, malloc failed`
 ### Solution

```bash
git config pack.windowMemory 256m
git config pack.packSizeLimit 2g
git config pack.deltaCacheSize 128m
git gc --aggressive
```

---

## Quick Reference Commands for Troubleshooting

### General Diagnostics

```bash
git --version                    # Check Git version
git config --list              # Check configuration
git status                      # Repository status
git log --oneline -10          # Recent commits
git remote -v                  # Check remotes
git branch -a                  # All branches
git fsck                       # Check repository integrity
```

### Recovery Commands

```bash
git reflog                     # See command history
git reset --hard HEAD@{N}     # Reset to previous state
git clean -fd                 # Remove untracked files
git stash                      # Save current changes
git stash pop                  # Restore stashed changes
```

### Emergency Reset

```bash
# Nuclear option - backup first!
cp -r .git .git-backup
git reset --hard HEAD
git clean -fdx
git gc --aggressive --prune=now
```

---

*This comprehensive guide covers the most common Git errors encountered by developers. Keep this as a reference for quick problem-solving.*
