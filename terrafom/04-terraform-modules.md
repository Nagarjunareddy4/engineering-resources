# 🚀 Terraform Modules - Build Reusable Infrastructure

This guide explains everything you need to know about Terraform Modules.

If you've ever wondered:

- What is a Terraform Module?
- Why do we need Modules?
- What problems do Modules solve?
- What is the difference between Root Modules and Child Modules?
- How do companies organize Terraform code?

This guide is for you.

---

# 📑 Table of Contents

1. What is a Terraform Module?
2. Why do we need Modules?
3. Root Module vs Child Module
4. Module Architecture
5. Common Interview Questions

---

# 1. What is a Terraform Module?

## Answer

A **Terraform Module** is a collection of Terraform configuration files that work together to perform a specific task.

Instead of writing the same Terraform code repeatedly, you can package it into a module and reuse it wherever needed.

Think of a Module as a reusable building block.

Just like developers create reusable functions in programming languages, DevOps engineers create reusable Modules in Terraform.

---

## Why do we need Modules?

Imagine you're deploying infrastructure for three different environments:

- Development
- QA
- Production

Each environment requires:

- Resource Group
- Virtual Network
- Subnet
- Network Security Group
- Storage Account

Without Modules, you'll end up copying the same Terraform code three times.

This leads to:

- Duplicate code
- Difficult maintenance
- More bugs
- Larger repositories

Modules solve this problem by allowing you to write the infrastructure once and reuse it everywhere.

---

## Without Modules

```text
Development

main.tf
│
├── Resource Group
├── Virtual Network
├── Storage Account
└── Subnet

QA

main.tf
│
├── Resource Group
├── Virtual Network
├── Storage Account
└── Subnet

Production

main.tf
│
├── Resource Group
├── Virtual Network
├── Storage Account
└── Subnet
```

The same code is duplicated multiple times.

---

## With Modules

```text
Infrastructure Module

│

├── Resource Group
├── Virtual Network
├── Storage Account
└── Subnet

        ▲
        │
        │
────────┼────────
        │
        ▼

Development

QA

Production
```

One module can now be reused across multiple environments.

---

## Real World Example

Suppose your organization deploys infrastructure for 20 applications.

Every application needs:

- Resource Group
- Virtual Network
- Storage Account

Instead of writing the same configuration 20 times, the platform team creates a reusable Module.

Each application simply calls that Module with different input values.

This reduces duplication and keeps infrastructure consistent across the organization.

---

## Example

```hcl
module "resource_group" {
  source   = "./modules/resource-group"

  name     = "production-rg"
  location = "Central India"
}
```

Instead of writing the Resource Group configuration again, Terraform loads it from the Module.

---

## Benefits

Using Modules provides:

- Code Reusability
- Consistency
- Easier Maintenance
- Smaller Terraform Files
- Faster Development
- Standardized Infrastructure

---

## Production Tip

If you find yourself copying the same Terraform configuration more than once, it's usually a good candidate for a Module.

Large organizations typically create Modules for common infrastructure such as:

- Virtual Networks
- AKS Clusters
- Storage Accounts
- Virtual Machines
- SQL Databases
- Key Vaults

---

## Interview Question

### Question

What is a Terraform Module?

### Answer

A Terraform Module is a reusable collection of Terraform configuration files that encapsulates infrastructure logic and can be used across multiple projects or environments.

---

# 2. Why do we need Modules?

## Answer

As Terraform projects grow, maintaining everything inside a single `main.tf` file becomes difficult.

Modules help organize infrastructure into smaller, reusable components.

Instead of repeating code, engineers reference existing Modules.

---

## Imagine This Scenario

You have three applications:

```text
Application A

↓

AKS Cluster

↓

Virtual Network

↓

Storage Account
```

```text
Application B

↓

AKS Cluster

↓

Virtual Network

↓

Storage Account
```

```text
Application C

↓

AKS Cluster

↓

Virtual Network

↓

Storage Account
```

Without Modules, every application contains identical Terraform code.

Now imagine updating the Virtual Network configuration.

You must update it in all three applications.

This increases maintenance effort and introduces the risk of inconsistencies.

---

## Module-Based Approach

```text
Application A
        │
        ▼
 Infrastructure Module

Application B
        │
        ▼
 Infrastructure Module

Application C
        │
        ▼
 Infrastructure Module
```

A single change to the Module automatically benefits every application that uses it.

---

## Production Tip

Modules encourage standardization.

Platform Engineering teams often maintain a central repository of approved Modules that development teams consume.

This ensures every project follows the same architecture, security standards, and naming conventions.

---

## Interview Question

### Question

Why do companies use Terraform Modules?

### Answer

Terraform Modules promote code reuse, simplify maintenance, reduce duplication, and help organizations standardize infrastructure deployments across teams and environments.

---

# 3. What is the difference between a Root Module and a Child Module?

## Answer

Every Terraform configuration has at least one **Root Module**.

A **Root Module** is the directory where you execute Terraform commands such as:

```bash
terraform init
terraform plan
terraform apply
```

