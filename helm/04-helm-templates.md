# 🚀 Helm Templates - Creating Dynamic Kubernetes Manifests

This guide explains everything you need to know about Helm Templates.

If you've ever wondered:

- What are Helm Templates?
- Why do we need templates?
- How does Helm generate Kubernetes YAML?
- What is Go Template syntax?
- How do templates work with values.yaml?

This guide is for you.

---

# 📑 Table of Contents

1. What are Helm Templates?
2. Why do we need Templates?
3. How Helm Templates Work
4. Understanding Go Template Syntax
5. Common Interview Questions

---

# 1. What are Helm Templates?

## Answer

A **Helm Template** is a Kubernetes manifest that contains **dynamic placeholders** instead of hardcoded values.

When Helm installs or upgrades a Chart, it replaces these placeholders with actual values from `values.yaml` or values supplied during deployment.

The final result is a standard Kubernetes YAML manifest.

---

## Why do we need Templates?

Imagine deploying the same application in multiple environments.

Development

```text
Replicas = 1

Image = nginx:latest
```

Production

```text
Replicas = 5

Image = nginx:1.27
```

Without templates, you would need separate Deployment YAML files.

With templates, one Deployment template works for every environment.

---

## Internal Workflow

```text
Template

        │

Contains Variables

        │

        ▼

values.yaml

        │

Replace Variables

        │

        ▼

Generated Kubernetes YAML

        │

        ▼

Kubernetes Cluster
```

---

## Real World Example

A Deployment template contains:

```yaml
replicas: {{ .Values.replicaCount }}
```

Development generates:

```yaml
replicas: 1
```

Production generates:

```yaml
replicas: 5
```

The template never changes.

---

## Benefits

Helm Templates provide:

- Reusable YAML
- Dynamic Configuration
- Environment Flexibility
- Reduced Duplication
- Easier Maintenance

---

## Production Tip

Keep templates generic and reusable.

Environment-specific values should always come from `values.yaml`.

---

## Interview Question

### Question

What is a Helm Template?

### Answer

A Helm Template is a Kubernetes manifest containing template expressions that Helm replaces with actual values during chart rendering.

---

# 2. Why do we need Templates?

## Answer

Without templates, Kubernetes manifests quickly become difficult to manage.

Consider three environments:

```text
Development

QA

Production
```

Each environment has different:

- Replica Counts
- Image Tags
- Resource Limits
- Service Types

Without templates, each environment requires separate YAML files.

---

## Without Templates

```text
deployment-dev.yaml

deployment-qa.yaml

deployment-prod.yaml
```

---

## With Templates

```text
deployment.yaml

↓

Reusable Template

↓

values-dev.yaml

values-qa.yaml

values-prod.yaml
```

One template supports every environment.

---

## Production Example

A single Deployment template is reused across all environments.

Only the values files change.

---

## Production Tip

Templates reduce maintenance effort because changes are made in one place instead of multiple YAML files.

---

## Interview Question

### Question

Why do we need Helm Templates?

### Answer

Helm Templates allow one Kubernetes manifest to be reused across multiple environments by replacing dynamic values during deployment.

---

# 3. How Helm Templates Work

## Answer

When a Helm Chart is installed, Helm performs several steps automatically.

---

## Internal Workflow

```text
helm install

        │

Read Chart

        │

Read values.yaml

        │

Read Templates

        │

Replace Variables

        │

Generate Kubernetes YAML

        │

Deploy to Kubernetes
```

---

## Example

Template

```yaml
spec:

  replicas: {{ .Values.replicaCount }}
```

values.yaml

```yaml
replicaCount: 3
```

Generated YAML

```yaml
spec:

  replicas: 3
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

The same Deployment template generates different Kubernetes manifests.

---

## Production Tip

Helm renders templates before sending them to the Kubernetes API Server.

Kubernetes receives standard YAML, not Helm template syntax.

---

## Interview Question

### Question

How do Helm Templates work?

### Answer

Helm reads values from `values.yaml`, replaces template expressions inside Kubernetes manifests, generates standard YAML, and then deploys it to Kubernetes.

---

# 4. Understanding Go Template Syntax

## Answer

Helm Templates use the **Go Template** language.

Template expressions are enclosed within:

```text
{{ }}
```

Everything inside these braces is evaluated by Helm.

---

## Example

Template

```yaml
metadata:

  name: {{ .Release.Name }}
