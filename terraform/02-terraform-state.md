# 🚀 Terraform State - The Ultimate Source of Truth

This guide explains everything you need to know about the Terraform State file.

If you've ever wondered:

- Why Terraform needs a state file
- What happens if you delete it
- How Terraform tracks infrastructure
- Why state files should never be committed to Git

This guide is for you.

---

# 📑 Table of Contents

1. What is Terraform State?
2. Why does Terraform need a State File?
3. Why is the State File called the Source of Truth?
4. What information does the State File store?
5. Where is the State File stored?
6. What happens during every Terraform deployment?
7. Common interview questions

---

# 1. What is Terraform State?

## Answer

Terraform State is a file that stores information about the infrastructure Terraform manages.

Its default name is:

```text
terraform.tfstate
```

Terraform uses this file to understand:

- Which resources already exist
- Which resources need to be created
- Which resources need to be updated
- Which resources need to be destroyed

Without the state file, Terraform has no memory of previously deployed infrastructure.

---

## Why do we need a State File?

Imagine you're managing infrastructure manually.

Every time you deploy new resources, you need to remember:

- Which Virtual Machine already exists?
- Which Storage Account was created?
- Which Network belongs to which VM?
- Which resources should be updated?

Keeping track manually becomes impossible as infrastructure grows.

Terraform solves this problem by maintaining a state file.

Think of it as Terraform's memory.

---

## Real World Example

Suppose your Terraform configuration creates:

- Resource Group
- Virtual Network
- AKS Cluster
- Azure Container Registry

After deployment, Terraform stores information about all these resources inside the state file.

The next time you run:

```bash
terraform plan
```

Terraform compares:

Current State

↓

Desired Configuration

↓

Execution Plan

Instead of recreating everything.

---

## How does it work?

```text
Developer

        │

terraform apply

        │

        ▼

Read Configuration

        │

        ▼

Read terraform.tfstate

        │

        ▼

Compare Infrastructure

        │

        ▼

Generate Execution Plan

        │

        ▼

Apply Changes

        │

        ▼

Update terraform.tfstate
```

---

## Production Tip

Never edit the Terraform State file manually.

Terraform automatically updates the state after every successful deployment.

Manual changes can cause infrastructure drift and unpredictable behavior.

---

## Interview Question

### Question

What is the Terraform State file?

### Answer

Terraform State is a file that stores metadata about infrastructure managed by Terraform.

Terraform uses it to determine the current state of infrastructure and calculate the changes required to reach the desired state.

---

# 2. Why does Terraform need a State File?

## Answer

Terraform needs the state file because cloud providers do not keep track of Terraform resources.

Azure knows that a Virtual Machine exists.

Terraform needs to know:

- Did Terraform create this VM?
- What is its Resource ID?
- What configuration was applied?
- Does it need to be updated?

The state file answers these questions.

---

## Without State

Imagine running:

```bash
terraform apply
```

Terraform creates:

- Resource Group
- Storage Account
- Virtual Network

Now you update only the Storage Account.

Without a state file, Terraform cannot determine:

- Which resources already exist
- Which resources changed
- Which resources are new

It would have no reliable way to calculate the execution plan.

---

## With State

Terraform compares:

```text
Current Infrastructure

        │

terraform.tfstate

        │

Desired Configuration

        │

        ▼

Execution Plan
```

This comparison allows Terraform to make only the necessary changes.

---

## Why is this important?

Instead of recreating all infrastructure every time, Terraform only performs incremental updates.

Benefits include:

- Faster deployments
- Reduced downtime
- Safer infrastructure changes
- Better collaboration
- Accurate change tracking

---

## Production Tip

Always protect your state file.

Losing the state file can make Terraform lose track of your infrastructure, even though the resources still exist in your cloud provider.

---

## Interview Question

### Question

Why is the Terraform State file required?

### Answer

Terraform uses the state file to map configuration to real infrastructure, detect changes, and determine what resources need to be created, updated, or destroyed.

---

# 3. Why is the State File called the Source of Truth?

## Answer

The Terraform State file is called the **Source of Truth** because it represents Terraform's understanding of your infrastructure.

Whenever Terraform runs, it trusts the state file to determine:

- Which resources exist
- Which resources Terraform manages
- Which resources have changed
- Which resources need to be created, updated, or destroyed

Without the state file, Terraform has no reliable way to determine the current state of your infrastructure.

---

## Why is it called the Source of Truth?

Imagine you're managing an Azure environment with:

- Resource Group
- Virtual Network
- AKS Cluster
- Azure Container Registry
- Storage Account

Terraform doesn't ask Azure:

> "What infrastructure should I manage?"

