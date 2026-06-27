# 🚀 Argo CD Fundamentals - GitOps for Kubernetes

This guide explains everything you need to know about Argo CD Fundamentals.

If you've ever wondered:

- What is Argo CD?
- What is GitOps?
- Why do we need Argo CD?
- How does Argo CD work?
- Why do companies use Argo CD in production?

This guide is for you.

---

# 📑 Table of Contents

1. What is Argo CD?
2. Why do we need Argo CD?
3. What is GitOps?
4. Argo CD Architecture
5. Common Interview Questions

---

# 1. What is Argo CD?

## Answer

**Argo CD** is a **GitOps Continuous Delivery (CD)** tool for Kubernetes.

It continuously monitors a Git repository and automatically synchronizes the Kubernetes cluster with the desired state stored in Git.

Instead of manually deploying applications using:

```bash
kubectl apply -f deployment.yaml
```

Argo CD automatically deploys and manages Kubernetes applications from Git.

---

## Why do we need Argo CD?

In traditional deployments:

```text
Developer

↓

Build Application

↓

kubectl apply

↓

Kubernetes
```

Every deployment requires manual execution.

With Argo CD:

```text
Developer

↓

Git Push

↓

Git Repository

↓

Argo CD

↓

Kubernetes Cluster
```

Simply pushing code or Kubernetes manifests to Git triggers deployment.

---

## Real World Example

A developer updates:

```text
deployment.yaml
```

and commits the changes.

```bash
git add .

git commit -m "Update image"

git push
```

Argo CD detects the change and updates the Kubernetes cluster automatically.

No manual deployment is required.

---

## Benefits

Argo CD provides:

- GitOps-based Deployments
- Continuous Delivery
- Automatic Synchronization
- Rollback Support
- Self-Healing
- Auditability

---

## Production Tip

Treat Git as the **single source of truth**.

Never make manual changes directly in production clusters.

---

## Interview Question

### Question

What is Argo CD?

### Answer

Argo CD is a GitOps-based Continuous Delivery tool for Kubernetes that automatically synchronizes cluster resources with the desired state stored in a Git repository.

---

# 2. Why do we need Argo CD?

## Answer

Managing Kubernetes manually becomes difficult as applications grow.

Problems include:

- Manual Deployments
- Configuration Drift
- Human Errors
- Difficult Rollbacks
- Inconsistent Environments

Argo CD solves these challenges using GitOps.

---

## Without Argo CD

```text
Developer

↓

kubectl apply

↓

Production Cluster

↓

Manual Verification
```

Every deployment depends on manual actions.

---

## With Argo CD

```text
Developer

↓

Git Commit

↓

Git Repository

↓

Argo CD

↓

Automatic Deployment

↓

Production Cluster
```

Deployments become automated, repeatable, and auditable.

---

## Production Example

A team manages:

- Development
- QA
- Staging
- Production

Instead of manually deploying to every cluster, Argo CD keeps all environments synchronized with Git.

---

## Production Tip

Avoid making direct changes using `kubectl edit` or `kubectl apply` in production.

Allow Argo CD to manage the cluster state.

---

## Interview Question

### Question

Why do we need Argo CD?

### Answer

Argo CD automates Kubernetes deployments using Git as the source of truth, reducing manual work, configuration drift, and deployment errors.

---

# 3. What is GitOps?

## Answer

**GitOps** is an operational model where Git becomes the **single source of truth** for infrastructure and application configuration.

Everything is stored in Git:

- Deployments
- Services
- ConfigMaps
- Secrets (encrypted or externally managed)
- Helm Charts
- Kustomize Configurations

Any change is made through Git commits.

---

## GitOps Workflow

```text
Developer

↓

Modify YAML

↓

Git Commit

↓

Git Push

↓

Git Repository

↓

Argo CD Detects Change

↓

Deploy to Kubernetes
```

---

## Key Principles

- Git is the source of truth.
- Changes are made through pull requests.
- Deployments are automated.
- Cluster state matches Git.
- Every change is auditable.

---

## Production Example

A developer changes:

```yaml
replicas: 3
```

to

```yaml
replicas: 5
```

After pushing to Git, Argo CD automatically updates the Deployment.

---

## Production Tip

Protect production branches using pull requests and code reviews before Argo CD synchronizes changes.

---

## Interview Question

### Question

What is GitOps?

### Answer

GitOps is an operational practice where Git stores the desired state of infrastructure and applications, and automation tools like Argo CD continuously synchronize that state with Kubernetes.

---

# 4. Argo CD Architecture

## Answer

Argo CD continuously compares the desired state in Git with the actual state in the Kubernetes cluster.

If differences are detected, Argo CD synchronizes the cluster.

