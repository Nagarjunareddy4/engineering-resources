# 🚀 Argo CD Sync, Health & Self-Healing

This guide explains everything you need to know about Synchronization, Health Monitoring, and Self-Healing in Argo CD.

If you've ever wondered:

- What is Synchronization?
- What are Sync Policies?
- What is Self-Healing?
- How does Argo CD detect configuration drift?
- How does Argo CD monitor application health?

This guide is for you.

---

# 📑 Table of Contents

1. What is Synchronization?
2. Why do we need Synchronization?
3. Manual vs Automatic Synchronization
4. Synchronization Workflow
5. Common Interview Questions

---

# 1. What is Synchronization?

## Answer

**Synchronization (Sync)** is the process of making the Kubernetes cluster match the **desired state** stored in Git.

Argo CD continuously compares:

- Desired State (Git Repository)
- Live State (Kubernetes Cluster)

If differences are detected, Argo CD synchronizes the cluster.

---

## Desired State

Stored in Git

```yaml
replicas: 3
```

---

## Live State

Running in Kubernetes

```yaml
replicas: 5
```

---

## Result

```text
OutOfSync

↓

Synchronization

↓

replicas = 3
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

Live Cluster

        │

Difference Found

        │

Synchronize

        ▼

Cluster Matches Git
```

---

## Real World Example

A developer updates:

```yaml
image:

  tag: v2.0.0
```

After pushing to Git, Argo CD synchronizes the Deployment and updates the running Pods.

---

## Benefits

Synchronization provides:

- Consistent Deployments
- Automatic Updates
- Drift Correction
- Reliable Environments
- GitOps Automation

---

## Production Tip

Treat Git as the only source of truth. Never make production changes directly in the Kubernetes cluster.

---

## Interview Question

### Question

What is Synchronization in Argo CD?

### Answer

Synchronization is the process of making the live Kubernetes cluster match the desired state stored in Git.

---

# 2. Why do we need Synchronization?

## Answer

Without synchronization, Kubernetes clusters gradually drift away from their intended configuration.

Common causes include:

- Manual Changes
- Failed Deployments
- Configuration Errors
- Emergency Fixes
- Human Mistakes

Synchronization continuously corrects these differences.

---

## Without Synchronization

```text
Git

↓

Deployment

↓

Manual Change

↓

Configuration Drift

↓

Different Environments
```

---

## With Synchronization

```text
Git

↓

Argo CD

↓

Compare

↓

Synchronize

↓

Cluster Matches Git
```

---

## Production Example

A developer accidentally changes:

```bash
kubectl scale deployment shopping-api --replicas=10
```

Git still contains:

```yaml
replicas: 3
```

Argo CD detects the difference and restores:

```text
Replicas = 3
```

---

## Production Tip

Avoid manual production changes that bypass Git, as they will be overwritten during synchronization.

---

## Interview Question

### Question

Why is Synchronization important?

### Answer

Synchronization keeps the Kubernetes cluster consistent with Git, preventing configuration drift and ensuring reliable deployments.

---

# 3. Manual vs Automatic Synchronization

## Answer

Argo CD supports two synchronization modes:

- Manual Sync
- Automatic Sync

---

## Manual Synchronization

The user explicitly starts synchronization.

CLI Example

```bash
argocd app sync shopping-app
```

Workflow

```text
Git Change

↓

OutOfSync

↓

Manual Sync

↓

Application Updated
```

---

## Automatic Synchronization

Argo CD automatically synchronizes whenever it detects changes in Git.

Workflow

```text
Git Commit

↓

Argo CD Detects Change

↓

Automatic Sync

↓

Cluster Updated
```

---

## Comparison

| Manual Sync | Automatic Sync |
|--------------|----------------|
| User starts deployment | Argo CD starts deployment |
| More operational control | Fully automated |
| Useful for testing | Ideal for production GitOps |

---

## Production Example

Development

```text
Manual Sync
```

Production

```text
Automatic Sync
```

with pull request approvals and automated testing.

---

## Production Tip

Enable Automatic Sync only after establishing a strong Git workflow with code reviews and CI validation.

---

## Interview Question

### Question

What is the difference between Manual Sync and Automatic Sync?

### Answer

