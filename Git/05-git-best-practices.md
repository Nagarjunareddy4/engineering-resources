# Git Best Practices

This guide explains the best practices followed by professional software development, DevOps, platform engineering, and SRE teams when using Git.

If you've ever wondered:

- How should a Git repository be organized?
- What are good branch naming conventions?
- How should commit messages be written?
- How do professional teams manage repositories?
- What Git practices improve collaboration?

This guide is for you.

---

# 📑 Table of Contents

1. Repository Organization
2. Branch Naming Conventions
3. Commit Message Standards
4. Repository Structure
5. Collaboration Principles
6. Common Interview Questions

---

# 1. Repository Organization

## Answer

A well-organized repository improves maintainability, collaboration, and onboarding.

Every repository should have a clear structure so developers can quickly understand where code, documentation, configuration, and tests belong.

---

## Recommended Structure

```text
project/

├── src/

├── tests/

├── docs/

├── scripts/

├── .github/

├── .gitignore

├── README.md

├── LICENSE

└── CONTRIBUTING.md
```

---

## Essential Files

| File | Purpose |
|------|----------|
| README.md | Project overview and setup instructions |
| .gitignore | Ignore unnecessary files |
| LICENSE | Project license |
| CONTRIBUTING.md | Contribution guidelines |
| .github/ | GitHub workflows and templates |

---

## Production Example

A microservice repository contains:

- Source code in `src/`
- Unit tests in `tests/`
- Deployment scripts in `scripts/`
- CI workflows in `.github/workflows/`
- Documentation in `docs/`

making the project easy for new developers to understand.

---

## Production Tip

Keep configuration, documentation, tests, and source code separated into dedicated directories.

---

## Interview Question

### Question

Why is repository organization important?

### Answer

A well-organized repository improves readability, collaboration, onboarding, and long-term maintainability.

---

# 2. Branch Naming Conventions

## Answer

Consistent branch names make repositories easier to understand and manage.

Branch names should describe the purpose of the work.

---

## Recommended Naming

### Feature

```text
feature/user-authentication
```

---

### Bug Fix

```text
bugfix/login-timeout
```

---

### Hotfix

```text
hotfix/payment-gateway
```

---

### Release

```text
release/v2.0.0
```

---

### Documentation

```text
docs/api-guide
```

---

## Workflow

```text
Issue

↓

Create Branch

↓

Develop

↓

Merge

↓

Delete Branch
```

---

## Production Example

Instead of creating:

```text
branch1
```

a developer creates:

```text
feature/order-history
```

making the branch purpose immediately clear.

---

## Production Tip

Use lowercase letters and hyphens or slashes consistently throughout the repository.

---

## Interview Question

### Question

What are good Git branch naming conventions?

### Answer

Use descriptive names such as `feature/`, `bugfix/`, `hotfix/`, `release/`, and `docs/` followed by a meaningful description.

---

# 3. Commit Message Standards

## Answer

Commit messages should clearly describe **what changed** and, when helpful, **why it changed**.

Good commit messages improve project history and simplify debugging.

---

## Good Examples

```text
Add JWT authentication

Fix Docker build failure

Update Kubernetes deployment manifest

Improve API error handling

Refactor user service
```

---

## Poor Examples

```text
Update

Fix

Changes

Done

Test
```

---

## Recommended Format

```text
Short Summary

(Optional detailed description)
```

---

## Conventional Commits (Optional Standard)

```text
feat: add user authentication

fix: resolve login timeout

docs: update installation guide

refactor: simplify deployment script

test: add unit tests
```

---

## Production Example

Instead of:

```text
Fixed issue
```

use:

```text
fix: resolve null pointer exception during user registration
```

---

## Production Tip

Keep the first line concise (around 50 characters when possible) and use the description for additional context if needed.

---

## Interview Question

### Question

What makes a good Git commit message?

### Answer

A good commit message is short, descriptive, and clearly explains what changed and, if necessary, why the change was made.

---

# 4. Repository Structure

## Answer

A consistent repository structure helps developers locate files quickly and reduces confusion.

---

## Example Structure

```text
project/

├── backend/

├── frontend/

├── infrastructure/

├── database/

├── docs/

├── tests/

└── scripts/
```

---

## Benefits

- Easier Navigation
- Better Maintainability
- Faster Onboarding
- Cleaner Codebase
- Simpler Automation

---

## Production Example

A DevOps repository separates:

- Terraform in `infrastructure/`
- Kubernetes manifests in `kubernetes/`
- Shell scripts in `scripts/`
- Documentation in `docs/`

making infrastructure management more organized.

---

## Production Tip

Avoid placing unrelated files in the repository root. Group related files into meaningful directories.

---

## Interview Question

### Question

Why should repositories follow a consistent structure?

### Answer

A consistent structure improves readability, maintainability, automation, and team collaboration.

---

# 5. Collaboration Principles

## Answer

Successful Git collaboration depends on clear communication and disciplined workflows.

---

## Recommended Principles

- Create a branch for every task.
- Keep commits focused and small.
- Open Pull Requests early.
- Review code before merging.
- Resolve conflicts promptly.
- Delete merged branches.
- Keep documentation updated.

---

## Collaboration Workflow

```text
Issue

↓

Feature Branch

↓

Commit

↓

Push

↓

Pull Request

↓

Review

↓

Merge
```

---

## Production Example

A team requires every change to go through a Pull Request with at least one approval and successful CI checks before merging into `main`.

---

## Production Tip

Treat Git history as shared documentation. Every commit, branch, and Pull Request should help future developers understand the project.

---

## Interview Question

### Question

What are good Git collaboration practices?

### Answer

Use feature branches, descriptive commits, code reviews, protected branches, CI validation, and maintain clear documentation.

---

# 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Repository Organization | Keep code, tests, docs, and scripts separated |
| Branch Naming | Use descriptive branch names |
| Commit Messages | Clearly explain changes |
| Repository Structure | Organize files consistently |
| Collaboration | Use branches, PRs, and code reviews |

---

# 6. Git Security Best Practices

## Answer

Source code repositories often contain sensitive business logic and deployment configurations.

Following Git security best practices helps protect applications, infrastructure, and confidential data.

---

## Security Best Practices

✅ Never commit passwords.

---

✅ Never commit API keys.

---

✅ Never commit private keys.

---

✅ Never commit certificates.

---

✅ Never commit `.env` files containing secrets.

---

## Use `.gitignore`

Example

```text
.env

*.pem

*.key

*.pfx

*.jks

node_modules/

dist/
```

---

## Use Secret Management

Instead of storing secrets in Git, use:

- Azure Key Vault
- AWS Secrets Manager
- HashiCorp Vault
- GitHub Secrets

---

## Production Example

A deployment pipeline retrieves database credentials from Azure Key Vault instead of storing them in the Git repository.

---

## Production Tip

Assume anything committed to Git may eventually become visible. Store secrets in dedicated secret management systems, not in source control.

---

## Interview Question

### Question

Why should secrets never be committed to Git?

### Answer

Because Git preserves commit history, making accidentally committed secrets difficult to remove completely. Use dedicated secret management solutions instead.

---

# 7. Repository Access Management

## Answer

Not every developer requires full access to every repository.

Grant users only the permissions necessary for their responsibilities.

---

## Common Permission Levels

| Role | Access |
|------|--------|
| Read | View repository |
| Write | Push commits |
| Maintain | Manage repository configuration |
| Admin | Full repository administration |

---

## Principle of Least Privilege

```text
User

↓

Minimum Required Permission

↓

Repository
```

---

## Production Example

Developers receive **Write** access to application repositories, while only repository administrators have **Admin** privileges to manage branch protection and settings.

---

## Production Tip

Review repository permissions regularly and remove access for users who no longer need it.

---

## Interview Question

### Question

Why is repository access management important?

### Answer

It protects source code by ensuring users have only the permissions required to perform their work.

---

# 8. Protecting Secrets

## Answer

Secrets include:

- Passwords
- API Keys
- SSH Keys
- Tokens
- Certificates
- Cloud Credentials

These should never be stored directly in the repository.

---

## Recommended Workflow

```text
Application

↓

Environment Variable

↓

Secret Manager

↓

Runtime
```

---

## Example

Instead of:

```text
password=MyPassword123
```

Use:

```text
DB_PASSWORD=${DB_PASSWORD}
```

The value is supplied through the environment or a secret manager.

---

## Production Example

A Kubernetes application retrieves database credentials from a Kubernetes Secret that is populated from an external secret management solution.

---

## Production Tip

Rotate credentials regularly and revoke exposed secrets immediately if they are accidentally committed.

---

## Interview Question

### Question

How should secrets be managed in Git projects?

### Answer

Secrets should be stored in secure secret management systems or environment variables rather than being committed to the repository.

---

# 9. Code Review Best Practices

