# 🚀 Terraform Workspaces - Managing Multiple Environments

This guide explains everything you need to know about Terraform Workspaces.

If you've ever wondered:

- What are Terraform Workspaces?
- Why do we need Workspaces?
- How do Workspaces work?
- What is the Default Workspace?
- How can the same Terraform code deploy multiple environments?

This guide is for you.

---

# 📑 Table of Contents

1. What are Terraform Workspaces?
2. Why do we need Workspaces?
3. How do Terraform Workspaces work?
4. What is the Default Workspace?
5. Common Interview Questions

---

# 1. What are Terraform Workspaces?

## Answer

A **Terraform Workspace** allows you to manage multiple independent State files using the same Terraform configuration.

Each workspace maintains its own State.

This means the same Terraform code can deploy completely different infrastructure for different environments.

Think of a Workspace as a separate environment that shares the same Terraform code but has its own infrastructure.

---

## Why do we need Workspaces?

Imagine your company has three environments.

- Development
- QA
- Production

Each environment requires:

- Resource Group
- Virtual Network
- Storage Account
- AKS Cluster

The infrastructure is almost identical.

The only differences are:

- Resource Names
- VM Sizes
- Number of Nodes
- Environment Variables

Without Workspaces, you might create three separate Terraform projects.

```text
terraform-dev/

terraform-qa/

terraform-prod/
```

Now imagine updating the Virtual Network.

You must update all three projects.

This quickly becomes difficult to maintain.

---

## With Workspaces

Instead of maintaining three projects:

```text
Terraform Project

        │

────────┼────────

        │

        ▼

Development Workspace

QA Workspace

Production Workspace
```

The same Terraform code is reused.

Each Workspace maintains its own State file.

---

## Real World Example

Suppose your Terraform code creates:

- Resource Group
- Virtual Network
- AKS Cluster

When using the **Development** workspace:

```text
development-rg

development-vnet

development-aks
```

When switching to the **Production** workspace:

```text
production-rg

production-vnet

production-aks
```

The code remains exactly the same.

Only the Workspace (and input values) change.

---

## Benefits

Terraform Workspaces provide:

- Environment Isolation
- Separate State Files
- Code Reusability
- Easier Testing
- Reduced Code Duplication

---

## Production Tip

Workspaces are useful for managing similar environments using the same infrastructure code.

However, many organizations still prefer separate state files and repositories for critical production environments to provide stronger isolation and governance.

We'll discuss this trade-off later in this guide.

---

## Interview Question

### Question

What is a Terraform Workspace?

### Answer

A Terraform Workspace is a feature that allows multiple State files to be managed from the same Terraform configuration, enabling the deployment of multiple environments using shared code.

---

# 2. Why do we need Workspaces?

## Answer

Without Workspaces, engineers often duplicate Terraform code for every environment.

For example:

```text
Development

↓

main.tf

↓

providers.tf

↓

variables.tf
```

```text
QA

↓

main.tf

↓

providers.tf

↓

variables.tf
```

```text
Production

↓

main.tf

↓

providers.tf

↓

variables.tf
```

Every project contains nearly identical files.

Updating infrastructure means making the same change multiple times.

---

## Workspace-Based Approach

Using Workspaces:

```text
Single Terraform Project

        │

terraform workspace

        │

────────┼────────

        │

Development

QA

Production
```

Terraform switches the active Workspace instead of maintaining multiple copies of the code.

---

## Why is this important?

Using Workspaces provides:

- Better organization
- Less duplicated code
- Easier maintenance
- Faster deployments
- Consistent infrastructure

---

## Production Example

Suppose a company deploys the same application to:

- Dev
- QA
- UAT

The infrastructure is identical except for:

- Resource names
- Node count
- Instance sizes

Instead of maintaining three repositories, the team uses one Terraform project with three Workspaces.

---

## Production Tip

Workspaces work best when environments are almost identical.

If Production has a completely different architecture from Development, separate Terraform projects or separate State files are usually a better choice.

---

## Interview Question

### Question

Why do we need Terraform Workspaces?

### Answer

Terraform Workspaces allow engineers to reuse the same Terraform configuration across multiple environments while maintaining separate State files for each environment.

---

# 3. How do Terraform Workspaces work?

## Answer

Terraform Workspaces work by maintaining a separate **Terraform State file** for each workspace.

Although all workspaces share the same Terraform configuration, each workspace manages its own infrastructure independently.

Think of a Workspace as an isolated environment with its own State.

This means:

- Development resources are tracked separately.
- QA resources are tracked separately.
- Production resources are tracked separately.

---

## How does it work?

Imagine you have only one Terraform project.

```text
terraform-project/

│

├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
└── terraform.tfvars
```

Now you create three workspaces:

- dev
- qa
- prod

Terraform internally maintains three different State files.

```text
Terraform Configuration

        │

        ▼

───────────────

dev

terraform.tfstate

───────────────

qa

terraform.tfstate

───────────────

prod

terraform.tfstate
```

Although the code is identical, every workspace tracks different infrastructure.

---

## Internal Workflow

```text
terraform workspace select dev

        │

        ▼

Load Dev State

        │

        ▼

terraform plan

        │

        ▼

terraform apply

        │

        ▼

Update Dev State
```

Now switch to Production.

```text
terraform workspace select prod

        │

        ▼

Load Production State

        │

        ▼

terraform plan

        │

        ▼

terraform apply

        │

        ▼

Update Production State
```

Notice that Terraform changes only the active workspace.

The Terraform code remains exactly the same.

---

## Real World Example

Suppose your Terraform code creates an Azure Resource Group.

When using the **dev** workspace:

```text
development-rg
```

When using the **qa** workspace:

```text
qa-rg
```

When using the **prod** workspace:

```text
production-rg
```

The infrastructure changes because the active workspace changes.

---

## Production Tip

Always verify the current workspace before running:

```bash
terraform apply
```

Deploying to the wrong workspace can accidentally modify the wrong environment.

---

## Interview Question

### Question

How do Terraform Workspaces work internally?

### Answer

Terraform maintains a separate State file for every workspace. When you switch workspaces, Terraform loads the corresponding State file while using the same Terraform configuration.

---

# 4. What is the Default Workspace?

## Answer

When you initialize a Terraform project for the first time, Terraform automatically creates a workspace named:

```text
default
```

Every Terraform project starts in this workspace.

If you don't create additional workspaces, all infrastructure will be managed inside the **default** workspace.

---

## How does it work?

```text
terraform init

        │

        ▼

Workspace Created

        │

        ▼

default
```

Terraform automatically uses the **default** workspace until you explicitly switch to another workspace.

---

## Check the Current Workspace

Use the following command:

```bash
terraform workspace show
```

Example Output:

```text
default
```

---

## List Available Workspaces

```bash
terraform workspace list
```

Example Output:

```text
* default
  dev
  qa
  prod
```

The `*` symbol indicates the currently active workspace.

---

## Why is the Default Workspace important?

The default workspace provides a starting point for every Terraform project.

For small learning projects, using the default workspace is perfectly acceptable.

For larger projects, engineers usually create dedicated workspaces for different environments.

---

## Production Tip

Avoid using the **default** workspace for production infrastructure.

Instead, create dedicated workspaces such as:

```text
dev

qa

uat

prod
```

This makes deployments safer and easier to manage.

---

## Interview Question

### Question

What is the Default Workspace in Terraform?

### Answer

The default workspace is the initial workspace automatically created by Terraform when a project is initialized. It is used unless another workspace is explicitly selected.

---

# 5. Creating and Switching Workspaces

## Create a New Workspace

Use:

```bash
terraform workspace new dev
```

Example Output:

```text
Created and switched to workspace "dev".
```

Terraform automatically:

- Creates a new State
- Switches to the new workspace
- Keeps the existing Terraform code unchanged

---

## Switch to Another Workspace

Use:

```bash
terraform workspace select prod
```

Terraform now loads the Production State.

---

## Verify Current Workspace

```bash
terraform workspace show
```

Example:

```text
prod
```

---

## List All Workspaces

```bash
terraform workspace list
```

Example:

```text
default
dev
qa
* prod
```

---

## Internal Workflow

```text
terraform workspace select prod

        │

        ▼

Locate Production State

        │

        ▼

Load Production State

        │

        ▼

terraform plan

        │

        ▼

terraform apply
```

---

## Production Tip

Before every deployment, verify the active workspace:

```bash
terraform workspace show
```

Many production deployment mistakes happen because engineers deploy from the wrong workspace.

---

## Interview Question

### Question

Which commands are used to manage Terraform Workspaces?

### Answer

Common commands include:

- `terraform workspace list`
- `terraform workspace show`
- `terraform workspace new`
- `terraform workspace select`
- `terraform workspace delete`

---

# 6. How do you delete a Terraform Workspace?

## Answer

