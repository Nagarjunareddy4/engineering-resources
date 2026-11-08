# 🚀 Terraform Variables - Build Flexible and Reusable Infrastructure

This guide explains everything you need to know about Terraform Variables.

If you've ever wondered:

- What are Terraform Variables?
- Why do we need Variables?
- How do Variables work?
- What are different Variable types?
- How do companies use Variables in Production?

This guide is for you.

---

# 📑 Table of Contents

1. What are Terraform Variables?
2. Why do we need Variables?
3. Variable Types
4. Variable Declaration
5. Common Interview Questions

---

# 1. What are Terraform Variables?

## Answer

Terraform Variables allow you to make your infrastructure flexible and reusable.

Instead of hardcoding values directly into your Terraform configuration, you can define variables and provide different values whenever needed.

Think of Variables as placeholders.

Rather than writing fixed values, you tell Terraform:

> "I'll provide this value later."

---

## Why do we need Variables?

Imagine you're creating an Azure Resource Group.

Without Variables:

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "production-rg"
  location = "Central India"
}
```

Now your manager asks you to deploy the same infrastructure to:

- Development
- QA
- Production

Without Variables, you'll have to edit the code every time.

---

## With Variables

Instead of hardcoding values:

```hcl
variable "resource_group_name" {
  type = string
}

variable "location" {
  type = string
}

resource "azurerm_resource_group" "rg" {
  name     = var.resource_group_name
  location = var.location
}
```

Now the same code can deploy multiple environments by simply changing the variable values.

---

## How does it work?

```text
Terraform Configuration

        │

Uses Variables

        │

        ▼

Variable Values

        │

        ▼

Terraform Resources

        │

        ▼

Azure Infrastructure
```

Terraform replaces the variable with the provided value before creating the infrastructure.

---

## Real World Example

Suppose your organization has three environments.

Development

```text
Resource Group

development-rg
```

QA

```text
Resource Group

qa-rg
```

Production

```text
Resource Group

production-rg
```

Instead of writing three Terraform projects, you write one project and provide different variable values.

---

## Benefits

Variables provide:

- Code Reusability
- Flexibility
- Cleaner Configuration
- Environment-specific Deployments
- Easier Maintenance

---

## Production Tip

Never hardcode environment-specific values.

Use Variables for:

- Resource Names
- Regions
- VM Sizes
- Tags
- Environment Names

This makes Terraform code reusable across Development, QA, UAT, and Production.

---

## Interview Question

### Question

What are Terraform Variables?

### Answer

Terraform Variables are placeholders that allow engineers to pass dynamic values into Terraform configurations, making infrastructure reusable and easier to maintain.

---

# 2. Why do we need Variables?

## Answer

Without Variables, every environment requires separate Terraform code.

This leads to duplicated code and makes infrastructure difficult to maintain.

Variables solve this problem by allowing the same Terraform configuration to work across multiple environments.

---

## Without Variables

```text
Development

main.tf

↓

production-rg
```

```text
QA

main.tf

↓

qa-rg
```

```text
Production

main.tf

↓

prod-rg
```

Every file contains almost identical code.

Changing infrastructure means editing multiple files.

---

## With Variables

```text
Terraform Configuration

        │

────────┼────────

        │

Development

↓

development-rg

QA

↓

qa-rg

Production

↓

production-rg
```

The Terraform configuration remains the same.

Only the variable values change.

---

## Why is this important?

Using Variables provides:

- Reusable Infrastructure
- Less Code Duplication
- Easier Collaboration
- Environment-specific Deployments
- Cleaner Terraform Projects

---

## Production Example

A company maintains a single Terraform project.

The CI/CD pipeline supplies different variable values for:

- Development
- QA
- UAT
- Production

The same Terraform code is reused across all environments.

---

## Production Tip

Treat Variables as configuration.

Avoid modifying Terraform code for every deployment.

Instead, provide different values through variable files or CI/CD pipelines.

---

## Interview Question

### Question

Why do we need Terraform Variables?

### Answer

Terraform Variables allow the same infrastructure code to be reused across multiple environments by accepting different input values instead of hardcoding configuration.

---

# 3. What are Variable Types?

## Answer

Terraform supports multiple data types.

Choosing the correct type helps Terraform validate input values and prevents configuration errors.

---

## Common Variable Types

| Type | Description |
|------|-------------|
| string | Text values |
| number | Numeric values |
| bool | true or false |
| list | Ordered collection |
| map | Key-value pairs |
| object | Structured data |
| tuple | Fixed ordered collection |
| set | Unordered unique collection |

---

## Example

### String

```hcl
variable "location" {
  type = string
}
```

---

### Number

```hcl
variable "vm_count" {
  type = number
}
```

---

### Boolean

```hcl
variable "enable_backup" {
  type = bool
}
```

---

### List

```hcl
variable "availability_zones" {
  type = list(string)
}
```

---

### Map

```hcl
variable "tags" {
  type = map(string)
}
```

---

## Production Tip

Always define explicit variable types.

Avoid relying on automatic type inference in production projects.

Strong typing improves readability, validation, and collaboration.

---

## Interview Question

### Question

Which Variable types are supported in Terraform?

### Answer

Terraform supports string, number, bool, list, map, object, tuple, and set variable types.

---

# 4. How do you declare a Variable?

## Answer

Variables are declared using the `variable` block.

Every variable has a unique name and can optionally include:

- Description
- Type
- Default Value
- Validation Rules
- Sensitive Flag

---

## Basic Syntax

```hcl
variable "location" {
  type = string
}
```

Terraform now expects a value for the variable named:

```text
location
```

You can reference it anywhere using:

```hcl
var.location
```

---

## Complete Example

```hcl
variable "resource_group_name" {
  description = "Name of the Azure Resource Group"

  type = string

  default = "development-rg"
}
```

---

## How does it work?

```text
Variable Declaration

        │

        ▼

