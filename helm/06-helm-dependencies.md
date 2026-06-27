# 🚀 Helm Dependencies - Managing Parent & Child Charts

This guide explains everything you need to know about Helm Dependencies.

If you've ever wondered:

- What are Helm Dependencies?
- Why do we need Dependencies?
- What is a Parent Chart?
- What is a Child Chart?
- How does Helm manage dependent Charts?

This guide is for you.

---

# 📑 Table of Contents

1. What are Helm Dependencies?
2. Why do we need Dependencies?
3. Parent & Child Charts
4. Dependency Architecture
5. Common Interview Questions

---

# 1. What are Helm Dependencies?

## Answer

Helm **Dependencies** allow one Helm Chart to include and manage other Helm Charts.

Instead of creating everything from scratch, you can reuse existing Charts such as:

- MySQL
- PostgreSQL
- Redis
- RabbitMQ
- NGINX
- Prometheus

These Charts become **dependencies** of your application.

---

## Why do we need Dependencies?

Imagine you're deploying an e-commerce application.

The application requires:

- Frontend
- Backend API
- MySQL Database
- Redis Cache

Without Dependencies:

```text
Shopping Chart

↓

Manually Create

Deployment

Service

Database

Redis

Configuration
```

Everything must be managed separately.

With Dependencies:

```text
Shopping Chart

↓

Uses

MySQL Chart

Redis Chart

↓

Single Deployment
```

---

## Internal Workflow

```text
Parent Chart

        │

Dependencies

────────────┼────────────

        │

MySQL Chart

Redis Chart

        │

Deploy Together

        ▼

Kubernetes Cluster
```

---

## Real World Example

A company deploys:

```text
Shopping Application

↓

Depends On

↓

MySQL

↓

Redis
```

Helm automatically installs all required components during deployment.

---

## Benefits

Helm Dependencies provide:

- Reusable Components
- Modular Applications
- Easier Maintenance
- Simplified Deployments
- Consistent Configurations

---

## Production Tip

Reuse trusted community Charts whenever possible instead of maintaining custom implementations for common services.

---

## Interview Question

### Question

What are Helm Dependencies?

### Answer

Helm Dependencies allow a Helm Chart to include and deploy other Helm Charts as part of a single application.

---

# 2. Why do we need Dependencies?

## Answer

Modern applications rarely consist of a single component.

A production application may require:

- Web Application
- Database
- Cache
- Message Queue
- Monitoring

Managing each component independently becomes complex.

Helm Dependencies simplify this process.

---

## Without Dependencies

```text
Application

↓

Deploy Database

↓

Deploy Redis

↓

Deploy Monitoring

↓

Deploy Application
```

Each component requires separate deployment steps.

---

## With Dependencies

```text
Parent Chart

↓

Contains Dependencies

↓

Single Helm Install

↓

Entire Application Stack
```

---

## Production Example

A DevOps engineer deploys a complete application stack using one command:

```bash
helm install shopping-app .
```

Helm automatically installs:

- Application
- MySQL
- Redis

---

## Production Tip

Keep your application Chart focused on business logic while using dependencies for shared infrastructure components.

---

## Interview Question

### Question

Why do we need Helm Dependencies?

### Answer

Helm Dependencies simplify deployments by allowing multiple related Charts to be installed and managed together.

---

# 3. Parent & Child Charts

## Answer

A Helm Dependency relationship consists of:

- Parent Chart
- Child Charts

The Parent Chart manages the deployment of all Child Charts.

---

## Example Structure

```text
shopping-app/

├── Chart.yaml
├── values.yaml
├── templates/
└── charts/
    ├── mysql/
    └── redis/
```

Here:

- `shopping-app` → Parent Chart
- `mysql` → Child Chart
- `redis` → Child Chart

---

## Internal Workflow

```text
Parent Chart

        │

────────────┼────────────

        │

Child Chart

(MySQL)

Child Chart

(Redis)

        │

Deploy Together
```

---

## Production Example

A blogging platform includes:

```text
Blog Application

↓

PostgreSQL

↓

Redis

↓

NGINX
```

Each supporting service is managed as a Child Chart.

---

## Production Tip

Keep the Parent Chart lightweight and use Child Charts for reusable services.

---

## Interview Question

### Question

What is the difference between a Parent Chart and a Child Chart?

### Answer

A Parent Chart manages the deployment, while Child Charts are dependency Charts deployed as part of the Parent Chart.

---

