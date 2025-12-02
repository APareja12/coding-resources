# Git Rebase

## Overview

Git rebase is a powerful command that moves or combines a sequence of commits to a new base commit. Unlike merge, which creates a new commit that ties together two branches, rebase rewrites the project history by creating brand new commits for each commit in the original branch. This results in a cleaner, linear project history that's easier to understand and navigate.

## Key Concepts

- **Linear history** - Creates a straight line of commits instead of merge diamonds
- **Rewriting history** - Changes commit hashes, so use carefully on shared branches
- **Interactive rebase** - Lets you edit, squash, reorder, or delete commits
- **Golden rule** - Never rebase commits that have been pushed to a public/shared branch
- **Conflicts** - May need to resolve conflicts for each commit being rebased
- **Fast-forward merge** - After rebasing, merging becomes a simple fast-forward

## Code Example

```bash
# Basic rebase workflow
# You're on feature branch, want to update with latest main
git checkout feature-branch
git rebase main

# What this does:
# 1. Finds the common ancestor of feature-branch and main
# 2. Saves your feature-branch commits temporarily
# 3. Resets feature-branch to match main
# 4. Replays your commits one by one on top of main

# If conflicts occur during rebase:
# 1. Fix the conflicts in your editor
git add <resolved-files>
git rebase --continue

# Or skip the problematic commit:
git rebase --skip

# Or abort the entire rebase:
git rebase --abort

# Interactive rebase - clean up your commits before pushing
git rebase -i HEAD~3  # Rebase last 3 commits

# This opens an editor with options:
# pick abc1234 Add feature A
# pick def5678 Fix typo
# pick ghi9012 Add feature B
#
# Commands:
# pick = use commit
# reword = change commit message
# edit = stop to amend commit
# squash = combine with previous commit
# fixup = like squash but discard this commit's message
# drop = remove commit

# Example: Squashing commits
# Change:
# pick abc1234 Add feature A
# pick def5678 Fix typo
# pick ghi9012 Add feature B
#
# To:
# pick abc1234 Add feature A
# squash def5678 Fix typo
# pick ghi9012 Add feature B

# Rebase onto a different branch
git rebase --onto new-base old-base feature-branch

# Pull with rebase instead of merge
git pull --rebase origin main

# Make pull always use rebase
git config pull.rebase true
```

```bash
# Practical example: Cleaning up before pull request
# You have messy commits on feature-branch:
# - "WIP"
# - "Fix bug"
# - "More fixes"
# - "Actually fix it this time"
# - "Add feature complete"

git checkout feature-branch
git rebase -i HEAD~5

# In the editor, squash the fix commits:
pick abc1234 Add feature complete
squash def5678 Actually fix it this time
squash ghi9012 More fixes
squash jkl3456 Fix bug
squash mno7890 WIP

# Save and close, write a clean commit message:
# "Add feature X with proper error handling"

# Now you have one clean commit instead of five messy ones!
git push --force-with-lease origin feature-branch
```

```bash
# Real workflow example
# Start feature
git checkout -b add-login-feature

# Make several commits
git commit -m "Add login form"
git commit -m "WIP styling"
git commit -m "Fix validation"
git commit -m "Add tests"

# Main branch has moved ahead
git checkout main
git pull origin main

# Rebase your feature onto latest main
git checkout add-login-feature
git rebase main

# Clean up commits before PR
git rebase -i HEAD~4  # Squash the WIP commits

# Push (need force since we rewrote history)
git push --force-with-lease origin add-login-feature
```

## Common Use Cases

- **Updating feature branches** - Keep your branch up-to-date with main without merge commits
- **Cleaning commit history** - Squash "fix typo" and "WIP" commits before submitting PR
- **Linear history** - Keep project history clean and easy to follow
- **Fixing commit messages** - Reword unclear or incorrect commit messages
- **Reordering commits** - Organize commits logically before merging
- **Removing sensitive data** - Delete commits that accidentally included secrets

## Additional Resources

- [Git Documentation: git-rebase](https://git-scm.com/docs/git-rebase)
- [Atlassian: Merging vs Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [Interactive Rebase Tutorial](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
- [The Golden Rule of Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing)
