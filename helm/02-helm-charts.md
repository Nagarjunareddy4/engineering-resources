# 🚀 Helm Charts - Packaging Kubernetes Applications

This guide explains everything you need to know about Helm Charts.

If you've ever wondered:

- What is a Helm Chart?
- What files are inside a Chart?
- What is Chart.yaml?
- What is values.yaml?
- How is a Helm Chart organized?

This guide is for you.

---

# 📑 Table of Contents

1. What is a Helm Chart?
2. Why do we need Helm Charts?
3. Helm Chart Structure
4. Understanding Chart.yaml
5. Common Interview Questions

---

# 1. What is a Helm Chart?

## Answer

A **Helm Chart** is a package that contains all the files required to deploy an application on Kubernetes.

Instead of managing multiple Kubernetes YAML files separately, Helm bundles them into a reusable Chart.

A Chart may include:

- Deployments
- Services
- ConfigMaps
- Secrets
- Ingress
- Persistent Volume Claims
- Horizontal Pod Autoscalers

---

## Why do we need Helm Charts?

Imagine deploying a web application.

Without Helm Charts:

```text
deployment.yaml

service.yaml

configmap.yaml

secret.yaml

ingress.yaml

pvc.yaml
```

Each file must be managed separately.

With a Helm Chart:

```text
shopping-app-chart

↓

Contains Everything

↓

Single Deployment Package
```

---

## Helm Chart Overview

```text
Developer

        │

Helm Chart

        │

────────────┼────────────

        │

Templates

Values

Metadata

Dependencies

        │

        ▼

Kubernetes Cluster
```

---

## Real World Example

A DevOps team creates a single Helm Chart for an e-commerce application.

The Chart includes:

- Frontend Deployment
- Backend Deployment
- Services
- ConfigMaps
- Secrets
- Ingress

The entire application can now be deployed with one command.

---

## Benefits

Helm Charts provide:

- Reusability
- Version Control
- Easier Deployments
- Consistent Configuration
- Simplified Upgrades
- Easy Rollbacks

---

## Production Tip

Design one Helm Chart for one logical application.

Avoid placing unrelated applications in the same Chart.

---

## Interview Question

### Question

What is a Helm Chart?

### Answer

A Helm Chart is a package containing Kubernetes resources, templates, configuration values, and metadata required to deploy an application.

---

# 2. Why do we need Helm Charts?

## Answer

Modern Kubernetes applications consist of many resources.

Managing them individually becomes difficult.

Helm Charts package everything into one reusable deployment unit.

---

## Without Helm Charts

```text
Deployment

↓

Service

↓

ConfigMap

↓

Secret

↓

Ingress

↓

PVC
```

Deploy each file individually.

---

## With Helm Charts

```text
Helm Chart

↓

One Package

↓

helm install
```

Everything is deployed together.

---

## Why is this important?

Helm Charts provide:

- Standardized Deployments
- Easier Maintenance
- Environment Reusability
- Better Version Control
- Simplified CI/CD Integration

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

The same Chart is reused.

Only configuration values change.

---

## Production Tip

Keep application logic inside templates and environment-specific settings inside `values.yaml`.

---

## Interview Question

### Question

Why do we need Helm Charts?

### Answer

Helm Charts package Kubernetes resources into reusable deployment units, making applications easier to deploy, upgrade, and maintain.

---

# 3. Helm Chart Structure

## Answer

A Helm Chart follows a standard directory structure.

---

## Example Structure

```text
my-chart/

├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│   └── _helpers.tpl
└── .helmignore
```

---

## Directory Explanation

### Chart.yaml

Stores Chart metadata.

---

### values.yaml

Stores default configuration values.

---

### templates/

Contains Kubernetes resource templates.

---

### charts/

Stores dependency Charts.

---

### .helmignore

Specifies files that should not be packaged.

---

## Internal Workflow

```text
Helm Chart

        │

────────────┼────────────

        │

Chart.yaml

Values.yaml

Templates/

Charts/

        │

        ▼

Rendered Kubernetes YAML

        │

        ▼

Cluster
```

---

## Production Example

Every production Chart follows a consistent structure so teams can easily understand, maintain, and update it.

---

## Production Tip

Never place generated Kubernetes manifests outside the `templates/` directory.

---

## Interview Question

### Question

What are the main files inside a Helm Chart?

