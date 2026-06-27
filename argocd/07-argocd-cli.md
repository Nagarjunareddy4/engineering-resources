# 🚀 Argo CD CLI - Command Line Management

This guide explains everything you need to know about the Argo CD Command Line Interface (CLI).

If you've ever wondered:

- What is the Argo CD CLI?
- Why do we need the CLI?
- How do I install the CLI?
- How do I authenticate?
- What are the most commonly used CLI commands?

This guide is for you.

---

# 📑 Table of Contents

1. What is the Argo CD CLI?
2. Why do we need the CLI?
3. Installing the Argo CD CLI
4. Logging in to Argo CD
5. Common Interview Questions

---

# 1. What is the Argo CD CLI?

## Answer

The **Argo CD CLI** is a command-line tool that allows users to interact with an Argo CD server.

It provides a fast and scriptable way to:

- Manage Applications
- Synchronize Deployments
- Monitor Health
- Configure Repositories
- Manage Clusters
- Automate GitOps Operations

Unlike the Web UI, the CLI is ideal for automation and CI/CD pipelines.

---

## CLI Architecture

```text
Administrator

        │

Argo CD CLI

        │

Argo CD API Server

        │

Application Controller

        ▼

Kubernetes Cluster
```

---

## Common Operations

Using the CLI, you can:

- Create Applications
- Delete Applications
- Synchronize Applications
- Roll Back Releases
- View Logs
- Check Health Status

---

## Real World Example

After a successful CI/CD pipeline, an automation script executes:

```bash
argocd app sync shopping-api
```

to deploy the latest application version.

---

## Benefits

The CLI provides:

- Automation
- Faster Operations
- Scriptability
- CI/CD Integration
- Remote Administration

---

## Production Tip

Use the CLI for automated deployment workflows and the Web UI for monitoring and troubleshooting.

---

## Interview Question

### Question

What is the Argo CD CLI?

### Answer

The Argo CD CLI is a command-line tool used to manage Argo CD applications, repositories, clusters, and deployments through the Argo CD API.

---

# 2. Why do we need the CLI?

## Answer

Although the Web UI is user-friendly, many operational tasks require automation.

The CLI enables:

- CI/CD Integration
- Batch Operations
- Scripting
- Remote Administration
- Automated Deployments

---

## Without CLI

```text
Developer

↓

Open Web UI

↓

Click Sync

↓

Deployment Complete
```

---

## With CLI

```text
Developer

↓

CI/CD Pipeline

↓

argocd app sync

↓

Automatic Deployment
```

---

## Production Example

A GitHub Actions workflow performs:

1. Build Application
2. Build Docker Image
3. Push Image
4. Update Git Repository
5. Trigger:

```bash
argocd app sync shopping-api
```

The entire deployment is automated.

---

## Production Tip

Integrate the Argo CD CLI into CI/CD pipelines instead of performing manual deployments.

---

## Interview Question

### Question

Why is the Argo CD CLI important?

### Answer

The CLI enables automation, scripting, and CI/CD integration, making GitOps deployments faster and more reliable.

---

# 3. Installing the Argo CD CLI

## Answer

The Argo CD CLI must be installed on the system from which you want to manage Argo CD.

It can be installed on:

- Linux
- macOS
- Windows

---

## Verify Installation

```bash
argocd version
```

Example Output

```text
argocd: v2.x.x

Server: v2.x.x
```

---

## Verify CLI Availability

```bash
argocd version --client
```

---

## Internal Workflow

```text
Install CLI

        │

Verify Version

        │

Connect to Server

        ▼

Ready for Management
```

---

## Production Example

A build agent in Azure DevOps installs the Argo CD CLI before executing deployment commands.

---

## Production Tip

Keep the CLI version compatible with your Argo CD Server version to avoid unexpected behavior.

---

## Interview Question

### Question

How do you verify that the Argo CD CLI is installed?

### Answer

Run `argocd version` or `argocd version --client` to verify the CLI installation.

---

# 4. Logging in to Argo CD

## Answer

Before using the CLI, you must authenticate with the Argo CD Server.

---

## Login Command

```bash
argocd login <ARGOCD_SERVER>
```

