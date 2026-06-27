# 🚀 Kubernetes Deployments - Managing Applications in Production

This guide explains everything you need to know about Kubernetes Deployments.

If you've ever wondered:

- What is a Deployment?
- Why do we need Deployments?
- How do Deployments work?
- How do Deployments manage ReplicaSets?
- Why are Deployments used in production?

This guide is for you.

---

# 📑 Table of Contents

1. What is a Deployment?
2. Why do we need Deployments?
3. How Deployments Work
4. Deployment Architecture
5. Common Interview Questions

---

# 1. What is a Deployment?

## Answer

A **Deployment** is a Kubernetes resource that manages the complete lifecycle of an application.

It ensures that:

- The desired number of Pods are running
- Applications can be updated safely
- Failed Pods are replaced automatically
- Previous versions can be restored

A Deployment does **not** manage Pods directly.

Instead, it manages **ReplicaSets**, and ReplicaSets manage **Pods**.

---

## Why do we need Deployments?

Managing Pods directly is difficult.

Even ReplicaSets only ensure the correct number of Pods are running.

Production applications also require:

- Rolling Updates
- Rollbacks
- Version History
- Zero Downtime Deployments
- Easy Scaling

Deployments provide all these capabilities.

---

## Deployment Overview

```text
Deployment

        │

Creates

        ▼

ReplicaSet

        │

Creates

        ▼

Pods

        │

Runs

        ▼

Containers
```

---

## Real World Example

Imagine an e-commerce website.

Version 1 of the application is running.

A new version needs to be released.

Instead of deleting all Pods and recreating them, the Deployment gradually replaces old Pods with new ones.

Users continue using the application without interruption.

---

## Benefits

Deployments provide:

- Rolling Updates
- Rollbacks
- Self-Healing
- High Availability
- Easy Scaling
- Deployment History

---

## Production Tip

In production environments, applications should almost always be deployed using **Deployments**, not standalone Pods or ReplicaSets.

---

## Interview Question

### Question

What is a Kubernetes Deployment?

### Answer

A Deployment is a Kubernetes resource that manages ReplicaSets and provides features such as rolling updates, rollbacks, scaling, deployment history, and high availability for applications.

---

# 2. Why do we need Deployments?

## Answer

ReplicaSets maintain the desired number of Pods.

However, they cannot safely update applications.

Imagine an application running:

```text
Version 1

↓

ReplicaSet

↓

3 Pods
```

Now you need to deploy Version 2.

Without Deployments, you would need to manually delete old Pods and create new ones.

This increases the risk of downtime.

Deployments automate this process.

---

## Without Deployments

```text
Version 1

↓

Delete All Pods

↓

Create New Pods

↓

Application Downtime
```

---

## With Deployments

```text
Version 1

↓

Deployment

↓

Rolling Update

↓

Version 2

↓

No Downtime
```

---

## Why is this important?

Deployments enable:

- Safer Releases
- Automatic Rollbacks
- Continuous Availability
- Version Tracking
- Easier Application Management

---

## Production Example

A banking application receives thousands of requests every second.

Deploying a new version without causing downtime is critical.

Deployments gradually replace old Pods with new Pods while users continue accessing the application.

---

## Production Tip

Deployments are designed for **stateless applications** such as:

- Web Applications
- REST APIs
- Microservices

---

## Interview Question

### Question

Why do we need Deployments in Kubernetes?

### Answer

Deployments provide advanced application management features such as rolling updates, rollbacks, scaling, deployment history, and zero-downtime releases.

---

# 3. How Deployments Work

## Answer

A Deployment does not create Pods directly.

Instead, it creates a ReplicaSet.

The ReplicaSet then creates and maintains the required Pods.

---

## Internal Workflow

```text
Deployment

        │

Creates

        ▼

ReplicaSet

        │

Maintains

        ▼

Pods

        │

Runs

        ▼

Containers
```

---

## Example

Desired State:

```text
Deployment

Replicas = 3
```

Current State:

```text
3 Running Pods
```

If one Pod fails:

```text
ReplicaSet

↓

Creates New Pod

↓

Deployment Remains Healthy
```

The Deployment continuously ensures that the application remains available.

---

## Production Example

Suppose a Worker Node crashes.

The ReplicaSet creates replacement Pods on healthy Worker Nodes.

