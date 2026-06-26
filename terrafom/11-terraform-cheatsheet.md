# 🚀 Terraform Cheatsheet - Quick Reference Guide

This cheatsheet is designed for quick revision before interviews, certifications, or production deployments.

It contains the most frequently used Terraform commands, workflows, and concepts in one place.

---

# 📑 Table of Contents

1. Terraform Workflow
2. Essential Commands
3. Project Structure
4. Terraform File Types
5. Quick Reference

---

# 1. Terraform Workflow

The recommended Terraform workflow is:

```text
Write Terraform Code

        │

        ▼

terraform init

        │

        ▼

terraform fmt

        │

        ▼

terraform validate

        │

        ▼

terraform plan

        │

        ▼

Review Changes

        │

        ▼

terraform apply

        │

        ▼

Infrastructure Created
```

---

## Workflow Commands

| Command | Purpose |
|----------|---------|
| `terraform init` | Initialize the project |
| `terraform fmt` | Format Terraform code |
| `terraform validate` | Validate configuration |
| `terraform plan` | Preview infrastructure changes |
| `terraform apply` | Deploy infrastructure |
| `terraform destroy` | Delete infrastructure |

---

# 2. Essential Terraform Commands

## Initialize Project

```bash
terraform init
```

---

## Format Code

```bash
terraform fmt
```

---

## Validate Configuration

```bash
terraform validate
```

---

## Preview Changes

```bash
terraform plan
```

---

## Deploy Infrastructure

```bash
terraform apply
```

---

## Auto Approve Deployment

```bash
terraform apply -auto-approve
```

---

## Destroy Infrastructure

```bash
terraform destroy
```

---

## Destroy Without Confirmation

```bash
terraform destroy -auto-approve
```

---

## Show Current State

```bash
terraform show
```

---

## List Outputs

```bash
terraform output
```

---

## Refresh Infrastructure State

```bash
terraform refresh
```

---

## Import Existing Resource

```bash
terraform import
```

---

# 3. Recommended Project Structure

```text
terraform-project/

│

├── backend.tf

├── providers.tf

├── versions.tf

├── variables.tf

├── terraform.tfvars

├── locals.tf

├── outputs.tf

├── main.tf

├── modules/

└── environments/
```

---

# 4. Terraform File Types

| File | Purpose |
|------|---------|
| `main.tf` | Main infrastructure resources |
| `variables.tf` | Variable declarations |
| `terraform.tfvars` | Variable values |
| `outputs.tf` | Output values |
| `locals.tf` | Local variables |
| `providers.tf` | Provider configuration |
| `backend.tf` | Backend configuration |
| `versions.tf` | Terraform and provider versions |

---

# 5. Quick Reference

## Infrastructure as Code

```text
Terraform
        │
        ▼
Infrastructure as Code
        │
        ▼
Cloud Infrastructure
```

---

## Terraform Execution Order

```text
Configuration

        │

Variables

        │

Providers

        │

State

        │

Execution Plan

        │

Cloud Provider

        │

Infrastructure
```

---

## Most Important Commands

| Command | Remember |
|----------|----------|
| `terraform init` | First command in every project |
| `terraform fmt` | Format code |
| `terraform validate` | Validate syntax |
| `terraform plan` | Preview changes |
| `terraform apply` | Deploy changes |
| `terraform destroy` | Remove infrastructure |

---

## Interview Quick Notes

✅ Terraform is Declarative.

✅ Terraform State is the Source of Truth.

✅ Providers communicate with Cloud APIs.

✅ Modules provide reusable infrastructure.

✅ Variables provide dynamic configuration.

✅ Workspaces manage multiple State files.

---

# 6. Terraform State Commands

## List Resources in State

```bash
terraform state list
```

---

## Show Resource Details

```bash
terraform state show <resource_name>
```

Example:

```bash
terraform state show azurerm_resource_group.rg
```

---

## Move a Resource in State

```bash
terraform state mv <source> <destination>
```

---

## Remove a Resource from State

```bash
terraform state rm <resource_name>
```

---

## Pull Remote State

```bash
terraform state pull
```

---

## Push State

```bash
terraform state push terraform.tfstate
```

---

## State Quick Reference

| Command | Purpose |
|----------|---------|
| `state list` | List all managed resources |
| `state show` | Display resource details |
| `state mv` | Rename or move resources |
| `state rm` | Remove resource from State |
| `state pull` | Download remote State |
| `state push` | Upload State |

