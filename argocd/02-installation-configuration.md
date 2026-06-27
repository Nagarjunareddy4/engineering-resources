# 🚀 Argo CD Installation & Configuration

This guide explains everything you need to know about installing and configuring Argo CD.

If you've ever wondered:

- How do I install Argo CD?
- What are the prerequisites?
- Which installation method should I choose?
- What components are installed?
- How do I verify the installation?

This guide is for you.

---

# 📑 Table of Contents

1. Prerequisites
2. Argo CD Installation Methods
3. Installing Argo CD
4. Argo CD Components
5. Common Interview Questions

---

# 1. Prerequisites

## Answer

Before installing Argo CD, ensure your Kubernetes environment is ready.

---

## Required Components

- Kubernetes Cluster
- kubectl
- Internet Connectivity
- Cluster Administrator Permissions
- Git Repository

---

## Verify Kubernetes Cluster

```bash
kubectl cluster-info
```

Example Output

```text
Kubernetes control plane is running
```

---

## Verify Nodes

```bash
kubectl get nodes
```

Example Output

```text
NAME        STATUS   ROLES

master      Ready    control-plane

worker01    Ready    <none>
```

---

## Verify kubectl

```bash
kubectl version --client
```

---

## Internal Workflow

```text
Kubernetes Cluster

        │

kubectl Installed

        │

Cluster Access

        │

Ready for Argo CD Installation
```

---

## Production Example

Before installing Argo CD into an AKS cluster, a DevOps engineer verifies:

- Cluster Health
- Node Status
- Namespace Permissions
- Internet Access

---

## Production Tip

Install Argo CD only after confirming the Kubernetes cluster is healthy and accessible.

---

## Interview Question

### Question

What are the prerequisites for installing Argo CD?

### Answer

A running Kubernetes cluster, kubectl, cluster administrator permissions, and access to a Git repository.

---

# 2. Argo CD Installation Methods

## Answer

Argo CD supports multiple installation methods depending on the environment.

---

## Common Installation Methods

### Official YAML Manifest

```bash
kubectl apply -f install.yaml
```

Recommended for:

- Learning
- Development
- Production

---

### Helm Chart

```bash
helm install argocd argo/argo-cd
```

Recommended for:

- Organizations already using Helm
- Automated deployments

---

### GitOps Installation

Argo CD can also install and manage itself using GitOps principles.

---

## Comparison

| Method | Recommended Use |
|----------|-----------------|
| Official Manifest | Simple installations |
| Helm | Helm-based environments |
| GitOps | Enterprise automation |

---

## Internal Workflow

```text
Choose Installation Method

        │

Deploy Components

        │

Verify Pods

        ▼

Argo CD Ready
```

---

## Production Example

Many enterprise teams deploy Argo CD using Helm while allowing Argo CD to manage future upgrades through GitOps.

---

## Production Tip

Use Helm or GitOps for production environments to simplify upgrades and version management.

---

## Interview Question

### Question

What are the common methods of installing Argo CD?

### Answer

Argo CD can be installed using the official Kubernetes manifests, Helm Charts, or GitOps-based self-management.

---

# 3. Installing Argo CD

## Answer

The official installation creates all required Argo CD components inside a dedicated namespace.

---

## Step 1: Create Namespace

```bash
kubectl create namespace argocd
```

---

## Step 2: Install Argo CD

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

## Step 3: Verify Installation

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

Deploy Components

        │

Start Pods

        │

Verify Pods

        ▼

Argo CD Installed
```

---

## Production Example

A DevOps engineer installs Argo CD into a dedicated namespace on an AKS cluster and verifies that all components reach the **Running** state before configuring applications.

---

## Production Tip

Wait until every Argo CD Pod reports **Running** before proceeding with configuration.

---

## Interview Question

### Question

How do you install Argo CD?

### Answer

Create the `argocd` namespace, apply the official installation manifest, and verify that all Argo CD Pods are running.

---

# 4. Argo CD Components

## Answer

Installing Argo CD deploys several components, each with a specific responsibility.

---

## Main Components

### argocd-server

Provides:

- Web UI
- REST API
- Authentication

---

### argocd-application-controller

Responsible for:

- Monitoring Applications
- Detecting Drift
- Synchronizing Resources

---

### argocd-repo-server

Responsible for:

- Cloning Git Repositories
- Rendering Helm Charts
- Processing Kustomize Configurations

---

### argocd-dex-server

Provides:

- Single Sign-On (SSO)
- Identity Integration

---

### argocd-redis

Provides:

- Caching
- Session Storage
- Performance Optimization

---

## Component Architecture

```text
Git Repository

        │

