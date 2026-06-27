# Git Branching & Merging

This guide explains everything you need to know about Git Branching and Merging.

If you've ever wondered:

- What is a Git Branch?
- Why do developers use branches?
- How do branches work?
- How do you create and switch branches?
- What is Git Merge?
- Why is branching important in DevOps?

This guide is for you.

---

# 📑 Table of Contents

1. What is Branching?
2. Why Use Branches?
3. Branch Architecture
4. Creating Branches
5. Switching Branches
6. Viewing & Managing Branches
7. Common Interview Questions

---

# 1. What is Branching?

## Answer

A **Git Branch** is an independent line of development.

Instead of making changes directly to the main codebase, developers create a branch where they can safely develop new features, fix bugs, or experiment without affecting the stable version.

Each branch has its own commits and development history until it is merged.

---

## Branch Architecture

```text
main

│

├── feature-login

│

├── feature-payment

│

└── bugfix-cart
```

---

## Key Characteristics

Git Branches provide:

- Parallel Development
- Safe Experimentation
- Feature Isolation
- Easier Collaboration
- Independent Commit History

---

## Real World Example

A team is developing an e-commerce platform.

Instead of everyone modifying the `main` branch:

- Developer A works on `feature-login`
- Developer B works on `feature-payment`
- Developer C works on `bugfix-checkout`

Each developer works independently until their changes are reviewed and merged.

---

## Benefits

Branching provides:

- Safe Development
- Easier Testing
- Better Collaboration
- Cleaner Release Management
- Faster Feature Development

---

## Production Tip

Never develop directly on the `main` branch. Create a dedicated branch for every feature, bug fix, or hotfix.

---

## Interview Question

### Question

What is a Git Branch?

### Answer

A Git Branch is an independent line of development that allows developers to work on changes without affecting the main codebase.

---

# 2. Why Use Branches?

## Answer

Branches allow multiple developers to work simultaneously on different tasks without interfering with each other's work.

Without branches:

- Developers would overwrite each other's code.
- Releases would become unstable.
- Testing would be difficult.
- Collaboration would slow down.

---

## Common Branch Types

| Branch | Purpose |
|---------|---------|
| `main` | Stable production-ready code |
| `develop` | Integration branch for ongoing development |
| `feature/*` | New feature development |
| `bugfix/*` | Bug fixes |
| `hotfix/*` | Critical production fixes |
| `release/*` | Preparing a software release |

---

## Workflow

```text
main

↓

Create Feature Branch

↓

Develop Feature

↓

Test

↓

Merge

↓

Delete Branch
```

---

## Production Example

A company has 20 developers working on the same application.

Each developer creates their own feature branch, ensuring everyone can work independently without affecting production code.

---

## Production Tip

Use descriptive branch names such as:

```text
feature/user-authentication

bugfix/login-timeout

hotfix/payment-failure
```

Avoid names like:

```text
test

branch1

new-feature
```

---

## Interview Question

### Question

Why do we use Git Branches?

### Answer

Git Branches allow developers to work independently on features, bug fixes, and experiments without impacting the stable codebase.

---

# 3. Git Branch Architecture

## Answer

Every Git repository starts with a default branch, typically named:

```text
main
```

New branches are created from existing branches and share the same history until new commits are added.

---

## Example Architecture

```text
A──B──C  (main)

      \

       D──E  (feature-login)
```

Commit **C** is the common ancestor.

After branching:

- `main` continues independently.
- `feature-login` has its own commits.

---

## Workflow

```text
Repository

↓

main

↓

Create Branch

↓

Independent Development

↓

Merge
```

---

## Production Example

A feature branch is created from the latest `main` branch before development begins.

Once development and testing are complete, it is merged back into `main`.

---

## Production Tip