Instead, it asks:

> "What infrastructure have I managed before?"

The answer comes from the **Terraform State file**.

---

## How does Terraform make decisions?

```text
Terraform Configuration
         │
         ▼
Desired Infrastructure
         │
         ▼
Read terraform.tfstate
         │
         ▼
Compare Desired vs Current State
         │
         ▼
Generate Execution Plan
         │
         ▼
Apply Required Changes
```

---

## Example

Current State:

```text
Resource Group
Virtual Network
Storage Account
```

Desired Configuration:

```text
Resource Group
Virtual Network
Storage Account
AKS Cluster
```

Terraform detects:

✅ AKS Cluster is missing

Execution Plan:

```text
+ Create AKS Cluster
```

Only the missing resource is created.

---

## Production Tip

The state file should always represent the latest deployed infrastructure.

Never manually modify cloud resources without updating Terraform, as this creates **configuration drift**.

---

## Interview Question

### Question

Why is the Terraform State file called the Source of Truth?

### Answer

Because Terraform relies on it to understand the current infrastructure and calculate the required changes.

---

# 4. What information does the State File store?

## Answer

The Terraform State file stores metadata about every resource managed by Terraform.

It does **not** store your Terraform code.

Instead, it stores information about the deployed infrastructure.

---

## Information Stored

The state file contains:

- Resource IDs
- Resource Names
- Resource Types
- Provider Information
- Dependencies
- Attribute Values
- Output Values
- Metadata

---

## Example

Suppose Terraform creates an Azure Resource Group.

Terraform stores information similar to:

```text
Resource Type:
azurerm_resource_group

Resource Name:
production-rg

Location:
Central India

Resource ID:
/subscriptions/.../resourceGroups/production-rg
```

Terraform uses this information during future deployments.

---

## What does Terraform NOT store?

Terraform does not store:

- Your `.tf` files
- Comments
- Documentation
- Git history
- README files

Those remain in your source code repository.

---

## Why is this information important?

When you execute:

```bash
terraform plan
```

Terraform compares:

Configuration Files

↓

State File

↓

Cloud Infrastructure

Then calculates exactly what needs to change.

---

## How does it work?

```text
Terraform Code

        │

        ▼

terraform.tfstate

        │

        ▼

Resource Metadata

        │

        ▼

Cloud Resources
```

---

## Production Tip

Treat the state file as sensitive data.

It may contain:

- Resource IDs
- IP Addresses
- DNS Names
- Sensitive Output Values

Always store it securely.

---

## Common Mistakes

❌ Assuming the state file only stores resource names.

❌ Believing Terraform queries every cloud resource during every deployment.

❌ Ignoring sensitive information stored inside the state.

---

## Interview Question

### Question

What information is stored inside the Terraform State file?

### Answer

The state file stores metadata about managed infrastructure, including resource IDs, attributes, dependencies, provider information, and outputs.

---

# 5. Where is the Terraform State File stored?

## Answer

By default, Terraform stores the state file locally in the working directory.

Default file:

```text
terraform.tfstate
```

---

## Local State

```text
terraform-project/

├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfstate
└── terraform.tfstate.backup
```

This is suitable for:

- Learning Terraform
- Personal projects
- Local testing

---

## Why is Local State not recommended for Production?

If multiple engineers work on the same project:

- Everyone has a different state file.
- State can become inconsistent.
- Simultaneous deployments may overwrite each other.
- Collaboration becomes difficult.

---

## Production Approach

Instead of local storage, use a **Remote Backend**.

Examples include:

- Azure Blob Storage
- AWS S3
- Terraform Cloud
- Google Cloud Storage

Remote state enables:

- Collaboration
- State Locking
- Encryption
- Backup
- Versioning

We'll explore Remote Backends in detail in:

**03-terraform-backend.md**

---

## Production Tip

Never use local state for production infrastructure.

Always configure a remote backend before deploying shared environments.

---

## Interview Question

### Question

Where is the Terraform State file stored?

### Answer

By default, Terraform stores the state file locally as `terraform.tfstate`. In production, it should be stored in a secure remote backend such as Azure Blob Storage or Terraform Cloud.

---

# 6. What happens if you accidentally delete the Terraform State file?

## Answer

Deleting the Terraform State file does **not** delete your cloud infrastructure.

Your Azure, AWS, or GCP resources continue to exist.

However, Terraform loses track of those resources.

Think of it like losing the blueprint of a building.

The building still exists, but you no longer know how it was constructed.

---

## Example

Suppose Terraform created:

- Resource Group
- Virtual Network
- AKS Cluster
- Storage Account

Everything is running successfully.

