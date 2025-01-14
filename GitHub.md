# Git and GitHub Commands Reference Guide

## Initial Setup and Configuration

### First-Time Git Setup
```bash
# Configure user information
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Configure default branch name
git config --global init.defaultBranch main

# View all configurations
git config --list
```

## Basic Git Commands

### Repository Setup
```bash
# Initialize new repository
git init

# Clone existing repository
git clone URL
git clone URL custom-folder-name

# Add remote repository
git remote add origin URL
git remote -v  # View remote repositories
```

### Basic Snapshotting
```bash
# Check status
git status

# Stage changes
git add filename           # Stage specific file
git add .                 # Stage all changes
git add *.js             # Stage with pattern
git add -p               # Stage changes interactively

# Commit changes
git commit -m "Commit message"
git commit -am "Add and commit message"  # Stage tracked files and commit
git commit --amend       # Modify last commit
```

### Branching and Merging
```bash
# Branch operations
git branch                  # List branches
git branch branch-name     # Create branch
git branch -d branch-name  # Delete branch
git branch -D branch-name  # Force delete branch
git branch -m new-name    # Rename current branch

# Switching branches
git checkout branch-name   # Switch to branch
git checkout -b new-branch # Create and switch to new branch
git switch branch-name    # Switch to branch (new command)
git switch -c new-branch  # Create and switch to new branch

# Merging
git merge branch-name     # Merge branch into current branch
git merge --abort        # Abort merge in case of conflicts
```

### Inspecting Changes
```bash
# View changes
git log                   # View commit history
git log --oneline        # Compact log view
git log --graph          # Graph view
git show commit-id       # Show specific commit
git diff                 # View unstaged changes
git diff --staged       # View staged changes
```

## Advanced Git Commands

### Stashing
```bash
# Stash operations
git stash                 # Stash changes
git stash save "message"  # Stash with message
git stash list           # List stashes
git stash pop            # Apply and remove stash
git stash apply         # Apply without removing
git stash drop          # Remove stash
git stash clear         # Clear all stashes
```

### History Modification
```bash
# Rewriting history
git rebase branch-name           # Rebase current branch
git rebase -i HEAD~3            # Interactive rebase
git reset HEAD~1                # Undo last commit (soft)
git reset --hard HEAD~1         # Undo last commit (hard)
git revert commit-id           # Create new commit that undoes changes
```

### Remote Operations
```bash
# Remote repository operations
git fetch origin              # Download objects and refs
git pull origin branch-name   # Fetch and merge changes
git push origin branch-name   # Push changes to remote
git push -u origin branch    # Set upstream branch
git remote prune origin      # Remove deleted remote branches
```

## GitHub Specific Commands and Operations

### GitHub CLI
```bash
# Authentication
gh auth login               # Login to GitHub
gh auth status             # Check auth status

# Repository operations
gh repo create             # Create new repository
gh repo clone owner/repo   # Clone repository
gh repo fork              # Fork repository
gh repo view              # View repository details
```

### Pull Requests
```bash
# Create and manage pull requests
gh pr create              # Create pull request
gh pr list               # List pull requests
gh pr checkout number    # Checkout pull request
gh pr review            # Review pull request
gh pr merge             # Merge pull request
```

### Issues
```bash
# Issue management
gh issue create          # Create issue
gh issue list           # List issues
gh issue view number    # View specific issue
gh issue close number   # Close issue
```

## Best Practices

1. Commit Messages
   - Write clear, concise commit messages
   - Use present tense ("Add feature" not "Added feature")
   - First line should be 50 characters or less
   - Include detailed description if needed after blank line

2. Branching
   - Use feature branches for new development
   - Keep main/master branch stable
   - Delete merged branches
   - Use descriptive branch names

3. Pull Requests
   - Keep PRs focused and small
   - Include detailed descriptions
   - Reference related issues
   - Request reviews from appropriate team members

## Common Workflows

### Feature Branch Workflow
```bash
git checkout main
git pull origin main
git checkout -b feature-branch
# Make changes
git add .
git commit -m "Add feature"
git push -u origin feature-branch
# Create pull request on GitHub
```

### Fixing Mistakes
```bash
# Undo staged changes
git reset filename

# Discard local changes
git checkout -- filename

# Undo commits
git reset --soft HEAD~1    # Keep changes staged
git reset --mixed HEAD~1   # Keep changes unstaged
git reset --hard HEAD~1    # Discard changes
```

## Troubleshooting

1. Authentication Issues
   - Check remote URL format
   - Verify SSH keys or tokens
   - Ensure correct permissions

2. Merge Conflicts
   - Use `git status` to identify conflicting files
   - Resolve conflicts in editor
   - Stage resolved files
   - Complete merge with commit

3. Large Files
   - Use Git LFS for large files
   - Configure `.gitignore` properly
   - Clean repository with `git gc`

## Security Best Practices

1. Never commit sensitive data
2. Use `.gitignore` for sensitive files
3. Regularly rotate access tokens
4. Use signed commits when possible
5. Review repository access permissions regularly
