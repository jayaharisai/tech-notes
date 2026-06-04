# 🐙 Complete Git & GitHub Guide: Beginner to Senior-Level Expert

> A comprehensive, end-to-end Git & GitHub reference — covering version control fundamentals, branching strategies, collaboration workflows, CI/CD, security, and everything you need to crack a senior engineering role.

---

## 📖 Table of Contents

1. [Introduction to Git & GitHub](#1-introduction-to-git--github)
2. [Installation & Initial Setup](#2-installation--initial-setup)
3. [Core Concepts – How Git Works](#3-core-concepts--how-git-works)
4. [Essential Commands – Daily Use](#4-essential-commands--daily-use)
5. [Branching & Merging](#5-branching--merging)
6. [Remote Repositories & GitHub](#6-remote-repositories--github)
7. [Undoing Things – Revert, Reset, Restore](#7-undoing-things--revert-reset-restore)
8. [Stashing & Cleaning](#8-stashing--cleaning)
9. [Tagging & Releases](#9-tagging--releases)
10. [Rewriting History – Rebase, Amend, Squash](#10-rewriting-history--rebase-amend-squash)
11. [Advanced Git – Internals & Power Commands](#11-advanced-git--internals--power-commands)
12. [Git Workflows for Teams](#12-git-workflows-for-teams)
13. [GitHub Features – PRs, Issues, Projects](#13-github-features--prs-issues-projects)
14. [GitHub Actions – CI/CD](#14-github-actions--cicd)
15. [GitHub Security & Access Control](#15-github-security--access-control)
16. [Git Hooks – Automation at Commit Level](#16-git-hooks--automation-at-commit-level)
17. [Submodules & Subtrees](#17-submodules--subtrees)
18. [Git Configuration & Aliases](#18-git-configuration--aliases)
19. [Troubleshooting Common Git Problems](#19-troubleshooting-common-git-problems)
20. [Git for DevOps – Pipelines, IaC, GitOps](#20-git-for-devops--pipelines-iac-gitops)
21. [Interview Prep – Senior Git & GitHub Role](#21-interview-prep--senior-git--github-role)
22. [Cheat Sheets & Quick References](#22-cheat-sheets--quick-references)

---

## 1. Introduction to Git & GitHub

### What is Git?
Git is a free, open-source **distributed version control system** created by Linus Torvalds in 2005 (yes, same person as Linux). It tracks changes to files over time so you can recall specific versions later, collaborate with others, and work in parallel without stepping on each other's toes.

**Key traits:**
- **Distributed** – every developer has a full copy of the repo, including history
- **Fast** – most operations are local, no network needed
- **Branching is cheap** – creating a branch is nearly instant

### What is GitHub?
GitHub is a **cloud-hosted platform** built on top of Git. It adds:
- Remote repository hosting
- Pull Requests (code review workflow)
- Issues, Projects, Discussions
- GitHub Actions (CI/CD)
- Security scanning, Dependabot, Secrets management

**Alternatives to GitHub:**

| Platform | Notable For |
|----------|-------------|
| GitLab | Built-in CI/CD, self-hosting |
| Bitbucket | Atlassian ecosystem (Jira) |
| Gitea | Lightweight self-hosted |
| Azure DevOps | Microsoft/enterprise integration |

### Why Git for senior roles?
- Every modern engineering team uses Git
- CI/CD pipelines are Git-event driven
- GitOps (infra as code) is the industry standard
- Code reviews, hotfix management, release branching — all Git

---

## 2. Installation & Initial Setup

### Installing Git

```bash
# Ubuntu / Debian
sudo apt update && sudo apt install git

# RHEL / CentOS / Fedora
sudo dnf install git

# macOS (Homebrew)
brew install git

# Windows
# Download from https://git-scm.com/download/win
```

### Verify Installation
```bash
git --version
# git version 2.x.x
```

### First-Time Global Configuration
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "vim"          # or nano, code, etc.
git config --global init.defaultBranch main    # default branch name
git config --global color.ui auto              # colored output
```

### View Configuration
```bash
git config --list                   # all settings
git config --global --list          # global only
git config user.name                # single value
```

### Configuration Levels

| Level | File | Scope |
|-------|------|-------|
| `--system` | `/etc/gitconfig` | All users on the machine |
| `--global` | `~/.gitconfig` | Your user account |
| `--local` | `.git/config` | This repo only (default) |

Local overrides global overrides system.

### Setting Up SSH Key for GitHub
```bash
# Generate key
ssh-keygen -t ed25519 -C "you@example.com"

# Start SSH agent and add key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Copy public key
cat ~/.ssh/id_ed25519.pub
# Paste this into GitHub → Settings → SSH and GPG keys → New SSH key

# Test connection
ssh -T git@github.com
# Hi username! You've successfully authenticated...
```

---

## 3. Core Concepts – How Git Works

### The Three Areas

```
Working Directory  →  Staging Area (Index)  →  Repository (.git)
    (edit files)         (git add)               (git commit)
```

| Area | Description |
|------|-------------|
| **Working Directory** | Where you edit files on disk |
| **Staging Area (Index)** | Snapshot prepared for the next commit |
| **Repository** | Permanent history stored in `.git/` |

### The Four Object Types (Git Internals)

| Object | Description |
|--------|-------------|
| **blob** | File content (no filename, just content) |
| **tree** | Directory listing (maps names to blobs/trees) |
| **commit** | Snapshot pointer + metadata (author, message, parent) |
| **tag** | Named pointer to a commit |

Every object is identified by a **SHA-1 hash** (40 hex chars). Git is essentially a content-addressable filesystem.

### File States in Git

```
Untracked → Staged → Committed → Modified → Staged → Committed...
```

| State | Meaning |
|-------|---------|
| **Untracked** | New file, Git doesn't know about it |
| **Staged** | Added to index via `git add`, ready to commit |
| **Committed** | Safely stored in `.git` history |
| **Modified** | Tracked file changed but not yet staged |

### What is HEAD?
`HEAD` is a pointer to the **currently checked-out commit** (usually the tip of your current branch). When you commit, HEAD moves forward. When you checkout another branch, HEAD moves to that branch's tip.

```bash
cat .git/HEAD
# ref: refs/heads/main
```

---

## 4. Essential Commands – Daily Use

### Starting a Repository
```bash
git init                        # initialize new repo in current dir
git init myproject              # create new dir and init
git clone https://github.com/user/repo.git    # clone remote repo
git clone git@github.com:user/repo.git        # clone via SSH
git clone <url> my-folder       # clone into custom folder name
```

### Checking Status and History
```bash
git status                      # show working tree status
git status -s                   # short format
git log                         # full commit history
git log --oneline               # compact one-line per commit
git log --oneline --graph --all # visual branch graph
git log -n 5                    # last 5 commits
git log --author="Bob"          # filter by author
git log --since="2 weeks ago"
git log -- file.txt             # history of a specific file
git show abc1234                # show a specific commit
git diff                        # unstaged changes
git diff --staged               # staged changes (vs last commit)
git diff main..feature          # diff between two branches
```

### Staging and Committing
```bash
git add file.txt                # stage a file
git add .                       # stage everything in current dir
git add -p                      # interactively stage hunks (very useful!)
git commit -m "Your message"    # commit with message
git commit -am "message"        # stage tracked files + commit (skips add)
git commit --amend              # modify the last commit (message or files)
```

### Viewing and Comparing
```bash
git show HEAD                   # latest commit details
git show HEAD~2                 # two commits before HEAD
git blame file.txt              # who changed each line and when
git log -p file.txt             # full patch history of a file
```

### .gitignore – Ignoring Files
```bash
# Create .gitignore in repo root
touch .gitignore
```

**Example `.gitignore`:**
```
# Dependencies
node_modules/
vendor/

# Build outputs
dist/
*.o
*.pyc
__pycache__/

# Environment files
.env
*.env.local

# OS files
.DS_Store
Thumbs.db

# IDE files
.idea/
.vscode/
*.swp
```

```bash
git check-ignore -v filename    # why is this file ignored?
git rm --cached file.txt        # stop tracking a file (keep on disk)
```

---

## 5. Branching & Merging

### Branch Basics
A branch is just a lightweight movable pointer to a commit. Creating one is instant — it's just a 41-byte file.

```bash
git branch                      # list local branches
git branch -a                   # list local + remote branches
git branch -v                   # list with last commit info
git branch feature-login        # create new branch
git checkout feature-login      # switch to branch
git checkout -b feature-login   # create and switch (old style)
git switch feature-login        # switch (modern, preferred)
git switch -c feature-login     # create and switch (modern)
git branch -d feature-login     # delete branch (safe, merged only)
git branch -D feature-login     # force delete branch
git branch -m old-name new-name # rename a branch
```

### Merging
```bash
git checkout main
git merge feature-login         # merge feature into main

# Fast-forward merge (linear history, no merge commit)
# Three-way merge (creates a merge commit)
git merge --no-ff feature-login # always create merge commit
git merge --squash feature-login # squash all commits into one staged change
```

### Merge Conflicts
Conflicts happen when two branches change the same lines.

```bash
git merge feature-login
# CONFLICT (content): Merge conflict in app.js
# Automatic merge failed; fix conflicts and then commit.
```

Git marks conflicts in the file:
```
<<<<<<< HEAD
const port = 3000;
=======
const port = 8080;
>>>>>>> feature-login
```

**Resolve manually**, then:
```bash
git add app.js
git commit                      # complete the merge
```

```bash
git merge --abort               # abort if you want to undo mid-merge
git mergetool                   # open a visual merge tool
```

### Useful Merge/Branch Info
```bash
git branch --merged             # branches already merged into current
git branch --no-merged          # branches not yet merged
git log main..feature           # commits in feature not in main
```

---

## 6. Remote Repositories & GitHub

### Working with Remotes
```bash
git remote -v                           # list remotes
git remote add origin git@github.com:user/repo.git   # add remote
git remote rename origin upstream       # rename remote
git remote remove origin                # remove remote
git remote set-url origin <new-url>     # change URL
```

### Fetching, Pulling, Pushing
```bash
git fetch origin                        # download remote changes (don't merge)
git fetch --all                         # fetch all remotes
git pull                                # fetch + merge (current branch)
git pull --rebase                       # fetch + rebase (cleaner history)
git push origin main                    # push local main to remote
git push -u origin feature-login        # push and set upstream tracking
git push --force-with-lease             # safe force push (checks remote hasn't changed)
git push origin --delete feature-login  # delete remote branch
```

### Tracking Branches
```bash
git branch -u origin/main              # set upstream for current branch
git branch -vv                         # see tracking info for all branches
```

### Forking Workflow (Open Source Contribution)
```bash
# 1. Fork on GitHub (click Fork button)
# 2. Clone your fork
git clone git@github.com:yourname/repo.git

# 3. Add upstream remote
git remote add upstream git@github.com:original/repo.git

# 4. Keep fork up to date
git fetch upstream
git checkout main
git merge upstream/main

# 5. Work on a feature branch and push to your fork
git checkout -b fix-bug
git push origin fix-bug

# 6. Open Pull Request on GitHub from your fork to original repo
```

### Syncing a Fork (Daily Habit)
```bash
git fetch upstream
git rebase upstream/main        # or merge
git push origin main
```

---

## 7. Undoing Things – Revert, Reset, Restore

> ⚠️ Know which commands are safe (don't rewrite history) vs destructive (do).

### git restore (Safe – Working Directory / Staging)
```bash
git restore file.txt            # discard unstaged changes in file
git restore .                   # discard all unstaged changes
git restore --staged file.txt   # unstage a file (keep changes in working dir)
```

### git revert (Safe – Creates New Commit)
Revert is the safe way to undo a commit that's already been pushed/shared.
```bash
git revert abc1234              # create new commit that undoes abc1234
git revert HEAD                 # revert last commit
git revert HEAD~3..HEAD         # revert last 3 commits
git revert --no-commit abc1234  # stage the revert without committing yet
```

### git reset (Rewrites History – Use with Caution)

| Mode | Staged | Working Dir | Use Case |
|------|--------|-------------|----------|
| `--soft` | Keeps | Keeps | Undo commit, keep changes staged |
| `--mixed` (default) | Clears | Keeps | Undo commit, keep changes unstaged |
| `--hard` | Clears | Clears | Completely discard commit + changes |

```bash
git reset --soft HEAD~1         # undo last commit, keep files staged
git reset --mixed HEAD~1        # undo last commit, keep files as modified
git reset --hard HEAD~1         # undo last commit, DESTROY changes
git reset --hard origin/main    # reset to match remote exactly
```

> ⚠️ Never `git reset --hard` on commits already pushed to a shared branch.

### Recovering Lost Commits (reflog)
```bash
git reflog                      # log of every HEAD movement
git checkout abc1234            # go back to any SHA from reflog
git branch recovery abc1234     # create branch from old SHA
```

`reflog` is your safety net. Git almost never truly deletes data — it stays accessible for ~90 days.

---

## 8. Stashing & Cleaning

### git stash (Temporarily Shelve Changes)
```bash
git stash                       # stash current changes
git stash push -m "WIP: login"  # stash with description
git stash list                  # list all stashes
git stash pop                   # apply latest stash and drop it
git stash apply stash@{2}       # apply specific stash (keep it in list)
git stash drop stash@{0}        # delete a stash
git stash clear                 # delete ALL stashes
git stash branch feature-wip    # create branch from stash (best practice)
```

**Stash including untracked files:**
```bash
git stash -u                    # include untracked files
git stash -a                    # include untracked + ignored files
```

### git clean (Remove Untracked Files)
```bash
git clean -n                    # dry run – show what would be removed
git clean -f                    # remove untracked files
git clean -fd                   # remove untracked files + directories
git clean -fX                   # remove only ignored files
git clean -fdx                  # remove untracked + ignored (full clean)
```

> ⚠️ `git clean` is irreversible. Always dry-run first with `-n`.

---

## 9. Tagging & Releases

Tags mark specific points in history — typically version releases.

### Types of Tags

| Type | Description |
|------|-------------|
| **Lightweight** | Just a pointer to a commit (like a branch that doesn't move) |
| **Annotated** | Full object with tagger name, email, date, message — recommended for releases |

### Tag Commands
```bash
git tag                             # list all tags
git tag v1.0.0                      # create lightweight tag
git tag -a v1.0.0 -m "Release 1.0" # create annotated tag
git tag -a v1.0.0 abc1234           # tag a specific commit
git show v1.0.0                     # show tag details
git push origin v1.0.0              # push a specific tag
git push origin --tags              # push all tags
git tag -d v1.0.0                   # delete local tag
git push origin --delete v1.0.0     # delete remote tag
```

### Semantic Versioning (SemVer)
```
v MAJOR . MINOR . PATCH
   1   .   2   .   3

MAJOR – breaking changes
MINOR – new features (backwards compatible)
PATCH – bug fixes
```

### GitHub Releases
On GitHub, creating a release from a tag adds:
- Release notes
- Binary assets / artifacts
- Changelog
- Auto-generated release notes from PR titles

---

## 10. Rewriting History – Rebase, Amend, Squash

> ⚠️ **Golden Rule:** Never rewrite history of commits already pushed to a shared branch. Only rewrite local or private feature branches.

### git commit --amend
```bash
git commit --amend -m "New message"     # fix last commit message
git commit --amend --no-edit            # add staged changes to last commit silently
```

### git rebase
Rebase replays your commits on top of another branch — produces a linear, clean history.

```bash
git checkout feature
git rebase main                         # replay feature commits on top of main
```

**Interactive rebase (most powerful):**
```bash
git rebase -i HEAD~4                    # interactively edit last 4 commits
```

Interactive commands:
| Command | Action |
|---------|--------|
| `pick` | Keep commit as-is |
| `reword` | Keep commit, edit message |
| `edit` | Pause to amend commit |
| `squash` | Combine with previous commit |
| `fixup` | Like squash but discard this message |
| `drop` | Remove commit entirely |
| `exec` | Run a shell command |

### Squashing Commits Before a PR (Common Workflow)
```bash
git rebase -i HEAD~5
# Change all 'pick' to 'squash' except the first
# Edit the combined commit message
git push --force-with-lease origin feature
```

### Rebase vs Merge

| | Rebase | Merge |
|--|--------|-------|
| History | Linear, clean | Preserves full branching |
| Commit SHAs | Changes (new commits) | Preserved |
| Best for | Feature branches, local cleanup | Long-lived branches, shared history |
| Safe on shared? | ❌ No | ✅ Yes |

### Cherry-Pick (Apply a Single Commit)
```bash
git cherry-pick abc1234             # apply one commit to current branch
git cherry-pick abc1234..def5678    # apply a range
git cherry-pick --no-commit abc1234 # apply changes without committing
```

---

## 11. Advanced Git – Internals & Power Commands

### git bisect (Binary Search for a Bug)
```bash
git bisect start
git bisect bad                      # current commit is broken
git bisect good v1.2.0              # this tag was working
# Git checks out a commit halfway between
# Test, then tell git:
git bisect good                     # or git bisect bad
# Repeat until Git reports the first bad commit
git bisect reset                    # return to HEAD when done
```

### git worktree (Multiple Working Directories)
```bash
git worktree add ../hotfix hotfix-branch    # checkout branch in separate dir
git worktree list
git worktree remove ../hotfix
```
Useful for: reviewing a PR while keeping your current work intact — no stashing needed.

### git reflog (Your Safety Net)
```bash
git reflog                          # every move of HEAD
git reflog show feature             # reflog for a specific branch
```

### git shortlog (Contribution Summary)
```bash
git shortlog -sn                    # commits per author
git shortlog -sn --since="1 month ago"
```

### git archive (Export Without .git)
```bash
git archive --format=zip HEAD > project.zip
git archive --format=tar.gz v1.0.0 > release.tar.gz
```

### git grep (Search Across All Files)
```bash
git grep "TODO"                     # search working tree
git grep "function login" HEAD~5    # search in a specific commit
```

### Sparse Checkout (Check Out Part of a Repo)
```bash
git clone --filter=blob:none --sparse https://github.com/user/repo.git
cd repo
git sparse-checkout set src/module-a
```

### Git Attributes (.gitattributes)
```
# Force LF line endings for shell scripts
*.sh text eol=lf

# Treat binary files correctly
*.png binary
*.pdf binary

# Custom diff driver for minified JS
*.min.js linguist-generated=true
```

---

## 12. Git Workflows for Teams

### 1. Feature Branch Workflow (Most Common)
Every feature gets its own branch off `main`. PR → review → merge back to `main`.

```
main
 └─ feature/login
 └─ feature/dashboard
 └─ fix/auth-bug
```

### 2. Gitflow Workflow (Release-Heavy Projects)

```
main           – production-ready code
develop        – integration branch
feature/*      – new features (branch from develop)
release/*      – release prep (branch from develop)
hotfix/*       – urgent fixes (branch from main)
```

```bash
# Start a feature
git checkout -b feature/login develop

# Finish a feature
git checkout develop
git merge --no-ff feature/login
git branch -d feature/login

# Start a release
git checkout -b release/1.0 develop

# Finish a release
git checkout main
git merge --no-ff release/1.0
git tag -a v1.0
git checkout develop
git merge --no-ff release/1.0

# Hotfix
git checkout -b hotfix/1.0.1 main
# fix bug
git checkout main
git merge --no-ff hotfix/1.0.1
git tag -a v1.0.1
git checkout develop
git merge --no-ff hotfix/1.0.1
```

### 3. GitHub Flow (Simple & Fast)
```
main is always deployable
  1. Branch from main
  2. Commit changes
  3. Open Pull Request
  4. Review and discuss
  5. Deploy from branch (test in production-like env)
  6. Merge to main
```

### 4. Trunk-Based Development (High-Frequency Deployment)
- Everyone commits directly to `main` (or short-lived branches < 1 day)
- Feature flags hide incomplete work
- Requires strong CI/CD and test coverage
- Used by Google, Facebook, Netflix

### Choosing a Workflow

| Team Type | Recommended Workflow |
|-----------|---------------------|
| Small startup, fast deploys | GitHub Flow |
| Scheduled releases (apps, libraries) | Gitflow |
| Large teams, continuous deployment | Trunk-Based Development |
| Open source project | Forking + Feature Branch |

---

## 13. GitHub Features – PRs, Issues, Projects

### Pull Requests (PRs)

A PR is a request to merge your branch into another, with built-in code review.

**Opening a PR:**
1. Push branch to GitHub
2. GitHub prompts "Compare & pull request"
3. Fill in title, description, reviewers, labels
4. Submit

**PR best practices:**
- Keep PRs small and focused (< 400 lines ideal)
- Write a clear description: what, why, how to test
- Link to the related issue: `Closes #42`
- Request specific reviewers
- Use draft PRs for work in progress

**PR Review:**
```
- Comment    → leave a note
- Approve    → ready to merge
- Request changes → must fix before merge
```

**Merging strategies on GitHub:**
| Option | Result |
|--------|--------|
| Merge commit | Preserves all commits + merge commit |
| Squash and merge | Squashes to one commit on main |
| Rebase and merge | Linear history, no merge commit |

### Issues
```markdown
# Good issue template
## What happened?
## Expected behavior?
## Steps to reproduce
## Environment (OS, version, browser)
```

**Linking issues to PRs:**
- `Closes #42` – auto-closes issue when PR merges
- `Fixes #42`, `Resolves #42` – same effect

**Labels:** `bug`, `enhancement`, `good first issue`, `help wanted`, `priority: high`

### GitHub Projects (Kanban / Roadmap)
- Create boards with columns: `Backlog`, `In Progress`, `In Review`, `Done`
- Link issues and PRs to project cards
- Use roadmap view for timeline planning
- Automate card movement with workflows

### GitHub Discussions
For open-ended conversations, questions, ideas — keeps Issues clean.

### Code Owners (.github/CODEOWNERS)
```
# Format: pattern  @user or @org/team
*                   @org/core-team
/docs/              @org/docs-team
/src/auth/          @alice @bob
*.yml               @org/devops-team
```
CODEOWNERS auto-requests reviews from the right people.

---

## 14. GitHub Actions – CI/CD

GitHub Actions lets you automate workflows triggered by Git events.

### Core Concepts

| Concept | Description |
|---------|-------------|
| **Workflow** | YAML file in `.github/workflows/` |
| **Event** | Trigger (push, PR, schedule, manual) |
| **Job** | Set of steps running on a runner |
| **Step** | Individual command or action |
| **Action** | Reusable unit (from Marketplace or custom) |
| **Runner** | VM that executes the job (ubuntu, windows, mac) |

### Basic Workflow Example – CI for Node.js
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Run linter
        run: npm run lint
```

### Deploy Workflow Example
```yaml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production

    steps:
      - uses: actions/checkout@v4

      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Push image
        run: docker push myapp:${{ github.sha }}

      - name: Deploy to server
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            docker pull myapp:${{ github.sha }}
            docker stop myapp || true
            docker run -d --name myapp -p 80:3000 myapp:${{ github.sha }}
```

### Scheduled Workflow (Cron)
```yaml
on:
  schedule:
    - cron: '0 2 * * *'     # 2am UTC daily
```

### Matrix Builds (Test on Multiple Versions)
```yaml
jobs:
  test:
    strategy:
      matrix:
        node-version: [18, 20, 22]
        os: [ubuntu-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
```

### Useful Context Variables
```yaml
${{ github.sha }}          # commit SHA
${{ github.ref }}          # branch/tag ref
${{ github.actor }}        # user who triggered
${{ github.repository }}   # owner/repo
${{ runner.os }}           # OS of runner
```

### Secrets
```bash
# Set in GitHub: Settings → Secrets and Variables → Actions
# Access in workflow:
${{ secrets.MY_SECRET }}
```

---

## 15. GitHub Security & Access Control

### Repository Visibility
- **Public** – anyone can see; contributors need access to push
- **Private** – only invited collaborators
- **Internal** (Enterprise) – visible to org members only

### Branch Protection Rules
Set in: *Settings → Branches → Branch protection rules*

Key options:
- Require pull request reviews before merging
- Require status checks to pass (CI must be green)
- Require signed commits
- Restrict who can push to branch
- Require linear history
- Include administrators

### Environments and Required Reviewers
```
Settings → Environments → New environment
- Name: production
- Required reviewers: @alice, @bob
- Deployment branches: main only
```

### SSH Keys vs Personal Access Tokens (PATs)

| Method | Use Case |
|--------|----------|
| SSH Key | Git push/pull over SSH |
| PAT (classic) | API calls, HTTPS git operations |
| Fine-grained PAT | Scoped to specific repos and permissions |
| GitHub App | Automated bots, integrations |

### Dependabot – Automated Security Updates
`.github/dependabot.yml`:
```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 10

  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "monthly"
```

### Code Scanning (Secret Scanning)
- Enable in *Settings → Security → Code security and analysis*
- **Secret scanning** – detects accidentally committed API keys, passwords
- **Code scanning (CodeQL)** – static analysis for vulnerabilities
- **Dependency review** – blocks PRs that introduce vulnerable deps

### Signed Commits (GPG)
```bash
gpg --gen-key
gpg --list-secret-keys --keyid-format=long
git config --global user.signingkey <KEY_ID>
git config --global commit.gpgsign true
git commit -S -m "Signed commit"
```

---

## 16. Git Hooks – Automation at Commit Level

Hooks are scripts in `.git/hooks/` that fire on Git events. They're not pushed to remotes — use tools like `husky` for team-shared hooks.

### Common Hook Types

| Hook | Trigger | Use Case |
|------|---------|----------|
| `pre-commit` | Before commit | Run linter, tests |
| `commit-msg` | After message typed | Enforce commit message format |
| `pre-push` | Before push | Run full test suite |
| `post-merge` | After merge | Install dependencies |
| `pre-rebase` | Before rebase | Safety checks |

### Example: pre-commit Hook
```bash
#!/bin/bash
# .git/hooks/pre-commit
# Make executable: chmod +x .git/hooks/pre-commit

echo "Running linter..."
npm run lint
if [ $? -ne 0 ]; then
    echo "Lint failed. Fix errors before committing."
    exit 1
fi
```

### Example: commit-msg Hook (Enforce Conventional Commits)
```bash
#!/bin/bash
# .git/hooks/commit-msg
MSG=$(cat "$1")
PATTERN="^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .{1,72}"

if ! echo "$MSG" | grep -qE "$PATTERN"; then
    echo "ERROR: Commit message must follow Conventional Commits format."
    echo "Example: feat(auth): add OAuth2 login"
    exit 1
fi
```

### Husky (Team-Shared Hooks via npm)
```bash
npm install --save-dev husky
npx husky init
# Creates .husky/ directory
echo "npm run lint" > .husky/pre-commit
echo "npm test" > .husky/pre-push
```

### Conventional Commits Standard
```
<type>(<scope>): <short description>

Types: feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert

Examples:
feat(auth): add Google OAuth login
fix(api): handle null response from payment gateway
docs(readme): update installation instructions
refactor(db): extract connection pool to separate module
```

---

## 17. Submodules & Subtrees

### Git Submodules
Submodules let you include one Git repo inside another — the parent repo stores a reference (SHA) to a specific commit of the child repo.

```bash
# Add a submodule
git submodule add https://github.com/user/lib.git libs/mylib

# Clone repo with submodules
git clone --recurse-submodules https://github.com/user/project.git

# After cloning without --recurse-submodules
git submodule init
git submodule update

# Update submodule to latest
cd libs/mylib
git pull origin main
cd ../..
git add libs/mylib
git commit -m "Update submodule to latest"

# Update all submodules
git submodule update --remote --merge
```

### Git Subtrees
Subtrees merge another repo's content into a subdirectory — simpler for contributors (no .gitmodules).

```bash
# Add subtree
git subtree add --prefix=libs/mylib https://github.com/user/lib.git main --squash

# Pull updates
git subtree pull --prefix=libs/mylib https://github.com/user/lib.git main --squash

# Push changes back
git subtree push --prefix=libs/mylib https://github.com/user/lib.git main
```

### Submodules vs Subtrees

| | Submodules | Subtrees |
|--|-----------|---------|
| Complexity | Higher | Lower |
| Contributors need to know | Yes | No |
| History | Separate | Merged |
| Push upstream | Easy | Harder |
| Best for | Shared libs with independent versioning | Vendor code, simple embedding |

---

## 18. Git Configuration & Aliases

### Useful Global Settings
```bash
git config --global core.autocrlf input        # LF on checkout (Linux/Mac)
git config --global core.autocrlf true         # CRLF on Windows
git config --global pull.rebase true           # rebase instead of merge on pull
git config --global fetch.prune true           # remove deleted remote branches on fetch
git config --global diff.colorMoved zebra      # color moved lines in diff
git config --global rerere.enabled true        # remember conflict resolutions
git config --global core.excludesfile ~/.gitignore_global
```

### Git Aliases (Shortcuts)
```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm "commit -m"
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.unstage "restore --staged"
git config --global alias.last "log -1 HEAD --stat"
git config --global alias.aliases "config --get-regexp alias"
```

After setting, use them like:
```bash
git st
git lg
git last
```

### ~/.gitconfig Example
```ini
[user]
    name = Your Name
    email = you@example.com

[core]
    editor = vim
    autocrlf = input

[pull]
    rebase = true

[fetch]
    prune = true

[alias]
    lg = log --oneline --graph --all --decorate
    st = status
    co = checkout

[diff]
    colorMoved = zebra

[rerere]
    enabled = true
```

### Global .gitignore
```bash
git config --global core.excludesfile ~/.gitignore_global
cat ~/.gitignore_global
```
```
.DS_Store
Thumbs.db
*.swp
.idea/
.vscode/
*.orig
```

---

## 19. Troubleshooting Common Git Problems

### Accidentally Committed to main Instead of a Branch
```bash
# Create branch at current position
git branch feature-accidental

# Reset main back (hard – removes the commit from main)
git reset --hard HEAD~1

# Switch to the new branch – your commit is safe there
git checkout feature-accidental
```

### Accidentally Committed Sensitive Data (Password, Key)
```bash
# 1. Remove from last commit immediately
git rm --cached secrets.env
echo "secrets.env" >> .gitignore
git commit --amend

# 2. If already pushed – remove from full history with git-filter-repo
pip install git-filter-repo
git filter-repo --path secrets.env --invert-paths

# 3. ALSO: Rotate the secret immediately – assume it's compromised
```

### "Your branch is behind origin/main"
```bash
git pull --rebase origin main
```

### Detached HEAD State
```bash
# You're in "detached HEAD" – not on any branch
git checkout -b new-branch    # save work by creating a branch
# or
git switch main               # go back, losing any commits made here
```

### Large File Accidentally Committed (Repo Too Big)
```bash
# Find large files in history
git rev-list --objects --all | sort -k 2 > objects.txt
git gc
git count-objects -vH

# Remove with git-filter-repo
git filter-repo --path bigfile.zip --invert-paths
```

For ongoing large files: use **Git LFS (Large File Storage)**:
```bash
git lfs install
git lfs track "*.psd"
git add .gitattributes
git add design.psd
git commit -m "Add design file via LFS"
```

### Merge Conflict – Accept Theirs or Ours Entirely
```bash
git checkout --theirs file.txt   # use incoming branch's version
git checkout --ours file.txt     # keep your version
git add file.txt
git commit
```

### Wrong Commit Message Already Pushed
```bash
# If only YOU use the branch:
git commit --amend -m "Correct message"
git push --force-with-lease origin feature

# If shared branch: use git revert instead
```

### Find Which Commit Introduced a Bug (Without bisect)
```bash
git log -S "buggy_function_name"    # find commits that added/removed this string
git log -G "regex_pattern"          # grep commit diffs
```

---

## 20. Git for DevOps – Pipelines, IaC, GitOps

### GitOps Principles
GitOps means using Git as the **single source of truth** for infrastructure and deployments:
1. Desired system state is declared in Git
2. Changes are made via PRs, not manual commands
3. An automated agent continuously syncs the running system to match the repo
4. Drift is detected and corrected automatically

**Tools:** ArgoCD (Kubernetes), Flux, Terraform Cloud

### Infrastructure as Code (IaC) + Git
```bash
# Terraform with Git
git init infra
# Structure:
# infra/
#   main.tf
#   variables.tf
#   environments/
#     dev/
#     prod/
```

**Best practices:**
- Never store `terraform.tfstate` in Git (use remote state: S3, Terraform Cloud)
- Never store secrets in Git (use Vault, AWS Secrets Manager)
- Tag releases: `v1.0.0` before deploying to prod
- Use branch protection on the `main`/`prod` branch of your IaC repo

### Versioning Docker Images with Git SHAs
```yaml
# In GitHub Actions:
docker build -t myapp:${{ github.sha }} .
docker tag myapp:${{ github.sha }} myapp:latest
```

### Monorepo vs Multi-repo

| | Monorepo | Multi-repo |
|--|---------|-----------|
| All services in one repo | ✅ | ❌ |
| Atomic cross-service changes | Easy | Hard |
| Tooling complexity | Higher | Lower |
| Used by | Google, Meta, Uber | Most teams |
| Tools | Nx, Turborepo, Bazel | Standard Git |

### Changelog Automation
```bash
# Using conventional-changelog
npx conventional-changelog-cli -p angular -i CHANGELOG.md -s

# Using release-please (GitHub Action)
# Automatically creates release PRs based on conventional commits
```

### Git in CD Pipelines – Tagging a Release
```bash
# Triggered on version tag push: v*
on:
  push:
    tags:
      - 'v*'
```

---

## 21. Interview Prep – Senior Git & GitHub Role

### Common Senior-Level Questions & Concepts

**1. What is the difference between git fetch and git pull?**
`git fetch` downloads remote changes but doesn't modify your working branch. `git pull` = `fetch` + `merge` (or `rebase`). Best practice: fetch first, review, then merge.

**2. When would you use git rebase vs git merge?**
Use `rebase` on local/private feature branches to keep a clean linear history before merging. Use `merge` (especially `--no-ff`) on shared branches to preserve the full history of integration points.

**3. How do you recover a deleted branch?**
```bash
git reflog                        # find the SHA of the last commit on the branch
git checkout -b recovered-branch <SHA>
```

**4. What is a fast-forward merge?**
When the target branch hasn't diverged from the source — Git simply moves the pointer forward. No merge commit is created. Use `--no-ff` to always create a merge commit.

**5. Explain HEAD, ORIG_HEAD, FETCH_HEAD, MERGE_HEAD**
- `HEAD` – current checkout position
- `ORIG_HEAD` – previous HEAD before a merge/rebase (for undoing)
- `FETCH_HEAD` – what was most recently fetched
- `MERGE_HEAD` – the commit being merged in

**6. How do you squash the last N commits?**
```bash
git rebase -i HEAD~N
# Mark all but the first as 'squash'
```

**7. A developer force-pushed to main and overwrote your colleague's work. How do you recover?**
```bash
git reflog show origin/main       # find the SHA before force push
git push --force origin <good-SHA>:main
```

**8. How does `git cherry-pick` differ from `git merge`?**
`cherry-pick` applies a single specific commit to your branch. `merge` brings the entire branch history. Cherry-pick is used for backporting fixes to release branches.

**9. What is `git rerere`?**
"Reuse Recorded Resolution" — Git remembers how you resolved a conflict and auto-applies it next time the same conflict appears (useful with long-running rebases).

**10. How would you set up a branching strategy for a team of 20 engineers?**
- Gitflow for teams with scheduled releases
- GitHub Flow for continuous deployment teams
- Trunk-based for advanced teams with feature flags
- Enforce with branch protection rules, required reviews, CI status checks

**11. What is the difference between `--force` and `--force-with-lease`?**
`--force` overwrites the remote no matter what. `--force-with-lease` checks that the remote hasn't been updated by someone else since your last fetch — much safer.

**12. How do you find which commit introduced a regression?**
```bash
git bisect start
git bisect bad HEAD
git bisect good v1.5.0
# Run tests at each step, tell git good/bad
git bisect reset
```

**13. How do you keep a long-running feature branch up to date?**
```bash
git fetch origin
git rebase origin/main    # preferred for clean history
# or
git merge origin/main
```

**14. What is a shallow clone and when is it useful?**
```bash
git clone --depth 1 <url>   # clone only the latest commit
```
Used in CI pipelines to speed up build times — you don't need the full history to run tests.

**15. How do you enforce commit message standards across a team?**
- Use a `commit-msg` git hook locally
- Use `commitlint` + `husky` for a shared, npm-distributed hook
- Add CI check that validates commit messages on every PR

**16. What is `git worktree` and when do you use it?**
Allows multiple working directories from the same repo. Useful for reviewing a PR without stashing, or building/releasing one branch while developing another.

**17. Explain GitHub Actions environment protection rules**
Environments (e.g., `production`) can require manual approval from specific reviewers before a deployment job runs, even if all CI checks pass.

**18. How do you handle secrets in GitHub Actions?**
Store in GitHub Secrets (Settings → Secrets), never hardcode in YAML. Use `${{ secrets.MY_SECRET }}` in workflows. Use OIDC token auth for cloud providers (no long-lived keys at all).

**19. What is CODEOWNERS and how does it help?**
`.github/CODEOWNERS` defines which team is responsible for which files. GitHub auto-requests their review on PRs that touch those files, ensuring the right people always review critical changes.

**20. How do you audit who merged a PR or pushed to main?**
- GitHub Audit Log (org-level) — records all events
- `git log --merges --first-parent main` — show merge commits
- GitHub API: `GET /repos/{owner}/{repo}/commits`

### Practical Senior Tasks
- Set up a full CI/CD pipeline with GitHub Actions: lint → test → build → deploy
- Configure branch protection: required reviews, required status checks, no direct pushes to main
- Write a release workflow that auto-tags, creates a GitHub Release, and publishes to npm
- Recover a repository from a bad force push using reflog
- Implement GitOps for a Kubernetes cluster using ArgoCD + GitHub

---

## 22. Cheat Sheets & Quick References

### Daily Git Commands

| Command | What it does |
|---------|-------------|
| `git status` | Show working tree status |
| `git add .` | Stage all changes |
| `git commit -m "msg"` | Commit with message |
| `git push` | Push to remote |
| `git pull --rebase` | Pull with rebase |
| `git log --oneline --graph` | Visual history |
| `git diff --staged` | Review staged changes |
| `git stash` | Shelve changes |
| `git stash pop` | Restore shelved changes |
| `git switch -c branch` | Create & switch branch |
| `git merge branch` | Merge branch |
| `git rebase main` | Rebase onto main |
| `git cherry-pick SHA` | Apply one commit |
| `git bisect` | Binary search for bug |
| `git reflog` | Safety net – all history |

### Undoing Things Quick Reference

| Situation | Command |
|-----------|---------|
| Discard file changes (unstaged) | `git restore file.txt` |
| Unstage a file | `git restore --staged file.txt` |
| Undo last commit (keep changes) | `git reset --soft HEAD~1` |
| Undo last commit (discard changes) | `git reset --hard HEAD~1` |
| Undo a pushed commit (safe) | `git revert HEAD` |
| Amend last commit message | `git commit --amend -m "new"` |
| Recover deleted branch | `git reflog` then `git checkout -b name SHA` |

### Branch Quick Reference

| Command | What it does |
|---------|-------------|
| `git branch` | List branches |
| `git switch -c name` | Create + switch |
| `git switch name` | Switch to branch |
| `git branch -d name` | Delete (safe) |
| `git branch -D name` | Force delete |
| `git push -u origin name` | Push + set upstream |
| `git push origin --delete name` | Delete remote branch |
| `git branch --merged` | Branches merged into current |

### Merge vs Rebase vs Cherry-pick

| Action | Use When |
|--------|---------|
| `merge` | Integrating completed features, shared branches |
| `rebase` | Cleaning up local history before PR |
| `cherry-pick` | Backporting a fix to a release branch |
| `squash` | Combining WIP commits before merging PR |

### GitHub Actions Trigger Events

| Event | Trigger |
|-------|---------|
| `push` | Any push to specified branches |
| `pull_request` | PR opened, updated, synchronized |
| `release` | GitHub release published |
| `schedule` | Cron-based |
| `workflow_dispatch` | Manual trigger (with inputs) |
| `workflow_call` | Called from another workflow |
| `repository_dispatch` | External API trigger |

### Commit Message Quick Format
```
feat:     new feature
fix:      bug fix
docs:     documentation change
style:    formatting (no code change)
refactor: code restructure
perf:     performance improvement
test:     adding/fixing tests
build:    build system, dependencies
ci:       CI/CD config
chore:    maintenance
revert:   revert a commit
```

### Key Files Reference

| File | Purpose |
|------|---------|
| `.gitignore` | Files for Git to ignore |
| `.gitattributes` | Per-path attributes (line endings, diff) |
| `.github/CODEOWNERS` | Auto-assign reviewers |
| `.github/workflows/*.yml` | GitHub Actions |
| `.github/PULL_REQUEST_TEMPLATE.md` | PR description template |
| `.github/ISSUE_TEMPLATE/` | Issue templates |
| `.husky/` | Git hooks (shared via npm) |
| `~/.gitconfig` | Global Git config |

---

## 💡 Final Words

This guide covers everything from `git init` to GitOps, from a single commit to managing releases across 100 engineers. To truly master Git & GitHub:

- 🧪 **Practice on real projects** – spin up repos, break things, recover them
- 🔁 **Simulate team workflows** – use two accounts, open PRs to yourself
- 📋 **Read the reflog** – understanding that Git rarely deletes anything removes all fear
- ⚙️ **Automate with Actions** – build a real CI/CD pipeline end-to-end
- 🌍 **Contribute to open source** – the forking workflow is the best teacher

> A senior Git practitioner isn't someone who memorizes every flag — it's someone who understands the object model, never panics after a bad rebase, and designs workflows that keep a team of 20 moving fast without stepping on each other.

---

## 📄 License

This guide is open-source and free to use for learning and reference purposes.

---

*Happy committing! 🐙*