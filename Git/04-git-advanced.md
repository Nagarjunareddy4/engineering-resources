# Git Advanced

This guide explains advanced Git concepts used by professional developers, DevOps engineers, and platform teams.

If you've ever wondered:

- How do I temporarily save unfinished work?
- Can I recover deleted commits?
- How do I find which commit introduced a bug?
- How do I clean up commit history before merging?

This guide is for you.

---

# 📑 Table of Contents

1. Git Stash
2. Git Reflog
3. Git Bisect
4. Interactive Rebase
5. Common Interview Questions

---

# 1. Git Stash

## Answer

**Git Stash** temporarily saves your uncommitted changes without creating a commit.

It allows you to switch branches or pull the latest changes while preserving your unfinished work.

---

## Workflow

```text
Working Directory

↓

git stash

↓

Clean Working Directory

↓

Switch Branch

↓

git stash pop

↓

Continue Work
```

---

## Common Commands

Save current changes

```bash
git stash
```

Save with a description

```bash
git stash push -m "WIP: login page"
```

View saved stashes

```bash
git stash list
```

Restore and remove latest stash

```bash
git stash pop
```

Restore without removing

```bash
git stash apply
```

Delete a stash

```bash
git stash drop stash@{0}
```

Clear all stashes

```bash
git stash clear
```

---

## Production Example

A developer is working on a payment feature when an urgent production issue needs attention.

Instead of committing incomplete code, they stash their changes, switch to the hotfix branch, fix the issue, and later restore the stashed work.

---

## Production Tip

Use meaningful stash messages when managing multiple work-in-progress tasks.

---

## Interview Question

### Question

What is Git Stash?

### Answer

Git Stash temporarily saves uncommitted changes so you can switch tasks without creating an incomplete commit.

---

# 2. Git Reflog

## Answer

**Git Reflog** records every movement of the HEAD pointer, including commits, resets, checkouts, rebases, and merges.

It can help recover commits that are no longer visible in the normal commit history.

---

## Workflow

```text
Commit

↓

Reset

↓

Commit Lost

↓

git reflog

↓

Recover Commit
```

---

## View Reflog

```bash
git reflog
```

Example Output

```text
HEAD@{0}

HEAD@{1}

HEAD@{2}
```

---

## Recover a Lost Commit

```bash
git reset --hard HEAD@{2}
```

or

```bash
git checkout <commit-id>
```

---

## Production Example

A developer accidentally performs:

```bash
git reset --hard
```

Using `git reflog`, they locate the previous commit and restore the lost work.

---

## Production Tip

Before assuming work is lost, check `git reflog`. It often contains references to commits that are no longer visible through `git log`.

---

## Interview Question

### Question

What is Git Reflog used for?

### Answer

Git Reflog tracks changes to the HEAD pointer and helps recover lost commits after operations such as reset, rebase, or checkout.

---

# 3. Git Bisect

## Answer

**Git Bisect** helps identify the exact commit that introduced a bug by performing a binary search through the commit history.

Instead of checking every commit manually, Git narrows down the search automatically.

---

## Workflow

```text
Good Commit

↓

Binary Search

↓

Bad Commit

↓

Identify Problematic Commit
```

---

## Start Bisect

```bash
git bisect start
```

Mark the current commit as bad

```bash
git bisect bad
```

Mark a known good commit

```bash
git bisect good <commit-id>
```

Git will automatically check out commits for testing.

After testing:

```bash
git bisect good
```

or

```bash
git bisect bad
```

Finish

```bash
git bisect reset
```

---

## Production Example

A production application crashes after a recent deployment.

Using `git bisect`, the team quickly identifies the specific commit that introduced the regression.

---

## Production Tip

Automated test suites make `git bisect` much faster because Git can automatically determine whether each checked-out commit is good or bad.

---

## Interview Question

### Question

What is Git Bisect?

### Answer

Git Bisect uses binary search to identify the commit that introduced a bug.

---

# 4. Interactive Rebase

## Answer

**Interactive Rebase** allows you to modify commit history before sharing it with others.

Common uses include:

- Reordering commits
- Squashing commits
- Editing commit messages
- Removing unnecessary commits

---

## Start Interactive Rebase

Last 3 commits

```bash
git rebase -i HEAD~3
```