Always create a feature branch from the latest version of `main` (or `develop`, depending on your team's workflow) to minimize merge conflicts.

---

## Interview Question

### Question

Does creating a branch copy the entire repository?

### Answer

No. Git branches are lightweight pointers to commits, making branch creation fast and storage-efficient.

---

# 4. Creating Branches

## Answer

A new branch is created to isolate development work.

---

## Create a Branch

```bash
git branch feature-login
```

---

## Create and Switch in One Command

```bash
git switch -c feature-login
```

Older Git versions:

```bash
git checkout -b feature-login
```

---

## Verify Branch Creation

```bash
git branch
```

Example Output

```text
* feature-login

  main
```

The `*` indicates the current branch.

---

## Workflow

```text
main

↓

git branch feature-login

↓

feature-login
```

---

## Production Example

A developer starts implementing user authentication by creating:

```text
feature-login
```

instead of modifying `main`.

---

## Production Tip

Create a new branch for every task, even for small bug fixes.

---

## Interview Question

### Question

How do you create a new Git Branch?

### Answer

Use:

```bash
git branch branch-name
```

or create and switch immediately with:

```bash
git switch -c branch-name
```

---

# 5. Switching Branches

## Answer

Switching branches changes the working directory to match another branch.

Git updates files to reflect the selected branch's latest commit.

---

## Switch Branch

```bash
git switch main
```

Older syntax:

```bash
git checkout main
```

---

## Switch to Another Branch

```bash
git switch feature-login
```

---

## Workflow

```text
feature-login

↓

git switch main

↓

main
```

---

## Production Example

After completing a feature, a developer switches back to `main` to pull the latest updates before starting another task.

---

## Production Tip

Commit or stash your changes before switching branches to avoid losing work or creating conflicts.

---

## Interview Question

### Question

How do you switch branches in Git?

### Answer

Use:

```bash
git switch branch-name
```

or, in older Git versions:

```bash
git checkout branch-name
```

---

# 6. Viewing & Managing Branches

## Answer

Git provides commands to list, rename, and delete branches.

---

## List Local Branches

```bash
git branch
```

---

## List All Branches

```bash
git branch -a
```

---

## Rename Current Branch

```bash
git branch -m new-branch-name
```

---

## Delete a Merged Branch

```bash
git branch -d feature-login
```

---

## Force Delete a Branch

```bash
git branch -D feature-login
```

Use this only if you're certain the branch is no longer needed.

---

## Production Example

After a feature is merged into `main`, the feature branch is deleted to keep the repository clean.

---

## Production Tip

Regularly remove merged branches to reduce repository clutter and make navigation easier.

---

## Interview Question

### Question

How do you delete a Git Branch?

### Answer

Use:

```bash
git branch -d branch-name
```

If the branch is not merged and you still want to delete it:

```bash
git branch -D branch-name
```

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Git Branch | Independent line of development |
| `main` | Stable production branch |
| Feature Branch | Used for new feature development |
| `git branch` | Create or list branches |
| `git switch` | Switch branches |
| `git switch -c` | Create and switch to a branch |
| `git branch -d` | Delete a merged branch |
| `git branch -m` | Rename a branch |

---

# 7. Git Merge

## Answer

A **Git Merge** combines changes from one branch into another.

It is commonly used after completing work on a feature branch.

For example:

```text
feature-login

↓

Merge

↓

main
```

After merging, all commits from the feature branch become part of the target branch.

---

## Merge Workflow

```text
main

│

├── feature-login

↓

Merge

↓

Updated main
```

---

## Merge Commands

Switch to the target branch

```bash
git switch main
```

Merge the feature branch

```bash
git merge feature-login
```

---

## Production Example

A developer completes a login feature, tests it, and merges it into the `main` branch after code review.

---

## Production Tip

Always update your target branch before merging.

```bash
git pull origin main
```

This reduces merge conflicts.

---

## Interview Question

### Question

What is Git Merge?

### Answer

Git Merge combines changes from one branch into another while preserving the commit history.

---

# 8. Fast-Forward Merge

## Answer

A **Fast-Forward Merge** occurs when the target branch has not changed since the feature branch was created.

Git simply moves the branch pointer forward.

---

## Architecture

```text
Before

A──B──C (main)

        \

         D──E (feature)

After

A──B──C──D──E (main)
```

No additional merge commit is created.

---

## Merge Command

```bash
git merge feature-login
```

Git automatically performs a fast-forward merge when possible.

---

## Production Example

A developer creates a small documentation update. Since no one else modified the `main` branch, Git performs a fast-forward merge.

---

## Production Tip

Fast-forward merges keep the commit history clean but may hide the context of feature development.

---

## Interview Question

### Question

What is a Fast-Forward Merge?

### Answer

A Fast-Forward Merge moves the target branch pointer forward without creating a merge commit because there are no divergent changes.

---

# 9. Three-Way Merge

## Answer

A **Three-Way Merge** occurs when both branches have new commits after branching.

Git uses:

- The common ancestor
- Source branch
- Target branch

to create a new merge commit.

---

## Architecture

```text
        D──E (feature)

       /

A──B──C

       \

        F──G (main)

↓

Merge Commit
```

---

## Result

```text
        D──E

       /    \

A──B──C------M

       \

        F──G
```

`M` represents the merge commit.

---

## Production Example

While one developer builds a new feature, another fixes bugs in `main`.

Git performs a three-way merge when the feature is merged back.

---

## Production Tip

Three-way merges preserve the full development history, making it easier to understand how features evolved.

---

## Interview Question

### Question

What is a Three-Way Merge?

### Answer

A Three-Way Merge combines two branches that have both diverged, creating a new merge commit.

---

# 10. Merge Conflicts

## Answer

A **Merge Conflict** occurs when Git cannot automatically determine which changes should be kept.

This usually happens when:

- Two developers modify the same lines.
- One developer deletes a file while another edits it.
- Files are renamed differently in separate branches.

---

## Example

Developer A

```text
Hello World
```

Developer B

```text
Hello Git
```

Git cannot automatically choose which version to keep.

---

## Conflict Workflow

```text
Branch A

↓

Conflict

↓

Manual Resolution

↓

Commit
```

---

## Production Example

Two developers update the same configuration file.

Git reports a merge conflict that must be resolved manually before completing the merge.

---

## Production Tip

Merge feature branches regularly to reduce the size and complexity of conflicts.

---

## Interview Question

### Question

What is a Merge Conflict?

### Answer

A Merge Conflict occurs when Git cannot automatically combine changes from different branches.

---

# 11. Resolving Merge Conflicts

## Answer

Git marks conflicting sections inside the affected files.

Example

```text
<<<<<<< HEAD

Current Branch

=======

Incoming Branch

>>>>>>> feature-login
```

The developer must edit the file, choose the correct content, and remove the conflict markers.

---

## Resolution Steps

1. Open the conflicted file.
2. Remove conflict markers.
3. Keep the correct code.
4. Save the file.
5. Stage the resolved file.

```bash
git add file.txt
```

6. Complete the merge.

```bash
git commit
```

---

## Workflow

```text
Conflict

↓

Manual Edit

↓

git add

↓

git commit

↓

Merge Complete
```

---

## Production Example

A team resolves a conflict in `application.yml` after two developers changed different application settings.

---

## Production Tip

Read the conflicting code carefully. Never remove conflict markers without understanding both changes.

---

## Interview Question

### Question

How do you resolve a Git Merge Conflict?

### Answer

Edit the conflicted file, remove conflict markers, keep the correct changes, stage the file, and complete the merge with a commit.

---

# 12. Production Branching Workflow

## Answer

Most teams follow a structured branching strategy.

---

## Example Workflow

```text
main

↓

feature/login

↓

Pull Request

↓

Code Review

↓

CI Pipeline

↓

Merge

↓

Deploy
```

---

## Typical Development Process

1. Pull the latest code.
2. Create a feature branch.
3. Develop the feature.
4. Commit changes.
5. Push the branch.
6. Create a Pull Request.
7. Complete code review.
8. Merge into `main`.
9. Delete the feature branch.

---

## Production Example

A DevOps team requires:

- Pull Request approval
- Successful CI pipeline
- Security scan
- Automated tests

before allowing any merge into the production branch.

---

## Production Tip

Protect important branches (`main`, `develop`) by requiring pull requests and preventing direct commits.

---

## Interview Question

### Question

What is a recommended Git branching workflow?

### Answer

Create a feature branch, develop and test changes, open a pull request, complete code review, merge into the main branch, and delete the feature branch.

---

# 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Git Merge | Combines branches |
| Fast-Forward Merge | No merge commit created |
| Three-Way Merge | Creates a merge commit |
| Merge Conflict | Git cannot automatically combine changes |
| Conflict Resolution | Edit → Stage → Commit |
| Pull Request | Request to merge reviewed code |

---

# 13. Git Rebase

## Answer

**Git Rebase** moves or reapplies commits from one branch onto another.

Unlike `git merge`, rebase creates a **linear commit history** by replaying commits on top of the target branch.

---

## Rebase Workflow

```text
Before

main

A──B──C

        \

         D──E (feature)

After Rebase

main

A──B──C──D──E
```

---

## Rebase Command

Switch to your feature branch

```bash
git switch feature-login
```

Rebase onto main

```bash
git rebase main
```

---

## Merge vs Rebase

| Merge | Rebase |
|--------|---------|
| Preserves history | Rewrites history |
| Creates merge commit | Creates linear history |
| Safe for shared branches | Best for local branches |

---

## Production Example

Before opening a Pull Request, a developer rebases their feature branch onto the latest `main` branch to keep the commit history clean.

---

## Production Tip

Avoid rebasing branches that have already been shared with other developers unless your team has agreed on that workflow.

---

## Interview Question

### Question

What is Git Rebase?

### Answer

Git Rebase reapplies commits from one branch onto another to create a cleaner, linear commit history.

---

# 14. Cherry-Pick

## Answer

**Cherry-pick** copies a specific commit from one branch and applies it to another branch.

Instead of merging an entire branch, you select only the commit you need.

---

## Workflow

```text
feature-login

A──B──C

        ↑

Cherry-pick Commit B

↓

main
```

---

## Command

```bash
git cherry-pick <commit-id>
```

Example

```bash
git cherry-pick a3f7d9e
```

---

## Production Example

A production hotfix branch requires only one bug-fix commit from a feature branch.

The developer cherry-picks that single commit instead of merging the complete feature branch.

---

## Production Tip

Use cherry-pick sparingly. Overusing it can create duplicate commits and make history harder to understand.

---

## Interview Question

### Question

What is Git Cherry-Pick?

### Answer

Git Cherry-Pick copies a specific commit from one branch and applies it to another branch.

---

# 15. Squash Merge

## Answer

A **Squash Merge** combines all commits from a feature branch into **one single commit** before merging.

---

## Before Squash

```text
feature-login

A

↓

B

↓

C

↓

D
```

---

## After Squash

```text
main

↓

Single Commit
```

---

## Command

```bash
git merge --squash feature-login
```

Then create the final commit:

```bash
git commit -m "Add login feature"
```

---

## Production Example

A feature branch contains 25 small development commits.

Before merging, the commits are squashed into one clean commit representing the completed feature.

---

## Production Tip

Squash feature branches containing many small "work in progress" commits to maintain a clean project history.

---

## Interview Question

### Question

What is a Squash Merge?

### Answer

A Squash Merge combines multiple commits into a single commit before merging into the target branch.

---

# 16. Branch Protection

## Answer

**Branch Protection** prevents accidental or unauthorized changes to important branches like:

```text
main

develop
```

Most Git hosting platforms support branch protection rules.

---

## Common Protection Rules

- Require Pull Requests
- Require Code Reviews
- Require Successful CI Builds
- Prevent Force Pushes
- Prevent Direct Commits
- Require Status Checks

---

## Workflow

```text
Developer

↓

Feature Branch

↓

Pull Request

↓

Code Review

↓

CI Checks

↓

Merge
```

---

## Production Example

The `main` branch is configured to require:

- Two approvals
- Successful unit tests
- Security scan
- Passing CI pipeline

before any merge is allowed.

---

## Production Tip

Protect production branches from direct commits to reduce deployment risks.

---

## Interview Question

### Question

Why is Branch Protection important?

### Answer

Branch Protection ensures that important branches can only be updated through approved and validated workflows.

---

# 17. Branching Best Practices

## Answer

Following consistent branching practices improves collaboration and repository quality.

---

## Recommended Practices

✅ Create a new branch for every feature or bug fix.

---

✅ Keep branches short-lived.

---

✅ Pull the latest changes before starting work.

---

✅ Open Pull Requests early for feedback.

---

✅ Delete merged branches.

---

✅ Use descriptive branch names.

Examples:

```text
feature/user-authentication

bugfix/cart-total

hotfix/payment-timeout
```

---

## Production Example

A DevOps team uses feature branches for every task and deletes them immediately after merging to keep the repository organized.

---

## Production Tip

Smaller, focused branches reduce merge conflicts and simplify code reviews.

---

## Interview Question

### Question

What are Git branching best practices?

### Answer

Use feature branches, descriptive names, pull requests, regular synchronization, and delete branches after merging.

---

# 18. Git Branch Troubleshooting

## Answer

Branch-related issues are common in collaborative development.

Understanding how to troubleshoot them saves time.

---

## Problem 1

### Wrong Branch

Check current branch

```bash
git branch
```

Switch

```bash
git switch main
```

---

## Problem 2

### Branch Already Exists

List branches

```bash
git branch
```

Rename if necessary

```bash
git branch -m new-name
```

---

## Problem 3

### Branch Not Updated

Fetch latest changes

```bash
git fetch
```

Then update

```bash
git pull
```

---

## Problem 4

### Merge Conflict

Resolve manually, then:

```bash
git add .

git commit
```

---

## Problem 5

### Rebase Conflict

Continue after resolving

```bash
git rebase --continue
```

Abort if necessary

```bash
git rebase --abort
```

---

## Production Example

A developer encounters a rebase conflict after syncing with the latest `main` branch.

They resolve the conflict, stage the changes, and continue the rebase using:

```bash
git rebase --continue
```

---

## Production Tip

Fetch frequently and keep your feature branch updated to minimize merge and rebase conflicts.

---

## Interview Question

### Question

How do you resolve a rebase conflict?

### Answer

Resolve the conflicting files, stage the changes, and continue the rebase using:

```bash
git rebase --continue
```

---

# 📌 Quick Revision

### Branching Workflow

```text
main

↓

Create Feature Branch

↓

Develop

↓

Commit

↓

Pull Request

↓

Merge

↓

Delete Branch
```

---

### Important Commands

```bash
git branch

git switch

git merge

git rebase

git cherry-pick

git branch -d

git branch -m

git fetch

git pull
```

---

# Summary

After completing this guide, you should understand:

✅ Git Branches

✅ Creating & Switching Branches

✅ Managing Branches

✅ Git Merge

✅ Fast-Forward Merge

✅ Three-Way Merge

✅ Merge Conflicts

✅ Conflict Resolution

✅ Git Rebase

✅ Cherry-Pick

✅ Squash Merge

✅ Branch Protection

✅ Branching Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is a Git Branch? | An independent line of development |
| Why use branches? | To isolate work and enable parallel development |
| What is Git Merge? | Combines changes from one branch into another |
| What is a Fast-Forward Merge? | Moves the branch pointer without creating a merge commit |
| What is a Three-Way Merge? | Combines divergent branches using a merge commit |
| What is a Merge Conflict? | Git cannot automatically combine changes |
| What is Git Rebase? | Replays commits onto another branch for a linear history |
| What is Cherry-Pick? | Copies a specific commit to another branch |
| What is a Squash Merge? | Combines multiple commits into one |
| Why protect branches? | To enforce reviews and prevent accidental changes |

---

# 🎉 Git Branching & Merging Module Completed

Congratulations! You have completed **Git Branching & Merging**, including:

- ✅ Git Branches
- ✅ Creating & Switching Branches
- ✅ Branch Management
- ✅ Git Merge
- ✅ Fast-Forward Merge
- ✅ Three-Way Merge
- ✅ Merge Conflicts & Resolution
- ✅ Git Rebase
- ✅ Git Cherry-Pick
- ✅ Squash Merge
- ✅ Branch Protection
- ✅ Branching Best Practices
- ✅ Branch Troubleshooting

You now have a strong understanding of collaborative Git workflows used by professional software development, DevOps, and cloud engineering teams.

---

# 🚀 What's Next?

➡️ **03-github-workflow.md**

In the next guide, you'll learn:

- GitHub Fundamentals
- Remote Repositories
- Clone, Fetch, Pull & Push
- Forks
- Pull Requests
- Code Reviews
- GitHub Issues
- GitHub Actions Overview
- Collaboration Best Practices
- Common Interview Questions