Variable Value

        │

        ▼

Terraform Resource

        │

        ▼

Azure Resource Group
```

Terraform replaces:

```text
var.resource_group_name
```

with

```text
development-rg
```

before creating the infrastructure.

---

## Production Tip

Always include:

- description
- type

for every variable.

This makes the code easier to understand for other engineers.

---

## Interview Question

### Question

How do you declare a Variable in Terraform?

### Answer

Variables are declared using the `variable` block and are referenced using the `var.<variable_name>` syntax.

---

# 5. What are Default Values?

## Answer

A default value allows Terraform to use a predefined value if the user doesn't provide one.

This makes variables optional.

---

## Example

```hcl
variable "location" {

  type = string

  default = "Central India"

}
```

Now Terraform automatically uses:

```text
Central India
```

unless another value is supplied.

---

## Without Default Value

```hcl
variable "location" {
  type = string
}
```

Terraform requires the user to provide a value.

Otherwise:

```bash
terraform apply
```

will fail.

---

## How does it work?

```text
User Provides Value?

        │

   Yes ───────► Use User Value

        │

        ▼

       No

        │

        ▼

Use Default Value
```

---

## Production Tip

Use defaults only for values that rarely change.

Examples:

- Azure Region
- Resource Tags
- Naming Prefix

Avoid default values for:

- Passwords
- Secrets
- Production Configuration

---

## Interview Question

### Question

What is the purpose of a default value?

### Answer

Default values allow Terraform to use predefined values when the user doesn't explicitly provide input.

---

# 6. What is Variable Validation?

## Answer

Variable Validation ensures that users provide valid input before Terraform creates infrastructure.

Instead of allowing incorrect values, Terraform validates them during execution.

---

## Why do we need Validation?

Imagine someone provides:

```text
location = "Moon"
```

Azure doesn't have a region called:

```text
Moon
```

Terraform can detect this before deployment.

---

## Example

```hcl
variable "environment" {

  type = string

  validation {

    condition = contains(
      ["dev", "qa", "prod"],
      var.environment
    )

    error_message = "Environment must be dev, qa, or prod."

  }

}
```

---

## How does it work?

```text
Input Value

        │

        ▼

Validation Rule

        │

────────┼────────

        │

Valid

        │

        ▼

Continue Deployment

        │

Invalid

        │

        ▼

Display Error
```

---

## Production Tip

Always validate variables such as:

- Environment
- Region
- VM Size
- Instance Count
- Storage SKU

This prevents deployment mistakes.

---

## Interview Question

### Question

Why do we use Variable Validation?

### Answer

Variable Validation ensures that only valid input values are accepted before infrastructure is deployed.

---

# 7. What are Sensitive Variables?

## Answer

Some variables contain confidential information.

Examples include:

- Passwords
- API Keys
- Client Secrets
- Database Credentials
- Access Tokens

Terraform allows you to mark these variables as **Sensitive**.

---

## Example

```hcl
variable "admin_password" {

  type = string

  sensitive = true

}
```

Terraform hides the value from normal CLI output.

---

## Important Note

Marking a variable as **Sensitive** does **not** encrypt it.

Sensitive values may still exist in the Terraform State file.

Always secure your State file.

---

## Production Tip

Never hardcode secrets inside Terraform code.

Instead use:

- Azure Key Vault
- AWS Secrets Manager
- HashiCorp Vault

---

## Interview Question

### Question

What is a Sensitive Variable?

### Answer

A Sensitive Variable hides confidential values from Terraform CLI output but does not encrypt them inside the Terraform State file.

---

# Summary

After completing this section, you should understand:

✅ Variable Declaration

✅ Default Values

✅ Variable Validation

✅ Sensitive Variables

---

# 8. What are Variable Files (`terraform.tfvars`)?

## Answer

Variable files allow you to store variable values outside your Terraform configuration.

Instead of hardcoding values inside your code, you can keep them in a separate file.

This makes your infrastructure cleaner, reusable, and easier to manage.

---

## Why do we need Variable Files?

Suppose you have the following variables:

- Resource Group Name
- Location
- Environment
- VM Size
- Number of Instances

Without a variable file, Terraform prompts you for every value during deployment.

With a variable file, Terraform reads all values automatically.

---

## Example

### variables.tf

```hcl
variable "resource_group_name" {
  type = string
}