Example

```bash
argocd login localhost:8080
```

---

## Self-Signed Certificate

For development environments:

```bash
argocd login localhost:8080 --insecure
```

---

## Verify Authentication

```bash
argocd account get-user-info
```

Example Output

```text
Logged In User:

admin
```

---

## Internal Workflow

```text
CLI

        │

Connect

        │

Authenticate

        │

Receive Token

        ▼

Execute Commands
```

---

## Production Example

A DevOps engineer authenticates to the production Argo CD server using Single Sign-On (SSO) before performing deployment operations.

---

## Production Tip

Use SSO or API tokens instead of username/password authentication for automation and enterprise deployments.

---

## Interview Question

### Question

How do you log in to Argo CD using the CLI?

### Answer

Use the `argocd login` command with the Argo CD Server address and authenticate using valid credentials, SSO, or an API token.

---

# 5. Managing Applications

## Answer

The Argo CD CLI provides commands to create, list, inspect, update, and delete Applications.

These commands help administrators and DevOps engineers manage GitOps deployments efficiently.

---

## List Applications

```bash
argocd app list
```

Example Output

```text
NAME            STATUS      HEALTH

shopping-api    Synced      Healthy

payment-api     OutOfSync   Healthy

inventory-api   Synced      Progressing
```

---

## Get Application Details

```bash
argocd app get shopping-api
```

Displays:

- Sync Status
- Health Status
- Repository
- Target Revision
- Namespace
- Cluster

---

## Internal Workflow

```text
CLI

        │

Request Application

        │

Argo CD API

        │

Application Information

        ▼

Display Details
```

---

## Production Example

A DevOps engineer checks the status of a production application before performing an upgrade.

---

## Production Tip

Use `argocd app get` to verify the application's health and synchronization status before making changes.

---

## Interview Question

### Question

How do you list all Applications in Argo CD?

### Answer

Use the `argocd app list` command to display all managed Applications.

---

# 6. Synchronizing Applications

## Answer

Synchronization updates the Kubernetes cluster to match the desired state stored in Git.

---

## Synchronize One Application

```bash
argocd app sync shopping-api
```

---

## Synchronize Multiple Applications

```bash
argocd app sync shopping-api payment-api
```

---

## Synchronize All Applications

```bash
argocd app sync --all
```

---

## Internal Workflow

```text
Git Repository

        │

Desired State

        │

Synchronize

        │

Apply Changes

        ▼

Cluster Updated
```

---

## Production Example

After approving a pull request, a pipeline triggers:

```bash
argocd app sync shopping-api
```

to deploy the latest application version.

---

## Production Tip

Always review the pending changes before synchronizing production applications.

---

## Interview Question

### Question

How do you synchronize an Application?

### Answer

Use the `argocd app sync <application-name>` command.

---

# 7. Refreshing Applications

## Answer

Refreshing forces Argo CD to re-evaluate the application's current state.

It does not deploy changes but updates the status information.

---

## Refresh an Application

```bash
argocd app get shopping-api --refresh
```

---

## Hard Refresh

```bash
argocd app get shopping-api --hard-refresh
```

A hard refresh clears cached information and retrieves the latest data from Git.

---

## Internal Workflow

```text
Refresh Request

        │

Read Git

        │

Compare Cluster

        │

Update Status

        ▼

Latest Information
```

---

## Production Example

A developer updates the Git repository.

A refresh immediately updates the displayed synchronization status.

---

## Production Tip

Use a hard refresh if you suspect cached repository information is outdated.

---

## Interview Question

### Question

What is the purpose of refreshing an Application?

### Answer

Refreshing updates the application's synchronization and health information without performing a deployment.

---

# 8. Application History & Rollback

## Answer

Argo CD maintains deployment history, making it easier to review previous revisions and perform rollbacks.

---

## View History

```bash
argocd app history shopping-api
```

Example Output

```text
ID    REVISION      DATE

1     a34bc12       Success

2     b89de21       Success

3     c90fg56       Success
```

---

## Roll Back

```bash
argocd app rollback shopping-api 2
```

This rolls the application back to revision **2**.

---

## Internal Workflow