Manual Sync requires a user to initiate synchronization, while Automatic Sync continuously deploys changes from Git without manual intervention.

---

# 4. Synchronization Workflow

## Answer

Every synchronization follows a predictable workflow.

---

## Workflow

```text
Developer

↓

Git Commit

↓

Git Push

↓

Repository Updated

↓

Argo CD Detects Change

↓

Compare Desired State

↓

OutOfSync

↓

Synchronize

↓

Healthy

↓

Synced
```

---

## Internal Architecture

```text
Git Repository

        │

Repository Server

        │

Application Controller

        │

Compare

        │

Synchronize

        ▼

Kubernetes Cluster
```

---

## Production Example

A developer updates a Helm Chart.

Argo CD:

1. Fetches the updated chart.
2. Renders the manifests.
3. Compares them with the cluster.
4. Applies only the required changes.
5. Marks the application as **Synced**.

---

## Production Tip

Monitor synchronization events to quickly identify failed or partial deployments.

---

## Interview Question

### Question

How does Argo CD perform synchronization?

### Answer

Argo CD retrieves the desired state from Git, compares it with the live Kubernetes cluster, detects differences, and applies the required changes to make both states match.

---

# 5. Sync Policies

## Answer

A **Sync Policy** defines how Argo CD synchronizes an Application with the desired state stored in Git.

Argo CD supports two sync policies:

- Manual Sync
- Automatic Sync

The sync policy is configured in the Application manifest.

---

## Example

```yaml
spec:

  syncPolicy:

    automated: {}
```

This enables automatic synchronization.

---

## Internal Workflow

```text
Application

        │

Read Sync Policy

────────────┼────────────

        │

Manual

Automatic

        │

Synchronize

        ▼

Cluster Updated
```

---

## Production Example

Development environments may use Manual Sync, while Production environments commonly use Automatic Sync after pull request approval and CI validation.

---

## Production Tip

Choose a sync policy based on your deployment workflow and organizational change management process.

---

## Interview Question

### Question

What is a Sync Policy in Argo CD?

### Answer

A Sync Policy determines how and when Argo CD synchronizes an Application with the desired state stored in Git.

---

# 6. Automatic Sync Configuration

## Answer

Automatic Sync allows Argo CD to deploy changes automatically whenever Git is updated.

No manual intervention is required.

---

## Enable Automatic Sync

```yaml
spec:

  syncPolicy:

    automated: {}
```

---

## Internal Workflow

```text
Developer

↓

Git Push

↓

Repository Updated

↓

Argo CD Detects Change

↓

Automatic Sync

↓

Application Updated
```

---

## Production Example

A developer updates the application image:

```yaml
image:

  tag: v2.1.0
```

After the commit is merged into the production branch, Argo CD automatically deploys the new version.

---

## Production Tip

Enable Automatic Sync only after implementing:

- Pull Requests
- Code Reviews
- CI Validation
- Automated Testing

---

## Interview Question

### Question

How do you enable Automatic Sync?

### Answer

Configure the `syncPolicy.automated` section in the Application manifest.

---

# 7. Pruning

## Answer

**Pruning** removes Kubernetes resources that exist in the cluster but are no longer defined in Git.

Without pruning, obsolete resources remain in the cluster.

---

## Example

Git Before

```text
deployment.yaml

service.yaml

configmap.yaml
```

Git After

```text
deployment.yaml

service.yaml
```

The ConfigMap has been removed from Git.

---

## Without Pruning

```text
ConfigMap

Still Exists
```

---

## With Pruning

```text
ConfigMap

Automatically Deleted
```

---

## Enable Pruning

```yaml
spec:

  syncPolicy:

    automated:

      prune: true
```

---

## Internal Workflow

```text
Git Repository

↓

Compare Resources

↓

Missing Resource

↓

Prune Enabled

↓

Delete Resource

↓

Cluster Matches Git
```

---

## Production Example

An old ConfigMap is removed from the repository.

Argo CD automatically deletes it from the Kubernetes cluster during synchronization.

---

## Production Tip

Enable pruning carefully in production because deleted resources cannot be recovered unless they still exist in Git or backups.

---

## Interview Question

### Question

What is Pruning in Argo CD?

### Answer

Pruning automatically removes Kubernetes resources that are no longer defined in the Git repository.