variable "location" {
  type = string
}

variable "environment" {
  type = string
}
```

---

### terraform.tfvars

```hcl
resource_group_name = "production-rg"

location = "Central India"

environment = "prod"
```

Terraform automatically loads:

```text
terraform.tfvars
```

---

## How does it work?

```text
Terraform Configuration

        │

Uses Variables

        │

        ▼

terraform.tfvars

        │

        ▼

Variable Values

        │

        ▼

Infrastructure Deployment
```

---

## Production Tip

Create separate variable files for each environment.

Example:

```text
dev.tfvars

qa.tfvars

uat.tfvars

prod.tfvars
```

This keeps your deployments organized.

---

## Interview Question

### Question

What is the purpose of `terraform.tfvars`?

### Answer

The `terraform.tfvars` file stores values for Terraform variables, allowing the same Terraform configuration to be reused across different environments.

---

# 9. How do you pass Variable values?

## Answer

Terraform supports multiple ways of passing variable values.

This gives engineers flexibility for local development, automation, and CI/CD pipelines.

---

## Method 1 - Default Value

```hcl
variable "location" {
  default = "Central India"
}
```

---

## Method 2 - terraform.tfvars

```hcl
location = "Central India"
```

---

## Method 3 - Custom Variable File

```bash
terraform apply -var-file="prod.tfvars"
```

---

## Method 4 - Command Line

```bash
terraform apply \
-var="location=Central India"
```

---

## Method 5 - Environment Variable

```bash
export TF_VAR_location="Central India"
```

Terraform automatically loads:

```text
TF_VAR_<variable_name>
```

---

## Internal Workflow

```text
Terraform

        │

Read Variables

        │

────────┼────────

        │

Default Value

terraform.tfvars

Custom tfvars

Environment Variable

CLI Variable

        │

        ▼

Terraform Resource
```

---

## Production Tip

Use:

- `terraform.tfvars` for local development.
- Environment Variables or CI/CD secrets for production pipelines.

---

## Interview Question

### Question

How can variable values be passed into Terraform?

### Answer

Terraform supports Default Values, `terraform.tfvars`, custom variable files, command-line variables, and environment variables.

---

# 10. What is Variable Precedence?

## Answer

Terraform may receive the same variable from multiple sources.

Terraform follows a predefined priority order to determine which value to use.

The highest-priority value overrides lower-priority values.

---

## Variable Precedence Order

Highest Priority

```text
CLI (-var)

↓

CLI (-var-file)

↓

*.auto.tfvars

↓

terraform.tfvars

↓

Environment Variables

↓

Default Values
```

---

## Example

Suppose:

Default Value

```text
Central India
```

terraform.tfvars

```text
East US
```

CLI

```bash
terraform apply -var="location=West Europe"
```

Terraform uses:

```text
West Europe
```

because CLI variables have the highest priority.

---

## Production Tip

Avoid mixing multiple sources for the same variable.

Use one consistent strategy for each environment.

---

## Interview Question

### Question

What is Variable Precedence?

### Answer

Variable Precedence defines the order Terraform follows when multiple values are provided for the same variable.

---

# 11. Common Mistakes

## Mistake 1

❌ Hardcoding values

```hcl
location = "Central India"
```

Use Variables instead.

---

## Mistake 2

❌ Hardcoding passwords

Never store secrets inside:

- Terraform Code
- terraform.tfvars
- Git Repository

Use:

- Azure Key Vault
- AWS Secrets Manager
- HashiCorp Vault

---

## Mistake 3

❌ Not defining variable types

Always specify:

```hcl
type = string
```

This improves validation and readability.

---

## Mistake 4

❌ Not validating input values

Always validate important inputs like:

- Environment
- Region
- VM Size

---

## Mistake 5

❌ Using one tfvars file for every environment

Instead use:

```text
dev.tfvars

qa.tfvars

uat.tfvars

prod.tfvars
```

---

# 12. Production Best Practices

Always follow these recommendations.

✅ Declare explicit variable types.

---

✅ Add descriptions for every variable.

---

✅ Validate important variables.

---

✅ Mark secrets as Sensitive.

---

✅ Store secrets in a Secret Manager.

---

✅ Use separate tfvars files for each environment.

---

✅ Keep Terraform code environment-independent.

---

# Summary

After completing this guide, you should understand:

✅ Terraform Variables

✅ Variable Types

✅ Variable Declaration

✅ Default Values

✅ Variable Validation

✅ Sensitive Variables

✅ Variable Files

✅ Variable Precedence

✅ Production Best Practices

---

# What's Next?

➡️ **07-terraform-functions.md**

In the next guide, you'll learn:

- What are Terraform Functions?
- String Functions
- Numeric Functions
- Collection Functions
- File Functions
- Encoding Functions
- Date & Time Functions
- Production Examples
- Best Practices
- Interview Questions