```text
Application

        │

History

        │

Select Revision

        │

Rollback

        ▼

Previous Version Restored
```

---

## Production Example

A faulty deployment reaches production.

The DevOps team identifies the last stable revision and executes:

```bash
argocd app rollback shopping-api 5
```

to restore the application.

---

## Production Tip

Review deployment history before performing a rollback to ensure the selected revision is stable.

---

## Interview Question

### Question

How do you roll back an Application?

### Answer

Use the `argocd app rollback <application-name> <revision>` command to restore a previous deployment revision.

---

# 9. Deleting Applications

## Answer

Applications that are no longer required can be removed using the CLI.

---

## Delete an Application

```bash
argocd app delete shopping-api
```

You may be prompted to confirm the deletion.

---

## Internal Workflow

```text
Delete Request

        │

Validate

        │

Remove Application

        ▼

Application Deleted
```

---

## Production Example

After decommissioning a legacy service, the corresponding Argo CD Application is deleted.

---

## Production Tip

Verify that an application is no longer needed before deleting it, especially in production environments.

---

## Interview Question

### Question

How do you delete an Argo CD Application?

### Answer

Use the `argocd app delete <application-name>` command.

---

## 📌 Quick Revision

| Command | Purpose |
|----------|---------|
| `argocd app list` | List all Applications |
| `argocd app get` | View Application details |
| `argocd app sync` | Synchronize an Application |
| `argocd app get --refresh` | Refresh Application status |
| `argocd app history` | View deployment history |
| `argocd app rollback` | Roll back to a previous revision |
| `argocd app delete` | Delete an Application |

---

# 10. Repository Management

## Answer

Argo CD connects to Git repositories to retrieve Kubernetes manifests, Helm Charts, and Kustomize configurations.

The CLI provides commands to manage these repositories.

---

## List Repositories

```bash
argocd repo list
```

---

## Add Repository

```bash
argocd repo add https://github.com/example/gitops.git
```

---

## Add Private Repository

```bash
argocd repo add https://github.com/example/private.git \
--username <USERNAME> \
--password <TOKEN>
```

---

## Remove Repository

```bash
argocd repo rm https://github.com/example/gitops.git
```

---

## Internal Workflow

```text
Repository

        │

Register

        │

Authenticate

        │

Store Configuration

        ▼

Ready for GitOps
```

---

## Production Example

A platform team registers a private GitHub repository containing Helm Charts and Kubernetes manifests.

Applications can now synchronize directly from that repository.

---

## Production Tip

Use SSH keys or Personal Access Tokens (PATs) instead of passwords when connecting private repositories.

---

## Interview Question

### Question

How do you list configured Git repositories?

### Answer

Use the `argocd repo list` command.

---

# 11. Cluster Management

## Answer

Argo CD can manage multiple Kubernetes clusters from a single control plane.

The CLI provides commands to register and manage those clusters.

---

## List Clusters

```bash
argocd cluster list
```

---

## Register a Cluster

```bash
argocd cluster add <kube-context>
```

---

## Remove a Cluster

```bash
argocd cluster rm <cluster-name>
```

---

## Internal Workflow

```text
Kubernetes Cluster

        │

Register

        │

Store Credentials

        │

Manage Applications

        ▼

GitOps Deployment
```

---

## Production Example

A company manages:

- Development Cluster
- QA Cluster
- Production Cluster

using a single Argo CD instance.

---

## Production Tip

Use Projects and RBAC to restrict which users can deploy to specific clusters.

---

## Interview Question

### Question

How do you register a Kubernetes cluster in Argo CD?

### Answer

Use the `argocd cluster add <kube-context>` command.

---

# 12. Account Management & Troubleshooting

## Account Management

### View Logged-in User

```bash
argocd account get-user-info
```

---

### Change Password

```bash
argocd account update-password
```

---

## Troubleshooting Commands

### Check Version

```bash
argocd version
```

---

### List Applications

```bash
argocd app list
```

---

### View Application Details

```bash
argocd app get shopping-api
```

---

### Refresh Application

```bash
argocd app get shopping-api --refresh
```

---

### Check Kubernetes Resources

