
# 📘 Git & GitHub Cheat Sheet

This file contains commonly used **Git** and **GitHub** commands for version control, collaboration, and repository management.

---

## 🔹 Git Configuration

```bash
# Set username
git config --global user.name "Your Name"

# Set email
git config --global user.email "your@email.com"

# Check config
git config --list
```

---

## 🔹 Create & Clone Repositories

```bash
# Initialize a new repo
git init

# Clone existing repo
git clone <repo-url>
```

---

## 🔹 Basic Git Workflow

```bash
# Check repo status
git status

# Stage a file
git add filename

# Stage all files
git add .

# Commit changes
git commit -m "your commit message"

# Push changes
git push origin main

# Pull changes from remote
git pull origin main
```

---

## 🔹 Branching

```bash
# List branches
git branch

# Create new branch
git branch branch-name

# Switch to branch
git checkout branch-name

# Create + switch
git checkout -b branch-name

# Merge branch into current
git merge branch-name

# Delete branch
git branch -d branch-name
```

---

## 🔹 Undo & Reset

```bash
# Unstage a file
git reset filename

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Revert commit by ID
git revert <commit-id>
```

---

## 🔹 Log & History

```bash
# View commit history
git log

# Compact log
git log --oneline --graph --all
```

---

## 🔹 GitHub Collaboration

```bash
# Fork a repo (done on GitHub UI)

# Create a pull request (done on GitHub UI)

# Add a remote upstream (after fork)
git remote add upstream <original-repo-url>

# Sync fork with upstream
git fetch upstream
git merge upstream/main
```

---

## 🔹 Stash

```bash
# Save uncommitted changes
git stash

# List stashes
git stash list

# Apply latest stash
git stash apply

# Drop latest stash
git stash drop
```

---

## 🔹 Tags

```bash
# Create tag
git tag v1.0.0

# Push tags
git push origin --tags
```



Do you want me to also create **short practical examples** (like creating a repo, making a branch, PR workflow) inside this file so it’s like a mini-guide instead of just a cheat sheet?