---

# 8. Self-Healing

## Answer

Self-Healing allows Argo CD to automatically correct manual changes made directly in the Kubernetes cluster.

This prevents configuration drift.

---

## Example

Git

```yaml
replicas: 3
```

Someone manually scales the Deployment:

```bash
kubectl scale deployment shopping-api --replicas=8
```

---

## Enable Self-Healing

```yaml
spec:

  syncPolicy:

    automated:

      selfHeal: true
```

---

## Internal Workflow

```text
Git

↓

Desired State

↓

Compare

↓

Live Cluster

↓

Difference Found

↓

Restore Configuration

↓

Cluster Matches Git
```

---

## Production Example

A production Deployment is accidentally modified using:

```bash
kubectl edit deployment shopping-api
```

Argo CD automatically restores the Deployment to the configuration stored in Git.

---

## Production Tip

Enable Self-Healing in production to prevent unauthorized manual configuration changes.

---

## Interview Question

### Question

What is Self-Healing in Argo CD?

### Answer

Self-Healing automatically restores Kubernetes resources to match the desired configuration stored in Git whenever manual changes are detected.

---

# 9. Drift Detection

## Answer

**Configuration Drift** occurs when the live Kubernetes cluster differs from the desired state stored in Git.

Argo CD continuously checks for these differences.

---

## Example

Git

```yaml
replicas: 3
```

Cluster

```yaml
replicas: 5
```

---

## Result

```text
OutOfSync
```

---

## Internal Workflow

```text
Desired State

↓

Compare

↓

Live State

↓

Difference?

────────────┼────────────

        │

Yes

↓

OutOfSync

↓

Synchronize

No

↓

Synced
```

---

## Production Example

A Service is manually modified using `kubectl edit`.

Argo CD detects the drift, marks the Application as **OutOfSync**, and restores the original configuration if Auto Sync and Self-Healing are enabled.

---

## Production Tip

Investigate the root cause of drift instead of repeatedly making manual corrections in the cluster.

---

## Interview Question

### Question

How does Argo CD detect configuration drift?

### Answer

Argo CD continuously compares the desired state stored in Git with the live Kubernetes cluster and marks the Application as **OutOfSync** when differences are detected.

---

## 📌 Quick Revision

| Feature | Purpose |
|----------|---------|
| Sync Policy | Defines synchronization behavior |
| Automatic Sync | Deploys changes automatically |
| Pruning | Deletes resources removed from Git |
| Self-Healing | Restores manual changes |
| Drift Detection | Detects differences between Git and the cluster |

---

# 10. Application Health

## Answer

Synchronization tells you whether the cluster matches Git.

**Health** tells you whether the application is actually running correctly.

An application can be:

- Synced but Unhealthy
- OutOfSync but Healthy

These are two different concepts.

---

## Example

Git

```text
Replicas = 3
```

Cluster

```text
Replicas = 3
```

Sync Status

```text
Synced
```

However, all Pods are in:

```text
CrashLoopBackOff
```

Health Status

```text
Degraded
```

---

## Internal Workflow

```text
Git

        │

Synchronization

        │

Application Resources

        │

Health Evaluation

        ▼

Healthy / Degraded
```

---

## Production Example

After a deployment:

- Sync Status = Synced
- Pods fail to start

Argo CD correctly reports:

```text
Health = Degraded
```

---

## Production Tip

Always monitor both **Sync Status** and **Health Status**. A synchronized application is not always a healthy application.

---

## Interview Question

### Question

What is Application Health in Argo CD?

### Answer

Application Health indicates whether the deployed Kubernetes resources are operating correctly, independent of synchronization status.

---

# 11. Health Statuses

## Answer

Argo CD evaluates the health of every managed resource.

---

## Common Health States

| Health Status | Meaning |
|---------------|---------|
| Healthy | All resources are operating normally |
| Progressing | Deployment is still in progress |
| Degraded | One or more resources are unhealthy |
| Missing | Required resources cannot be found |
| Suspended | Resource execution has been intentionally paused |
| Unknown | Health cannot be determined |

---

## Internal Workflow

```text
Monitor Resources

        │

Evaluate Status

        │

Display Health

        ▼

Dashboard
```

---

## Production Example

Pods

```text
Running
```

Application