Repo Server

        │

Application Controller

        │

Kubernetes API Server

        ▼

Cluster

-------------------------

Web Browser

        │

Argo CD Server

        │

Authentication

        ▼

Dex Server

-------------------------

Redis

↓

Caching
```

---

## Production Example

When a developer pushes changes to Git:

- Repo Server fetches the manifests.
- Application Controller compares Git with the cluster.
- Argo CD Server displays the application status in the UI.

---

## Production Tip

Monitor all Argo CD components to ensure continuous GitOps synchronization.

---

## Interview Question

### Question

What are the main components of Argo CD?

### Answer

The main components are Argo CD Server, Application Controller, Repository Server, Dex Server, and Redis.

---

# 5. Accessing the Argo CD Web UI

## Answer

After installing Argo CD, the Web UI allows you to manage and monitor Kubernetes applications.

Since the Argo CD Server is initially exposed as a **ClusterIP** service, it is not directly accessible from outside the cluster.

The simplest way to access it is through port forwarding.

---

## Check the Service

```bash
kubectl get svc -n argocd
```

Example Output

```text
NAME              TYPE        CLUSTER-IP

argocd-server     ClusterIP   10.96.10.20
```

---

## Port Forward

```bash
kubectl port-forward svc/argocd-server \
-n argocd 8080:443
```

---

## Open the Browser

```text
https://localhost:8080
```

Ignore the browser certificate warning because Argo CD uses a self-signed certificate by default.

---

## Internal Workflow

```text
Browser

        │

localhost:8080

        │

Port Forward

        │

argocd-server

        ▼

Argo CD Web UI
```

---

## Production Example

In production environments, organizations typically expose the Argo CD Server using:

- Ingress
- LoadBalancer
- API Gateway

instead of port forwarding.

---

## Production Tip

Use an Ingress with TLS for secure access instead of relying on port forwarding in production.

---

## Interview Question

### Question

How do you access the Argo CD Web UI after installation?

### Answer

Use `kubectl port-forward` to expose the `argocd-server` service locally, or configure an Ingress or LoadBalancer for production access.

---

# 6. Initial Admin Login

## Answer

Argo CD creates a default administrator account during installation.

Username

```text
admin
```

The initial password is stored in a Kubernetes Secret.

---

## Retrieve the Password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d
```

Example Output

```text
X7fK9LmP2Q
```

---

## Login Credentials

```text
Username: admin

Password: <Retrieved Password>
```

---

## Internal Workflow

```text
Kubernetes Secret

        │

Retrieve Password

        │

Login

        ▼

Argo CD Dashboard
```

---

## Production Example

Immediately after installation, a DevOps engineer logs in using the initial credentials and configures authentication before allowing other users access.

---

## Production Tip

Change the default administrator password immediately after the first login.

---

## Interview Question

### Question

How do you retrieve the initial Argo CD admin password?

### Answer

Retrieve it from the `argocd-initial-admin-secret` Kubernetes Secret using `kubectl`.

---

# 7. Installing the Argo CD CLI

## Answer

The Argo CD CLI allows you to manage applications directly from the command line.

It is commonly used for:

- Automation
- CI/CD Pipelines
- Application Management
- Troubleshooting

---

## Verify Installation

```bash
argocd version
```

Example Output

```text
argocd: v2.x.x
```

---

## Login to Argo CD

```bash
argocd login localhost:8080
```

If using the self-signed certificate:

```bash
argocd login localhost:8080 --insecure
```

---

## Verify Connection

```bash
argocd account get-user-info
```

---

## Internal Workflow

```text
CLI

        │

Connect to Server

        │

Authenticate

        │

Execute Commands

        ▼

Manage Applications
```

---

## Production Example

CI/CD pipelines authenticate using the Argo CD CLI before synchronizing applications after a successful build.

---

## Production Tip

Use API tokens or Single Sign-On (SSO) instead of passwords for automated environments.