---

## Architecture

```text
Developer

        │

Git Push

        │

        ▼

Git Repository

        │

Argo CD

────────────┼────────────

        │

Compare Desired State

        │

Detect Drift

        │

Synchronize

        ▼

Kubernetes API Server

        │

        ▼

Kubernetes Cluster
```

---

## Main Components

### Git Repository

Stores the desired Kubernetes manifests.

---

### Argo CD Server

Provides the Web UI, REST API, and authentication.

---

### Repository Server

Fetches manifests from Git repositories.

---

### Application Controller

Continuously compares Git with the cluster and performs synchronization.

---

### Kubernetes Cluster

Runs the deployed applications.

---

## Production Example

A developer updates a Helm Chart in Git.

Argo CD detects the new commit, renders the manifests, compares them with the cluster, and applies only the required changes.

---

## Production Tip

Separate application code repositories from GitOps configuration repositories for improved security and maintainability.

---

## Interview Question

### Question

What are the main components of Argo CD?

### Answer

The main components are the Git Repository, Argo CD Server, Repository Server, Application Controller, and the Kubernetes Cluster.

---

# 5. Installing Argo CD

## Answer

Argo CD is typically installed into its own Kubernetes namespace.

The official installation manifests deploy all required Argo CD components.

---

## Create Namespace

```bash
kubectl create namespace argocd
```

---

## Install Argo CD

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

## Verify Installation

```bash
kubectl get pods -n argocd
```

Example Output

```text
argocd-server

argocd-repo-server

argocd-application-controller

argocd-dex-server

argocd-redis
```

---

## Internal Workflow

```text
Create Namespace

        │

Install Components

        │

Start Pods

        │

Argo CD Ready
```

---

## Production Example

A DevOps engineer installs Argo CD once per Kubernetes cluster and manages all application deployments through it.

---

## Production Tip

Install Argo CD in a dedicated namespace (typically `argocd`) to simplify management and access control.

---

## Interview Question

### Question

How do you install Argo CD?

### Answer

Create the `argocd` namespace and apply the official Argo CD installation manifests.

---

# 6. Argo CD Web UI

## Answer

Argo CD provides a powerful Web UI for managing Kubernetes applications.

The UI allows you to:

- View Applications
- Compare Desired vs Live State
- Monitor Synchronization
- View Resource Health
- Trigger Manual Syncs
- Perform Rollbacks

---

## Typical Dashboard

```text
Applications

↓

Application Status

↓

Sync Status

↓

Health Status

↓

Resource Tree
```

---

## Internal Workflow

```text
Developer

        │

Open Web UI

        │

View Application

        │

Compare Git

        │

Sync Changes
```

---

## Production Example

A DevOps engineer monitors production applications from the Argo CD dashboard and quickly identifies OutOfSync or unhealthy resources.

---

## Production Tip

Restrict Web UI access using Role-Based Access Control (RBAC) and Single Sign-On (SSO).

---

## Interview Question

### Question

What can you do from the Argo CD Web UI?

### Answer

The Web UI allows users to monitor applications, compare desired and live states, synchronize deployments, perform rollbacks, and troubleshoot Kubernetes resources.

---

# 7. Argo CD CLI

## Answer

Argo CD also provides a Command-Line Interface (CLI) for managing applications.

The CLI is commonly used in:

- CI/CD Pipelines
- Automation Scripts
- Administrative Tasks

---

## Login

```bash
argocd login <ARGOCD_SERVER>
```

---

## List Applications

```bash
argocd app list
```

---

## View Application

```bash
argocd app get shopping-app
```

---

## Synchronize Application

```bash
argocd app sync shopping-app
```

---

## Internal Workflow

```text
CLI

        │

Connect to Argo CD

        │

Execute Command

        │

Update Cluster
```

---

## Production Example

A CI/CD pipeline triggers:

```bash
argocd app sync shopping-app
```

after a successful build when manual synchronization is required.

---

## Production Tip

Use the CLI for automation and scripting, while using the Web UI for monitoring and troubleshooting.

---

## Interview Question

### Question

Why is the Argo CD CLI used?

### Answer

The Argo CD CLI is used for automation, scripting, application management, and integration with CI/CD pipelines.

---

# 8. Argo CD Applications & Projects

## Answer

In Argo CD, an **Application** represents a deployed workload.

A **Project** groups related applications and defines security boundaries.

---

## Application

Represents:

- Git Repository
- Target Cluster
- Namespace
- Deployment Method

---

## Project

Controls:

- Allowed Repositories
- Allowed Clusters
- Allowed Namespaces
- Resource Permissions

---

## Internal Workflow

```text
Project

        │

Contains

────────────┼────────────

        │

Application A

Application B

Application C
```