## Answer

Code reviews improve software quality, security, and maintainability.

A good review focuses on both functionality and long-term maintainability.

---

## Review Checklist

- Correctness
- Readability
- Performance
- Security
- Error Handling
- Testing
- Documentation

---

## Workflow

```text
Developer

↓

Pull Request

↓

Reviewer

↓

Feedback

↓

Approval

↓

Merge
```

---

## Production Example

During a review, a teammate identifies a potential SQL injection vulnerability before the code is merged into the production branch.

---

## Production Tip

Keep Pull Requests small and focused. Smaller changes are easier to review and reduce the likelihood of defects.

---

## Interview Question

### Question

What should you look for during a code review?

### Answer

Review functionality, readability, security, performance, testing, documentation, and adherence to coding standards.

---

# 10. Enterprise Git Workflows

## Answer

Large organizations use standardized Git workflows to ensure consistency across teams.

---

## Typical Enterprise Workflow

```text
Issue

↓

Feature Branch

↓

Development

↓

Commit

↓

Push

↓

Pull Request

↓

Code Review

↓

CI Pipeline

↓

Security Scan

↓

Approval

↓

Merge

↓

Release

↓

Production
```

---

## Common Enterprise Practices

- Protected Branches
- Required Pull Requests
- Mandatory Code Reviews
- Automated CI/CD Pipelines
- Static Code Analysis
- Security Scanning
- Signed Commits
- Release Tagging

---

## Production Example

A banking application requires:

- Two reviewer approvals
- Successful unit tests
- Static application security testing (SAST)
- Dependency vulnerability scan
- Passing CI pipeline

before any production merge is permitted.

---

## Production Tip

Automate quality checks wherever possible to ensure every change is validated consistently.

---

## Interview Question

### Question

What are common enterprise Git workflow practices?

### Answer

Protected branches, feature branches, pull requests, mandatory reviews, automated testing, security scans, CI/CD pipelines, and release tagging.

---

# 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Git Security | Never commit secrets |
| Access Management | Apply least privilege |
| Secret Management | Use external secret stores |
| Code Reviews | Improve quality and security |
| Enterprise Workflow | PR → Review → CI → Merge → Release |

---

# 11. Git Anti-Patterns

## Answer

Git Anti-Patterns are poor practices that make repositories difficult to maintain, review, and troubleshoot.

Avoiding these mistakes keeps repositories clean and collaboration efficient.

---

## Common Anti-Patterns

### ❌ Committing directly to `main`

Instead:

```text
Feature Branch

↓

Pull Request

↓

Review

↓

Merge
```

---

### ❌ Large Commits

Bad Example

```text
500 files changed
```

Good Example

```text
Feature 1

↓

Feature 2

↓

Bug Fix

↓

Documentation
```

Each change should have its own commit.

---

### ❌ Poor Commit Messages

Bad

```text
Update

Fix

Changes

Done
```

Good

```text
feat: add user authentication

fix: resolve Docker build failure

docs: update installation guide
```

---

### ❌ Force Push to Shared Branches

Avoid:

```bash
git push --force
```

Use only when absolutely necessary and never on protected shared branches.

---

### ❌ Committing Generated Files

Avoid committing:

```text
node_modules/

dist/

build/

coverage/

tmp/
```

Use `.gitignore` instead.

---

## Production Example

A team accidentally force-pushes to the `main` branch and overwrites commits from other developers.

Protected branches and pull request workflows help prevent this type of issue.

---

## Production Tip

If an action could affect your teammates, verify it twice before executing it.

---

## Interview Question

### Question

What are common Git anti-patterns?

### Answer

Common anti-patterns include committing directly to `main`, creating large commits, writing poor commit messages, force-pushing shared branches, and committing generated files or secrets.

---

# 12. Repository Maintenance

## Answer

Repositories require regular maintenance to remain organized and performant.

---

## Recommended Maintenance Tasks

- Delete merged branches.
- Remove obsolete tags.
- Update documentation.
- Archive inactive repositories.
- Keep dependencies current.
- Run repository maintenance.

---

## Useful Commands

Delete merged branch

```bash
git branch -d feature-login
```

Garbage collection

```bash
git gc
```

Check repository status

```bash
git status
```

View branch list

```bash
git branch
```

---

## Production Example

A DevOps team performs monthly repository maintenance by deleting merged branches, updating documentation, and optimizing repository storage.

---

## Production Tip

A clean repository improves navigation, onboarding, and long-term maintainability.