---

# 7. Workspace Commands

## List Workspaces

```bash
terraform workspace list
```

---

## Show Current Workspace

```bash
terraform workspace show
```

---

## Create Workspace

```bash
terraform workspace new dev
```

---

## Switch Workspace

```bash
terraform workspace select prod
```

---

## Delete Workspace

```bash
terraform workspace delete dev
```

---

## Workspace Quick Reference

| Command | Purpose |
|----------|---------|
| `workspace list` | Show all Workspaces |
| `workspace show` | Show active Workspace |
| `workspace new` | Create a Workspace |
| `workspace select` | Switch Workspace |
| `workspace delete` | Delete Workspace |

---

# 8. Variable Quick Reference

## Declare Variable

```hcl
variable "location" {
  type = string
}
```

---

## Access Variable

```hcl
var.location
```

---

## Default Value

```hcl
default = "Central India"
```

---

## Sensitive Variable

```hcl
sensitive = true
```

---

## Variable Validation

```hcl
validation {
  condition = ...
}
```

---

## Variable Files

```text
terraform.tfvars

dev.tfvars

qa.tfvars

prod.tfvars
```

---

## Pass Variables

```bash
terraform apply -var="location=Central India"
```

---

## Custom Variable File

```bash
terraform apply -var-file="prod.tfvars"
```

---

# 9. Module Quick Reference

## Local Module

```hcl
module "network" {

  source = "./modules/network"

}
```

---

## GitHub Module

```hcl
module "network" {

  source = "git::https://github.com/company/modules.git//network"

}
```

---

## Module Structure

```text
modules/

├── network/

├── storage/

├── aks/

└── keyvault/
```

---

# 10. Lifecycle Quick Reference

## Create Before Destroy

```hcl
create_before_destroy = true
```

---

## Prevent Destroy

```hcl
prevent_destroy = true
```

---

## Ignore Changes

```hcl
ignore_changes = [
  tags
]
```

---

## Replace Trigger

```hcl
replace_triggered_by = [
  azurerm_storage_account.storage
]
```

---

## Lifecycle Summary

| Meta-Argument | Purpose |
|--------------|---------|
| `create_before_destroy` | Reduce downtime |
| `prevent_destroy` | Protect resources |
| `ignore_changes` | Ignore external updates |
| `replace_triggered_by` | Force replacement |

---

# 11. Variable Precedence

Terraform follows this order when multiple values are provided for the same variable.

```text
Highest Priority

CLI (-var)

        │

CLI (-var-file)

        │

*.auto.tfvars

        │

terraform.tfvars

        │

Environment Variables

        │

Default Value

Lowest Priority
```

---

# 12. Provider Block

```hcl
provider "azurerm" {

  features {}

}
```

---

# 13. Backend Block

```hcl
terraform {

  backend "azurerm" {

    resource_group_name  = "terraform-rg"

    storage_account_name = "tfstate123"

    container_name       = "tfstate"

    key                  = "prod.tfstate"

  }

}
```

---

# 14. Most Used Terraform Functions

## String Functions

| Function | Example |
|----------|---------|
| `upper()` | `upper("devops")` |
| `lower()` | `lower("DEVOPS")` |
| `title()` | `title("terraform guide")` |
| `replace()` | `replace("dev","dev","prod")` |
| `split()` | `split("-", "dev-web-rg")` |
| `join()` | `join("-", ["dev","web","rg"])` |

---

## Collection Functions

| Function | Example |
|----------|---------|
| `length()` | `length(var.subnets)` |
| `contains()` | `contains(["dev","qa"], "dev")` |
| `lookup()` | `lookup(var.vm_sizes, "prod")` |
| `merge()` | `merge(local.tags, var.tags)` |
| `concat()` | `concat(var.list1, var.list2)` |
| `distinct()` | `distinct(var.environments)` |

---

## File Functions

| Function | Example |
|----------|---------|
| `file()` | `file("startup.sh")` |
| `templatefile()` | `templatefile("config.tpl", {})` |

---

## Encoding Functions

| Function | Example |
|----------|---------|
| `jsonencode()` | Convert object to JSON |
| `jsondecode()` | Parse JSON |
| `base64encode()` | Encode text |
| `base64decode()` | Decode Base64 |

---

## Date & Time Functions

| Function | Example |
|----------|---------|
| `timestamp()` | Current UTC time |
| `formatdate()` | Format date |
| `timeadd()` | Add duration |