---

## Interview Question

### Question

Why is the Argo CD CLI used?

### Answer

The Argo CD CLI is used for automation, application management, scripting, and integration with CI/CD pipelines.

---

# 8. Verifying the Installation

## Answer

After installation, verify that every Argo CD component is running correctly.

---

## Check Pods

```bash
kubectl get pods -n argocd
```

Expected Status

```text
Running
```

---

## Check Services

```bash
kubectl get svc -n argocd
```

---

## Check Deployments

```bash
kubectl get deployments -n argocd
```

---

## Verify CLI Connection

```bash
argocd app list
```

If no applications exist, the command returns an empty list without errors.

---

## Internal Workflow

```text
Verify Pods

        │

Verify Services

        │

Verify Deployments

        │

Verify CLI

        ▼

Installation Successful
```

---

## Production Example

Before onboarding application teams, platform engineers verify:

- All Pods are Running.
- The Web UI is accessible.
- CLI authentication works.
- Repository connectivity is functioning.

---

## Production Tip

Always verify the complete installation before creating your first Argo CD Application.

---

## Interview Question

### Question

How do you verify an Argo CD installation?

### Answer

Verify the Pods, Services, Deployments, Web UI access, and CLI connectivity to ensure Argo CD is functioning correctly.

---

# 9. Initial Configuration Tasks

## Answer

Once Argo CD is installed, several configuration steps are recommended before deploying applications.

---

## Recommended Tasks

- Change the default admin password.
- Configure Single Sign-On (SSO).
- Configure RBAC.
- Add Git repositories.
- Configure TLS certificates.
- Configure notifications.
- Enable monitoring.

---

## Production Workflow

```text
Install Argo CD

↓

Verify Installation

↓

Secure Admin Account

↓

Configure Git Repository

↓

Configure RBAC

↓

Deploy Applications
```

---

## Production Example

A production platform team completes all security and access configurations before granting developers access to Argo CD.

---

## Production Tip

Treat Argo CD as a production platform. Secure it before deploying business-critical applications.

---

## Interview Question

### Question

What should you configure immediately after installing Argo CD?

### Answer

Change the default password, configure authentication, set up RBAC, connect Git repositories, and secure the installation before deploying applications.

---

## 📌 Quick Revision

| Task | Command / Purpose |
|------|-------------------|
| Port Forward | `kubectl port-forward svc/argocd-server -n argocd 8080:443` |
| Get Admin Password | Retrieve `argocd-initial-admin-secret` |
| Login CLI | `argocd login` |
| Verify Pods | `kubectl get pods -n argocd` |
| Verify Services | `kubectl get svc -n argocd` |
| Verify Deployments | `kubectl get deployments -n argocd` |

---

# 10. Connecting a Git Repository

## Answer

Argo CD deploys applications by reading Kubernetes manifests, Helm Charts, or Kustomize configurations from a Git repository.

Before creating applications, you must register your Git repository with Argo CD.

---

## Supported Git Providers

- GitHub
- GitLab
- Azure DevOps
- Bitbucket
- AWS CodeCommit

---

## Add a Repository

```bash
argocd repo add https://github.com/example/k8s-manifests.git
```

---

## Private Repository

```bash
argocd repo add https://github.com/example/private-repo.git \
--username <USERNAME> \
--password <TOKEN>
```

---

## Verify Repository

```bash
argocd repo list
```

---

## Internal Workflow

```text
Git Repository

        │

Register Repository

        │

Argo CD Stores Credentials

        │

Reads Kubernetes Manifests

        ▼

Ready for Deployment
```

---

## Production Example

A DevOps team connects an Azure DevOps Git repository containing:

- Helm Charts
- Kubernetes Manifests
- Environment Configurations

Argo CD continuously monitors the repository for changes.

---

## Production Tip

Use Personal Access Tokens (PATs) or SSH keys instead of passwords when connecting private repositories.

---

## Interview Question

### Question

How do you connect a Git repository to Argo CD?

### Answer

Register the repository using the `argocd repo add` command and provide authentication credentials if the repository is private.

---

# 11. Registering Kubernetes Clusters

## Answer

Argo CD can manage one or many Kubernetes clusters.

The cluster where Argo CD is installed is automatically registered.

Additional clusters must be added manually.

