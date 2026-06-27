# Git Fundamentals

This guide explains everything you need to know about Git Fundamentals.

If you've ever wondered:

- What is Git?
- Why do we use Git?
- How does Git work?
- What is a Repository?
- What is a Commit?
- How does the Git workflow function?

This guide is for you.

---

# 📑 Table of Contents

1. What is Git?
2. Why Git?
3. Git Architecture
4. Git Repository
5. Git Commit
6. Git Workflow
7. Common Interview Questions

---

# 1. What is Git?

## Answer

**Git** is a **Distributed Version Control System (DVCS)** used to track changes in source code and collaborate with other developers.

Unlike traditional version control systems, every developer has a complete copy of the repository, including its history.

Git helps teams:

- Track code changes
- Collaborate efficiently
- Restore previous versions
- Manage multiple features simultaneously
- Merge code safely

---

## Architecture

```text
Developer

↓

Git Repository

↓

Version History

↓

Source Code
```

---

## Key Characteristics

Git provides:

- Distributed Architecture
- Fast Performance
- Branching & Merging
- Version History
- Offline Development
- Collaboration

---

## Real World Example

A team of developers works on an e-commerce application.

Each developer creates their own branch, makes changes, commits code locally, and later merges those changes into the main branch after code review.

---

## Benefits

Git provides:

- Version Control
- Collaboration
- Backup of Code History
- Easy Rollback
- Parallel Development

---

## Production Tip

Commit small, meaningful changes instead of one large commit. This makes reviews, debugging, and rollbacks much easier.

---

## Interview Question

### Question

What is Git?

### Answer

Git is a Distributed Version Control System that tracks changes in files and enables multiple developers to collaborate efficiently.

---

# 2. Why Git?

## Answer

Git solves many challenges faced during software development.

Without Git:

- Code could be overwritten.
- Tracking changes would be difficult.
- Collaboration would become error-prone.
- Recovering old versions would be nearly impossible.

---

## Common Use Cases

- Version Control
- Team Collaboration
- Feature Development
- Bug Fixing
- Release Management
- CI/CD Pipelines

---

## Workflow

```text
Write Code

↓

Track Changes

↓

Commit

↓

Share

↓

Merge
```

---

## Production Example

A production application has hundreds of developers working simultaneously.

Git allows each developer to work independently while keeping all changes organized and traceable.

---

## Production Tip

Use Git from the beginning of every project—even personal projects—to maintain a complete history of your work.

---

## Interview Question

### Question

Why do we use Git?

### Answer

Git helps developers track changes, collaborate with teams, maintain version history, and safely manage source code.

---

# 3. Git Architecture

## Answer

Git has three main working areas where code moves during development.

---

## Git Architecture

```text
Working Directory

↓

Staging Area (Index)

↓

Local Repository

↓

Remote Repository
```

---

## Components

| Component | Purpose |
|-----------|---------|
| Working Directory | Where you edit files |
| Staging Area | Holds changes before committing |
| Local Repository | Stores commits on your computer |
| Remote Repository | Shared repository (GitHub, GitLab, Azure Repos) |

---

## Workflow

```text
Modify File

↓

git add

↓

Staging Area

↓

git commit

↓

Local Repository

↓

git push

↓

Remote Repository
```

---

## Production Example

A developer modifies application code, stages the changes, commits them locally, tests the application, and finally pushes the commits to GitHub.

---

## Production Tip

Understand the difference between the Working Directory, Staging Area, and Repository—this is one of the most commonly tested Git concepts in interviews.

---

## Interview Question

### Question

What are the main areas in Git Architecture?

### Answer

Working Directory, Staging Area, Local Repository, and Remote Repository.

---

# 4. Git Repository

## Answer

A **Git Repository (Repo)** is a storage location that contains:

- Source Code
- Commit History
- Branches
- Tags
- Configuration

Repositories can be:

- Local
- Remote

---

## Initialize a Repository

```bash
git init
```

---

## Clone an Existing Repository

```bash
git clone https://github.com/example/project.git
```

---

## Repository Structure

