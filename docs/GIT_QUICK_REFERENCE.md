# Git Quick Reference Guide

## Essential Git Commands at a Glance

### Setup & Configuration
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --list                    # View all settings
```

### Repository Basics
```bash
git init                             # Initialize new repository
git clone <url>                      # Clone existing repository
git status                           # Check status
git log --oneline                    # View commit history
```

### Staging & Committing
```bash
git add <file>                       # Stage a file
git add .                            # Stage all changes
git commit -m "message"              # Commit changes
git commit --amend                   # Modify last commit
```

### Branching
```bash
git branch                           # List branches
git branch <branch-name>             # Create branch
git checkout <branch-name>           # Switch branch
git checkout -b <branch-name>        # Create & switch branch
git branch -d <branch-name>          # Delete branch
```

### Merging & Rebasing
```bash
git merge <branch-name>              # Merge branch
git rebase <branch-name>             # Rebase onto branch
git merge --abort                    # Cancel merge
```

### Remote Operations
```bash
git remote -v                        # List remotes
git push origin <branch>             # Push to remote
git pull origin <branch>             # Fetch & merge
git fetch origin                     # Fetch without merging
```

### Advanced Operations
```bash
git stash                            # Save work temporarily
git cherry-pick <commit>             # Apply specific commit
git revert <commit>                  # Undo commit (safe)
git reset --soft HEAD~1              # Undo commit, keep changes
git reset --hard HEAD~1              # Completely undo commit
```

### Viewing Changes
```bash
git diff                             # Show unstaged changes
git diff --staged                    # Show staged changes
git diff <branch1> <branch2>         # Compare branches
git show <commit>                    # Show commit details
```

### Undoing Changes
```bash
git checkout -- <file>               # Discard file changes
git restore <file>                   # Restore file (newer syntax)
git clean -fd                        # Remove untracked files
```

---

## Common Scenarios

### Scenario 1: Oops, committed to wrong branch
```bash
git reset --soft HEAD~1              # Undo commit, keep changes
git checkout -b correct-branch       # Create correct branch
git commit -m "message"              # Commit to correct branch
```

### Scenario 2: Need to pull latest changes
```bash
git stash                            # Save your work
git pull origin main                 # Get latest
git stash pop                        # Restore your work
```

### Scenario 3: Fix a bug in production
```bash
git checkout main                    # Go to main
git pull origin main                 # Get latest
git checkout -b hotfix-bug-123       # Create hotfix branch
# ... fix bug ...
git commit -am "Fix bug #123"        # Commit fix
git push origin hotfix-bug-123       # Push to remote
# Create Pull Request
```

### Scenario 4: Backport fix to older version
```bash
git log --oneline main | grep "Fix" # Find fix commit
git checkout release-1.0             # Switch to old version
git cherry-pick abc1234              # Apply fix
git push origin release-1.0          # Push to remote
```

---

## Command Cheat Sheet

| Task | Command |
|------|---------|
| Create repo | `git init` |
| Clone repo | `git clone <url>` |
| Check status | `git status` |
| Stage files | `git add <file>` or `git add .` |
| Commit | `git commit -m "message"` |
| Create branch | `git checkout -b <branch>` |
| Switch branch | `git checkout <branch>` |
| Merge branch | `git merge <branch>` |
| Push changes | `git push origin <branch>` |
| Pull changes | `git pull origin <branch>` |
| View history | `git log --oneline` |
| Save work | `git stash` |
| Restore work | `git stash pop` |
| Undo commit | `git reset --soft HEAD~1` |
| Undo changes | `git checkout -- <file>` |

---

## Git Aliases for Efficiency

Add these to your `.gitconfig`:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --graph --oneline --all'
```

Then use:
```bash
git st                               # git status
git co <branch>                      # git checkout
git cm -m "msg"                      # git commit
git unstage <file>                   # Remove from staging
git last                             # Show last commit
git visual                           # View commit graph
```

---

**Pro Tips:**
- Use `git status` frequently to stay aware
- Write clear, descriptive commit messages
- Pull before pushing to avoid conflicts
- Use branches for all features
- Keep commits small and focused

---

**Last Updated:** August 25, 2026
