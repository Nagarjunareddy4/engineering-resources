# 🚀 Terraform Fundamentals Every DevOps Engineer Should Know

Most engineers know the commands.

The best engineers understand **why** they exist.

This guide answers some of the most common Terraform interview questions that every DevOps engineer should be able to explain confidently.

---

## 1. What command initializes your Terraform working directory?

### Answer

Use:

```bash
terraform init
```

### What it does

- Downloads required provider plugins
- Initializes the configured backend
- Downloads child modules
- Creates the `.terraform` directory
- Prepares the working directory for Terraform operations

### Example

```bash
terraform init
```

---

## 2. How do you pass dynamic values into your Terraform code?

### Answer

Terraform uses **variables** to make infrastructure reusable.

### Example

```hcl
variable "location" {
  type    = string
  default = "Central India"
}

resource "azurerm_resource_group" "rg" {
  name     = "demo-rg"
  location = var.location
}
```

### Values can be passed using

- `terraform.tfvars`
- `*.auto.tfvars`
- Environment Variables (`TF_VAR_*`)
- `-var`
- `-var-file`

---

## 3. How can you preview infrastructure changes before applying them?

### Answer

Use:

```bash
terraform plan
```

### What it does

Terraform compares:

- Current State
- Desired Configuration

Then shows:

- Resources to Create
- Resources to Modify
- Resources to Destroy

without making any changes.

---

## 4. What is the ultimate source of truth for your infrastructure?

### Answer

The **Terraform State File**.

```
terraform.tfstate
```

### Why is it important?

The state file stores:

- Resource IDs
- Current infrastructure state
- Metadata
- Dependency information

Terraform uses it to determine what needs to change.

---

## 5. How do you prevent two engineers from deploying infrastructure simultaneously?

### Answer

Use **Remote State + State Locking**.

Examples:

- Azure Blob Storage (Lease Lock)
- AWS S3 + DynamoDB
- Terraform Cloud

State locking ensures only one Terraform operation runs at a time.

---

## 6. How do you reuse the same Terraform code for Dev, QA, and Production?

### Answer

Use:

- Modules
- Variables
- Backend Configuration
- Workspaces (where appropriate)

This allows the same codebase to deploy different environments.

---

## 7. How do you retrieve the public IP after deployment?

### Answer

Use **Outputs**.

Example:

```hcl
output "public_ip" {
  value = azurerm_public_ip.example.ip_address
}
```

Retrieve it using:

```bash
terraform output
```

---

## 8. Should you store secrets in the Terraform state file?

### Answer

**No.**

Even if variables are marked as sensitive, Terraform may still store their values in the state file.

Instead, use:

- Azure Key Vault
- AWS Secrets Manager
- HashiCorp Vault

Always encrypt and secure your remote state.

---

## 9. What happens if you accidentally delete the Terraform state file?

### Answer

Terraform loses track of the infrastructure it manages.

The cloud resources still exist, but Terraform no longer knows about them.

Possible recovery options:

- Restore from backup
- Recover remote state
- Re-import resources

Example:

```bash
terraform import
```

---

# 💡 Quick Interview Tips

✔ Always use a remote backend in production.

✔ Never commit `.tfstate` files to Git.

✔ Always run `terraform plan` before `terraform apply`.

✔ Store secrets in a secret manager, not in Terraform code.

✔ Reuse infrastructure with modules.

---

## 📚 Want to Learn More?

If you found this guide helpful, consider exploring:

- Terraform Modules
- Remote State Management
- State Locking
- Terraform Workspaces
- Infrastructure as Code Best Practices

---

## 👋 Connect With Me

**Nagarjuna Reddy**

Helping engineers understand how real production systems work.

🔗 GitHub: https://github.com/Nagarjunareddy4

🌐 Portfolio: https://www.nagarjunareddy.in