---

## Common Actions

```text
pick

reword

edit

squash

fixup

drop
```

---

## Example

Before

```text
Commit A

Commit B

Commit C
```

After Squash

```text
Single Commit
```

---

## Workflow

```text
Multiple Commits

↓

Interactive Rebase

↓

Clean Commit History
```

---

## Production Example

Before opening a Pull Request, a developer squashes several "WIP" commits into one well-structured commit with a clear message.

---

## Production Tip

Use interactive rebase only on local or private branches. Avoid rewriting the history of shared branches unless your team has agreed on that workflow.

---

## Interview Question

### Question

What is Interactive Rebase?

### Answer

Interactive Rebase allows developers to edit, reorder, combine, or remove commits before sharing them with others.

---

# 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Git Stash | Temporarily save uncommitted changes |
| `git stash pop` | Restore and remove the latest stash |
| Git Reflog | Recover lost commits |
| Git Bisect | Find the commit that introduced a bug |
| Interactive Rebase | Clean and rewrite commit history |

---

# 5. Git Hooks

## Answer

**Git Hooks** are scripts that Git executes automatically when specific events occur.

They help automate development tasks and enforce project standards.

---

## Common Hook Types

| Hook | Trigger |
|------|----------|
| pre-commit | Before creating a commit |
| commit-msg | After entering a commit message |
| pre-push | Before pushing commits |
| post-merge | After a successful merge |
| post-checkout | After switching branches |

---

## Hook Location

```text
.git/hooks/
```

---

## Example: Pre-Commit Hook

```bash
#!/bin/bash

echo "Running tests..."

npm test
```

Make it executable:

```bash
chmod +x .git/hooks/pre-commit
```

---

## Workflow

```text
Developer

↓

git commit

↓

Pre-Commit Hook

↓

Tests Pass

↓

Commit Created
```

---

## Production Example

A development team configures a pre-commit hook to:

- Run unit tests
- Check code formatting
- Perform static code analysis

before allowing commits.

---

## Production Tip

Keep Git hooks fast and lightweight. Long-running hooks can reduce developer productivity.

---

## Interview Question

### Question

What are Git Hooks?

### Answer

Git Hooks are scripts that automatically execute before or after Git events such as commits, merges, and pushes.

---

# 6. Git Submodules

## Answer

A **Git Submodule** allows one Git repository to include another Git repository as a dependency.

This is useful when multiple projects share the same library or component.

---

## Architecture

```text
Main Repository

│

├── Application Code

│

└── Shared Library (Submodule)
```

---

## Add a Submodule

```bash
git submodule add https://github.com/company/shared-library.git
```

---

## Clone Repository with Submodules

```bash
git clone --recurse-submodules <repository-url>
```

---

## Update Submodules

```bash
git submodule update --init --recursive
```

---

## Production Example

An organization maintains a shared authentication library that is included as a submodule in several microservice repositories.

---

## Production Tip

Use submodules only when independent versioning of shared code is required. For many projects, package managers are a simpler alternative.

---

## Interview Question

### Question

What is a Git Submodule?

### Answer

A Git Submodule allows one Git repository to reference another repository while maintaining separate version histories.

---

# 7. Git Worktrees

## Answer

**Git Worktrees** allow multiple working directories to be attached to the same Git repository.

This enables developers to work on multiple branches simultaneously without cloning the repository multiple times.

---

## Workflow

```text
Repository

↓

Worktree A → main

↓

Worktree B → feature-login

↓

Worktree C → hotfix
```

---

## Create a Worktree

```bash
git worktree add ../feature-login feature-login
```

---

## List Worktrees

```bash
git worktree list
```

---

## Remove a Worktree

```bash
git worktree remove ../feature-login
```

---

## Production Example

A developer works on a production hotfix while continuing feature development in a separate worktree.

---

## Production Tip

Use worktrees when switching frequently between long-running branches to avoid repeated branch checkouts.

---

## Interview Question

### Question

What is a Git Worktree?

### Answer

A Git Worktree allows multiple working directories for different branches within a single Git repository.

---

# 8. Git Clean

## Answer

**Git Clean** removes untracked files and directories from the working directory.

This is useful when cleaning up build artifacts or temporary files.

---

## Preview Files to Remove

