# 🚀 Argo CD Applications & Projects

This guide explains everything you need to know about Argo CD Applications and Projects.

If you've ever wondered:

- What is an Argo CD Application?
- Why do we need Applications?
- What is an Argo CD Project?
- How are Applications organized?
- How do Applications and Projects work together?

This guide is for you.

---

# 📑 Table of Contents

1. What is an Argo CD Application?
2. Why do we need Applications?
3. What is an Argo CD Project?
4. Relationship Between Applications and Projects
5. Common Interview Questions

---

# 1. What is an Argo CD Application?

## Answer

An **Application** is the primary resource managed by Argo CD.

It represents a Kubernetes application whose desired state is stored in a Git repository.

An Application tells Argo CD:

- Which Git repository to monitor
- Which branch or revision to use
- Where the Kubernetes manifests are located
- Which cluster to deploy to
- Which namespace to deploy into

---

## Simple Architecture

```text
Git Repository

↓

Application

↓

Argo CD

↓

Kubernetes Cluster
```

---

## Application Contains

Every Argo CD Application includes:

- Source Repository
- Target Revision
- Path
- Destination Cluster
- Namespace
- Synchronization Policy

---

## Real World Example

Suppose your Git repository contains:

```text
k8s/

├── deployment.yaml
├── service.yaml
└── ingress.yaml
```

Create one Application that points to:

```text
Repository

↓

k8s/

↓

Deploy Everything
```

Argo CD continuously keeps those manifests synchronized.

---

## Benefits

Applications provide:

- Continuous Deployment
- Version Tracking
- Automatic Synchronization
- Rollbacks
- Health Monitoring

---

## Production Tip

Create one Application per logical workload.

For example:

```text
shopping-api

payment-service

inventory-service
```

instead of placing everything into one large Application.

---

## Interview Question

### Question

What is an Argo CD Application?

### Answer

An Argo CD Application is a custom resource that defines how a Kubernetes application is deployed and synchronized from a Git repository.

---

# 2. Why do we need Applications?

## Answer

Without Applications, Argo CD wouldn't know:

- Which Git repository to monitor
- Which manifests belong together
- Which cluster to deploy to
- Which namespace to use

Applications provide all this information in one place.

---

## Without Applications

```text
Git Repository

↓

Many YAML Files

↓

No Deployment Information
```

---

## With Applications

```text
Git Repository

↓

Application Definition

↓

Argo CD

↓

Deploy to Kubernetes
```

---

## Production Example

A company has three microservices:

```text
Frontend

Backend

Payments
```

Each service has its own Argo CD Application.

Each team deploys independently without affecting other services.

---

## Production Tip

Avoid creating one Application for dozens of unrelated services.

Smaller Applications are easier to monitor, troubleshoot, and upgrade.

---

## Interview Question

### Question

Why do we need Argo CD Applications?

### Answer

Applications define what to deploy, where to deploy it, and how Argo CD should keep it synchronized with Git.

---

# 3. What is an Argo CD Project?

## Answer

A **Project** is a logical container used to organize multiple Applications.

Projects also provide security boundaries by controlling:

- Allowed Git repositories
- Allowed Kubernetes clusters
- Allowed namespaces
- Allowed resource types

---

## Simple Architecture

```text
Project

↓

Applications

↓

Deployments
```

---

## Example

```text
Production Project

↓

shopping-api

payment-service

inventory-service
```

---

## Benefits

Projects provide:

- Access Control
- Environment Isolation
- Repository Restrictions
- Cluster Restrictions
- Namespace Restrictions

---

## Production Example

Project

```text
Production
```

Allowed

```text
Git Repository

↓

Production Cluster

↓

production Namespace
```

Any Application outside these rules is rejected.

---

## Production Tip

Create separate Projects for:

- Development
- QA
- Staging
- Production

This improves security and operational control.

---

## Interview Question

### Question

What is an Argo CD Project?

### Answer

A Project is a logical grouping of Applications that defines deployment boundaries, repository access, cluster access, and namespace restrictions.

---

# 4. Relationship Between Applications and Projects

## Answer

Applications belong to Projects.

A Project defines the rules.

Applications operate within those rules.

---

## Architecture

```text
Project

        │

────────────┼────────────

        │

Application A

Application B

Application C

        │

Deploy

↓

Kubernetes Cluster
```

