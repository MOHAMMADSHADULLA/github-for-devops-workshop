# 🚀 Git & GitHub for DevOps — Complete Command Reference

> A practical reference guide for DevOps Engineers covering Git workflows, GitHub collaboration, CI/CD integration, and team best practices.

---

## 📋 Table of Contents

- [Git Setup & Configuration](#-git-setup--configuration)
- [Repository Operations](#-repository-operations)
- [Staging & Committing](#-staging--committing)
- [Branching](#-branching)
- [Merging & Rebasing](#-merging--rebasing)
- [Remote Operations](#-remote-operations)
- [Undoing Changes](#-undoing-changes)
- [Stashing](#-stashing)
- [Tags & Releases](#-tags--releases)
- [Logs & Inspecting History](#-logs--inspecting-history)
- [Git for DevOps — Advanced](#-git-for-devops--advanced)
- [GitHub CLI (gh)](#-github-cli-gh)
- [GitHub Actions — CI/CD Basics](#-github-actions--cicd-basics)
- [DevOps Git Workflow (GitFlow)](#-devops-git-workflow-gitflow)
- [Quick Reference Cheatsheet](#-quick-reference-cheatsheet)

---

## ⚙️ Git Setup & Configuration

```bash
# Set your identity (required before first commit)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Set default branch name to main
git config --global init.defaultBranch main

# Set default editor (VS Code)
git config --global core.editor "code --wait"

# View all config
git config --list

# Store credentials (avoid typing password every time)
git config --global credential.helper store

# Set up SSH key (recommended for GitHub)
ssh-keygen -t ed25519 -C "you@example.com"
cat ~/.ssh/id_ed25519.pub   # Copy this to GitHub → Settings → SSH Keys

# Test SSH connection
ssh -T git@github.com
```

---

## 📁 Repository Operations

```bash
# Initialize a new local repo
git init

# Initialize with a specific branch name
git init -b main

# Clone a remote repo
git clone https://github.com/user/repo.git

# Clone with SSH (recommended)
git clone git@github.com:user/repo.git

# Clone into a specific folder
git clone https://github.com/user/repo.git my-folder

# Clone only latest snapshot (faster for large repos — useful in CI/CD)
git clone --depth 1 https://github.com/user/repo.git
```

---

## 📝 Staging & Committing

```bash
# Check status of working directory
git status

# Stage a specific file
git add filename.txt

# Stage all changes
git add .

# Stage parts of a file interactively
git add -p filename.txt

# Commit staged changes
git commit -m "feat: add Dockerfile for app"

# Stage and commit in one step (only for tracked files)
git commit -am "fix: update nginx config"

# Amend the last commit (before pushing)
git commit --amend -m "corrected commit message"

# Empty commit (useful to trigger CI/CD pipelines)
git commit --allow-empty -m "ci: trigger pipeline"
```

> **Commit Message Convention (DevOps Standard):**
> ```
> feat: new feature
> fix: bug fix
> ci: CI/CD related changes
> docs: documentation update
> chore: maintenance tasks
> refactor: code restructure
> test: adding tests
> ```

---

## 🌿 Branching

```bash
# List all local branches
git branch

# List all branches (local + remote)
git branch -a

# Create a new branch
git branch feature/deploy-pipeline

# Switch to a branch
git checkout feature/deploy-pipeline

# Create and switch in one step
git checkout -b feature/deploy-pipeline

# Modern way (Git 2.23+)
git switch -c feature/deploy-pipeline

# Rename current branch
git branch -m new-branch-name

# Delete a branch (after merge)
git branch -d feature/deploy-pipeline

# Force delete unmerged branch
git branch -D feature/deploy-pipeline

# Delete remote branch
git push origin --delete feature/deploy-pipeline
```

---

## 🔀 Merging & Rebasing

```bash
# Merge a branch into current branch
git merge feature/deploy-pipeline

# Merge with a commit message (no fast-forward)
git merge --no-ff feature/deploy-pipeline -m "merge: deploy pipeline feature"

# Abort a merge conflict
git merge --abort

# Rebase current branch onto main
git rebase main

# Interactive rebase (squash/edit last 3 commits)
git rebase -i HEAD~3

# Abort rebase
git rebase --abort

# Continue rebase after resolving conflict
git rebase --continue

# Cherry-pick a specific commit from another branch
git cherry-pick <commit-hash>
```

---

## 🌐 Remote Operations

```bash
# Add remote origin
git remote add origin git@github.com:user/repo.git

# View remotes
git remote -v

# Push to remote (first time)
git push -u origin main

# Push to remote (subsequent)
git push

# Push all branches
git push --all origin

# Push tags
git push --tags

# Pull (fetch + merge)
git pull origin main

# Fetch without merging
git fetch origin

# Fetch all remotes
git fetch --all

# Remove a remote
git remote remove origin

# Change remote URL (e.g., switch HTTP to SSH)
git remote set-url origin git@github.com:user/repo.git
```

---

## ↩️ Undoing Changes

```bash
# Discard changes in working directory (before staging)
git restore filename.txt
git checkout -- filename.txt   # older syntax

# Unstage a file (keep changes in working dir)
git restore --staged filename.txt
git reset HEAD filename.txt    # older syntax

# Undo last commit (keep changes staged)
git reset --soft HEAD~1

# Undo last commit (keep changes unstaged)
git reset --mixed HEAD~1

# Undo last commit and discard changes (DANGEROUS)
git reset --hard HEAD~1

# Revert a commit safely (creates new commit — safe for shared branches)
git revert <commit-hash>

# Discard ALL local changes (DANGEROUS)
git checkout .
git restore .
```

---

## 🗃️ Stashing

```bash
# Stash current changes
git stash

# Stash with a name
git stash push -m "wip: nginx config update"

# List all stashes
git stash list

# Apply latest stash (keep it in stash)
git stash apply

# Apply and remove latest stash
git stash pop

# Apply a specific stash
git stash apply stash@{2}

# Drop a stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

---

## 🏷️ Tags & Releases

```bash
# Create a lightweight tag
git tag v1.0.0

# Create an annotated tag (recommended for releases)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Tag a specific commit
git tag -a v1.0.0 <commit-hash> -m "Hotfix release"

# List all tags
git tag

# Push a specific tag to remote
git push origin v1.0.0

# Push all tags
git push origin --tags

# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0
```

---

## 🔍 Logs & Inspecting History

```bash
# View commit history
git log

# Compact one-line log
git log --oneline

# Log with graph (branch visualization)
git log --oneline --graph --all

# Show last N commits
git log -5

# Log with author filter
git log --author="Shadulla"

# Log by date range
git log --after="2025-01-01" --before="2025-12-31"

# Show changes in a commit
git show <commit-hash>

# Compare two branches
git diff main feature/deploy-pipeline

# Compare staged changes
git diff --staged

# Show who changed each line (blame)
git blame filename.txt

# Search commits by message keyword
git log --grep="Dockerfile"

# Search commits that changed a string
git log -S "nginx"
```

---

## 🛠️ Git for DevOps — Advanced

```bash
# ---- Submodules (for shared infra repos) ----
git submodule add https://github.com/user/shared-infra.git
git submodule update --init --recursive

# ---- Worktree (work on multiple branches simultaneously) ----
git worktree add ../hotfix-branch hotfix/critical-fix
git worktree list
git worktree remove ../hotfix-branch

# ---- Hooks (automate pre-commit / pre-push checks) ----
# Located in .git/hooks/
# Example: pre-commit hook to run linter
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/bash
echo "Running pre-commit checks..."
npm run lint
EOF
chmod +x .git/hooks/pre-commit

# ---- Bisect (find which commit introduced a bug) ----
git bisect start
git bisect bad                # current commit is broken
git bisect good v1.0.0        # this version was good
# git bisects automatically — test and mark each step
git bisect good               # or git bisect bad
git bisect reset              # end bisect

# ---- Clean untracked files (useful in CI environments) ----
git clean -fd                 # remove untracked files and dirs
git clean -fdx                # also remove .gitignore'd files
git clean -n                  # dry run first (show what would be removed)

# ---- Sparse Checkout (clone only specific folders) ----
git clone --filter=blob:none --sparse https://github.com/user/large-repo.git
cd large-repo
git sparse-checkout set infrastructure/terraform
```

---

## 💻 GitHub CLI (gh)

```bash
# Install GitHub CLI
# Ubuntu/Debian:
sudo apt install gh

# Authenticate
gh auth login

# ---- Repos ----
gh repo create my-devops-project --public
gh repo clone user/repo
gh repo view --web

# ---- Pull Requests ----
gh pr create --title "feat: add CI pipeline" --body "Adds GitHub Actions workflow"
gh pr list
gh pr view 42
gh pr merge 42 --squash
gh pr checkout 42

# ---- Issues ----
gh issue create --title "Bug: deployment fails" --label "bug"
gh issue list
gh issue close 10

# ---- Workflows (GitHub Actions) ----
gh workflow list
gh workflow run deploy.yml
gh run list
gh run view 12345
gh run watch

# ---- Releases ----
gh release create v1.0.0 --title "v1.0.0" --notes "Initial release"
gh release list
gh release download v1.0.0
```

---

## ⚡ GitHub Actions — CI/CD Basics

```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .

      - name: Push to Docker Hub
        env:
          DOCKER_USER: ${{ secrets.DOCKER_USERNAME }}
          DOCKER_PASS: ${{ secrets.DOCKER_PASSWORD }}
        run: |
          echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
          docker push myapp:${{ github.sha }}
```

> **Key Concepts:**
> - `on:` — triggers (push, PR, schedule, manual)
> - `jobs:` — parallel units of work
> - `steps:` — sequential commands within a job
> - `secrets:` — store sensitive values in GitHub → Settings → Secrets
> - `${{ github.sha }}` — unique commit hash (great for Docker image tags)

---

## 🔄 DevOps Git Workflow (GitFlow)

```
main          ──────●──────────────────────────●──────────────▶
                    │                           │ (merge via PR)
develop       ──────●────────●────────●─────────●──────────────▶
                             │        │
feature/      ───────────────●────────●  (create PR to develop)
                             
release/      ──────────────────────────●──────●  (tag + merge to main)
                                              │
hotfix/       ────────────────────────────────●──●  (fix + merge to main & develop)
```

**Branch Strategy:**

| Branch | Purpose |
|--------|---------|
| `main` | Production-ready code only |
| `develop` | Integration branch for features |
| `feature/*` | New features / enhancements |
| `release/*` | Pre-release testing & fixes |
| `hotfix/*` | Emergency production fixes |

---

## 📌 Quick Reference Cheatsheet

| Task | Command |
|------|---------|
| Initialize repo | `git init` |
| Clone repo | `git clone <url>` |
| Check status | `git status` |
| Stage all | `git add .` |
| Commit | `git commit -m "message"` |
| Push | `git push origin main` |
| Pull | `git pull origin main` |
| New branch | `git checkout -b branch-name` |
| Switch branch | `git switch branch-name` |
| Merge | `git merge branch-name` |
| Stash | `git stash` |
| Undo last commit | `git reset --soft HEAD~1` |
| View log | `git log --oneline --graph` |
| Create tag | `git tag -a v1.0 -m "Release"` |
| Push tag | `git push origin v1.0` |
| Delete remote branch | `git push origin --delete branch` |
| Trigger CI pipeline | `git commit --allow-empty -m "ci: trigger"` |

---

## 📚 Useful Resources

- [Official Git Docs](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Oh My Git! — Interactive Learning](https://ohmygit.org/)

---

> **Author:** Mohammad Shadulla  
> **Role Target:** DevOps / Cloud Engineer  
> **Last Updated:** April 2026