```bash
kubectl get pods -n argocd
```

---

## Internal Workflow

```text
Issue Reported

        │

Check CLI

        │

Verify Server

        │

Inspect Application

        │

Review Logs

        ▼

Resolve Issue
```

---

## Production Example

A deployment fails.

The engineer:

1. Checks application status.
2. Refreshes the application.
3. Reviews Kubernetes Pods.
4. Examines controller logs.
5. Resolves the issue.

---

## Production Tip

Use both the Argo CD CLI and `kubectl` together for effective troubleshooting.

---

## Interview Question

### Question

Which commands are commonly used for Argo CD troubleshooting?

### Answer

Common commands include `argocd app get`, `argocd app list`, `argocd version`, `argocd app get --refresh`, and `kubectl get pods`.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Using the Web UI for repetitive administrative tasks.

Use the CLI for automation and bulk operations.

---

## Mistake 2

❌ Running CLI and Server versions that are significantly different.

Keep the CLI version compatible with the Argo CD Server.

---

## Mistake 3

❌ Storing usernames and passwords in scripts.

Use API tokens or Single Sign-On (SSO).

---

## Mistake 4

❌ Ignoring failed synchronization messages.

Always inspect the application details and controller logs.

---

## Mistake 5

❌ Making production changes directly with `kubectl`.

Always modify Git and let Argo CD synchronize the cluster.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Use the CLI for CI/CD automation.

---

✅ Authenticate using API tokens or SSO.

---

✅ Keep CLI and Server versions compatible.

---

✅ Review application status before synchronizing.

---

✅ Use Projects and RBAC for multi-team environments.

---

✅ Store repository credentials securely.

---

✅ Monitor synchronization and health regularly.

---

## 📌 Quick Revision

### CLI Workflow

```text
Install CLI

↓

Login

↓

Manage Applications

↓

Synchronize

↓

Monitor

↓

Troubleshoot
```

---

### Most Common Commands

```bash
argocd login

argocd app list

argocd app get

argocd app sync

argocd app history

argocd app rollback

argocd repo list

argocd cluster list

argocd account get-user-info
```

---

### CLI Management Areas

```text
Applications

↓

Repositories

↓

Clusters

↓

Accounts

↓

Troubleshooting
```

---

# Summary

After completing this guide, you should understand:

✅ CLI Installation

✅ Authentication

✅ Managing Applications

✅ Synchronization

✅ Refresh Operations

✅ Rollbacks

✅ Repository Management

✅ Cluster Management

✅ Account Management

✅ Troubleshooting Commands

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is the Argo CD CLI? | A command-line tool for managing Argo CD resources and deployments |
| Why is the CLI important? | It enables automation, scripting, and CI/CD integration |
| How do you log in to Argo CD? | `argocd login <server>` |
| How do you list Applications? | `argocd app list` |
| How do you synchronize an Application? | `argocd app sync <application-name>` |
| How do you view deployment history? | `argocd app history <application-name>` |
| How do you roll back an Application? | `argocd app rollback <application-name> <revision>` |
| How do you register a repository? | `argocd repo add <repository-url>` |
| How do you register a cluster? | `argocd cluster add <kube-context>` |
| Why should API tokens be used? | To securely authenticate automation tools and CI/CD pipelines |

---

# 🎉 Argo CD CLI Module Completed

Congratulations! You have now mastered the **Argo CD CLI**, including:

- ✅ CLI Installation
- ✅ Authentication
- ✅ Application Management
- ✅ Synchronization
- ✅ Refresh Operations
- ✅ Rollbacks
- ✅ Repository Management
- ✅ Cluster Management
- ✅ Account Management
- ✅ Troubleshooting
- ✅ Production CLI Best Practices

You are now ready to automate and manage Argo CD deployments efficiently using the command line.

---

# What's Next?

➡️ **08-argocd-best-practices.md**

In the next guide, you'll learn:

- GitOps Repository Structure
- Branching Strategies
- Multi-Environment Deployments
- Security Best Practices
- High Availability (HA)
- Backup & Disaster Recovery
- Monitoring & Alerting
- Performance Optimization
- Common Mistakes to Avoid
- Interview Questions