Terraform allows you to delete workspaces that are no longer required.

However, you **cannot delete the workspace that is currently active**.

Before deleting a workspace, switch to another workspace.

---

## Delete a Workspace

```bash
terraform workspace delete dev
```

If the workspace still contains managed infrastructure, Terraform displays a warning.

---

## Safe Workflow

```text
Current Workspace

dev

        │

        ▼

Switch Workspace

terraform workspace select default

        │

        ▼

Delete Workspace

terraform workspace delete dev
```

---

## Production Tip

Never delete a workspace unless you are certain the infrastructure is no longer required.

Deleting a workspace does **not automatically destroy cloud resources**.

Always verify what infrastructure is associated with the workspace before removing it.

---

## Interview Question

### Question

Can you delete the currently active Terraform Workspace?

### Answer

No.

Terraform requires you to switch to another workspace before deleting the current one.

---

# 7. Terraform Workspaces vs Separate State Files

## Answer

One of the most common questions in Terraform interviews is:

> Should I use Workspaces or separate State files?

The answer depends on the complexity of your infrastructure.

---

## Terraform Workspaces

Best suited for environments that are **almost identical**.

Example:

```text
Development

↓

QA

↓

Production
```

The infrastructure remains the same.

Only values such as:

- Resource Names
- VM Size
- Number of Instances

change.

---

## Separate State Files

Best suited when each environment has significantly different infrastructure.

Example:

```text
Development

↓

1 Virtual Machine

↓

Small Database
```

Production:

```text
10 Virtual Machines

↓

AKS Cluster

↓

Load Balancer

↓

Key Vault

↓

Application Gateway
```

Here, Workspaces become difficult to manage.

Separate State files provide better isolation.

---

## Comparison

| Feature | Workspaces | Separate State Files |
|----------|------------|----------------------|
| Same Infrastructure | ✅ | ✅ |
| Completely Different Infrastructure | ❌ | ✅ |
| Simple Projects | ✅ | ❌ |
| Enterprise Projects | ⚠️ Sometimes | ✅ |
| Isolation | Medium | High |
| Flexibility | Medium | High |

---

## Production Tip

Most enterprise organizations prefer:

- Separate State files
- Separate Backend configurations
- Separate CI/CD pipelines

for Production environments.

Workspaces are commonly used for:

- Development
- QA
- Testing
- Sandboxes

---

## Interview Question

### Question

When should you use Workspaces instead of Separate State files?

### Answer

Use Workspaces when environments are nearly identical and only configuration values differ.

Use Separate State files when environments have different architectures or require stronger isolation.

---

# 8. Common Mistakes

## Mistake 1

❌ Forgetting to check the active Workspace.

Always verify before deployment:

```bash
terraform workspace show
```

---

## Mistake 2

❌ Assuming Workspaces create separate Terraform code.

They don't.

Workspaces only create separate State files.

The Terraform configuration remains the same.

---

## Mistake 3

❌ Using Workspaces for completely different infrastructures.

Example:

Development:

```text
Virtual Machine
```

Production:

```text
AKS

Application Gateway

Key Vault

Redis

SQL Database
```

These environments should use separate Terraform projects or separate State files.

---

## Mistake 4

❌ Sharing one Workspace across multiple teams.

Each environment should have a clearly defined deployment strategy.

---

## Mistake 5

❌ Deploying without checking the current Workspace.

This is one of the most common causes of accidental Production deployments.

---

# 9. Best Practices

Always follow these recommendations.

✅ Name Workspaces clearly.

Example:

```text
dev

qa

uat

prod
```

---

✅ Verify the active Workspace before every deployment.

```bash
terraform workspace show
```

---

✅ Keep infrastructure consistent across Workspaces.

---

✅ Use Variables to customize environments.

---

✅ Store State in a Remote Backend.

---

✅ Protect Production deployments using CI/CD approvals.

---

# Summary

After completing this guide, you should understand:

✅ What Terraform Workspaces are

✅ Why Workspaces exist

✅ How Workspaces work internally

✅ Default Workspace

✅ Creating Workspaces

✅ Switching Workspaces

✅ Deleting Workspaces

✅ Workspaces vs Separate State files

✅ Production Best Practices

---

# What's Next?

➡️ **06-terraform-functions.md**

In the next guide, you'll learn:

- What are Terraform Functions?
- Built-in Functions
- String Functions
- Numeric Functions
- Collection Functions
- File Functions
- Encoding Functions
- Date & Time Functions
- Production Examples
- Interview Questions