```text
Project Folder

├── .git/
├── src/
├── README.md
└── package.json
```

The hidden `.git` directory stores all Git metadata and history.

---

## Production Example

A company stores its application source code in a GitHub repository where developers collaborate through branches and pull requests.

---

## Production Tip

Never delete the `.git` directory unless you intentionally want to remove version history from the project.

---

## Interview Question

### Question

What is a Git Repository?

### Answer

A Git Repository is a storage location that contains project files along with their complete version history and metadata.

---

# 5. Git Commit

## Answer

A **Commit** is a snapshot of the project's changes at a specific point in time.

Every commit has a unique SHA (hash) that identifies it permanently.

---

## Create a Commit

Stage changes

```bash
git add .
```

Commit changes

```bash
git commit -m "Add user authentication feature"
```

---

## View Commit History

```bash
git log
```

Compact view

```bash
git log --oneline
```

---

## Workflow

```text
Modify Files

↓

Stage Changes

↓

Commit

↓

History Updated
```

---

## Production Example

A developer fixes a production bug and creates a commit with the message:

```text
Fix payment gateway timeout issue
```

This provides clear documentation of the change.

---

## Production Tip

Write descriptive commit messages that explain **what** changed and **why** it changed.

---

## Interview Question

### Question

What is a Git Commit?

### Answer

A Git Commit is a snapshot of changes that records the state of a project at a specific point in time.

---

# 6. Git Workflow

## Answer

The Git workflow describes how developers make, save, and share code changes.

---

## Basic Workflow

```text
Create/Modify Files

↓

git status

↓

git add

↓

git commit

↓

git push
```

---

## Common Commands

Check repository status

```bash
git status
```

Stage changes

```bash
git add .
```

Commit changes

```bash
git commit -m "Your commit message"
```

Push to remote

```bash
git push
```

---

## Production Example

A developer:

1. Creates a feature.
2. Stages the changes.
3. Commits the work.
4. Pushes the branch to GitHub.
5. Opens a Pull Request for review.

---

## Production Tip

Run tests before committing and pushing changes to reduce build failures in CI/CD pipelines.

---

## Interview Question

### Question

What is the basic Git workflow?

### Answer

Modify files → Stage changes → Commit changes → Push commits to the remote repository.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Git | Distributed Version Control System |
| Repository | Stores project and version history |
| Working Directory | Where files are edited |
| Staging Area | Prepares changes for commit |
| Commit | Snapshot of changes |
| Local Repository | Stores commits locally |
| Remote Repository | Shared repository |
| `git init` | Initialize repository |
| `git clone` | Copy remote repository |
| `git add` | Stage changes |
| `git commit` | Save changes |
| `git push` | Upload commits |

---

# 7. Installing Git

## Answer

Git is available for Linux, Windows, and macOS.

---

## Install on Ubuntu/Debian

```bash
sudo apt update

sudo apt install git -y
```

---

## Install on RHEL/CentOS

```bash
sudo yum install git -y
```

or

```bash
sudo dnf install git -y
```

---

## Install on macOS

```bash
brew install git
```

---

## Verify Installation

```bash
git --version
```

Example Output

```text
git version 2.43.0
```

---

## Production Example

Before cloning a repository on a new Linux server, verify Git is installed using:

```bash
git --version
```

---

## Production Tip

Always use a recent Git version to receive bug fixes, security patches, and new features.

---

## Interview Question

### Question

How do you verify Git installation?

### Answer

Run:

```bash
git --version
```

---

# 8. Git Configuration

## Answer

Git uses configuration settings to identify who creates commits and how Git behaves.

Configurations can be:

- System Level
- Global Level
- Local Repository Level

---

## Configure Username

```bash
git config --global user.name "Nagarjuna Reddy"
```

---

## Configure Email

```bash
git config --global user.email "your-email@example.com"
```

---

## View Configuration

```bash
git config --list
```

---

## View a Specific Configuration

```bash
git config user.name
```

---

## Configuration Levels