---

# 15. Production Best Practices

Always follow these recommendations.

✅ Store Terraform State in a Remote Backend.

---

✅ Enable State Locking.

---

✅ Enable State Versioning.

---

✅ Use reusable Modules.

---

✅ Use Variables instead of hardcoding values.

---

✅ Store secrets in Azure Key Vault or another Secret Manager.

---

✅ Review every execution plan.

```bash
terraform plan
```

---

✅ Deploy using CI/CD pipelines.

---

✅ Protect the Production branch.

---

✅ Lock Provider versions.

---

# 16. Common Troubleshooting Commands

## Validate Configuration

```bash
terraform validate
```

---

## Format Code

```bash
terraform fmt
```

---

## Show Current State

```bash
terraform show
```

---

## Show Outputs

```bash
terraform output
```

---

## Refresh State

```bash
terraform refresh
```

---

## Reinitialize Terraform

```bash
terraform init -reconfigure
```

---

## Upgrade Providers

```bash
terraform init -upgrade
```

---

# 17. Rapid Fire Interview Revision

| Question | Answer |
|----------|--------|
| Infrastructure as Code Tool? | Terraform |
| Source of Truth? | Terraform State |
| Initialize Project? | `terraform init` |
| Preview Changes? | `terraform plan` |
| Apply Changes? | `terraform apply` |
| Delete Infrastructure? | `terraform destroy` |
| Reusable Code? | Module |
| Dynamic Input? | Variable |
| Multiple State Files? | Workspace |
| Built-in Helper? | Function |
| Protect Resource? | `prevent_destroy` |
| Ignore Attribute Changes? | `ignore_changes` |
| Reduce Downtime? | `create_before_destroy` |
| Shared State? | Remote Backend |
| Provider Purpose? | Communicate with Cloud APIs |
| Validate Code? | `terraform validate` |
| Format Code? | `terraform fmt` |
| Store Secrets? | Azure Key Vault / Secret Manager |
| Team Collaboration? | Remote Backend + Git + CI/CD |
| Production Deployment? | CI/CD Pipeline |

---

# 18. Production Deployment Checklist

Before every deployment, verify:

| Checklist | Status |
|-----------|--------|
| Terraform Initialized | ✅ |
| Code Formatted | ✅ |
| Configuration Validated | ✅ |
| Variables Verified | ✅ |
| Backend Configured | ✅ |
| Remote State Enabled | ✅ |
| State Locking Enabled | ✅ |
| Secrets Stored Securely | ✅ |
| Execution Plan Reviewed | ✅ |
| Pull Request Approved | ✅ |
| CI/CD Pipeline Ready | ✅ |

---

# 19. 5-Minute Revision

## Terraform Workflow

```text
init

↓

fmt

↓

validate

↓

plan

↓

apply

↓

destroy
```

---

## Most Important Files

```text
main.tf

variables.tf

terraform.tfvars

providers.tf

backend.tf

outputs.tf

locals.tf

versions.tf
```

---

## Most Important Concepts

- Infrastructure as Code
- Providers
- Terraform State
- Remote Backend
- Modules
- Variables
- Workspaces
- Functions
- Lifecycle
- CI/CD

---

## Most Important Commands

```bash
terraform init

terraform fmt

terraform validate

terraform plan

terraform apply

terraform destroy
```

---

# 🎯 Final Tips

Remember these golden rules:

1. Never hardcode secrets.
2. Always use a Remote Backend in production.
3. Review `terraform plan` before applying changes.
4. Keep your code modular and reusable.
5. Use Variables instead of hardcoded values.
6. Store Terraform code in Git.
7. Automate deployments using CI/CD.
8. Lock Provider versions.
9. Secure your Terraform State.
10. Test changes in Development before Production.

---

# 🎉 Congratulations!

You have completed the complete Terraform learning roadmap.

You now understand:

- ✅ Terraform Fundamentals
- ✅ State Management
- ✅ Remote Backends
- ✅ Modules
- ✅ Workspaces
- ✅ Variables
- ✅ Functions
- ✅ Lifecycle Meta-Arguments
- ✅ Production Best Practices
- ✅ Interview Preparation
- ✅ Production Cheat Sheet

You are now well-prepared to build real-world Infrastructure as Code solutions, contribute to enterprise Terraform projects, and confidently answer Terraform interview questions.