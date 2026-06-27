# 🚀 Helm Values - Managing Configuration in Helm Charts

This guide explains everything you need to know about Helm Values.

If you've ever wondered:

- What is `values.yaml`?
- Why do we need `values.yaml`?
- How does Helm use values?
- How do templates read values?
- Why are values important in production?

This guide is for you.

---

# 📑 Table of Contents

1. What is values.yaml?
2. Why do we need values.yaml?
3. How Helm Uses Values
4. Understanding the values.yaml File
5. Common Interview Questions

---

# 1. What is values.yaml?

## Answer

The **values.yaml** file stores the default configuration values for a Helm Chart.

Instead of hardcoding values inside Kubernetes manifests, Helm reads configuration from `values.yaml` and injects it into templates during deployment.

This makes Helm Charts reusable across different environments.

---

## Why do we need values.yaml?

Imagine deploying the same application in three environments.

Development

```text
Replicas = 1

Image Tag = latest

Service Type = ClusterIP
```

Production

```text
Replicas = 5

Image Tag = v2.0.0

Service Type = LoadBalancer
```

Without `values.yaml`, you would need separate YAML files for each environment.

With `values.yaml`, only the configuration changes while the templates remain the same.

---

## Internal Workflow

```text
values.yaml

        │

Default Configuration

        │

        ▼

Helm Templates

        │

Replace Variables

        │

        ▼

Generated Kubernetes YAML
```

---

## Real World Example

A shopping application uses:

```text
Development

replicaCount = 1

---------------------

Production

replicaCount = 5
```

The Deployment template stays exactly the same.

Only `values.yaml` changes.

---

## Benefits

Using `values.yaml` provides:

- Reusable Charts
- Environment-specific Configuration
- Easy Maintenance
- Better Readability
- Simplified Deployments

---

## Production Tip

Never hardcode environment-specific values inside templates.

Store all configurable values inside `values.yaml`.

---

## Interview Question

### Question

What is `values.yaml`?

### Answer

`values.yaml` stores the default configuration values that Helm uses to render Kubernetes templates.

---

# 2. Why do we need values.yaml?

## Answer

Applications rarely use the same configuration in every environment.

For example:

Development

```text
1 Replica
```

Testing

```text
2 Replicas
```

Production

```text
5 Replicas
```

Instead of editing templates every time, Helm allows configuration to be changed through `values.yaml`.

---

## Without values.yaml

```text
Deployment.yaml

↓

Hardcoded Values

↓

Edit File Every Deployment
```

---

## With values.yaml

```text
Deployment Template

↓

Reads Values

↓

Generate Environment-Specific YAML
```

---

## Production Example

A company deploys the same application to:

```text
Development

↓

QA

↓

Production
```

Each environment has different:

- Replica Count
- Image Tag
- Resource Limits
- Service Type

The templates remain unchanged.

---

## Production Tip

Keep templates generic and store all environment-specific settings inside values files.

---

## Interview Question

### Question

Why do we need `values.yaml`?

### Answer

`values.yaml` separates configuration from templates, allowing the same Helm Chart to be reused across multiple environments.

---

# 3. How Helm Uses Values

## Answer

When a Helm Chart is installed, Helm performs several steps automatically.

---

## Internal Workflow

```text
helm install

        │

Read values.yaml

        │

Read Templates

        │

Replace Variables

        │

Generate Kubernetes YAML

        │

Deploy Resources
```

---

## Example

values.yaml

```yaml
replicaCount: 3
```

Template

```yaml
spec:
  replicas: {{ .Values.replicaCount }}
```

Generated YAML

```yaml
spec:
  replicas: 3
```

Helm replaces the template variable with the value from `values.yaml`.

---

## Production Example

A Deployment template is used across all environments.

Only the values change:

Development

```yaml
replicaCount: 1
```

Production

```yaml
replicaCount: 5
```

---

## Production Tip

Helm templates should always reference `.Values` instead of fixed values.

---

## Interview Question

### Question

How does Helm use values during deployment?

### Answer

Helm reads values from `values.yaml`, replaces template variables with those values, generates Kubernetes manifests, and deploys them to the cluster.

---

# 4. Understanding the values.yaml File

## Answer

The `values.yaml` file contains configuration in YAML format.

It can store:

- Replica Count
- Container Images
- Service Configuration
- Resource Limits
- Ingress Settings
- Environment Variables

---

## Example

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.27"

service:
  type: ClusterIP
  port: 80

resources:
  requests:
    cpu: "100m"
    memory: "128Mi"

  limits:
    cpu: "500m"
    memory: "512Mi"
```

---

## Internal Structure

```text
values.yaml

        │

Application Settings

────────────┼────────────

        │

Image

Service

Resources

Ingress

Autoscaling

