# Git and GitHub Commands Cheat Sheet

This cheat sheet provides a comprehensive list of essential Git and GitHub commands, categorized for ease of use.

---

## 1. Setup and Configuration

- Configure user name and email:
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "youremail@example.com"
  ```
- Check current configuration:
  ```bash
  git config --list
  ```

---

## 2. Repository Management

- Initialize a new Git repository:
  ```bash
  git init
  ```
- Clone an existing repository:
  ```bash
  git clone <repository-url>
  ```

---

## 3. Staging and Committing Changes

- Check the status of the repository:
  ```bash
  git status
  ```
- Track new files or stage changes:
  ```bash
  git add <file-name>       # Add a specific file
  git add .                 # Add all files
  ```
- Commit staged changes:
  ```bash
  git commit -m "Commit message"
  ```
- Amend the last commit:
  ```bash
  git commit --amend
  ```

---

## 4. Branch Management

- List all branches:
  ```bash
  git branch
  ```
- Create a new branch:
  ```bash
  git branch <branch-name>
  ```
- Switch to another branch:
  ```bash
  git checkout <branch-name>
  ```
- Create and switch to a new branch:
  ```bash
  git checkout -b <branch-name>
  ```
- Delete a branch:
  ```bash
  git branch -d <branch-name>  # Only if fully merged
  git branch -D <branch-name>  # Force delete
  ```

---

## 5. Synchronization with Remote

- Add a remote repository:
  ```bash
  git remote add origin <repository-url>
  ```
- List remote repositories:
  ```bash
  git remote -v
  ```
- Push changes to remote:
  ```bash
  git push origin <branch-name>
  ```
- Pull changes from remote:
  ```bash
  git pull origin <branch-name>
  ```
- Fetch changes (without merging):
  ```bash
  git fetch
  ```

---

## 6. Merging and Rebasing

- Merge branches:
  ```bash
  git merge <branch-name>
  ```
- Rebase a branch:
  ```bash
  git rebase <branch-name>
  ```
- Abort a rebase:
  ```bash
  git rebase --abort
  ```

---

## 7. Undoing Changes

- Undo unstaged changes:
  ```bash
  git checkout -- <file-name>
  ```
- Unstage changes:
  ```bash
  git reset <file-name>
  ```
- Reset to a specific commit:
  ```bash
  git reset --hard <commit-hash>
  ```
- Revert a commit:
  ```bash
  git revert <commit-hash>
  ```

---

## 8. Viewing History and Logs

- View commit history:
  ```bash
  git log
  git log --oneline
  ```
- Show changes in a specific commit:
  ```bash
  git show <commit-hash>
  ```
- View differences between commits:
  ```bash
  git diff <commit-hash-1> <commit-hash-2>
  ```
- View differences in the working directory:
  ```bash
  git diff
  ```

---

## 9. Stashing Changes

- Stash changes:
  ```bash
  git stash
  ```
- View stashes:
  ```bash
  git stash list
  ```
- Apply the most recent stash:
  ```bash
  git stash apply
  ```
- Drop the most recent stash:
  ```bash
  git stash drop
  ```

---

## 10. Collaboration

- Fork a repository (via GitHub UI).
- Create a pull request (via GitHub UI).
- Resolve merge conflicts.

---

## 11. Tags

- Create a new tag:
  ```bash
  git tag <tag-name>
  ```
- Push a tag:
  ```bash
  git push origin <tag-name>
  ```
- List all tags:
  ```bash
  git tag
  ```

---

## 12. Miscellaneous

- Show current branch:
  ```bash
  git branch --show-current
  ```
- Show current commit:
  ```bash
  git rev-parse HEAD
  ```
- Clean untracked files:
  ```bash
  git clean -f
  ```

---

## Useful Shortcuts

- Combine add and commit:
  ```bash
  git commit -am "Message"
  ```
- Push a branch for the first time:
  ```bash
  git push -u origin <branch-name>
  ```

---

This cheat sheet covers the most commonly used Git and GitHub commands. For more advanced usage, consult the [Git documentation](https://git-scm.com/doc).