---

## Real World Example

Production Project

```text
Production

↓

shopping-api

↓

payment-service

↓

inventory-service
```

Development Project

```text
Development

↓

shopping-api-dev

↓

payment-service-dev
```

Each Project has different permissions and deployment targets.

---

## Internal Workflow

```text
Developer

↓

Create Application

↓

Assign Project

↓

Project Validation

↓

Deploy

↓

Kubernetes Cluster
```

---

## Production Tip

Use Projects to separate environments and teams rather than placing every Application into the default Project.

---

## Interview Question

### Question

What is the relationship between an Application and a Project?

### Answer

An Application belongs to a Project. The Project defines where the Application can deploy and which repositories, clusters, and namespaces it is allowed to use.

---

# 5. Creating an Argo CD Application

## Answer

Before Argo CD can deploy an application, an **Application** resource must be created.

This Application tells Argo CD:

- Which Git repository to monitor
- Which Kubernetes manifests or Helm Chart to use
- Which cluster to deploy to
- Which namespace to deploy into

---

## Application Workflow

```text
Git Repository

        │

Create Application

        │

Argo CD

        │

Read Manifests

        │

Deploy

        ▼

Kubernetes Cluster
```

---

## Two Ways to Create Applications

### 1. Web UI

Create applications using the Argo CD Dashboard.

---

### 2. CLI

Create applications using:

```bash
argocd app create
```

---

## Production Example

A DevOps engineer creates an application for:

```text
shopping-api

↓

Git Repository

↓

Deploy to Production Namespace
```

Argo CD automatically manages future deployments.

---

## Production Tip

Store Application definitions in Git whenever possible instead of creating them manually through the UI.

---

## Interview Question

### Question

How can an Argo CD Application be created?

### Answer

Applications can be created through the Argo CD Web UI, CLI, or declaratively using Kubernetes YAML manifests.

---

# 6. Declarative vs Imperative Applications

## Answer

Argo CD supports two methods of creating Applications.

---

## Imperative Approach

Applications are created manually.

Example

```bash
argocd app create shopping-app \
--repo https://github.com/example/gitops.git \
--path kubernetes \
--dest-server https://kubernetes.default.svc \
--dest-namespace production
```

---

### Workflow

```text
CLI

↓

Create Application

↓

Stored in Cluster
```

---

## Declarative Approach

Applications are stored as YAML files.

Example

```yaml
apiVersion: argoproj.io/v1alpha1

kind: Application

metadata:

  name: shopping-app
```

Deploy using:

```bash
kubectl apply -f application.yaml
```

---

### Workflow

```text
Application YAML

↓

Git Repository

↓

kubectl apply

↓

Application Created
```

---

## Comparison

| Declarative | Imperative |
|-------------|------------|
| Stored in Git | Created manually |
| Version Controlled | Not version controlled |
| Easy to review | Harder to audit |
| Preferred for GitOps | Useful for quick testing |

---

## Production Example

Production teams almost always use the declarative approach because Application definitions become part of the GitOps workflow.

---

## Production Tip

Treat Argo CD Applications as code and keep them under version control.

---

## Interview Question

### Question

Which approach is recommended for creating Argo CD Applications?

### Answer

The declarative approach is recommended because Application definitions are stored in Git, version-controlled, and easier to manage.

---

# 7. Understanding the Application YAML

## Answer

An Argo CD Application is defined as a Kubernetes Custom Resource (CR).

---

## Example

```yaml
apiVersion: argoproj.io/v1alpha1

kind: Application

metadata:

  name: shopping-app

spec:

  project: default

  source:

    repoURL: https://github.com/example/gitops.git

    targetRevision: main

    path: kubernetes

  destination:

    server: https://kubernetes.default.svc

    namespace: production
```

---

## Main Sections

### Metadata

Defines:

- Application Name
- Labels
- Annotations

---

### Project

Specifies which Argo CD Project the Application belongs to.

---

### Source

Defines:

- Git Repository
- Branch
- Directory

---

### Destination

Defines:

- Kubernetes Cluster
- Namespace

---

## Internal Workflow

```text
Application YAML

        │

Read Source

        │

Read Destination

        │

Deploy Resources

        ▼

Cluster
```

---

## Production Example