---

## Production Example

Project

```text
Production
```

Applications

```text
shopping-app

payment-service

inventory-service
```

The Project restricts which repositories and clusters these applications can use.

---

## Production Tip

Use Projects to isolate Development, QA, and Production applications and enforce security policies.

---

## Interview Question

### Question

What is the difference between an Application and a Project in Argo CD?

### Answer

An Application represents a deployed workload, while a Project groups applications and defines access, repository, cluster, and namespace restrictions.

---

# 9. Synchronization Process

## Answer

Synchronization is the process of making the Kubernetes cluster match the desired state stored in Git.

Argo CD continuously compares:

- Desired State (Git)
- Live State (Cluster)

If differences exist, the application becomes **OutOfSync**.

---

## Synchronization Workflow

```text
Git Repository

        │

Desired State

        │

Compare

        │

Live Cluster

        │

Difference Found

        │

Synchronize

        ▼

Cluster Updated
```

---

## Sync States

| State | Meaning |
|---------|----------|
| Synced | Cluster matches Git |
| OutOfSync | Cluster differs from Git |
| Unknown | Application status cannot be determined |

---

## Production Example

A developer updates:

```yaml
replicas: 5
```

Git now differs from the running cluster.

Argo CD marks the application as:

```text
OutOfSync
```

After synchronization:

```text
Synced
```

---

## Production Tip

Monitor synchronization status continuously to detect configuration drift before it impacts production.

---

## Interview Question

### Question

What does synchronization mean in Argo CD?

### Answer

Synchronization is the process of making the Kubernetes cluster match the desired state defined in the Git repository.

---

## 📌 Quick Revision

| Feature | Purpose |
|---------|---------|
| Installation | Deploy Argo CD components |
| Web UI | Monitor and manage applications |
| CLI | Automate and administer Argo CD |
| Application | Represents a deployed workload |
| Project | Groups applications and controls permissions |
| Synchronization | Keeps the cluster aligned with Git |

---

# 10. Automatic Synchronization

## Answer

One of Argo CD's most powerful features is **Automatic Synchronization (Auto Sync)**.

When Auto Sync is enabled, Argo CD continuously watches the Git repository.

Whenever a new commit is detected, Argo CD automatically synchronizes the Kubernetes cluster.

No manual deployment is required.

---

## Manual Deployment

```text
Developer

↓

Git Push

↓

Wait

↓

Run Deployment

↓

Application Updated
```

---

## Automatic Synchronization

```text
Developer

↓

Git Push

↓

Git Repository

↓

Argo CD Detects Change

↓

Automatic Synchronization

↓

Application Updated
```

---

## Production Example

A developer updates:

```yaml
image:
  tag: v2.0.0
```

After pushing the change to Git:

```bash
git push origin main
```

Argo CD detects the commit and updates the Kubernetes Deployment automatically.

---

## Production Tip

Enable Auto Sync only after validating your Git workflow with pull requests, code reviews, and automated testing.

---

## Interview Question

### Question

What is Auto Sync in Argo CD?

### Answer

Auto Sync allows Argo CD to automatically synchronize Kubernetes resources whenever changes are detected in the Git repository.

---

# 11. Self-Healing

## Answer

Self-Healing allows Argo CD to automatically restore the cluster when someone manually changes Kubernetes resources.

This ensures the cluster always matches the desired state stored in Git.

---

## Example

Current Desired State

```yaml
replicas: 3
```

Someone manually changes:

```bash
kubectl scale deployment shopping-app --replicas=10
```

Cluster State

```text
Replicas = 10
```

Git State

```text
Replicas = 3
```

Argo CD detects the drift and restores:

```text
Replicas = 3
```

---

## Internal Workflow

```text
Git Repository

        │

Desired State

        │

Compare

        │

Cluster State

        │

Difference Found

        │

Automatically Correct

        ▼

Cluster Matches Git
```

---

## Production Example

An engineer accidentally modifies a production Deployment using `kubectl edit`.

Within seconds, Argo CD restores the original configuration from Git.

---

## Production Tip

Enable Self-Healing in production to prevent unauthorized or accidental manual configuration changes.

---

## Interview Question

### Question

What is Self-Healing in Argo CD?

### Answer

Self-Healing automatically restores Kubernetes resources to match the desired state stored in Git whenever configuration drift is detected.

---

# 12. Drift Detection

## Answer

**Configuration Drift** occurs when the actual Kubernetes cluster differs from the desired configuration stored in Git.

Argo CD continuously compares both states.

---

## Drift Example

Git

```yaml
replicas: 3
```

Cluster

```yaml
replicas: 5
```

Result

```text
OutOfSync
```

---

## Internal Workflow