Security
```

---

## Production Example

A production `values.yaml` may define:

- 5 Replicas
- LoadBalancer Service
- CPU & Memory Limits
- TLS Enabled
- Production Image Tag

while Development uses lighter settings.

---

## Production Tip

Organize `values.yaml` into logical sections to improve readability and maintenance.

---

## Interview Question

### Question

What kind of configuration is typically stored in `values.yaml`?

### Answer

`values.yaml` typically stores configurable settings such as replica counts, container images, service configuration, resource limits, ingress settings, and application-specific values.

---

# 5. Accessing Values with .Values

## Answer

Helm templates access values stored in `values.yaml` using the **`.Values`** object.

Whenever Helm renders a template, it replaces `.Values` with the corresponding value from `values.yaml`.

---

## Example

values.yaml

```yaml
replicaCount: 3
```

Template

```yaml
spec:
  replicas: {{ .Values.replicaCount }}
```

Generated YAML

```yaml
spec:
  replicas: 3
```

---

## Internal Workflow

```text
values.yaml

        │

Read Value

        │

        ▼

.Values

        │

Replace Template Variable

        │

        ▼

Generated Kubernetes YAML
```

---

## Production Example

Development

```yaml
replicaCount: 1
```

Production

```yaml
replicaCount: 5
```

The same Deployment template works for both environments.

---

## Production Tip

Always reference configuration using `.Values` instead of hardcoding values inside templates.

---

## Interview Question

### Question

How do Helm templates access values?

### Answer

Helm templates use the `.Values` object to read configuration from `values.yaml`.

---

# 6. Nested Values

## Answer

`values.yaml` supports nested structures to organize configuration.

This makes large Helm Charts easier to maintain.

---

## Example

values.yaml

```yaml
image:

  repository: nginx

  tag: "1.27"

service:

  type: ClusterIP

  port: 80
```

Template

```yaml
image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

Generated YAML

```yaml
image: nginx:1.27
```

---

## Internal Workflow

```text
.Values

        │

Image

        │

Repository

        │

Tag

        │

Generated YAML
```

---

## Production Example

A production application stores:

- Image Repository
- Image Tag
- Pull Policy

inside the `image` section for better organization.

---

## Production Tip

Group related values together instead of creating long flat lists of configuration.

---

## Interview Question

### Question

How do you access nested values in Helm?

### Answer

Nested values are accessed using dot notation, for example:

```text
.Values.image.repository
```

---

# 7. Default Values

## Answer

The values defined in `values.yaml` are the **default values** used by Helm.

If the user does not provide alternative values during installation, Helm automatically uses these defaults.

---

## Example

values.yaml

```yaml
replicaCount: 2

service:

  type: ClusterIP
```

Install

```bash
helm install myapp .
```

Helm automatically uses:

```text
Replicas = 2

Service = ClusterIP
```

---

## Production Example

Development teams keep safe default values inside `values.yaml`.

Production deployments override only the required settings.

---

## Production Tip

Provide sensible default values so Charts work immediately after installation.

---

## Interview Question

### Question

What are default values in Helm?

### Answer

Default values are the configuration settings defined in `values.yaml` that Helm uses unless they are overridden.

---

# 8. Overriding Values

## Answer

Helm allows values to be overridden during installation or upgrade.

This avoids modifying `values.yaml` directly.

---

## Override a Single Value

```bash
helm install myapp . --set replicaCount=5
```

---

## Override Multiple Values

```bash
helm install myapp . \
--set image.tag=2.0.0 \
--set service.type=LoadBalancer
```

---

## Internal Workflow

```text
values.yaml

        │

Default Values

        │

────────────┼────────────

        │

--set Values

        │

Override Defaults

        │

        ▼

Generated YAML
```

---

## Production Example

Development

```bash
helm install shopping .
```

Production

```bash
helm install shopping . \
--set replicaCount=5 \
--set image.tag=2.1.0
```

The Chart remains unchanged.

---

## Production Tip

Use `--set` for small configuration changes.

For larger environments, prefer dedicated values files.

---

## Interview Question

### Question

How do you override values during installation?

### Answer

Use the `--set` option with the `helm install` or `helm upgrade` command.

---

# 9. Multiple Values Files

## Answer

Large applications often require different configuration for different environments.

Instead of modifying one `values.yaml`, Helm allows multiple values files.

---

## Example

```text
values.yaml

values-dev.yaml

values-qa.yaml

values-prod.yaml
```

---

## Install with a Values File

```bash
helm install shopping . \
-f values-prod.yaml
```

---

## Internal Workflow

```text
Helm Chart

        │

values-prod.yaml

        │

Override Defaults

        │

        ▼

Deploy Production Configuration
```

---

## Production Example

Development

```bash
helm install shopping . \
-f values-dev.yaml
```

Production

```bash
helm install shopping . \
-f values-prod.yaml
```

The same Chart is reused across all environments.

---

## Production Tip

Maintain separate values files for Development, QA, Staging, and Production to simplify deployments and reduce configuration errors.

---

## Interview Question

### Question

How do you use multiple values files in Helm?

### Answer

Use the `-f` option to specify an alternate values file during installation or upgrade.

---

## 📌 Quick Revision