### Answer

The main files are:

- Chart.yaml
- values.yaml
- templates/
- charts/
- .helmignore

---

# 4. Understanding Chart.yaml

## Answer

`Chart.yaml` is the metadata file of a Helm Chart.

It provides information about the Chart itself.

---

## Example

```yaml
apiVersion: v2

name: shopping-app

description: Helm Chart for Shopping Application

type: application

version: 1.0.0

appVersion: "2.3.1"
```

---

## Important Fields

### apiVersion

Specifies the Helm Chart API version.

---

### name

The Chart name.

---

### description

A short description of the Chart.

---

### type

Defines the Chart type.

Common values:

```text
application

library
```

---

### version

The version of the Helm Chart.

---

### appVersion

The version of the application being deployed.

---

## Production Example

```text
Chart Version

↓

1.2.0

---------------------

Application Version

↓

3.5.8
```

The Chart version and application version can change independently.

---

## Production Tip

Follow Semantic Versioning (SemVer) for the `version` field to make upgrades predictable.

---

## Interview Question

### Question

What is the purpose of `Chart.yaml`?

### Answer

`Chart.yaml` stores the metadata of a Helm Chart, including its name, version, description, type, and application version.

---

# 5. Understanding values.yaml

## Answer

The **values.yaml** file stores the default configuration values for a Helm Chart.

Instead of hardcoding values inside Kubernetes manifests, Helm reads values from this file and injects them into templates during deployment.

---

## Why do we need values.yaml?

Imagine deploying the same application in multiple environments.

Development:

```text
Replicas = 1

Image Tag = latest
```

Production:

```text
Replicas = 5

Image Tag = v2.0.0
```

Without `values.yaml`, you would need to edit the YAML templates manually.

With `values.yaml`, only the configuration changes.

---

## Example

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: latest

service:
  type: ClusterIP
  port: 80
```

---

## Internal Workflow

```text
values.yaml

        │

Read Values

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

## Production Example

Development

```yaml
replicaCount: 1
```

Production

```yaml
replicaCount: 5
```

The templates remain unchanged.

---

## Production Tip

Store all environment-specific configuration inside **values.yaml** instead of modifying template files.

---

## Interview Question

### Question

What is the purpose of `values.yaml`?

### Answer

`values.yaml` stores the default configuration values used by Helm templates during chart rendering.

---

# 6. Understanding templates/

## Answer