Now someone accidentally deletes:

```text
terraform.tfstate
```

The cloud resources still exist.

But Terraform no longer knows they exist.

---

## What happens internally?

```text
Azure Resources

    │

    ▼

Still Running

    │

    ▼

terraform.tfstate Deleted

    │

    ▼

Terraform Loses Resource Mapping

    │

    ▼

Next terraform plan

    │

    ▼

Terraform Thinks Nothing Exists
```

---

## Possible Problems

Terraform may attempt to:

- Create duplicate resources
- Fail because resources already exist
- Produce unexpected execution plans

---

## Recovery Options

If the state file is lost, you can recover by:

- Restoring a backup
- Recovering the remote backend
- Importing existing resources

Example:

```bash
terraform import
```

---

## Production Tip

Always enable:

- Remote State
- Versioning
- Automatic Backups

Never rely solely on a local state file.

---

## Interview Question

### Question

What happens if the Terraform State file is deleted?

### Answer

The infrastructure remains in the cloud, but Terraform loses track of it. Recovery typically involves restoring the state or importing existing resources.

---

# 7. What is Configuration Drift?

## Answer

Configuration Drift occurs when cloud infrastructure is modified outside Terraform.

This causes the actual infrastructure to differ from the Terraform configuration and state.

---

## Example

Terraform creates:

```text
Virtual Machine

Size:

Standard_B2s
```

Later, someone changes the VM size manually through the Azure Portal.

Terraform configuration still expects:

```text
Standard_B2s
```

Azure now has:

```text
Standard_D4s_v5
```

This difference is called **Configuration Drift**.

---

## How does Drift happen?

```text
Terraform Apply

        │

        ▼

Infrastructure Created

        │

        ▼

Engineer Changes Azure Portal

        │

        ▼

Terraform Configuration ≠ Cloud Infrastructure
```

---

## Why is Drift dangerous?

Configuration Drift can lead to:

- Unexpected deployments
- Infrastructure inconsistencies
- Failed deployments
- Difficult troubleshooting
- Compliance issues

---

## Production Tip

Never modify production infrastructure directly through the cloud portal.

Always make changes through Terraform code.

---

## Interview Question

### Question

What is Configuration Drift?

### Answer

Configuration Drift occurs when infrastructure changes outside Terraform, causing the deployed infrastructure to differ from the Terraform configuration.

---

# 8. What is State Locking?

## Answer

State Locking prevents multiple engineers or automation pipelines from modifying the same Terraform State simultaneously.

Without state locking, concurrent deployments can corrupt the state file.

---

## Example

Imagine two engineers execute:

```bash
terraform apply
```

at the same time.

Without locking:

```text
Engineer A

terraform apply

        │

        ▼

Updates State

        ▲

        │

terraform apply

Engineer B
```

Both engineers attempt to modify the same state file.

This can lead to:

- Corrupted state
- Failed deployments
- Lost infrastructure metadata

---

## With State Locking

```text
Engineer A

terraform apply

        │

        ▼

🔒 Lock State

        │

        ▼

Deployment

        │

        ▼

Unlock State

        │

        ▼

Engineer B Can Continue
```

---

## Where is State Locking available?

State locking is supported by:

- Azure Blob Storage
- Terraform Cloud
- AWS S3 + DynamoDB
- Google Cloud Storage (with supported mechanisms)

---

## Production Tip

Always use a backend that supports state locking for production environments.

---

## Interview Question

### Question

Why is State Locking important?

### Answer

State Locking prevents multiple Terraform operations from modifying the same state simultaneously, protecting the infrastructure state from corruption.

---

# 9. Terraform State Best Practices

## Always Follow These Practices

✅ Store State Remotely

Never use local state for production.

---

✅ Enable Versioning

Always keep previous versions of the state.

---

✅ Enable State Locking

Prevent concurrent deployments.

---

✅ Encrypt State Files

Protect sensitive information stored in the state.

---

✅ Restrict Access

Only authorized engineers should access the state.

---

✅ Never Commit State Files

Add the following to `.gitignore`

```gitignore
terraform.tfstate
terraform.tfstate.*
```

---

## Production Checklist

Before using Terraform in Production, verify:

- Remote Backend Configured
- State Locking Enabled
- Versioning Enabled
- Encryption Enabled
- Backup Enabled
- Least Privilege Access Configured

---

# Summary

After completing this guide, you should understand:

- What Terraform State is
- Why Terraform needs a State file
- Why the State file is the Source of Truth
- What information the State stores
- Where the State file is stored
- What happens if the State file is deleted
- Configuration Drift
- State Locking
- Production Best Practices

---