| Level | Scope |
|--------|-------|
| System | Entire machine |
| Global | Current user |
| Local | Current repository only |

---

## Production Example

Each developer configures their Git username and email before making commits so that commit history correctly identifies the author.

---

## Production Tip

Use your organization's email address for work repositories to maintain accurate commit history.

---

## Interview Question

### Question

How do you configure your Git username?

### Answer

```bash
git config --global user.name "Your Name"
```

---

# 9. git status

## Answer

`git status` displays the current state of the repository.

It shows:

- Modified files
- Staged files
- Untracked files
- Current branch

---

## Command

```bash
git status
```

---

## Workflow

```text
Working Directory

↓

git status

↓

Repository Status
```

---

## Production Example

Before committing changes, a developer runs:

```bash
git status
```

to verify that the correct files are staged.

---

## Production Tip

Make it a habit to run `git status` before every commit.

---

## Interview Question

### Question

What does `git status` do?

### Answer

It displays the current state of the working directory and staging area.

---

# 10. git diff

## Answer

`git diff` displays differences between files, commits, or branches.

It helps review changes before creating a commit.

---

## Compare Working Directory

```bash
git diff
```

---

## Compare Staged Changes

```bash
git diff --staged
```

---

## Compare Two Commits

```bash
git diff commit1 commit2
```

---

## Workflow

```text
Modified File

↓

git diff

↓

Changed Lines
```

---

## Production Example

A developer reviews code changes with `git diff` before submitting a pull request.

---

## Production Tip

Always review your changes before committing to avoid accidentally committing unwanted code.

---

## Interview Question

### Question

What is `git diff` used for?

### Answer

`git diff` shows the differences between files, commits, or branches.

---

# 11. git log

## Answer

`git log` displays the commit history of a repository.

It provides information such as:

- Commit ID
- Author
- Date
- Commit Message

---

## View Commit History

```bash
git log
```

Compact View

```bash
git log --oneline
```

Graph View

```bash
git log --graph --oneline --all
```

---

## Production Example

A DevOps engineer reviews recent commits to identify which change introduced a deployment issue.

---

## Production Tip

Use:

```bash
git log --oneline
```

for a concise overview of commit history.

---

## Interview Question

### Question

What is the purpose of `git log`?

### Answer

`git log` displays the commit history of a Git repository.

---

# 12. git show

## Answer

`git show` displays detailed information about a specific commit.

It includes:

- Commit metadata
- Author
- Date
- Changed files
- Code differences

---

## Show Latest Commit

```bash
git show
```

---

## Show Specific Commit

```bash
git show <commit-id>
```

Example

```bash
git show a3b2c4d
```

---

## Production Example

A developer inspects a production hotfix commit to verify exactly what changes were made.

---

## Production Tip

Use `git show` when investigating issues introduced by a particular commit.

---

## Interview Question

### Question

What does `git show` display?

### Answer

It displays detailed information about a commit, including metadata and code changes.

---

# 13. .gitignore

## Answer

The `.gitignore` file specifies files and directories that Git should ignore.

Ignored files are not tracked or committed.

---

## Example

```text
node_modules/

.env

*.log

dist/

build/
```

---

## Workflow

```text
Project Files

↓

.gitignore

↓

Ignored Files

↓

Not Tracked
```

---

## Production Example

Sensitive files such as:

```text
.env
```

are excluded from version control to prevent secrets from being committed.

---

## Production Tip

Never commit:

- API Keys
- Passwords
- Certificates
- Private Keys
- Environment Files

Use `.gitignore` to exclude them.

---

## Interview Question

### Question

What is `.gitignore`?

### Answer

`.gitignore` is a configuration file that tells Git which files and directories should not be tracked.

---

# 14. Git Best Practices

## Answer

Following Git best practices improves collaboration and repository maintainability.

---

## Recommended Practices

✅ Commit small, logical changes.

---

✅ Write meaningful commit messages.

---

✅ Pull the latest changes before starting work.

---

✅ Review changes with:

```bash
git diff
```

---

✅ Check repository status:

```bash
git status
```

before committing.

---