```

Generated YAML

```yaml
metadata:

  name: shopping-app
```

---

## Reading Values

```yaml
{{ .Values.replicaCount }}
```

---

## Reading Chart Information

```yaml
{{ .Chart.Name }}
```

---

## Reading Release Information

```yaml
{{ .Release.Name }}
```

---

## Internal Workflow

```text
Go Template

↓

Read Object

↓

Replace Value

↓

Generated YAML
```

---

## Production Example

A Deployment template uses:

```yaml
image:

  repository: {{ .Values.image.repository }}

  tag: {{ .Values.image.tag }}
```

Different environments automatically deploy different image versions.

---

## Production Tip

Learn the basic Go Template syntax before working with advanced Helm features such as loops and conditional statements.

---

## Interview Question

### Question

What syntax do Helm Templates use?

### Answer

Helm Templates use the Go Template language, where template expressions are written inside `{{ }}`.

---

# 5. Built-in Objects

## Answer

Helm provides several built-in objects that templates can access during rendering.

These objects provide information about:

- Chart
- Release
- Values
- Files
- Kubernetes Capabilities

---

## Common Built-in Objects

| Object | Purpose |
|---------|---------|
| `.Values` | Configuration values from `values.yaml` |
| `.Release` | Release information |
| `.Chart` | Chart metadata |
| `.Files` | Files inside the chart |
| `.Capabilities` | Kubernetes cluster capabilities |

---

## Example

```yaml
metadata:
  name: {{ .Release.Name }}
```

---

## Read Chart Name

```yaml
{{ .Chart.Name }}
```

---

## Read Application Version

```yaml
{{ .Chart.AppVersion }}
```

---

## Read Values

```yaml
{{ .Values.replicaCount }}
```

---

## Internal Workflow

```text
Helm Chart

        │

Built-in Objects

        │

────────────┼────────────

        │

.Values

.Release

.Chart

.Files

        │

        ▼

Generated YAML
```

---

## Production Example

A production Deployment template automatically generates resource names using:

```yaml
{{ .Release.Name }}
```

This avoids hardcoding names.

---

## Production Tip

Use built-in objects instead of fixed values to make Charts reusable.

---

## Interview Question

### Question

What are Helm Built-in Objects?

### Answer

Built-in Objects provide information about the Chart, Release, Values, Files, and Kubernetes environment during template rendering.

---

# 6. Variables

## Answer

Variables allow you to store values temporarily inside a template.

This avoids repeating the same expression multiple times.

---

## Example

```yaml
{{- $appName := .Chart.Name }}

metadata:
  name: {{ $appName }}
```

---

## Internal Workflow

```text
Read Chart Name

        │

Store in Variable

        │

Reuse Variable

        │

Generated YAML
```

---

## Production Example

Instead of writing:

```yaml
{{ .Release.Name }}
```

multiple times, store it in a variable and reuse it throughout the template.

---

## Production Tip

Use variables to improve template readability and reduce duplication.

---

## Interview Question

### Question

How do you declare a variable in a Helm template?

### Answer

Variables are declared using the `:=` operator.

Example:

```yaml
{{- $name := .Chart.Name }}
```

---

# 7. Pipelines

## Answer

A **Pipeline** passes the output of one function into another.

It is represented using the pipe (`|`) operator.

This makes templates easier to read.

---

## Example

```yaml
{{ .Values.name | upper }}
```

If:

```yaml
name: shopping-app
```

Generated Output

```text
SHOPPING-APP
```

---

## Internal Workflow

```text
Read Value

        │

Pass to Function

        │