The Deployment continues managing the overall application lifecycle without manual intervention.

---

## Production Tip

The Deployment manages the application, while the ReplicaSet manages the Pods.

This separation makes Kubernetes highly modular and reliable.

---

## Interview Question

### Question

How does a Deployment work?

### Answer

A Deployment creates and manages ReplicaSets, and ReplicaSets create and maintain Pods. This layered architecture enables rolling updates, rollbacks, and high availability.

---

# 4. Deployment Architecture

## Answer

Deployments sit at the top of the Kubernetes application management hierarchy.

---

## Architecture

```text
Deployment

        │

        ▼

ReplicaSet

        │

────────┼────────

        │

Pod 1

Pod 2

Pod 3

        │

Containers
```

---

## Component Responsibilities

### Deployment

- Manage application lifecycle
- Create ReplicaSets
- Perform Rolling Updates
- Maintain Deployment History
- Support Rollbacks

---

### ReplicaSet

- Maintain the desired number of Pods
- Replace failed Pods
- Scale Pods

---

### Pods

- Execute application containers

---

## Production Tip

When you update a Deployment, Kubernetes creates a **new ReplicaSet** instead of modifying the existing one.

This allows safe rollbacks if the new version encounters issues.

---

## Interview Question

### Question

What is the relationship between Deployments, ReplicaSets, and Pods?

### Answer

A Deployment manages ReplicaSets, ReplicaSets manage Pods, and Pods run the application containers.

---

# 5. Deployment YAML

## Answer

A Deployment is created using a YAML manifest.

The most important fields are:

- replicas
- selector
- template
- strategy

---

## Example

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:

    metadata:
      labels:
        app: nginx

    spec:

      containers:

      - name: nginx

        image: nginx:latest

        ports:

        - containerPort: 80
```

---

## Important Fields

### replicas

Defines the desired number of Pods.

```yaml
replicas: 3
```

---

### selector

Identifies which Pods belong to the Deployment.

```yaml
selector:

  matchLabels:

    app: nginx
```

---

### template

Defines how new Pods should be created.

Whenever Kubernetes creates a new Pod, it uses this template.

---

### strategy

Defines how updates are performed.

Default:

```yaml
strategy:

  type: RollingUpdate
```

---

## Production Tip

Always keep the Labels inside the Pod Template identical to the Deployment Selector.

---

## Interview Question

### Question

What are the most important fields in a Deployment?

### Answer

The most important fields are:

- replicas
- selector
- template
- strategy

---

# 6. Deployment Strategies

## Answer

Deployment Strategies define how Kubernetes replaces old Pods with new Pods during an application update.

Kubernetes supports two strategies:

- RollingUpdate
- Recreate

---

## RollingUpdate (Default)

Pods are updated gradually.

Old Pods are removed only after new Pods become available.

```text
Version 1

Pod 1

Pod 2

Pod 3

↓

Rolling Update

↓

Pod 1 (v2)

Pod 2 (v2)

Pod 3 (v2)
```

Advantages:

- Minimal Downtime
- Safer Deployments
- Production Ready

---

## Recreate

All existing Pods are deleted first.

New Pods are created afterwards.

```text
Version 1

↓

Delete All Pods

↓

Create Version 2 Pods

↓

Application Available
```

Advantages:

- Simple
- Useful when old and new versions cannot run together

Disadvantages:

- Downtime

---

## Production Tip

Use **RollingUpdate** for almost all production workloads.

---

## Interview Question

### Question

What are the two Deployment strategies?

### Answer

Kubernetes supports:

- RollingUpdate
- Recreate

RollingUpdate is the default and recommended strategy for production.

---

# 7. Scaling Deployments

## Answer

Scaling changes the number of running Pods managed by a Deployment.

Scaling can be:

- Manual
- Automatic

---

## Manual Scaling

Current:

```text
3 Pods
```

Desired:

```text
5 Pods
```

Deployment creates:

```text
2 New Pods
```

---

## Scale Down

Current:

```text
5 Pods
```

Desired:

```text
2 Pods
```

Deployment removes:

```text
3 Pods
```

---

## Internal Workflow

```text
Deployment

        │

Replica Count Changed

        │

        ▼

ReplicaSet

        │