---

## Interview Question

### Question

Why is repository maintenance important?

### Answer

Repository maintenance improves performance, reduces clutter, and keeps projects organized and easier to manage.

---

# 13. Enterprise Git Checklist

## Answer

Professional engineering teams often use a standard checklist before merging changes.

---

## Development Checklist

✅ Latest changes pulled

```bash
git pull
```

---

✅ Feature branch created

---

✅ Code builds successfully

---

✅ Tests passed

---

✅ No secrets committed

---

✅ Meaningful commit messages

---

✅ Pull Request created

---

✅ Code reviewed

---

✅ CI pipeline successful

---

✅ Branch ready to merge

---

## Production Example

Every Pull Request in an enterprise environment must satisfy automated quality gates before it can be merged into the production branch.

---

## Production Tip

Automate as many checklist items as possible using CI/CD pipelines and branch protection rules.

---

## Interview Question

### Question

What should you verify before merging a Pull Request?

### Answer

Ensure the code builds successfully, tests pass, code review is complete, CI checks succeed, and no sensitive information is committed.

---

# 14. Production Git Checklist

## Answer

Before deploying software to production, verify that repository practices have been followed.

---

## Deployment Checklist

- Correct branch
- Latest code pulled
- Pull Request approved
- CI/CD pipeline successful
- Security scan passed
- Release tag created
- Documentation updated
- Rollback plan available

---

## Production Workflow

```text
Feature Complete

↓

Pull Request

↓

Review

↓

CI Pipeline

↓

Security Scan

↓

Merge

↓

Release Tag

↓

Production Deployment
```

---

## Production Example

A production deployment proceeds only after:

- Pull Request approval
- Automated testing
- Security validation
- Release tagging
- Deployment approval

---

## Production Tip

Every production deployment should have a clear rollback strategy and a tagged release.

---

## Interview Question

### Question

Why are release tags important?

### Answer

Release tags identify specific production versions, making rollbacks, auditing, and version tracking much easier.

---

# 📌 Final Git Cheat Sheet

## Repository Commands

```bash
git init

git clone

git status

git add

git commit

git push

git pull

git fetch
```

---

## Branch Commands

```bash
git branch

git switch

git merge

git rebase

git cherry-pick
```

---

## Recovery Commands

```bash
git stash

git reflog

git restore

git reset

git revert
```

---

## Repository Maintenance

```bash
git gc

git clean

git archive

git worktree
```

---

# 🎯 Top 10 Git Interview Questions

1. What is Git?
2. What is the difference between Git and GitHub?
3. What is a Git Branch?
4. What is the difference between Merge and Rebase?
5. What is Git Stash?
6. What is Git Reflog?
7. What is a Pull Request?
8. What is a Merge Conflict?
9. What are Git Hooks?
10. What are Git best practices?

---

# 💡 Expert Interview Tips

### Before the Interview

- Practice common Git commands.
- Understand branching strategies.
- Learn merge vs rebase.
- Review GitHub workflows.
- Know how to recover from mistakes.

---

### During the Interview

- Explain **why** you use a command, not just **how**.
- Mention production best practices.
- Use real-world examples from team collaboration.
- Discuss how Git integrates with CI/CD pipelines.

---

### Example Response Structure

```text
Requirement

↓

Git Feature

↓

Commands

↓

Best Practice

↓

Production Example
```

Interviewers value practical reasoning and workflow knowledge more than memorizing commands.

---

# 🎉 Git Learning Path Completed

Congratulations! You have completed the entire **Git** module of the **Engineer Resources** roadmap.

You now have comprehensive knowledge of:

- ✅ Git Fundamentals
- ✅ Branching & Merging
- ✅ GitHub Workflow
- ✅ Advanced Git
- ✅ Git Best Practices

This provides a strong foundation for software development, DevOps engineering, cloud engineering, Kubernetes, platform engineering, and Site Reliability Engineering (SRE).

---

# 🚀 What's Next?

➡️ **Terraform/**

Start with:

**`01-terraform-fundamentals.md`**

Topics include:

- What is Infrastructure as Code (IaC)?
- Why Terraform?
- Terraform Architecture
- Providers & Resources
- Terraform Workflow (`init`, `plan`, `apply`, `destroy`)
- State Files
- Variables & Outputs
- Modules
- Best Practices
- Common Interview Questions

By completing the Terraform module, you'll be ready to provision and manage cloud infrastructure using Infrastructure as Code in production environments.