```bash
git clean -n
```

---

## Remove Untracked Files

```bash
git clean -f
```

---

## Remove Files and Directories

```bash
git clean -fd
```

---

## Remove Ignored Files

```bash
git clean -fx
```

---

## Workflow

```text
Untracked Files

↓

git clean

↓

Clean Working Directory
```

---

## Production Example

Before running a clean build, a CI server removes temporary files using `git clean` to ensure a consistent build environment.

---

## Production Tip

Always preview changes with:

```bash
git clean -n
```

before permanently deleting files.

---

## Interview Question

### Question

What does `git clean` do?

### Answer

`git clean` removes untracked files and directories from the working directory.

---

# 9. Git Archive

## Answer

**Git Archive** creates a compressed archive of a repository without including the `.git` directory.

It is commonly used to distribute source code for releases.

---

## Create ZIP Archive

```bash
git archive --format=zip HEAD -o project.zip
```

---

## Create TAR Archive

```bash
git archive --format=tar HEAD -o project.tar
```

---

## Workflow

```text
Repository

↓

git archive

↓

ZIP / TAR File
```

---

## Production Example

A release engineer packages application source code into a ZIP archive for customers who require source delivery.

---

## Production Tip

Use `git archive` when you need project source code without exposing repository history.

---

## Interview Question

### Question

What is Git Archive?

### Answer

Git Archive creates a compressed archive of repository contents without including Git metadata.

---

# 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Git Hooks | Automate Git events |
| Git Submodule | Repository inside another repository |
| Git Worktree | Multiple working directories |
| Git Clean | Remove untracked files |
| Git Archive | Export repository without `.git` |

---

# 10. Advanced Git Troubleshooting

## Answer

As projects grow, Git issues become more complex. Understanding common problems and their solutions is essential for efficient development.

---

## Problem 1

### Accidentally Deleted a Commit

Recover using:

```bash
git reflog
```

Restore the commit:

```bash
git reset --hard HEAD@{2}
```

---

## Problem 2

### Wrong Branch

Check the current branch

```bash
git branch
```

Switch to the correct branch

```bash
git switch feature-login
```

---

## Problem 3

### Detached HEAD

Check status

```bash
git status
```

Create a new branch if you want to keep your work

```bash
git switch -c recover-work
```

---

## Problem 4

### Merge Conflict

Resolve conflicts manually.

Then:

```bash
git add .

git commit
```

---

## Problem 5

### Rebase Conflict

Resolve conflicts.

Continue:

```bash
git rebase --continue
```

Abort if necessary:

```bash
git rebase --abort
```

---

## Production Example

During a release, a developer accidentally resets the current branch.

Using `git reflog`, the team restores the lost commits within minutes.

---

## Production Tip

Never panic after losing commits. Check `git reflog` before assuming work is permanently lost.

---

## Interview Question

### Question

How do you recover lost commits?

### Answer

Use `git reflog` to locate the previous commit and restore it using `git reset` or by creating a new branch from the recovered commit.

---

# 11. Repository Performance Optimization

## Answer

Large repositories can become slower over time due to extensive history, large files, and unnecessary objects.

Git provides maintenance commands to improve performance.

---

## Repository Maintenance

Garbage Collection

```bash
git gc
```

Prune unreachable objects

```bash
git prune
```

Check repository size

```bash
git count-objects -vH
```

---

## Workflow

```text
Repository Growth

↓

Maintenance

↓

Optimized Repository
```

---

## Production Example

A long-running repository with thousands of commits periodically runs `git gc` to optimize storage and improve performance.

---

## Production Tip

Schedule repository maintenance for large projects, especially on self-hosted Git servers.

---

## Interview Question

### Question

What does `git gc` do?

### Answer

`git gc` performs garbage collection by compressing repository data and removing unnecessary objects to improve performance.

---

# 12. Managing Large Repositories

## Answer

Large repositories require careful planning to maintain performance and scalability.

---

## Best Practices

- Avoid committing generated files.
- Exclude build artifacts using `.gitignore`.
- Keep binary files out of Git whenever possible.
- Use Git LFS (Large File Storage) for large assets.
- Remove obsolete branches regularly.

---

## Git LFS Example

Track large files

```bash
git lfs track "*.zip"
```