# 4. Dependency Architecture

## Answer

Helm resolves dependencies before rendering templates.

When the Parent Chart is installed:

1. Dependencies are downloaded.
2. Dependency templates are rendered.
3. Parent templates are rendered.
4. All Kubernetes manifests are generated.
5. Resources are deployed together.

---

## Architecture

```text
Developer

        │

helm install

        │

        ▼

Parent Chart

        │

Read Dependencies

        │

────────────┼────────────

        │

Redis Chart

MySQL Chart

        │

Render Templates

        │

Generate Kubernetes YAML

        ▼

Deploy to Cluster
```

---

## Production Example

Installing an application automatically provisions:

- Application Pods
- Database Pods
- Cache Pods
- Services
- Persistent Storage

without requiring separate Helm commands.

---

## Production Tip

Always verify dependency versions before upgrading your Parent Chart to avoid compatibility issues.

---

## Interview Question

### Question

How does Helm process dependencies?

### Answer

Helm downloads dependency Charts, renders their templates along with the Parent Chart, generates Kubernetes manifests, and deploys all resources together.

---

# 5. Defining Dependencies in Chart.yaml

## Answer

Helm Dependencies are defined inside the **Chart.yaml** file.

Each dependency specifies:

- Chart Name
- Version
- Repository

When Helm processes the Parent Chart, it downloads these dependency Charts automatically.

---

## Example

```yaml
apiVersion: v2

name: shopping-app

version: 1.0.0

dependencies:

- name: mysql
  version: "9.10.0"
  repository: "https://charts.bitnami.com/bitnami"

- name: redis
  version: "18.0.0"
  repository: "https://charts.bitnami.com/bitnami"
```

---

## Internal Workflow

```text
Chart.yaml

        │

Read Dependencies

        │

────────────┼────────────

        │

MySQL

Redis

        │

Download Charts

        ▼

charts/
```

---

## Production Example

A DevOps engineer defines MySQL and Redis as dependencies.

Running a dependency update automatically downloads both Charts into the project.

---

## Production Tip

Always specify exact dependency versions instead of using floating versions to ensure predictable deployments.

---

## Interview Question

### Question

Where are Helm Dependencies defined?

### Answer

Helm Dependencies are defined in the `dependencies` section of the `Chart.yaml` file.

---

# 6. The charts/ Directory

## Answer