Generate Output
```

---

## Production Example

Convert resource names to uppercase before using them in labels or annotations.

---

## Production Tip

Use pipelines to chain multiple functions together for cleaner templates.

---

## Interview Question

### Question

What is a Pipeline in Helm?

### Answer

A Pipeline passes the output of one template expression into another function using the `|` operator.

---

# 8. Common Template Functions

## Answer

Helm includes many built-in template functions.

Some of the most commonly used are:

- `default`
- `quote`
- `upper`
- `lower`
- `trim`

---

## default

Provides a fallback value if one is not supplied.

Example

```yaml
{{ .Values.replicaCount | default 1 }}
```

---

## quote

Wraps a value in double quotes.

Example

```yaml
{{ .Values.image.tag | quote }}
```

Generated Output

```text
"1.27"
```

---

## upper

Converts text to uppercase.

```yaml
{{ .Values.name | upper }}
```

---

## lower

Converts text to lowercase.

```yaml
{{ .Values.name | lower }}
```

---

## trim

Removes leading and trailing spaces.

```yaml
{{ .Values.name | trim }}
```

---

## Production Example

If no replica count is provided:

```yaml
replicas: {{ .Values.replicaCount | default 2 }}
```

The application automatically uses:

```text
2 Replicas
```

---

## Production Tip

Use the `default` function to make Charts more resilient when optional values are omitted.

---

## Interview Question

### Question

What does the `default` function do in Helm?

### Answer

The `default` function returns a fallback value if the requested value is empty or undefined.

---

# 9. Template Rendering Example

## Answer

Let's see how Helm renders a template.

---

## values.yaml

```yaml
replicaCount: 3

image:
  repository: nginx
  tag: "1.27"
```

---

## deployment.yaml

```yaml
spec:

  replicas: {{ .Values.replicaCount }}

containers:

- image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

---

## Generated YAML

```yaml
spec:

  replicas: 3

containers:

- image: "nginx:1.27"
```

Helm replaces all template expressions before sending the manifest to Kubernetes.

---

## Production Tip

Always test rendered templates before deployment.

Use:

```bash
helm template .
```

to preview the generated YAML without installing the Chart.

---

## Interview Question

### Question

How can you preview the rendered Kubernetes manifests?

### Answer

Use:

```bash
helm template .
```

This generates the Kubernetes YAML locally without deploying it.

---

## 📌 Quick Revision

| Feature | Example |
|---------|---------|
| Read Value | `.Values.replicaCount` |
| Read Chart Name | `.Chart.Name` |
| Read Release Name | `.Release.Name` |
| Variable | `$name := .Chart.Name` |
| Pipeline | `\| upper` |
| Default Value | `default 1` |
| Preview YAML | `helm template .` |

---

# 10. if Conditions

## Answer

Helm supports conditional logic using the **if** statement.

This allows templates to create Kubernetes resources only when specific conditions are true.

---

## Why do we need if Conditions?

Suppose an application uses Ingress only in Production.

Development

```text
Ingress Disabled
```

Production

```text
Ingress Enabled
```

Instead of maintaining two separate templates, Helm conditionally creates the Ingress resource.

---

## Example

values.yaml

```yaml
ingress:
  enabled: true
```

Template

```yaml
{{- if .Values.ingress.enabled }}

apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: my-ingress

{{- end }}
```

---

## Internal Workflow

```text
Read Value

↓

ingress.enabled

↓

Is True?

↓

Yes

↓

Generate Ingress

--------------------

No

↓

Skip Resource
```

---

## Production Example

Development:

```yaml
ingress:
  enabled: false
```

Production:

```yaml
ingress:
  enabled: true
```

The same Chart works for both environments.

---

## Production Tip

Use conditional logic for optional resources such as:

- Ingress
- Autoscaling
- ServiceAccount
- NetworkPolicy

---

## Interview Question

### Question

Why are `if` conditions used in Helm?

### Answer

`if` conditions allow Helm to generate Kubernetes resources only when specified configuration values are enabled.

---

# 11. range Loops

## Answer

The **range** keyword is used to iterate over lists and maps.

Instead of repeating template code manually, Helm automatically generates resources for each item.

---

## Example

values.yaml

```yaml
ports:

- 80

- 443
```

Template

```yaml
ports:

{{- range .Values.ports }}

- containerPort: {{ . }}

{{- end }}
```

---

## Generated YAML

