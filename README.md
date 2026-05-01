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
# 🪝 Git Hooks — Complete Guide

Git hooks are scripts that run automatically at specific points in the Git workflow (before/after commit, push, merge, etc.). They live in `.git/hooks/` or a shared `hooks/` folder.

---

## 📁 Project Structure

```
your-project/
├── hooks/
│   ├── pre-commit          # Runs before a commit is created
│   ├── prepare-commit-msg  # Runs before commit message editor opens
│   ├── commit-msg          # Validates the commit message
│   ├── post-commit         # Runs after a commit is created
│   ├── pre-push            # Runs before a push
│   ├── pre-rebase          # Runs before a rebase
│   ├── post-merge          # Runs after a merge
│   ├── post-checkout       # Runs after a checkout
│   └── install.sh          # Script to install all hooks
└── README.md
```

---

## ⚙️ How to Install Hooks

### Option 1 — Manual Symlink (Recommended)
```bash
# From your project root
chmod +x hooks/*
git config core.hooksPath hooks
```

### Option 2 — install.sh Script
```bash
#!/bin/bash
# hooks/install.sh
HOOKS_DIR="$(pwd)/hooks"
GIT_HOOKS_DIR="$(pwd)/.git/hooks"

for hook in "$HOOKS_DIR"/*; do
  hook_name=$(basename "$hook")
  if [ "$hook_name" != "install.sh" ]; then
    ln -sf "$HOOKS_DIR/$hook_name" "$GIT_HOOKS_DIR/$hook_name"
    chmod +x "$GIT_HOOKS_DIR/$hook_name"
    echo "✅ Installed: $hook_name"
  fi
done
echo "All hooks installed!"
```
Run it once:
```bash
bash hooks/install.sh
```

---

## 🪝 All Git Hooks — With Examples

---

### 1. `pre-commit` — Lint & Format Before Commit
Runs before the commit is created. Exit non-zero to abort.

```bash
#!/bin/bash
# hooks/pre-commit

echo "🔍 Running pre-commit checks..."

# --- Python: flake8 lint ---
files=$(git diff --cached --name-only --diff-filter=ACM | grep '\.py$')
if [ -n "$files" ]; then
  echo "🐍 Linting Python files..."
  flake8 $files
  if [ $? -ne 0 ]; then
    echo "❌ flake8 failed. Fix errors before committing."
    exit 1
  fi
fi

# --- JavaScript/TypeScript: ESLint ---
js_files=$(git diff --cached --name-only --diff-filter=ACM | grep -E '\.(js|ts|jsx|tsx)$')
if [ -n "$js_files" ]; then
  echo "📜 Linting JS/TS files..."
  npx eslint $js_files
  if [ $? -ne 0 ]; then
    echo "❌ ESLint failed."
    exit 1
  fi
fi

# --- Shell scripts: shellcheck ---
sh_files=$(git diff --cached --name-only --diff-filter=ACM | grep '\.sh$')
if [ -n "$sh_files" ]; then
  echo "🐚 Checking shell scripts..."
  shellcheck $sh_files || exit 1
fi

echo "✅ pre-commit passed!"
exit 0
```

---

### 2. `prepare-commit-msg` — Auto-prepend Branch Name
Runs before the commit message editor opens. Injects context into the message.

```bash
#!/bin/bash
# hooks/prepare-commit-msg
COMMIT_MSG_FILE=$1
COMMIT_SOURCE=$2

# Prepend branch name to commit message (skip merges)
if [ -z "$COMMIT_SOURCE" ]; then
  BRANCH=$(git symbolic-ref --short HEAD 2>/dev/null)
  if [ -n "$BRANCH" ]; then
    sed -i.bak "1s/^/[$BRANCH] /" "$COMMIT_MSG_FILE"
  fi
fi
```

---

### 3. `commit-msg` — Enforce Conventional Commits
Validates the commit message format.

```bash
#!/bin/bash
# hooks/commit-msg
COMMIT_MSG=$(cat "$1")

# Conventional commits pattern: type(scope): description
PATTERN="^(feat|fix|docs|style|refactor|test|chore|ci|perf|revert)(\(.+\))?: .{1,72}"

if ! echo "$COMMIT_MSG" | grep -qE "$PATTERN"; then
  echo "❌ Invalid commit message format!"
  echo ""
  echo "Expected format: type(scope): description"
  echo "Examples:"
  echo "  feat(auth): add login endpoint"
  echo "  fix(api): handle null response"
  echo "  docs: update README"
  echo ""
  echo "Types: feat | fix | docs | style | refactor | test | chore | ci | perf | revert"
  exit 1
fi

echo "✅ Commit message is valid."
exit 0
```

---

### 4. `post-commit` — Notify After Commit
Runs after a commit. Cannot abort the commit.