```text
Healthy
```

Pods

```text
ImagePullBackOff
```

Application

```text
Degraded
```

---

## Production Tip

Investigate **Degraded** and **Unknown** applications immediately, especially in production environments.

---

## Interview Question

### Question

What does a **Healthy** application mean?

### Answer

A Healthy application means all managed Kubernetes resources are functioning as expected.

---

# 12. Sync Statuses

## Answer

Sync Status shows whether the live Kubernetes cluster matches the desired state stored in Git.

---

## Common Sync States

| Sync Status | Meaning |
|-------------|---------|
| Synced | Cluster matches Git |
| OutOfSync | Cluster differs from Git |
| Unknown | Synchronization status cannot be determined |

---

## Example

Git

```yaml
replicas: 3
```

Cluster

```yaml
replicas: 5
```

Status

```text
OutOfSync
```

After synchronization

```text
Synced
```

---

## Internal Workflow

```text
Git Repository

↓

Desired State

↓

Compare

↓

Live Cluster

↓

Status

↓

Synced / OutOfSync
```

---

## Production Example

A developer updates a Helm Chart in Git.

Before synchronization:

```text
OutOfSync
```

After synchronization:

```text
Synced
```

---

## Production Tip

Configure monitoring and alerts for **OutOfSync** applications to detect deployment drift quickly.

---

## Interview Question

### Question

What is the difference between **Synced** and **OutOfSync**?

### Answer

Synced means the cluster matches Git, while OutOfSync means the live cluster differs from the desired state stored in Git.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Making production changes using:

```bash
kubectl edit
```

Always update Git instead.

---

## Mistake 2

❌ Ignoring OutOfSync applications.

Investigate the reason before forcing synchronization.

---

## Mistake 3

❌ Enabling Automatic Sync without a proper Git review process.

Use pull requests, approvals, and CI validation first.

---

## Mistake 4

❌ Confusing Sync Status with Health Status.

Remember:

- Sync = Git matches Cluster.
- Health = Application is working correctly.

---

## Mistake 5

❌ Enabling Pruning without understanding its impact.

Pruning permanently removes resources that no longer exist in Git.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Use Git as the single source of truth.

---

✅ Enable Automatic Sync only after implementing CI/CD validation.

---

✅ Enable Self-Healing for production workloads.

---

✅ Enable Pruning carefully after testing.

---

✅ Monitor both Sync Status and Health Status.

---

✅ Configure alerts for OutOfSync and Degraded applications.

---

✅ Never bypass Git with manual production changes.

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

Self-Heal

↓

Healthy Cluster
```

---

### Sync Process

```text
Desired State

↓

Compare

↓

OutOfSync

↓

Synchronize

↓

Synced
```

---

### Health Evaluation

```text
Resources Running

↓

Health Check

↓

Healthy

or

Degraded
```

---

# Summary

After completing this guide, you should understand:

✅ Synchronization

✅ Manual Sync

✅ Automatic Sync

✅ Sync Policies

✅ Pruning

✅ Self-Healing

✅ Drift Detection

✅ Application Health

✅ Health Statuses

✅ Sync Statuses

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Synchronization? | The process of making the Kubernetes cluster match the desired state stored in Git |
| What is Automatic Sync? | Argo CD automatically deploys changes from Git |
| What is Pruning? | Removing Kubernetes resources that no longer exist in Git |
| What is Self-Healing? | Automatically restoring manual changes to match Git |
| What is Configuration Drift? | A mismatch between Git and the live Kubernetes cluster |
| What does **Healthy** mean? | All managed resources are operating correctly |
| What does **Degraded** mean? | One or more resources are unhealthy |
| What does **Synced** mean? | The cluster matches the desired state in Git |
| What does **OutOfSync** mean? | The cluster differs from the desired state in Git |
| Why should Git be the source of truth? | It provides version control, auditability, and consistent deployments |

---

# What's Next?

➡️ **05-application-sets.md**

In the next guide, you'll learn:

- What are ApplicationSets?
- Why ApplicationSets are Needed
- ApplicationSet Controller
- Generators (List, Git, Cluster, Matrix, Merge)
- ApplicationSet Templates
- Multi-Cluster Deployments
- Production Use Cases
- Best Practices
- Common Interview Questions