✅ Never commit secrets or credentials.

---

✅ Use feature branches instead of committing directly to the main branch.

---

## Production Example

A development team follows a workflow where every feature is developed in its own branch, reviewed through a Pull Request, and merged only after successful testing.

---

## Production Tip

Treat every commit as a permanent record. Keep commits focused, descriptive, and easy to understand.

---

## Interview Question

### Question

What are Git best practices?

### Answer

Use small commits, descriptive commit messages, feature branches, `.gitignore`, regular pulls, and code reviews before merging.

---

# 📌 Quick Revision

| Command | Purpose |
|----------|---------|
| `git --version` | Verify Git installation |
| `git config` | Configure Git settings |
| `git status` | View repository status |
| `git diff` | Compare changes |
| `git log` | View commit history |
| `git show` | Display commit details |
| `.gitignore` | Ignore files from tracking |

---

# 15. Understanding the Git Staging Area

## Answer

The **Staging Area (Index)** is an intermediate area between your **Working Directory** and the **Local Repository**.

It allows you to choose exactly which changes should be included in the next commit.

---

## Workflow

```text
Working Directory

↓

git add

↓

Staging Area

↓

git commit

↓

Local Repository
```

---

## Common Commands

Stage all changes

```bash
git add .
```

Stage a specific file

```bash
git add app.js
```

View staged changes

```bash
git diff --staged
```

---

## Production Example

A developer modifies five files but only stages two files related to a bug fix.

This keeps the commit focused and easier to review.

---

## Production Tip

Stage only related changes together. Avoid mixing bug fixes, new features, and formatting changes in a single commit.

---

## Interview Question

### Question

Why does Git use a Staging Area?

### Answer

The staging area allows developers to review and organize changes before creating a commit.

---

# 16. Undoing Changes

## Answer

Git provides multiple ways to undo changes depending on whether they are:

- Untracked
- Modified
- Staged
- Committed

---

## Restore a Modified File

```bash
git restore file.txt
```

---

## Unstage a File

```bash
git restore --staged file.txt
```

---

## Reset to Previous Commit

Soft reset

```bash
git reset --soft HEAD~1
```

Mixed reset

```bash
git reset HEAD~1
```

Hard reset

```bash
git reset --hard HEAD~1
```

---

## Revert a Commit

```bash
git revert <commit-id>
```

Unlike `git reset`, `git revert` creates a new commit that reverses the changes without rewriting history.

---

## Production Example

A faulty commit has already been pushed to the shared repository.

Instead of using `git reset`, the team uses:

```bash
git revert
```

to safely undo the changes while preserving commit history.

---

## Production Tip

Avoid using `git reset --hard` on shared branches because it rewrites history and may affect other team members.

---

## Interview Question

### Question

What is the difference between `git reset` and `git revert`?

### Answer

`git reset` moves the branch pointer and can rewrite history, while `git revert` creates a new commit that safely undoes previous changes.

---

# 17. Git Tags

## Answer

A **Tag** is a reference to a specific commit, commonly used to mark releases or important milestones.

---

## Create a Tag

```bash
git tag v1.0.0
```

---

## List Tags

```bash
git tag
```

---

## Push a Tag

```bash
git push origin v1.0.0
```

---

## Workflow

```text
Commit

↓

Tag

↓

Release Version
```

---

## Production Example

After successfully deploying version 2.0.0 of an application, the release commit is tagged:

```text
v2.0.0
```

making it easy to identify and roll back if needed.

---

## Production Tip

Use semantic versioning (e.g., `v1.0.0`, `v1.2.3`) for release tags.

---

## Interview Question

### Question

What are Git Tags used for?

### Answer

Git Tags mark important commits, such as software releases, making them easy to reference later.

---

# 18. Git Aliases

## Answer

Git aliases allow you to create shortcuts for frequently used commands.

---

## Create an Alias

```bash
git config --global alias.st status
```

---

## Examples

Alias for status

```bash
git st
```

Alias for log

```bash
git config --global alias.lg "log --oneline --graph --all"
```

Use

