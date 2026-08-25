# Git Troubleshooting Guide

## Common Git Issues and Solutions

### 1. Accidentally Committed to Wrong Branch

**Problem:** You made commits on `main` instead of `feature-branch`

**Solution:**
```bash
# See what you committed
git log --oneline -3

# Save those commits to a branch
git branch feature-branch

# Reset main to the state before your commits
git reset --hard origin/main

# Switch to your feature branch with the commits
git checkout feature-branch
```

---

### 2. Need to Undo the Last Commit

**Option A: Keep Changes (Safest)**
```bash
git reset --soft HEAD~1
# Changes remain staged, ready to re-commit
```

**Option B: Unstage Changes**
```bash
git reset HEAD~1
# Changes remain in working directory, unstaged
```

**Option C: Completely Discard (Dangerous)**
```bash
git reset --hard HEAD~1
# All changes are lost!
```

**Recover After Mistake:**
```bash
git reflog                           # Find the commit hash
git reset --hard <commit-hash>       # Restore to that point
```

---

### 3. Merge Conflicts

**When Pulling:**
```bash
git pull origin main
# Conflict occurs!

# View conflicted files
git status

# Edit conflicted files (look for <<<<<<, ======, >>>>>> markers)
# After fixing conflicts:
git add <fixed-file>
git commit -m "Resolve merge conflicts"
git push origin <branch>
```

**When Merging Branches:**
```bash
git merge feature-branch
# Conflict!

# Resolve manually or use merge tool
git mergetool

# After resolution
git add <file>
git commit -m "Merge feature-branch"
```

**Abort Merge if Needed:**
```bash
git merge --abort
```

---

### 4. Cherry-Pick Conflicts

**Problem:** Cherry-pick fails due to conflicts

```bash
git cherry-pick abc1234
# Conflict detected!

# Resolve conflicts
# Edit the conflicted files

git add <resolved-files>
git cherry-pick --continue

# Or abort
git cherry-pick --abort
```

---

### 5. Lost Commits or Changes

**Find Lost Work:**
```bash
git reflog
# Shows history of HEAD changes
# Output example:
# abc1234 HEAD@{0}: reset: moving to HEAD~1
# def5678 HEAD@{1}: commit: My feature
# ghi9012 HEAD@{2}: checkout: switching to main

# Restore to the lost commit
git reset --hard def5678
```

---

### 6. Stash Issues

**Can't Remember What's in Stash:**
```bash
git stash list
# View all stashes
# Output: stash@{0}: WIP: feature X
#         stash@{1}: WIP: bug fix

# See what's in a specific stash
git stash show stash@{0}
# Shows files changed
git stash show -p stash@{0}
# Shows actual changes
```

**Applied Stash Multiple Times:**
```bash
git stash list
git stash drop stash@{0}  # Remove the one you don't need
```

---

### 7. Detached HEAD State

**Problem:** You're in "detached HEAD" state

```bash
# This happens when you checkout a commit directly
git checkout abc1234
# HEAD detached at abc1234

# Fix: Create a branch from current position
git checkout -b save-my-work

# Then switch to your main branch
git checkout main
```

---

### 8. File Accidentally Deleted

**Restore Recent Delete:**
```bash
git checkout HEAD -- <filename>
# Restores from last commit
```

**Restore from Specific Commit:**
```bash
git log --oneline -- <filename>
# Find when file was deleted

git checkout <commit-hash>~1 -- <filename>
# Restore from before deletion
```

---

### 9. Undo Pushed Commits (Public Branch)

**Safe Method: Use Revert**
```bash
# Find the commit to undo
git log --oneline

# Revert (creates new commit that undoes changes)
git revert abc1234
git push origin main
```

**Dangerous Method: Force Push (Use Carefully!)**
```bash
git reset --hard HEAD~1
git push origin main --force
# ⚠️ Only if no one else is working on this branch!
```

---

### 10. Accidentally Pushed Sensitive Data

**Immediate Action:**
```bash
# Revert the commit
git revert <commit-with-secret>
git push origin main

# Rotate your secrets/passwords immediately
# The file history is still there, but the current version is clean
```

**Nuclear Option: Rewrite History**
```bash
# Use BFG or git-filter-branch to remove from history
# This requires force-push and coordination with team
git filter-branch --tree-filter 'rm -f <secret-file>' HEAD
git push origin main --force-with-lease
```

---

### 11. Wrong Remote URL

**Check Remote:**
```bash
git remote -v
# View all remotes
```

**Update Remote:**
```bash
git remote set-url origin <new-url>
git remote -v
# Verify change
```

---

### 12. Cannot Pull - Local Changes

**Solution:**
```bash
# Option 1: Stash your changes
git stash
git pull origin main
git stash pop

# Option 2: Commit first, then pull
git commit -am "WIP: my changes"
git pull origin main
# Resolve any conflicts

# Option 3: Hard pull (discard local changes)
git fetch origin
git reset --hard origin/main
```

---

### 13. Rebase Issues

**Rebase Conflicts:**
```bash
git rebase main
# Conflict during rebase!

# Resolve conflicts
# Then continue
git add <resolved-files>
git rebase --continue

# Or abort
git rebase --abort
```

---

### 14. Large File Committed

**Remove from History:**
```bash
# Remove from last commit only
git rm --cached <large-file>
git commit --amend
git push origin main

# Add to .gitignore for future
echo "large-file.zip" >> .gitignore
git add .gitignore
git commit -m "Ignore large files"
```

---

## Prevention Tips

| Problem | Prevention |
|---------|-----------|
| Wrong branch commits | Check `git status` before committing |
| Merge conflicts | Pull frequently, communicate with team |
| Lost work | Use `git stash` before switching branches |
| Accidental force push | Use `--force-with-lease` instead of `--force` |
| Sensitive data leak | Use `.gitignore`, pre-commit hooks, and secret scanning |
| Large files | Configure Git LFS or add to `.gitignore` |

---

## Emergency Recovery

**Last Resort - Reflog:**
```bash
git reflog                           # See all recent actions
git reset --hard <ref>@{<number>}    # Restore to that point
```

**Most problems can be recovered with `git reflog`!**

---

**Last Updated:** August 25, 2026