---

## List Registered Clusters

```bash
argocd cluster list
```

---

## Add a Cluster

```bash
argocd cluster add <kube-context>
```

---

## Internal Workflow

```text
Argo CD

        │

Register Cluster

        │

Store Cluster Credentials

        │

Deploy Applications

        ▼

Managed Kubernetes Cluster
```

---

## Production Example

One Argo CD instance manages:

```text
Development Cluster

↓

QA Cluster

↓

Staging Cluster

↓

Production Cluster
```

from a single control plane.

---

## Production Tip

Use separate Projects and RBAC policies when managing multiple clusters.

---

## Interview Question

### Question

Can one Argo CD instance manage multiple Kubernetes clusters?

### Answer

Yes. Argo CD supports multi-cluster management by registering additional Kubernetes clusters.

---

# 12. Common Beginner Mistakes

## Mistake 1

❌ Installing Argo CD without verifying cluster health.

Always confirm the cluster is healthy before installation.

---

## Mistake 2

❌ Keeping the default administrator password.

Change it immediately after the first login.

---

## Mistake 3

❌ Exposing the Web UI without TLS.

Always secure production access using HTTPS.

---

## Mistake 4

❌ Using administrator privileges for every user.

Configure RBAC and grant only the required permissions.

---

## Mistake 5

❌ Connecting directly to the production branch without a review process.

Use pull requests and approvals before changes are synchronized.

---

## Production Tip

Treat Argo CD as a critical production platform and secure it accordingly.

---

# 13. Production Best Practices

Always follow these recommendations.

---

✅ Install Argo CD in a dedicated namespace.

---

✅ Protect the Web UI using TLS and authentication.

---

✅ Configure RBAC for all users and teams.

---

✅ Use SSH keys or PATs for Git authentication.

---

✅ Protect production branches with pull requests.

---

✅ Regularly back up Argo CD configuration.

---

✅ Monitor Argo CD components and application health.

---

## Production Workflow

```text
Install Argo CD

↓

Verify Installation

↓

Secure Access

↓

Configure Git

↓

Register Clusters

↓

Configure RBAC

↓

Deploy Applications

↓

Monitor Continuously
```

---

# 📌 Quick Revision

### Installation Workflow

```text
Create Namespace

↓

Install Components

↓

Verify Pods

↓

Access Web UI

↓

Login

↓

Install CLI

↓

Connect Git

↓

Register Clusters

↓

Ready for GitOps
```

---

### Most Common Commands

```bash
kubectl create namespace argocd

kubectl get pods -n argocd

kubectl port-forward svc/argocd-server -n argocd 8080:443

argocd login localhost:8080

argocd repo add

argocd repo list

argocd cluster list

argocd cluster add
```

---

### Core Components

```text
argocd-server

↓

argocd-repo-server

↓

argocd-application-controller

↓

argocd-dex-server

↓

argocd-redis
```

---

# Summary

After completing this guide, you should understand:

✅ Prerequisites

✅ Installation Methods

✅ Installing Argo CD

✅ Component Overview

✅ Accessing the Web UI

✅ Initial Admin Login

✅ Installing the CLI

✅ Verifying Installation

✅ Connecting Git Repositories

✅ Registering Kubernetes Clusters

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What are the prerequisites for Argo CD? | A Kubernetes cluster, kubectl, and administrator access |
| Which namespace is commonly used for Argo CD? | `argocd` |
| How do you access the Web UI locally? | `kubectl port-forward` |
| Where is the initial admin password stored? | `argocd-initial-admin-secret` |
| Which command lists registered repositories? | `argocd repo list` |
| Which command registers a new Git repository? | `argocd repo add` |
| Which command lists managed clusters? | `argocd cluster list` |
| Can Argo CD manage multiple clusters? | Yes |
| Why should RBAC be configured? | To enforce least-privilege access and improve security |
| Why should production branches be protected? | To ensure all deployment changes are reviewed before synchronization |

---

# What's Next?

➡️ **03-applications-projects.md**

In the next guide, you'll learn:

- What is an Argo CD Application?
- Creating Applications
- Declarative vs Imperative Applications
- Projects
- Application Sources
- Destination Clusters & Namespaces
- Labels & Annotations
- Production Best Practices
- Common Interview Questions