```bash
git lg
```

---

## Production Example

Developers create aliases for commonly used commands to improve productivity and reduce typing.

---

## Production Tip

Keep aliases simple and meaningful so they are easy to remember.

---

## Interview Question

### Question

What is a Git alias?

### Answer

A Git alias is a custom shortcut for frequently used Git commands.

---

# 19. Git Troubleshooting

## Answer

Git issues are common during collaborative development.

Knowing how to diagnose and resolve them is an important skill.

---

## Problem 1

### Changes Not Committed

Check repository status

```bash
git status
```

---

## Problem 2

### Wrong File Staged

Unstage it

```bash
git restore --staged file.txt
```

---

## Problem 3

### Wrong Commit Message

Amend the latest commit

```bash
git commit --amend
```

---

## Problem 4

### Detached HEAD

Identify your branch

```bash
git branch
```

Switch back

```bash
git switch main
```

---

## Problem 5

### Merge Conflict

Steps:

1. Open the conflicted file.
2. Resolve the conflict manually.
3. Stage the resolved file.

```bash
git add file.txt
```

4. Complete the merge.

```bash
git commit
```

---

## Production Example

Two developers modify the same file in different branches.

Git reports a merge conflict, which is resolved manually before completing the merge.

---

## Production Tip

Pull the latest changes frequently to reduce the likelihood of merge conflicts.

---

## Interview Question

### Question

How do you resolve a Git merge conflict?

### Answer

Review the conflicting file, manually resolve the differences, stage the resolved file, and complete the merge with a commit.

---

# 📌 Quick Revision

### Git Workflow

```text
Modify Files

↓

git status

↓

git add

↓

git commit

↓

git push
```

---

### Undo Workflow

```text
Modified File

↓

git restore

↓

Working Directory Restored
```

---

### Important Commands

```bash
git init

git clone

git status

git add

git commit

git log

git show

git diff

git restore

git reset

git revert

git tag

git push

git config
```

---

# Summary

After completing this guide, you should understand:

✅ Git Fundamentals

✅ Git Architecture

✅ Repositories

✅ Commits

✅ Git Workflow

✅ Git Configuration

✅ `git status`

✅ `git diff`

✅ `git log`

✅ `git show`

✅ `.gitignore`

✅ Staging Area

✅ Undoing Changes

✅ Git Tags

✅ Git Aliases

✅ Git Troubleshooting

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Git? | A Distributed Version Control System |
| What is a Repository? | A storage location for project files and version history |
| What is a Commit? | A snapshot of project changes |
| What is the Staging Area? | An intermediate area where changes are prepared before committing |
| What does `git status` do? | Displays the repository status |
| What does `git diff` do? | Shows differences between files or commits |
| What is `.gitignore`? | Specifies files Git should ignore |
| `git reset` vs `git revert` | Reset rewrites history; revert safely creates a new undo commit |
| What are Git Tags? | Labels for important commits, often used for releases |
| What are Git Aliases? | Custom shortcuts for Git commands |

---

# 🎉 Git Fundamentals Module Completed

Congratulations! You have completed **Git Fundamentals**, including:

- ✅ What is Git?
- ✅ Why Git?
- ✅ Git Architecture
- ✅ Git Repositories
- ✅ Commits
- ✅ Git Workflow
- ✅ Git Configuration
- ✅ Repository Status & History
- ✅ `.gitignore`
- ✅ Staging Area
- ✅ Undoing Changes
- ✅ Git Tags
- ✅ Git Aliases
- ✅ Git Troubleshooting

You now have a solid foundation in Git, enabling you to track changes, collaborate with teams, manage repositories, and confidently use Git in modern software development and DevOps workflows.

---

# 🚀 What's Next?

➡️ **02-branching-merging.md**

In the next guide, you'll learn:

- What are Branches?
- Why Branching is Important
- Creating & Switching Branches
- Merging Branches
- Fast-Forward vs Three-Way Merge
- Merge Conflicts
- Rebasing
- Cherry-Picking
- Branch Management Best Practices
- Common Interview Questions