```bash
#!/bin/bash
# hooks/post-commit
BRANCH=$(git symbolic-ref --short HEAD)
HASH=$(git rev-parse --short HEAD)
MSG=$(git log -1 --pretty=%B | head -1)

echo ""
echo "✅ Committed on [$BRANCH]: $HASH — $MSG"

# Optional: send Slack/webhook notification
# curl -s -X POST -H 'Content-type: application/json' \
#   --data "{\"text\":\"New commit on $BRANCH: $MSG ($HASH)\"}" \
#   YOUR_SLACK_WEBHOOK_URL
```

---

### 5. `pre-push` — Run Tests Before Push
Runs before `git push`. Exit non-zero to abort.

```bash
#!/bin/bash
# hooks/pre-push
echo "🚀 Running pre-push checks..."

# --- Run Python tests ---
if [ -f "pytest.ini" ] || [ -f "setup.cfg" ] || [ -d "tests" ]; then
  echo "🧪 Running pytest..."
  pytest --tb=short -q
  if [ $? -ne 0 ]; then
    echo "❌ Tests failed! Push aborted."
    exit 1
  fi
fi

# --- Run JS tests ---
if [ -f "package.json" ]; then
  echo "🧪 Running npm test..."
  npm test -- --watchAll=false
  if [ $? -ne 0 ]; then
    echo "❌ JS tests failed! Push aborted."
    exit 1
  fi
fi

# --- Block push to main/master directly ---
BRANCH=$(git symbolic-ref --short HEAD)
if [ "$BRANCH" = "main" ] || [ "$BRANCH" = "master" ]; then
  echo "⚠️  Direct push to $BRANCH is not allowed!"
  echo "   Please create a feature branch and open a PR."
  exit 1
fi

echo "✅ pre-push passed!"
exit 0
```

---

### 6. `pre-rebase` — Guard Against Risky Rebases
```bash
#!/bin/bash
# hooks/pre-rebase
UPSTREAM=$1
BRANCH=$2

# Prevent rebasing main or master
if [ "$BRANCH" = "main" ] || [ "$BRANCH" = "master" ]; then
  echo "❌ Rebasing main/master is not allowed!"
  exit 1
fi

echo "✅ Rebase allowed."
exit 0
```

---

### 7. `post-merge` — Auto-install Dependencies After Merge
```bash
#!/bin/bash
# hooks/post-merge
CHANGED=$(git diff-tree -r --name-only --no-commit-id ORIG_HEAD HEAD)

# Python: reinstall if requirements changed
if echo "$CHANGED" | grep -q "requirements.*\.txt"; then
  echo "📦 requirements.txt changed — running pip install..."
  pip install -r requirements.txt
fi

# Node: reinstall if package.json changed
if echo "$CHANGED" | grep -q "package-lock\.json"; then
  echo "📦 package-lock.json changed — running npm install..."
  npm install
fi
```

---

### 8. `post-checkout` — Setup After Switching Branch
```bash
#!/bin/bash
# hooks/post-checkout
PREV_HEAD=$1
NEW_HEAD=$2
BRANCH_SWITCH=$3

# Only act on branch switches (not file checkouts)
if [ "$BRANCH_SWITCH" = "1" ]; then
  BRANCH=$(git symbolic-ref --short HEAD)
  echo "🌿 Switched to branch: $BRANCH"

  # Auto-install deps if needed
  if [ -f "package.json" ]; then
    npm install --silent
  fi
fi
```

---

## 🧩 Hook Cheat Sheet

| Hook | When it runs | Can abort? | Common Use |
|------|-------------|------------|------------|
| `pre-commit` | Before commit created | ✅ Yes | Lint, format, secrets scan |
| `prepare-commit-msg` | Before message editor | ✅ Yes | Inject branch name |
| `commit-msg` | After message entered | ✅ Yes | Enforce commit format |
| `post-commit` | After commit created | ❌ No | Notifications |
| `pre-push` | Before push | ✅ Yes | Run tests, block main push |
| `pre-rebase` | Before rebase | ✅ Yes | Guard protected branches |
| `post-merge` | After merge | ❌ No | Install dependencies |
| `post-checkout` | After checkout | ❌ No | Environment setup |

---

## 🔐 Bonus: Secrets Scanning in `pre-commit`

Add this to your `pre-commit` hook to block accidental credential commits:

```bash
# Scan for secrets/credentials
echo "🔐 Scanning for secrets..."
if git diff --cached | grep -iE "(api_key|secret_key|password|token|aws_access|private_key)\s*=\s*['\"][^'\"]{8,}"; then
  echo "❌ Potential secret detected! Remove it before committing."
  exit 1
fi
```

---

## 💡 Tips

- Always `chmod +x hooks/<hook-name>` to make hooks executable
- Use `git commit --no-verify` to skip hooks in emergencies
- Share hooks with your team via `git config core.hooksPath hooks` (committed to the repo)
- For complex workflows, consider **Husky** (Node.js) or **pre-commit** (Python) frameworks

---

*Generated for DevOps workflow setup*

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