| Feature | Example |
|---------|---------|
| Access Value | `.Values.replicaCount` |
| Nested Value | `.Values.image.repository` |
| Default Values | `values.yaml` |
| Override Value | `--set replicaCount=5` |
| Environment Values | `-f values-prod.yaml` |

---

# 10. Values Precedence

## Answer

Helm supports multiple ways of providing configuration values.

When the same value is defined in multiple places, Helm follows a precedence order.

The value with the highest precedence overrides the others.

---

## Values Precedence Order

```text
Lowest Priority

↓

values.yaml

↓

Additional Values Files (-f)

↓

--set

↓

--set-string

↓

Highest Priority
```

---

## Example

Default values

```yaml
replicaCount: 2
```

Production values

```yaml
replicaCount: 5
```

Command

```bash
helm install shopping . \
-f values-prod.yaml \
--set replicaCount=10
```

Final Generated Value

```text
replicaCount = 10
```

because `--set` has higher precedence.

---

## Internal Workflow

```text
values.yaml

        │

Merge Values

        │

values-prod.yaml

        │

Merge Again

        │

--set

        │

Highest Priority Wins

        ▼

Generated YAML
```

---

## Production Tip

Understand values precedence before troubleshooting unexpected configuration.

---

## Interview Question

### Question

Which Helm value has the highest priority?

### Answer

Values provided using `--set` override values from values files and the default `values.yaml`.

---

# 11. Merging Values

## Answer

Helm automatically merges configuration from multiple values files.

Only the values you override are replaced.

Everything else continues using the default values.

---

## Default values.yaml

```yaml
image:

  repository: nginx

  tag: latest

service:

  type: ClusterIP

  port: 80
```

---

## values-prod.yaml

```yaml
image:

  tag: "2.0.0"

service:

  type: LoadBalancer
```

---

## Final Result

```yaml
image:

  repository: nginx

  tag: "2.0.0"

service:

  type: LoadBalancer

  port: 80
```

Notice that only the overridden values changed.

---

## Production Example

One Helm Chart is reused across:

```text
Development

↓

QA

↓

Staging

↓

Production
```

Each environment overrides only the required values.

---

## Production Tip

Keep shared configuration inside `values.yaml` and override only environment-specific settings.

---

## Interview Question

### Question

How does Helm merge multiple values files?

### Answer

Helm combines the values files and overrides only the matching keys while preserving the remaining default values.

---

# 12. Common Beginner Mistakes

## Mistake 1

❌ Hardcoding configuration inside templates.

Always reference `.Values`.

---

## Mistake 2

❌ Editing templates for each environment.

Use separate values files instead.

---

## Mistake 3

❌ Putting passwords directly inside values.yaml.

Store sensitive information in Kubernetes Secrets or external secret management solutions.

---

## Mistake 4

❌ Using `--set` for large production configurations.

Use dedicated values files for better readability and version control.

---

## Mistake 5

❌ Forgetting values precedence.

Unexpected overrides can lead to incorrect deployments.

---

# 13. Production Best Practices

Always follow these recommendations.

---

✅ Keep templates generic.

---

✅ Store configuration in values files.

---

✅ Create separate values files for:

- Development
- QA
- Staging
- Production

---

✅ Store values files in Git.

---

✅ Use `--set` only for small overrides.

---

✅ Never store secrets in plain text.

---

✅ Validate changes before production deployments.

---

## 📌 Quick Revision

### Values Workflow

```text
values.yaml

↓

Read Configuration

↓

Templates

↓

Replace Variables

↓

Generated Kubernetes YAML

↓

Deploy
```

---

### Values Precedence

```text
values.yaml

↓

values-prod.yaml

↓

--set

↓

Final Value
```

---

### Common Commands

```bash
helm install myapp .

helm install myapp . -f values-prod.yaml

helm install myapp . --set replicaCount=5

helm upgrade myapp . -f values-prod.yaml

helm upgrade myapp . --set image.tag=2.0.0
```

---

# Summary

After completing this guide, you should understand:

✅ values.yaml

✅ Default Values

✅ Nested Values

✅ `.Values`

✅ Overriding Values

✅ `--set`

✅ Multiple Values Files

✅ Values Precedence

✅ Value Merging

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is `values.yaml`? | Stores the default configuration values for a Helm Chart |
| How are values accessed in templates? | Using `.Values` |
| How do you access nested values? | `.Values.image.repository` |
| How do you override values? | Using `--set` or `-f` |
| How do you use another values file? | `-f values-prod.yaml` |
| Which value has the highest priority? | `--set` |
| Can multiple values files be used? | Yes |
| What should templates contain? | Reusable logic, not environment-specific values |
| Where should production configuration be stored? | Separate values files |
| Should secrets be stored in `values.yaml`? | No |

---

# What's Next?

➡️ **04-helm-templates.md**

In the next guide, you'll learn:

- What are Helm Templates?
- Go Template Syntax
- Variables
- Built-in Objects
- `if` Conditions
- `range` Loops
- Template Functions
- `include` and `template`
- Pipelines
- Production Best Practices
- Common Interview Questions