```text
Desired State (Git)

        │

Compare

        │

Live Cluster

        │

Difference?

────────────┼────────────

        │

Yes

↓

OutOfSync

↓

Sync Required

No

↓

Synced
```

---

## Production Example

A Pod image is manually updated in the cluster.

Argo CD detects the mismatch and marks the application as:

```text
OutOfSync
```

If Auto Sync is enabled, the original Git configuration is restored automatically.

---

## Production Tip

Investigate the cause of configuration drift instead of repeatedly applying manual changes.

---

## Interview Question

### Question

What is configuration drift?

### Answer

Configuration drift occurs when the live Kubernetes resources no longer match the desired configuration stored in Git.

---

# 13. Rollback Concepts

## Answer

Git naturally provides version history.

Instead of manually restoring Kubernetes resources, rollback is performed by restoring a previous Git commit.

---

## GitOps Rollback

```text
Commit 1

↓

Commit 2

↓

Commit 3

↓

Problem Found

↓

Revert Commit

↓

Git Updated

↓

Argo CD Synchronizes

↓

Previous Version Restored
```

---

## Production Example

Version 3 introduces a production issue.

The team reverts the Git commit:

```bash
git revert <commit-id>

git push origin main
```

Argo CD automatically deploys the previous stable version.

---

## Production Tip

Always perform rollbacks through Git instead of modifying Kubernetes resources directly.

---

## Interview Question

### Question

How are rollbacks performed in Argo CD?

### Answer

Rollbacks are typically performed by reverting the Git commit to a previous version. Argo CD detects the change and synchronizes the cluster automatically.

---

# 14. Common Beginner Mistakes

## Mistake 1

❌ Editing Kubernetes resources manually.

Always update Git instead.

---

## Mistake 2

❌ Treating Git as a backup instead of the source of truth.

All production changes should originate from Git.

---

## Mistake 3

❌ Deploying directly to production without testing changes in Development or QA.

---

## Mistake 4

❌ Ignoring OutOfSync applications.

Investigate and resolve drift immediately.

---

## Mistake 5

❌ Giving unrestricted access to Argo CD.

Implement RBAC and least-privilege access.

---

# 15. Production Best Practices

Always follow these recommendations.

---

✅ Use Git as the single source of truth.

---

✅ Protect production branches using pull requests.

---

✅ Enable Auto Sync after establishing a reliable review process.

---

✅ Enable Self-Healing for production applications.

---

✅ Separate Development, QA, Staging, and Production using Argo CD Projects.

---

✅ Monitor Sync and Health status regularly.

---

✅ Never bypass Git by applying production changes manually.

---

## 📌 Quick Revision

### GitOps Workflow

```text
Developer

↓

Git Commit

↓

Git Push

↓

Argo CD

↓

Compare

↓

Synchronize

↓

Kubernetes Cluster
```

---

### Argo CD Features

```text
Automatic Sync

↓

Self-Healing

↓

Drift Detection

↓

Rollback

↓

Continuous Delivery
```

---

### Common Commands

```bash
argocd app list

argocd app get shopping-app

argocd app sync shopping-app

kubectl get pods -n argocd
```

---

# Summary

After completing this guide, you should understand:

✅ Argo CD Fundamentals

✅ GitOps

✅ Argo CD Architecture

✅ Installation

✅ Web UI

✅ CLI

✅ Applications

✅ Projects

✅ Synchronization

✅ Automatic Sync

✅ Self-Healing

✅ Drift Detection

✅ Git-Based Rollbacks

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Argo CD? | A GitOps Continuous Delivery tool for Kubernetes |
| What is GitOps? | Managing infrastructure and applications using Git as the source of truth |
| What is Auto Sync? | Automatically synchronizes the cluster when Git changes |
| What is Self-Healing? | Automatically restores resources to match Git |
| What is Configuration Drift? | A mismatch between Git and the live Kubernetes cluster |
| How does Argo CD detect drift? | By continuously comparing the desired state in Git with the live cluster state |
| How are rollbacks performed? | By reverting Git commits and allowing Argo CD to synchronize |
| What is an Argo CD Application? | A deployed workload managed by Argo CD |
| What is an Argo CD Project? | A logical grouping of applications with access and deployment policies |
| Why is Git called the source of truth? | Because the desired application state is defined and managed in Git |

---

# What's Next?

➡️ **02-installation-configuration.md**

In the next guide, you'll learn:

- Argo CD Installation Methods
- Namespace Setup
- Component Overview
- Accessing the Argo CD UI
- Initial Admin Login
- Installing the Argo CD CLI
- Connecting to Kubernetes Clusters
- Repository Configuration
- Production Installation Best Practices
- Common Interview Questions