Create/Delete Pods

        │

Desired State Achieved
```

---

## Production Example

During a festival sale, an e-commerce application experiences increased traffic.

The DevOps team increases replicas from:

```text
5

↓

20
```

The Deployment creates additional Pods automatically.

---

## Production Tip

For production systems, use the **Horizontal Pod Autoscaler (HPA)** instead of manually changing replica counts.

---

## Interview Question

### Question

How do you scale a Deployment?

### Answer

A Deployment is scaled by changing the replica count. Kubernetes creates or removes Pods until the desired state is reached.

---

# 8. 🔍 What really happens when you update a Deployment?

## Answer

This is one of the most frequently asked Kubernetes interview questions.

Suppose your application is running:

```text
Version 1

↓

Deployment

↓

ReplicaSet (v1)

↓

3 Pods
```

You update the container image.

---

## Internal Workflow

```text
kubectl apply

        │

        ▼

API Server

        │

Deployment Updated

        │

        ▼

Create New ReplicaSet

        │

────────────┼────────────

        │

Old ReplicaSet

New ReplicaSet

        │

        ▼

New Pods Created

        │

Old Pods Deleted

        │

        ▼

Deployment Completed
```

---

## Step-by-Step Flow

1. Deployment specification changes.
2. API Server stores the new configuration.
3. Deployment creates a new ReplicaSet.
4. The new ReplicaSet starts creating new Pods.
5. Old Pods are gradually terminated.
6. Traffic shifts to the new Pods.
7. The Deployment finishes successfully.

---

## Production Example

Application:

```text
v1.0
```

Updated to:

```text
v2.0
```

Instead of replacing all Pods simultaneously, Kubernetes gradually replaces Pods one at a time.

Users continue using the application without downtime.

---

## Production Tip

Never update Pods manually.

Always update the Deployment.

Kubernetes handles the rest automatically.

---

## Interview Question

### Question

What happens internally when a Deployment is updated?

### Answer

Kubernetes creates a new ReplicaSet, gradually creates new Pods, removes old Pods, and completes the update while maintaining application availability.

---

# 9. Rolling Updates

## Answer

A **Rolling Update** is the default Deployment strategy in Kubernetes.

Instead of replacing all Pods at once, Kubernetes gradually replaces old Pods with new Pods.

This ensures that the application remains available during the update.

---

## Why do we need Rolling Updates?

Imagine your application has:

```text
10 Running Pods
```

A new version needs to be deployed.

Without Rolling Updates:

```text
Delete 10 Pods

↓

Create 10 New Pods

↓

Application Downtime
```

With Rolling Updates:

```text
Pod 1 → Updated

↓

Pod 2 → Updated

↓

Pod 3 → Updated

↓

...

↓

Pod 10 → Updated
```

Users continue accessing the application while the update is in progress.

---

## Rolling Update Workflow

```text
Deployment

        │

Create New ReplicaSet

        │

────────────┼────────────

        │

Scale Up New Pods

        │

Scale Down Old Pods

        │

Repeat

        │

        ▼

Deployment Complete
```

---

## Production Example

Current Version:

```text
v1.0

3 Pods
```

New Version:

```text
v2.0
```

Kubernetes:

- Creates one new Pod
- Waits until it's healthy
- Removes one old Pod
- Repeats the process until all Pods run Version 2

---

## Production Tip

Rolling Updates significantly reduce downtime and deployment risk.

---

## Interview Question

### Question

What is a Rolling Update?

### Answer

A Rolling Update gradually replaces old Pods with new Pods while keeping the application available during deployment.

---

# 10. Rollbacks

## Answer

Sometimes deployments fail.

A new application version may contain:

- Bugs
- Configuration Errors
- Startup Failures

Instead of manually redeploying the previous version, Kubernetes allows you to roll back quickly.

---

## Rollback Workflow

```text
Version 1

↓

Version 2

↓

Deployment Failed

↓

Rollback

↓