A single Application YAML deploys an entire microservice using Helm Charts stored in Git.

---

## Production Tip

Keep Application manifests inside a dedicated GitOps repository for centralized management.

---

## Interview Question

### Question

What are the main sections of an Argo CD Application manifest?

### Answer

The main sections are `metadata`, `spec.project`, `spec.source`, and `spec.destination`.

---

# 8. Source Configuration

## Answer

The **source** section tells Argo CD where to retrieve the application manifests.

---

## Example

```yaml
source:

  repoURL: https://github.com/example/gitops.git

  targetRevision: main

  path: kubernetes
```

---

## Source Fields

| Field | Purpose |
|---------|----------|
| repoURL | Git repository URL |
| targetRevision | Git branch, tag, or commit |
| path | Directory containing manifests |

---

## Internal Workflow

```text
Repository

↓

Branch

↓

Directory

↓

Read Files

↓

Deploy
```

---

## Production Example

Git Repository

```text
gitops/

├── dev/

├── qa/

├── production/
```

Each Application points to the appropriate directory.

---

## Production Tip

Use branches for development workflows and tags or release branches for production deployments.

---

## Interview Question

### Question

What does the `source` section define?

### Answer

The `source` section specifies the Git repository, revision, and path that Argo CD uses to retrieve application manifests.

---

# 9. Destination Configuration

## Answer

The **destination** section tells Argo CD where the application should be deployed.

---

## Example

```yaml
destination:

  server: https://kubernetes.default.svc

  namespace: production
```

---

## Destination Fields

| Field | Purpose |
|---------|----------|
| server | Target Kubernetes cluster |
| namespace | Target namespace |

---

## Internal Workflow

```text
Application

↓

Destination Cluster

↓

Namespace

↓

Deploy Resources
```

---

## Production Example

Development

```text
Namespace

↓

dev
```

Production

```text
Namespace

↓

production
```

Different Applications can deploy to different namespaces or even different clusters.

---

## Production Tip

Avoid deploying multiple unrelated applications into the same namespace unless required by your organizational design.

---

## Interview Question

### Question

What does the `destination` section define?

### Answer

The `destination` section specifies the target Kubernetes cluster and namespace where the application will be deployed.

---

## 📌 Quick Revision

| Feature | Purpose |
|----------|---------|
| Application | Defines a deployment |
| Declarative | YAML stored in Git |
| Imperative | CLI-created Application |
| Source | Git repository configuration |
| Destination | Target cluster and namespace |
| Project | Groups and secures Applications |

---

# 10. Labels & Annotations

## Answer

Argo CD Applications support Kubernetes **Labels** and **Annotations**.

They help organize, identify, and automate application management.

---

## Labels

Labels are used for:

- Filtering Applications
- Grouping Applications
- Automation
- Reporting

Example

```yaml
metadata:

  labels:

    environment: production

    team: platform

    application: shopping-api
```

---

## Annotations

Annotations store additional metadata.

Example

```yaml
metadata:

  annotations:

    owner: DevOps Team

    description: Shopping API Production Deployment
```

---

## Internal Workflow

```text
Application

        │

Labels

        │

Annotations

        │

Filtering

        ▼

Management
```

---

## Production Example

Applications are labeled using:

```text
environment=production

team=payments

region=india
```

This simplifies searching, reporting, and automation.

---

## Production Tip

Adopt consistent labeling standards across all Argo CD Applications.

---

## Interview Question

### Question

Why are Labels used in Argo CD Applications?

### Answer

Labels help organize, filter, and manage Applications efficiently across different environments and teams.

---

# 11. Multiple Source Types

## Answer

Argo CD supports deploying applications from different source types.

---

## Supported Sources

- Kubernetes YAML
- Helm Charts
- Kustomize
- Jsonnet
- OCI-based Helm Charts

---

## Examples

### Kubernetes Manifests

```text
deployment.yaml

service.yaml
```

---

### Helm Chart

```text
shopping-chart/
```

---

### Kustomize

```text
base/

overlays/
```

---

## Internal Workflow

```text
Git Repository

        │

Select Source

────────────┼────────────

        │

YAML

Helm

Kustomize

Jsonnet

        │

Render

        ▼

Deploy
```

---

## Production Example

A company uses:

- Helm for microservices
- Kustomize for infrastructure
- YAML for simple applications