```yaml
ports:

- containerPort: 80

- containerPort: 443
```

---

## Internal Workflow

```text
Read List

↓

Loop Through Items

↓

Generate YAML

↓

Deploy
```

---

## Production Example

A container exposes multiple ports.

Instead of writing each port manually, Helm generates them automatically using `range`.

---

## Production Tip

Use `range` whenever you need to generate multiple resources or repeated sections from lists.

---

## Interview Question

### Question

What is the purpose of `range` in Helm?

### Answer

`range` iterates over lists or maps and generates template output for each item.

---

# 12. include and template

## Answer

Large Helm Charts often reuse common template logic.

Instead of duplicating code, Helm provides:

- `include`
- `template`

Both allow reusable template definitions from `_helpers.tpl`.

---

## Example

_helpers.tpl

```yaml
{{- define "shopping.fullname" -}}

{{ .Release.Name }}-{{ .Chart.Name }}

{{- end }}
```

Template

```yaml
metadata:

  name: {{ include "shopping.fullname" . }}
```

---

## Generated YAML

```yaml
metadata:

  name: shopping-prod-shopping
```

---

## Internal Workflow

```text
Template

↓

Call include

↓

Read Helper

↓

Generate Output

↓

Insert into YAML
```

---

## Production Example

Instead of manually creating labels and names in every resource, helper templates generate them consistently across the Chart.

---

## Production Tip

Store reusable naming conventions, labels, and annotations inside `_helpers.tpl`.

---

## Interview Question

### Question

What is the difference between `include` and `template`?

### Answer

Both reuse helper templates. `include` returns the rendered output so it can be used in pipelines, while `template` writes the rendered output directly into the template.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Hardcoding values inside templates.

Always use `.Values`.

---

## Mistake 2

❌ Duplicating template code.

Move reusable logic into `_helpers.tpl`.

---

## Mistake 3

❌ Forgetting to validate templates.

Always run:

```bash
helm lint .
```

before deployment.

---

## Mistake 4

❌ Deploying without previewing templates.

Use:

```bash
helm template .
```

to inspect the generated YAML.

---

## Mistake 5

❌ Using complex template logic.

Keep templates simple and move application logic into the application itself whenever possible.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Keep templates clean and readable.

---

✅ Store configuration inside `values.yaml`.

---

✅ Use helper templates for reusable code.

---

✅ Use `if` for optional resources.

---

✅ Use `range` for lists.

---

✅ Preview generated YAML using:

```bash
helm template .
```

---

✅ Validate Charts using:

```bash
helm lint .
```

---

## 📌 Quick Revision

### Helm Template Workflow

```text
values.yaml

↓

Templates

↓

Replace Variables

↓

Apply Conditions

↓

Generate YAML

↓

Kubernetes Cluster
```

---

### Template Features

```text
.Values

↓

Variables

↓

if

↓

range

↓

include

↓

Generated YAML
```

---

### Common Commands

```bash
helm template .

helm lint .

helm install myapp .

helm upgrade myapp .

helm rollback myapp 1
```

---

# Summary

After completing this guide, you should understand:

✅ Helm Templates

✅ Go Template Syntax

✅ Built-in Objects

✅ Variables

✅ Pipelines

✅ Template Functions

✅ if Conditions

✅ range Loops

✅ include

✅ template

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What language do Helm templates use? | Go Template |
| How do templates read configuration? | `.Values` |
| How do you create optional resources? | `if` |
| How do you loop through a list? | `range` |
| Which object stores Chart metadata? | `.Chart` |
| Which object stores Release information? | `.Release` |
| How do you reuse template logic? | `include` or `template` |
| How do you preview generated YAML? | `helm template .` |
| How do you validate templates? | `helm lint .` |
| Where should reusable helper functions be stored? | `_helpers.tpl` |

---

# What's Next?

➡️ **05-helm-release-management.md**

In the next guide, you'll learn:

- What is a Helm Release?
- Release Lifecycle
- `helm install`
- `helm list`
- `helm status`
- `helm history`
- `helm upgrade`
- `helm rollback`
- `helm uninstall`
- Production Best Practices
- Common Interview Questions