Version 1 Restored
```

---

## Command

```bash
kubectl rollout undo deployment nginx
```

---

## Production Example

A developer deploys Version 2.

Users begin receiving HTTP 500 errors.

The DevOps engineer executes:

```bash
kubectl rollout undo deployment nginx
```

Kubernetes restores the previous working ReplicaSet.

---

## Production Tip

Always verify the new version before deleting old deployment revisions.

---

## Interview Question

### Question

What is a Rollback?

### Answer

A Rollback restores a Deployment to a previously working version using Deployment revision history.

---

# 11. Deployment Revisions

## Answer

Every time a Deployment is updated, Kubernetes creates a new **Revision**.

Each revision corresponds to a ReplicaSet.

This allows Kubernetes to:

- Track deployment history
- Compare versions
- Roll back safely

---

## Revision History

```text
Revision 1

↓

Version 1.0

↓

Revision 2

↓

Version 1.1

↓

Revision 3

↓

Version 2.0
```

---

## View Revision History

```bash
kubectl rollout history deployment nginx
```

---

## Production Tip

Maintain sufficient revision history to support rollbacks after production releases.

---

## Interview Question

### Question

What is a Deployment Revision?

### Answer

A Deployment Revision is a versioned snapshot of a Deployment that enables history tracking and rollbacks.

---

# 12. Pause and Resume Deployments

## Answer

Kubernetes allows you to temporarily pause a Deployment.

While paused:

- Configuration changes are accepted.
- No rollout occurs.

Once resumed, Kubernetes applies all pending changes.

---

## Pause Deployment

```bash
kubectl rollout pause deployment nginx
```

---

## Resume Deployment

```bash
kubectl rollout resume deployment nginx
```

---

## Production Example

A DevOps engineer needs to make several Deployment changes.

Instead of triggering multiple rollouts, the Deployment is paused.

After all changes are complete, the Deployment is resumed, resulting in a single rollout.

---

## Production Tip

Pause Deployments when applying multiple related configuration changes.

---

## Interview Question

### Question

Why would you pause a Deployment?

### Answer

Pausing a Deployment prevents rollouts while multiple configuration changes are being made, reducing unnecessary deployments.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Updating Pods directly.

Always update the Deployment.

---

## Mistake 2

❌ Deleting ReplicaSets manually.

ReplicaSets are managed by Deployments.

---

## Mistake 3

❌ Assuming Deployments support stateful applications.

Stateful workloads should generally use **StatefulSets**.

---

## Mistake 4

❌ Ignoring rollout status.

Always verify Deployment progress.

```bash
kubectl rollout status deployment nginx
```

---

## Mistake 5

❌ Deploying directly to Production.

Test changes in Development and QA before Production.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Use Rolling Updates for production deployments.

---

✅ Configure Liveness and Readiness Probes.

---

✅ Set CPU and Memory Requests & Limits.

---

✅ Use Deployment revisions for safe rollbacks.

---

✅ Monitor rollout status after every deployment.

---

✅ Use CI/CD pipelines for Kubernetes deployments.

---

## 📌 Quick Revision

### Deployment Workflow

```text
Deployment

↓

ReplicaSet

↓

Pods

↓

Containers
```

---

### Deployment Update Flow

```text
Update Deployment

↓

New ReplicaSet

↓

New Pods

↓

Old Pods Removed

↓

Deployment Complete
```

---

### Deployment Features

- Rolling Updates
- Rollbacks
- Scaling
- Revision History
- Pause & Resume
- Self-Healing

---

# Summary

After completing this guide, you should understand:

✅ Deployments

✅ Deployment YAML

✅ Deployment Strategies

✅ Rolling Updates

✅ Rollbacks

✅ Revision History

✅ Scaling

✅ Pause & Resume

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What manages ReplicaSets? | Deployment |
| Default Deployment Strategy? | RollingUpdate |
| Restore previous version? | Rollback |
| View rollout history? | `kubectl rollout history` |
| Rollback command? | `kubectl rollout undo` |
| Pause Deployment? | `kubectl rollout pause` |
| Resume Deployment? | `kubectl rollout resume` |
| Check rollout status? | `kubectl rollout status` |
| Supports zero-downtime deployments? | Yes (Rolling Updates) |
| Recommended for production apps? | Yes |

---

# What's Next?

➡️ **06-services.md**

In the next guide, you'll learn:

- What is a Service?
- Why do we need Services?
- ClusterIP
- NodePort
- LoadBalancer
- ExternalName
- Service Discovery
- DNS in Kubernetes
- What really happens when a request reaches a Service?
- Production Best Practices
- Common Interview Questions