A **Child Module** is any module that is called from another Terraform configuration using the `module` block.

Think of the Root Module as the **main application**, while Child Modules are **reusable components** that perform specific tasks.

---

## Root Module

The Root Module is the entry point of every Terraform project.

Example:

```text
terraform-project/

│

├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
└── terraform.tfvars
```

Running:

```bash
terraform apply
```

from this directory executes the Root Module.

---

## Child Module

A Child Module is referenced using the `module` block.

Example:

```hcl
module "network" {
  source = "./modules/network"

  resource_group_name = "production-rg"
  location            = "Central India"
}
```

Terraform loads the module from:

```text
modules/

└── network/
```

and executes its configuration.

---

## How does it work?

```text
                Root Module

                     │

        ┌────────────┼────────────┐

        ▼            ▼            ▼

Network Module   AKS Module   Storage Module

        │            │            │

        ▼            ▼            ▼

Azure Resources Created
```

The Root Module coordinates the deployment, while Child Modules create specific infrastructure components.

---

## Real World Example

Imagine an enterprise deploying infrastructure.

Instead of placing everything in one `main.tf` file:

```text
AKS

Networking

Storage

Key Vault

SQL Database

Load Balancer
```

The platform team separates each component into its own Module.

```text
Root Module

│

├── Network Module
├── AKS Module
├── Storage Module
├── SQL Module
└── Key Vault Module
```

This makes the infrastructure easier to understand, test, and maintain.

---

## Production Tip

Keep your Root Module lightweight.

The Root Module should mainly:

- Configure Providers
- Configure Backend
- Define Variables
- Call Child Modules

Business logic should live inside Child Modules.

---

## Interview Question

### Question

What is the difference between a Root Module and a Child Module?

### Answer

The Root Module is the main Terraform configuration where Terraform commands are executed.

A Child Module is a reusable Terraform configuration that is called by another module using the `module` block.

---

# 4. Local Modules vs Remote Modules

## Answer

Terraform supports two ways of using Modules:

- Local Modules
- Remote Modules

The choice depends on how the Module is shared.

---

# Local Modules

A Local Module exists inside the same Terraform project.

Example folder structure:

```text
terraform-project/

│

├── main.tf

├── modules/

│      ├── network/

│      ├── aks/

│      └── storage/
```

Calling the module:

```hcl
module "network" {
  source = "./modules/network"
}
```

---

## Advantages

- Easy to develop
- Simple to understand
- Perfect for learning
- Good for small projects

---

## Limitations

- Difficult to share across repositories
- Duplicate modules across projects
- Harder to maintain in large organizations

---

# Remote Modules

A Remote Module is stored outside the current project.

Examples:

- GitHub Repository
- Azure DevOps Repository
- Terraform Registry
- Private Module Registry

Example:

```hcl
module "network" {
  source = "git::https://github.com/company/terraform-modules.git//network"
}
```

Terraform downloads the Module automatically.

---

## How does it work?

```text
Terraform Project

        │

module "network"

        │

        ▼

GitHub Repository

(terraform-modules)

        │

        ▼

Download Module

        │

        ▼

Deploy Infrastructure
```

---

## Advantages

- Easy to share
- Centralized maintenance
- Version control
- Standardized infrastructure
- Reusable across multiple teams

---

## Real World Example

A Platform Engineering team maintains:

```text
terraform-modules/

│

├── network

├── aks

├── keyvault

├── storage

├── sql

└── monitoring
```

Every application team simply references these Modules instead of creating infrastructure from scratch.

---

## Production Tip

Enterprise organizations almost always use Remote Modules.

This allows multiple teams to consume approved infrastructure templates while maintaining governance and consistency.

---

## Interview Question

### Question

What is the difference between a Local Module and a Remote Module?

### Answer

A Local Module exists inside the same Terraform project, while a Remote Module is stored in an external source such as GitHub or the Terraform Registry and can be shared across multiple projects.

---

# 5. How does Terraform load a Module?

## Answer

When Terraform encounters a `module` block, it locates the module source, downloads it if necessary, validates the configuration, and integrates it into the execution plan.

---

## Internal Workflow

```text
terraform init

        │

        ▼

Read Root Module

        │

        ▼

Locate Module Block

        │

        ▼

Download Module
(Local or Remote)

        │

        ▼

Load Variables

        │

        ▼

Validate Configuration

        │

        ▼

Include in Execution Plan

        │

        ▼

Deploy Infrastructure
```

---

## What happens internally?

1. Terraform reads the Root Module.
2. It identifies all `module` blocks.
3. It downloads Remote Modules if required.
4. It loads Local Modules from the filesystem.
5. Variables are passed into the Module.
6. Outputs are returned to the Root Module.
7. Resources become part of the execution plan.

---

## Production Tip

Terraform downloads Remote Modules only during:

```bash
terraform init
```

If you update the Module source or version, run:

```bash
terraform init -upgrade
```

to download the latest version.

---

## Interview Question

### Question

When does Terraform download Modules?

### Answer

Terraform downloads Remote Modules during `terraform init`. Local Modules are loaded directly from the local filesystem.