All are managed by the same Argo CD instance.

---

## Production Tip

Choose the source type that best fits your application's deployment requirements. Helm is ideal for reusable templates, while Kustomize is well suited for environment-specific customization.

---

## Interview Question

### Question

Which source types are supported by Argo CD?

### Answer

Argo CD supports Kubernetes manifests, Helm Charts, Kustomize, Jsonnet, and OCI-based Helm Charts.

---

# 12. Application Health

## Answer

Argo CD continuously evaluates the health of managed applications.

Health status indicates whether the application is operating correctly.

---

## Common Health States

| Status | Meaning |
|---------|---------|
| Healthy | Application is running normally |
| Progressing | Deployment is in progress |
| Degraded | One or more resources have failed |
| Missing | Required resources are missing |
| Unknown | Health cannot be determined |

---

## Internal Workflow

```text
Application

        │

Monitor Resources

        │

Evaluate Health

        │

Display Status

        ▼

Dashboard
```

---

## Production Example

A Deployment has Pods in the `CrashLoopBackOff` state.

Argo CD marks the Application as:

```text
Degraded
```

allowing engineers to investigate quickly.

---

## Production Tip

Monitor Health Status together with Sync Status to identify deployment issues early.

---

## Interview Question

### Question

What does a **Degraded** Application mean?

### Answer

A Degraded Application indicates that one or more managed Kubernetes resources are unhealthy or have failed.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Creating one large Application for every workload.

Create smaller Applications aligned with individual services or logical workloads.

---

## Mistake 2

❌ Using the default Project for all Applications.

Create separate Projects for Development, QA, Staging, and Production.

---

## Mistake 3

❌ Ignoring Application Health warnings.

Investigate **Progressing**, **Degraded**, or **Missing** states promptly.

---

## Mistake 4

❌ Editing deployed resources manually with `kubectl`.

Always update the Git repository and let Argo CD synchronize the changes.

---

## Mistake 5

❌ Deploying Applications without code reviews.

Protect Git branches with pull requests and approval workflows.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Store every Application manifest in Git.

---

✅ Organize Applications by service or domain.

---

✅ Create dedicated Projects for each environment.

---

✅ Use meaningful Labels and Annotations.

---

✅ Enable monitoring and alerting for Health and Sync Status.

---

✅ Review Application definitions through pull requests.

---

✅ Follow the GitOps principle: Git is the single source of truth.

---

## 📌 Quick Revision

### Application Lifecycle

```text
Git Repository

↓

Application

↓

Project

↓

Argo CD

↓

Synchronize

↓

Kubernetes Cluster
```

---

### Core Application Fields

```text
metadata

↓

project

↓

source

↓

destination

↓

syncPolicy
```

---

### Common Source Types

```text
YAML

↓

Helm

↓

Kustomize

↓

Jsonnet

↓

OCI Helm
```

---

# Summary

After completing this guide, you should understand:

✅ Argo CD Applications

✅ Projects

✅ Declarative Applications

✅ Imperative Applications

✅ Application Manifest Structure

✅ Source Configuration

✅ Destination Configuration

✅ Labels & Annotations

✅ Supported Source Types

✅ Application Health

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is an Argo CD Application? | A custom resource that defines how an application is deployed from Git |
| What is an Argo CD Project? | A logical grouping of Applications with security and deployment boundaries |
| Which approach is recommended for creating Applications? | Declarative (YAML stored in Git) |
| What does the `source` section define? | The Git repository, revision, and manifest path |
| What does the `destination` section define? | The target cluster and namespace |
| Which source types does Argo CD support? | YAML, Helm, Kustomize, Jsonnet, and OCI Helm Charts |
| What does a **Healthy** status indicate? | The application is running as expected |
| What does a **Degraded** status indicate? | One or more managed resources are unhealthy |
| Why are Labels used? | To organize, filter, and automate application management |
| Why should Applications be stored in Git? | To support GitOps, version control, and auditability |

---

# What's Next?

➡️ **04-sync-health-self-healing.md**

In the next guide, you'll learn:

- What is Synchronization?
- Manual vs Automatic Sync
- Sync Policies
- Pruning
- Self-Healing
- Health Checks
- Drift Detection
- Sync Waves
- Sync Windows
- Production Best Practices
- Common Interview Questions