Commit the tracking configuration

```bash
git add .gitattributes

git commit -m "Track ZIP files with Git LFS"
```

---

## Production Example

A game development team stores large media assets using Git LFS while keeping source code in the main Git repository.

---

## Production Tip

Use Git for source code, not as a file storage system.

---

## Interview Question

### Question

What is Git LFS?

### Answer

Git LFS (Large File Storage) stores large files outside the normal Git object database while keeping lightweight references inside the repository.

---

# 13. Advanced Git Best Practices

## Answer

Advanced teams follow disciplined Git workflows to ensure reliability, maintainability, and collaboration.

---

## Recommended Practices

✅ Commit small, logical changes.

---

✅ Use descriptive commit messages.

---

✅ Rebase local feature branches before opening Pull Requests.

---

✅ Protect important branches.

---

✅ Keep feature branches short-lived.

---

✅ Avoid force-pushing shared branches.

---

✅ Review code before merging.

---

✅ Tag production releases.

---

## Production Workflow

```text
Issue

↓

Feature Branch

↓

Commit

↓

Rebase

↓

Pull Request

↓

Code Review

↓

CI Pipeline

↓

Merge

↓

Release Tag
```

---

## Production Example

A financial services company enforces:

- Protected branches
- Mandatory code reviews
- Passing CI pipelines
- Signed commits
- Release tagging

before deploying to production.

---

## Production Tip

Treat your Git history as project documentation. A clean history simplifies debugging, auditing, and onboarding new team members.

---

## Interview Question

### Question

What are advanced Git best practices?

### Answer

Use clean commit history, feature branches, protected branches, code reviews, CI validation, release tags, and avoid rewriting shared history.

---

# 📌 Quick Revision

### Advanced Git Workflow

```text
Feature Branch

↓

Commit

↓

Interactive Rebase

↓

Push

↓

Pull Request

↓

Review

↓

Merge

↓

Tag

↓

Release
```

---

### Important Commands

```bash
git stash

git reflog

git bisect

git rebase -i

git worktree

git clean

git archive

git gc

git prune

git lfs
```

---

# Summary

After completing this guide, you should understand:

✅ Git Stash

✅ Git Reflog

✅ Git Bisect

✅ Interactive Rebase

✅ Git Hooks

✅ Git Submodules

✅ Git Worktrees

✅ Git Clean

✅ Git Archive

✅ Advanced Git Troubleshooting

✅ Repository Performance Optimization

✅ Git LFS

✅ Advanced Git Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Git Stash? | Temporarily stores uncommitted changes |
| What is Git Reflog? | Tracks HEAD movements and helps recover lost commits |
| What is Git Bisect? | Finds the commit that introduced a bug using binary search |
| What is Interactive Rebase? | Rewrites and cleans commit history |
| What are Git Hooks? | Scripts triggered by Git events |
| What is a Git Submodule? | A repository embedded inside another repository |
| What is a Git Worktree? | Multiple working directories for one repository |
| What does `git clean` do? | Removes untracked files and directories |
| What does `git gc` do? | Optimizes repository storage |
| What is Git LFS? | Manages large files outside Git's normal object database |

---

# 🎉 Git Advanced Module Completed

Congratulations! You have completed **Git Advanced**, including:

- ✅ Git Stash
- ✅ Git Reflog
- ✅ Git Bisect
- ✅ Interactive Rebase
- ✅ Git Hooks
- ✅ Git Submodules
- ✅ Git Worktrees
- ✅ Git Clean
- ✅ Git Archive
- ✅ Advanced Git Troubleshooting
- ✅ Repository Performance Optimization
- ✅ Git LFS
- ✅ Advanced Git Best Practices

You now have an advanced understanding of Git internals, collaboration workflows, repository maintenance, and troubleshooting—skills expected of senior Software Engineers, DevOps Engineers, Platform Engineers, and Site Reliability Engineers (SREs).

---

# 🚀 What's Next?

➡️ **05-git-best-practices.md**

In the final Git guide, you'll learn:

- Repository Organization
- Commit Message Standards
- Branch Naming Conventions
- Collaboration Best Practices
- Security Best Practices
- Git Anti-Patterns
- Enterprise Git Workflows
- Production Checklist
- Git Interview Tips