Downloaded dependency Charts are stored in the **charts/** directory.

This directory contains all Child Charts required by the Parent Chart.

---

## Example Structure

```text
shopping-app/

├── Chart.yaml
├── values.yaml
├── templates/
└── charts/
    ├── mysql-9.10.0.tgz
    └── redis-18.0.0.tgz
```

---

## Internal Workflow

```text
Chart.yaml

        │

Dependencies

        │

Download

        │

        ▼

charts/

        │

Render Templates

        ▼

Deploy
```

---

## Production Example

A CI/CD pipeline downloads dependency Charts before packaging the Parent Chart.

The packaged Chart contains everything required for deployment.

---

## Production Tip

Do not manually edit packaged dependency Charts inside the `charts/` directory.

Update the dependency version in `Chart.yaml` instead.

---

## Interview Question

### Question

What is the purpose of the `charts/` directory?

### Answer

The `charts/` directory stores dependency Charts that are deployed together with the Parent Chart.

---

# 7. helm dependency update

## Answer

The `helm dependency update` command downloads or updates dependency Charts based on the `Chart.yaml` file.

---

## Command

```bash
helm dependency update
```

---

## Internal Workflow

```text
Read Chart.yaml

        │

Read Dependency List

        │

Download Latest Matching Versions

        │

Store in charts/

        ▼

Update Chart.lock
```

---

## Production Example

After adding Redis as a dependency:

```bash
helm dependency update
```

Helm downloads the Redis Chart and stores it in the `charts/` directory.

---

## Production Tip

Run `helm dependency update` whenever dependency versions change.

---

## Interview Question

### Question

What does `helm dependency update` do?

### Answer

It downloads or updates dependency Charts based on the `Chart.yaml` configuration and refreshes the dependency lock file.

---

# 8. helm dependency build

## Answer

The `helm dependency build` command rebuilds the `charts/` directory using the existing **Chart.lock** file.

Unlike `helm dependency update`, it does not fetch newer compatible versions.

---

## Command

```bash
helm dependency build
```

---

## Internal Workflow

```text
Chart.lock

        │

Read Locked Versions

        │

Download Exact Versions

        │

Populate charts/

        ▼

Ready for Deployment
```

---

## Difference Between Update and Build

| Command | Purpose |
|----------|---------|
| `helm dependency update` | Downloads the latest matching dependency versions and updates `Chart.lock` |
| `helm dependency build` | Rebuilds dependencies using the versions already recorded in `Chart.lock` |

---

## Production Example

A CI/CD pipeline uses:

```bash
helm dependency build
```

to ensure every deployment uses the exact dependency versions that were previously tested.

---

## Production Tip

Use `helm dependency build` in automated pipelines to guarantee repeatable and consistent deployments.

---

## Interview Question

### Question

What is the difference between `helm dependency update` and `helm dependency build`?

### Answer

`helm dependency update` downloads the latest matching dependency versions and updates the lock file, while `helm dependency build` installs the exact versions specified in `Chart.lock`.

---

# 9. Dependency Versioning

## Answer

Each dependency has its own version.

Helm uses these versions to ensure compatibility between the Parent Chart and Child Charts.

---

## Example

```yaml
dependencies:

- name: mysql
  version: "9.10.0"

- name: redis
  version: "18.0.0"
```

---

## Internal Workflow

```text
Chart.yaml

        │

Dependency Version

        │

Download Matching Chart

        │

Deploy Compatible Version
```

---

## Production Example

Instead of always downloading the newest MySQL Chart, the Parent Chart specifies a tested version.

This prevents unexpected behavior caused by incompatible updates.

---

## Production Tip

Pin dependency versions in production environments and upgrade them only after proper testing.

---

## Interview Question

### Question

Why should dependency versions be specified explicitly?

### Answer

Specifying exact versions ensures consistent, predictable, and reproducible deployments across different environments.

---

## 📌 Quick Revision

| Feature | Purpose |
|---------|---------|
| `dependencies` | Define Child Charts |
| `charts/` | Stores downloaded dependency Charts |
| `helm dependency update` | Download or update dependencies |
| `helm dependency build` | Rebuild dependencies from `Chart.lock` |
| Dependency Version | Controls which Chart version is used |

---

# 10. Understanding Chart.lock

## Answer

When Helm downloads dependencies, it generates a **Chart.lock** file.

The `Chart.lock` file records the exact versions of all dependency Charts used during dependency resolution.

This ensures that every deployment uses the same tested dependency versions.

---

## Why do we need Chart.lock?

Imagine your application depends on:

- MySQL
- Redis

Today:

```text
MySQL 9.10.0

Redis 18.0.0
```

Tomorrow, newer versions become available.

Without a lock file, Helm could download newer versions that may introduce compatibility issues.

With `Chart.lock`, Helm always installs the tested versions.

---

## Example

```yaml
dependencies:

- name: mysql
  repository: https://charts.bitnami.com/bitnami
  version: 9.10.0

- name: redis
  repository: https://charts.bitnami.com/bitnami
  version: 18.0.0

digest: sha256:xxxxxxxxxxxxxxxx

generated: "2026-01-20T10:15:00Z"
```

---

## Internal Workflow

```text
Chart.yaml

        │

Dependency Resolution

        │

Generate Chart.lock

        │

Store Exact Versions

        │

Future Builds Use Same Versions
```

---

## Production Example

A CI/CD pipeline runs:

```bash
helm dependency build
```

Helm reads the versions from `Chart.lock`, ensuring that every deployment is identical across Development, QA, and Production.

---

## Production Tip

Commit `Chart.lock` to your Git repository to guarantee reproducible deployments.

---

## Interview Question

### Question

What is the purpose of `Chart.lock`?

### Answer

`Chart.lock` stores the exact dependency versions resolved by Helm, ensuring consistent and repeatable deployments.

---

# 11. Dependency Conditions

## Answer

Sometimes a dependency should only be installed in specific environments.

Helm supports **conditions** to enable or disable dependencies based on values.

---

## Example

Chart.yaml

```yaml
dependencies:

- name: redis
  version: "18.0.0"
  repository: "https://charts.bitnami.com/bitnami"
  condition: redis.enabled
```

values.yaml

```yaml
redis:

  enabled: true
```

---

## Internal Workflow

```text
Read values.yaml

        │

redis.enabled

        │

────────────┼────────────

        │

true

↓

Install Redis

false

↓

Skip Redis
```

---

## Production Example

Development

```yaml
redis:

  enabled: false
```

Production

```yaml
redis:

  enabled: true
```

The same Parent Chart behaves differently based on configuration.

---

## Production Tip

Use dependency conditions for optional components such as Redis, monitoring stacks, or message queues.

---

## Interview Question

### Question

What is a dependency condition in Helm?

### Answer

A dependency condition allows a Child Chart to be enabled or disabled based on values defined in `values.yaml`.

---

# 12. Dependency Tags

## Answer

**Tags** allow multiple dependencies to be grouped together and enabled or disabled as a unit.

This is useful when several components belong to the same feature.

---

## Example

Chart.yaml

```yaml
dependencies:

- name: prometheus
  version: "25.0.0"
  repository: "https://prometheus-community.github.io/helm-charts"
  tags:
    - monitoring

- name: grafana
  version: "8.0.0"
  repository: "https://grafana.github.io/helm-charts"
  tags:
    - monitoring
```

values.yaml

```yaml
tags:

  monitoring: true
```

---

## Internal Workflow

```text
Monitoring Tag

        │

────────────┼────────────

        │

Prometheus

Grafana

        │

Deploy Together
```

---

## Production Example

Enable the monitoring stack only in Production while keeping it disabled in Development.

---

## Production Tip

Use tags to simplify the management of related dependencies.

---

## Interview Question

### Question

What are dependency tags in Helm?

### Answer

Dependency tags group multiple dependency Charts together so they can be enabled or disabled using a single configuration value.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Copying dependency Charts manually into the `charts/` directory.

Always manage dependencies through the `dependencies` section in `Chart.yaml`.

---

## Mistake 2

❌ Using floating dependency versions.

Specify tested versions to ensure predictable deployments.

---

## Mistake 3

❌ Ignoring `Chart.lock`.

Always commit `Chart.lock` to version control.

---

## Mistake 4

❌ Bundling unrelated services as dependencies.

Only include components that are logically part of the application.

---

## Mistake 5

❌ Forgetting to update dependencies after modifying `Chart.yaml`.

Run:

```bash
helm dependency update
```

to synchronize the project.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Define dependencies in `Chart.yaml`.

---

✅ Commit `Chart.lock` to Git.

---

✅ Use exact dependency versions.

---

✅ Test dependency upgrades in Development before Production.

---

✅ Use dependency conditions for optional services.

---

✅ Use tags to manage related components.

---

✅ Rebuild dependencies in CI/CD using:

```bash
helm dependency build
```

---

## 📌 Quick Revision

### Dependency Workflow

```text
Chart.yaml

↓

Define Dependencies

↓

helm dependency update

↓

Download Charts

↓

Generate Chart.lock

↓

helm install

↓

Deploy Parent + Child Charts
```

---

### Most Common Commands

```bash
helm dependency update

helm dependency build

helm install

helm upgrade
```

---

### Key Files

```text
Chart.yaml

↓

Dependency Definitions

---------------------

Chart.lock

↓

Locked Dependency Versions

---------------------

charts/

↓

Downloaded Dependency Charts
```

---

# Summary

After completing this guide, you should understand:

✅ Helm Dependencies

✅ Parent & Child Charts

✅ Dependency Architecture

✅ `dependencies` in `Chart.yaml`

✅ `charts/` Directory

✅ `helm dependency update`

✅ `helm dependency build`

✅ `Chart.lock`

✅ Dependency Conditions

✅ Dependency Tags

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What are Helm Dependencies? | Child Charts managed by a Parent Chart |
| Where are dependencies defined? | `Chart.yaml` |
| Where are downloaded dependencies stored? | `charts/` |
| Which command downloads dependencies? | `helm dependency update` |
| Which command rebuilds dependencies from the lock file? | `helm dependency build` |
| What is `Chart.lock`? | A file containing the exact resolved dependency versions |
| What is a dependency condition? | A setting that enables or disables a dependency |
| What are dependency tags? | A way to group multiple dependencies under one configuration |
| Should `Chart.lock` be committed to Git? | Yes |
| Why pin dependency versions? | To ensure reproducible and stable deployments |

---

# What's Next?

➡️ **07-helm-hooks.md**

In the next guide, you'll learn:

- What are Helm Hooks?
- Hook Lifecycle
- Pre-install Hooks
- Post-install Hooks
- Pre-upgrade Hooks
- Post-upgrade Hooks
- Pre-delete Hooks
- Hook Weights
- Hook Delete Policies
- Production Best Practices
- Common Interview Questions