The **templates/** directory contains Kubernetes resource templates.

These templates are converted into standard Kubernetes YAML during deployment.

Typical files include:

- deployment.yaml
- service.yaml
- ingress.yaml
- configmap.yaml
- secret.yaml
- pvc.yaml

---

## Example Structure

```text
templates/

├── deployment.yaml
├── service.yaml
├── ingress.yaml
├── configmap.yaml
├── secret.yaml
└── _helpers.tpl
```

---

## Example Template

```yaml
spec:
  replicas: {{ .Values.replicaCount }}
```

During deployment, Helm replaces:

```text
{{ .Values.replicaCount }}
```

with:

```text
2
```

(or any value provided.)

---

## Internal Workflow

```text
Template

↓

Read Values

↓

Replace Variables

↓

Generate YAML

↓

Deploy to Kubernetes
```

---

## Production Example

Instead of creating separate Deployment YAML files for Development and Production, a single template is reused with different values.

---

## Production Tip

Never hardcode configuration values inside templates.

Always reference `values.yaml`.

---

## Interview Question

### Question

What is the purpose of the `templates/` directory?

### Answer

The `templates/` directory contains Kubernetes manifest templates that Helm renders using values from `values.yaml`.

---

# 7. Understanding _helpers.tpl

## Answer

The **_helpers.tpl** file contains reusable template functions.

Instead of repeating the same template code in multiple files, helper templates allow you to define it once and reuse it.

---

## Example

```yaml
{{- define "shopping-app.fullname" -}}

{{ .Release.Name }}-{{ .Chart.Name }}

{{- end }}
```

Use it inside another template.

```yaml
metadata:
  name: {{ include "shopping-app.fullname" . }}
```

---

## Internal Workflow

```text
_helpers.tpl

↓

Reusable Function

↓

Called from Templates

↓

Generated YAML
```

---

## Production Example

Instead of manually writing resource names across every template, helper templates generate consistent names automatically.

---

## Production Tip

Use `_helpers.tpl` for:

- Resource Names
- Labels
- Common Annotations
- Naming Standards

---

## Interview Question

### Question

Why is `_helpers.tpl` used?

### Answer

`_helpers.tpl` stores reusable template functions that reduce duplication and maintain consistency across Helm templates.

---

# 8. Understanding charts/

## Answer

The **charts/** directory stores dependency Charts.

If your application depends on another Helm Chart, it is stored here.

---

## Example

```text
shopping-app/

↓

charts/

↓

redis/

↓

mysql/
```

The shopping application depends on Redis and MySQL.

---

## Internal Workflow

```text
Main Chart

        │

Dependencies

        │

────────────┼────────────

        │

Redis Chart

MySQL Chart

        │

Deploy Together
```

---

## Production Example

An application automatically deploys:

- Redis
- PostgreSQL

as dependencies during installation.

---

## Production Tip

Manage dependencies using `Chart.yaml` instead of manually copying Charts into the `charts/` directory whenever possible.

---

## Interview Question

### Question

What is the purpose of the `charts/` directory?

### Answer

The `charts/` directory stores dependency Charts that are installed together with the main Helm Chart.

---

# 9. Understanding .helmignore

## Answer

The **.helmignore** file works similarly to `.gitignore`.

It tells Helm which files should not be included when packaging a Chart.

---

## Example

```text
.git/

README.md

*.log

*.tmp
```

---

## Why do we need .helmignore?

Without `.helmignore`, unnecessary files may be included in the packaged Chart.

This increases package size and may expose unwanted files.

---

## Production Example

Exclude:

- Local test files
- Temporary files
- Git metadata
- IDE configuration files

from packaged Charts.

---

## Production Tip

Keep your packaged Charts clean by excluding development artifacts.

---

## Interview Question

### Question

What is `.helmignore`?

### Answer

`.helmignore` specifies files and directories that should be excluded when packaging a Helm Chart.

---

# 📌 Quick Revision

| File | Purpose |
|------|---------|
| `Chart.yaml` | Chart Metadata |
| `values.yaml` | Default Configuration Values |
| `templates/` | Kubernetes Resource Templates |
| `_helpers.tpl` | Reusable Template Functions |
| `charts/` | Dependency Charts |
| `.helmignore` | Exclude Files from Chart Package |

---

# 10. Creating Your First Helm Chart

## Answer

Helm provides a command to generate a complete Chart structure automatically.

Instead of creating every file manually, Helm creates a production-ready directory structure.

---

## Create a Chart

```bash
helm create my-first-chart
```

---

## Generated Structure

```text
my-first-chart/

├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── serviceaccount.yaml
│   ├── hpa.yaml
│   ├── NOTES.txt
│   ├── _helpers.tpl
│   └── tests/
└── .helmignore
```

---

## Internal Workflow

```text
helm create

        │

Generate Chart Structure

        │

Create Default Templates

        │

Ready for Customization
```

---

## Production Example

Instead of writing every Kubernetes manifest from scratch, a DevOps engineer creates a new Helm Chart using:

```bash
helm create shopping-app
```

The generated templates are then customized for the application.

---

## Production Tip

Use the generated Chart as a starting point and remove unnecessary templates before deploying to production.

---

## Interview Question

### Question

How do you create a new Helm Chart?

### Answer

Use:

```bash
helm create <chart-name>
```

---

# 11. Installing a Local Helm Chart

## Answer

After creating or modifying a Chart, you can install it directly from your local machine.

---

## Install Local Chart

```bash
helm install shopping-app ./my-first-chart
```

---

## Internal Workflow

```text
Local Chart

        │

Render Templates

        │

Replace Values

        │

Generate YAML

        │

Deploy Resources

        ▼

Kubernetes Cluster
```

---

## Verify Installation

```bash
helm list
```

---

## Verify Kubernetes Resources

```bash
kubectl get all
```

---

## Production Example

A CI/CD pipeline builds the latest Chart and deploys it directly to an AKS cluster using:

```bash
helm install shopping-app ./chart
```

---

## Production Tip

Always validate your Chart before installing it.

```bash
helm lint .
```

---

## Interview Question

### Question

How do you install a local Helm Chart?

### Answer

Use:

```bash
helm install <release-name> <chart-directory>
```

---

# 12. Packaging a Helm Chart

## Answer

Once a Chart is complete, it can be packaged into a compressed archive for sharing or publishing.

---

## Package Command

```bash
helm package my-first-chart
```

---

## Example Output

```text
my-first-chart-1.0.0.tgz
```

---

## Internal Workflow

```text
Helm Chart

        │

Package

        │

Create .tgz File

        │

Upload to Repository

        ▼

Reusable Chart
```

---

## Production Example

A DevOps team packages every release and uploads it to an internal Helm Repository such as:

- Azure Container Registry (OCI)
- Harbor
- JFrog Artifactory

---

## Production Tip

Package Charts only after validating them with:

```bash
helm lint
```

---

## Interview Question

### Question

How do you package a Helm Chart?

### Answer

Use:

```bash
helm package <chart-directory>
```

---

# 13. Validating a Helm Chart

## Answer

Before deploying a Chart, validate it for common errors.

---

## Lint Command

```bash
helm lint .
```

---

## Example Output

```text
1 chart(s) linted

0 chart(s) failed
```

---

## Why use helm lint?

It checks for:

- YAML syntax errors
- Missing metadata
- Invalid templates
- Common Chart mistakes

---

## Production Example

A CI/CD pipeline runs:

```bash
helm lint .
```

before every deployment to prevent invalid Charts from reaching production.

---

## Production Tip

Treat `helm lint` as a mandatory quality check in your deployment pipeline.

---

## Interview Question

### Question

What is the purpose of `helm lint`?

### Answer

`helm lint` validates a Helm Chart and detects common syntax and template errors before deployment.

---

# 14. Common Beginner Mistakes

## Mistake 1

❌ Hardcoding values inside templates.

Always use `values.yaml`.

---

## Mistake 2

❌ Editing generated Kubernetes resources manually after deployment.

Manage resources through Helm instead.

---

## Mistake 3

❌ Ignoring `helm lint`.

Always validate Charts before deployment.

---

## Mistake 4

❌ Storing sensitive values directly in `values.yaml`.

Use Kubernetes Secrets or external secret management solutions.

---

## Mistake 5

❌ Mixing multiple unrelated applications inside one Chart.

Create one Chart per logical application.

---

# 15. Production Best Practices

Always follow these recommendations.

---

✅ Keep Charts small and modular.

---

✅ Follow Semantic Versioning for Chart versions.

---

✅ Store Charts in Git.

---

✅ Validate Charts using:

```bash
helm lint
```

---

✅ Test rendered templates before deployment.

---

✅ Use environment-specific values files.

---

✅ Package and publish Charts through CI/CD pipelines.

---

## 📌 Quick Revision

### Helm Chart Structure

```text
Chart.yaml

↓

values.yaml

↓

templates/

↓

_helpers.tpl

↓

charts/

↓

.helmignore
```

---

### Helm Chart Lifecycle

```text
helm create

↓

Modify Templates

↓

helm lint

↓

helm package

↓

helm install

↓

helm upgrade
```

---

### Most Common Commands

```bash
helm create

helm lint

helm package

helm install

helm upgrade

helm rollback

helm uninstall
```

---

# Summary

After completing this guide, you should understand:

✅ Helm Charts

✅ Chart Structure

✅ Chart.yaml

✅ values.yaml

✅ templates/

✅ _helpers.tpl

✅ charts/

✅ .helmignore

✅ Creating Charts

✅ Packaging Charts

✅ Installing Local Charts

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is a Helm Chart? | A package containing Kubernetes resources |
| Which file stores Chart metadata? | `Chart.yaml` |
| Which file stores default values? | `values.yaml` |
| Where are Kubernetes templates stored? | `templates/` |
| Which file contains reusable template functions? | `_helpers.tpl` |
| Which directory stores dependency Charts? | `charts/` |
| How do you create a Chart? | `helm create <chart-name>` |
| How do you validate a Chart? | `helm lint` |
| How do you package a Chart? | `helm package <chart-directory>` |
| How do you install a local Chart? | `helm install <release-name> <chart-directory>` |

---

# What's Next?

➡️ **03-helm-values.md**

In the next guide, you'll learn:

- What is `values.yaml`?
- Default Values
- Overriding Values
- Command-Line Values
- Multiple Values Files
- Environment-Specific Configuration
- Best Practices
- Production Examples
- Common Interview Questions