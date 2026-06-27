# 🚀 Kubernetes ConfigMaps & Secrets - Managing Configuration Securely

This guide explains everything you need to know about Kubernetes ConfigMaps and Secrets.

If you've ever wondered:

- What is a ConfigMap?
- What is a Secret?
- Why shouldn't we hardcode configuration?
- How do ConfigMaps and Secrets work?
- When should we use ConfigMaps vs Secrets?

This guide is for you.

---

# 📑 Table of Contents

1. What is a ConfigMap?
2. Why do we need ConfigMaps?
3. What is a Secret?
4. ConfigMap vs Secret
5. Common Interview Questions

---

# 1. What is a ConfigMap?

## Answer

A **ConfigMap** is a Kubernetes resource used to store **non-sensitive configuration data** separately from your application.

Instead of hardcoding configuration values inside your application or Docker image, you store them in a ConfigMap.

Your application reads these values at runtime.

---

## Why do we need ConfigMaps?

Imagine your application contains:

```text
Database Host

Redis Host

API URL

Application Name

Log Level
```

If these values are hardcoded inside the application:

- Every environment requires rebuilding the image.
- Configuration changes become difficult.
- The same image cannot be reused across environments.

ConfigMaps solve this problem by separating configuration from application code.

---

## Without ConfigMap

```text
Application

↓

Hardcoded Configuration

↓

Rebuild Docker Image

↓

Deploy Again
```

---

## With ConfigMap

```text
Application

        │

        ▼

ConfigMap

        │

Provides Configuration

        │

        ▼

Application Starts
```

The same application image can now run in Development, QA, and Production using different ConfigMaps.

---

## Real World Example

Suppose an application connects to different databases.

Development:

```text
dev-db.company.com
```

Production:

```text
prod-db.company.com
```

Instead of creating two Docker images, you create two ConfigMaps.

The application reads the appropriate configuration during startup.

---

## Benefits

ConfigMaps provide:

- Environment-specific Configuration
- Easier Maintenance
- Reusable Docker Images
- Centralized Configuration
- Simplified Updates

---

## Production Tip

Use ConfigMaps only for **non-sensitive** information.

Never store passwords, API keys, or tokens inside ConfigMaps.

---

## Interview Question

### Question

What is a ConfigMap?

### Answer

A ConfigMap is a Kubernetes resource used to store non-sensitive configuration data separately from application code and container images.

---

# 2. Why do we need ConfigMaps?

## Answer

Applications often run in multiple environments.

For example:

Development

QA

Production

Each environment usually has different:

- Database Servers
- API URLs
- Log Levels
- Feature Flags
- Application Settings

Hardcoding these values creates maintenance challenges.

ConfigMaps allow applications to receive environment-specific configuration without changing the application code.

---

## Without ConfigMaps

```text
Development

↓

Build Docker Image

↓

QA

↓

Build Docker Image

↓

Production

↓

Build Docker Image
```

Multiple images must be maintained.

---

## With ConfigMaps

```text
One Docker Image

        │

────────┼────────

        │

Dev ConfigMap

QA ConfigMap

Prod ConfigMap

        │

Application Starts
```

The same image works everywhere.

---

## Production Example

Application:

```text
Spring Boot API
```

Development Config:

```text
LOG_LEVEL=DEBUG
```

Production Config:

```text
LOG_LEVEL=ERROR
```

Only the ConfigMap changes.

The application image remains the same.

---

## Production Tip

Keep configuration outside your application.

This follows the **Twelve-Factor App** methodology and simplifies deployments.

---

## Interview Question

### Question

Why do we need ConfigMaps?

### Answer

ConfigMaps separate application configuration from application code, allowing the same container image to run in different environments with different settings.

---

# 3. What is a Secret?

## Answer

A **Secret** is a Kubernetes resource used to store **sensitive information** securely.

Examples include:

- Passwords
- API Keys
- OAuth Tokens
- SSH Keys
- TLS Certificates
- Database Credentials

Secrets help prevent sensitive information from being stored directly in application code or configuration files.

---

## Why do we need Secrets?

Imagine an application contains:

```text
Database Password

API Token

Azure Storage Key

JWT Secret
```

If these values are hardcoded:

- Anyone with access to the source code can see them.
- Credentials become difficult to rotate.
- Security risks increase significantly.

Secrets provide a safer way to manage sensitive data.

---

## Without Secret

```text
Application

↓

Hardcoded Password

↓

Security Risk
```

---

## With Secret

```text
Application

        │

Reads Secret

        │

        ▼

Kubernetes Secret

        │

Provides Credentials

        │

Application Starts
```

---

## Real World Example

Instead of writing:

```text
DB_PASSWORD=Admin123
```

inside your application,

store it in a Kubernetes Secret and inject it into the Pod during runtime.

---

## Benefits

Secrets provide:

- Secure Credential Storage
- Centralized Management
- Easier Credential Rotation
- Better Separation of Concerns
- Integration with Secret Management Solutions

---

## Production Tip

Although Kubernetes Secrets are encoded (Base64), they are **not encrypted by default**.

Enable **Encryption at Rest** and integrate with secret management solutions such as:

- Azure Key Vault
- AWS Secrets Manager
- HashiCorp Vault

for production workloads.

---

## Interview Question

### Question

What is a Kubernetes Secret?

### Answer

A Secret is a Kubernetes resource used to securely store sensitive information such as passwords, API keys, certificates, and tokens.

---

# 4. ConfigMap vs Secret

## Comparison

| ConfigMap | Secret |
|------------|--------|
| Stores Non-Sensitive Data | Stores Sensitive Data |
| Plain Configuration | Credentials & Keys |
| Application Settings | Passwords & Tokens |
| Readable Configuration | Base64 Encoded (and can be encrypted at rest) |
| Safe for Public Configuration | Requires Restricted Access |

---

## Examples

### ConfigMap

- Application Name
- Database Host
- API URL
- Log Level
- Feature Flags

---

### Secret

- Database Password
- Azure Client Secret
- JWT Secret
- SSH Private Key
- TLS Certificate

---

## Production Tip

A simple rule to remember:

**If exposing the value could create a security risk, store it in a Secret. Otherwise, store it in a ConfigMap.**

---

## Interview Question

### Question

What is the difference between a ConfigMap and a Secret?

### Answer

A ConfigMap stores non-sensitive configuration data, while a Secret stores sensitive information such as passwords, API keys, and certificates.

---

# 5. Creating a ConfigMap

## Answer

A ConfigMap can be created using a YAML manifest or directly from the command line.

The most common production approach is using YAML.

---

## ConfigMap YAML Example

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_NAME: "Shopping App"
  LOG_LEVEL: "INFO"
  DATABASE_HOST: "mysql-service"
```

---

## Create the ConfigMap

```bash
kubectl apply -f configmap.yaml
```

---

## Verify

```bash
kubectl get configmaps
```

---

## Describe

```bash
kubectl describe configmap app-config
```

---

## Production Example

Development ConfigMap

```text
LOG_LEVEL=DEBUG
DATABASE_HOST=mysql-dev
```

Production ConfigMap

```text
LOG_LEVEL=ERROR
DATABASE_HOST=mysql-prod
```

The application image remains unchanged.

---

## Production Tip

Store ConfigMaps in Git as part of your Infrastructure as Code, but never include sensitive values.

---

## Interview Question

### Question

How do you create a ConfigMap?

### Answer

A ConfigMap can be created using a YAML manifest or the `kubectl create configmap` command.

---

# 6. Creating a Secret

## Answer

Secrets are created similarly to ConfigMaps, but they contain sensitive information.

---

## Secret YAML Example

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: database-secret

type: Opaque

stringData:
  username: admin
  password: Admin@123
```

---

## Create the Secret

```bash
kubectl apply -f secret.yaml
```

---

## Verify

```bash
kubectl get secrets
```

---

## Describe

```bash
kubectl describe secret database-secret
```

---

## View Secret Data

```bash
kubectl get secret database-secret -o yaml
```

---

## Production Tip

Use `stringData` when writing Secret manifests. Kubernetes automatically converts it to Base64-encoded `data`.

---

## Interview Question

### Question

How do you create a Secret?

### Answer

A Secret can be created using a YAML manifest or the `kubectl create secret` command.

---

# 7. Using ConfigMaps as Environment Variables

## Answer

The most common way to use a ConfigMap is by exposing its values as environment variables inside a container.

---

## Example

```yaml
env:

- name: APP_NAME
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_NAME

- name: LOG_LEVEL
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: LOG_LEVEL
```

---

## Internal Workflow

```text
ConfigMap

        │

Environment Variables

        │

        ▼

Application Container

        │

Reads Values

        │

Application Starts
```

---

## Production Example

Your application reads:

```text
APP_NAME

LOG_LEVEL

DATABASE_HOST
```

directly from the ConfigMap during startup.

---

## Production Tip

Environment variables are ideal for simple configuration values.

---

## Interview Question

### Question

How can a ConfigMap be used inside a Pod?

### Answer

A ConfigMap can be exposed as environment variables or mounted as files inside the Pod.

---

# 8. Mounting ConfigMaps as Volumes

## Answer

Instead of environment variables, a ConfigMap can also be mounted as files.

Each key becomes a separate file.

---

## Example

```yaml
volumes:

- name: config-volume

  configMap:
    name: app-config

containers:

- name: app

  image: nginx

  volumeMounts:

  - name: config-volume

    mountPath: /etc/config
```

---

## Internal Workflow

```text
ConfigMap

        │

Mounted as Volume

        │

        ▼

/etc/config

        │

Application Reads File
```

---

## Production Example

Applications such as NGINX, Apache, and Prometheus commonly read configuration files mounted from ConfigMaps.

---

## Production Tip

Use volume mounts when applications expect configuration files instead of environment variables.

---

## Interview Question

### Question

When should you mount a ConfigMap as a volume?

### Answer

Use a ConfigMap volume when an application expects configuration files instead of environment variables.

---

# 9. Using Secrets inside Pods

## Answer

Secrets can be consumed in the same ways as ConfigMaps:

- Environment Variables
- Mounted Volumes

---

## Secret as Environment Variable

```yaml
env:

- name: DB_PASSWORD

  valueFrom:

    secretKeyRef:

      name: database-secret

      key: password
```

---

## Secret as Volume

```yaml
volumes:

- name: secret-volume

  secret:

    secretName: database-secret
```

---

## Internal Workflow

```text
Secret

        │

Environment Variable

or

Mounted File

        │

        ▼

Application Container
```

---

## Production Example

A Java application retrieves its database password from a Secret instead of storing it in `application.properties`.

---

## Production Tip

Avoid logging Secret values or exposing them in application output.

Restrict access using Kubernetes RBAC.

---

## Interview Question

### Question

How can a Secret be consumed by a Pod?

### Answer

A Secret can be exposed as environment variables or mounted as files inside the Pod.

---

# 10. Secret Types

## Answer

Kubernetes supports several Secret types.

---

## Common Secret Types

| Secret Type | Purpose |
|--------------|---------|
| Opaque | Generic key-value data |
| kubernetes.io/tls | TLS certificates |
| kubernetes.io/dockerconfigjson | Docker Registry Credentials |
| kubernetes.io/basic-auth | Username & Password |
| kubernetes.io/ssh-auth | SSH Keys |
| kubernetes.io/service-account-token | Service Account Tokens |

---

## Most Common Type

```yaml
type: Opaque
```

Used for:

- Database Passwords
- API Keys
- Tokens
- Credentials

---

## Production Tip

Use the appropriate Secret type for your workload to improve clarity and integration with Kubernetes features.

---

## Interview Question

### Question

What is the default Secret type?

### Answer

The default Secret type is **Opaque**, which stores generic key-value pairs.

---

# 11. 🔍 What really happens when a Pod reads a ConfigMap or Secret?

## Answer

One of the most common interview questions is:

> "What happens internally when a Pod uses a ConfigMap or Secret?"

Let's understand the complete workflow.

Suppose a Pod is configured to use:

- ConfigMap
- Secret

When the Pod starts, Kubernetes automatically injects those values into the Pod.

---

## Internal Workflow

```text
Developer

        │

kubectl apply

        │

        ▼

API Server

        │

Reads Pod Specification

        │

        ▼

Find ConfigMap

Find Secret

        │

        ▼

Kubelet

        │

Injects Values

(Environment Variables)

or

(Mounted Files)

        │

        ▼

Container Starts

        │

Application Reads Configuration
```

---

## Step-by-Step Flow

1. The Pod specification references a ConfigMap or Secret.
2. The API Server validates that the ConfigMap or Secret exists.
3. The Scheduler assigns the Pod to a Worker Node.
4. The Kubelet retrieves the required ConfigMap and Secret.
5. Values are injected as:
   - Environment Variables
   - Mounted Files
6. The container starts.
7. The application reads the configuration during runtime.

---

## Production Example

A Spring Boot application requires:

```text
Database Host

Database Username

Database Password
```

The configuration is stored as:

```text
ConfigMap

↓

DATABASE_HOST

↓

mysql-service
```

Sensitive credentials are stored as:

```text
Secret

↓

DATABASE_PASSWORD

↓

********
```

The application starts without any hardcoded configuration.

---

## Production Tip

Changing a ConfigMap or Secret does **not always** restart existing Pods.

For most applications, you should perform a Deployment rollout to ensure the new values are loaded.

---

## Interview Question

### Question

What happens internally when a Pod uses a ConfigMap or Secret?

### Answer

The Kubelet retrieves the referenced ConfigMap or Secret and injects the values into the Pod as environment variables or mounted files before the container starts.

---

# 12. ConfigMap vs Secret Best Practices

## ConfigMap

Use ConfigMaps for:

- Application Name
- API URL
- Database Host
- Log Level
- Feature Flags
- Application Configuration

---

## Secret

Use Secrets for:

- Database Passwords
- API Keys
- OAuth Tokens
- TLS Certificates
- SSH Keys
- Cloud Credentials

---

## Production Rule

```text
Public Information

↓

ConfigMap

---------------------

Sensitive Information

↓

Secret
```

---

## Production Tip

A simple rule:

If exposing the value would create a security risk, use a Secret.

Otherwise, use a ConfigMap.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Storing passwords inside ConfigMaps.

Always use Secrets for sensitive data.

---

## Mistake 2

❌ Hardcoding credentials inside Docker images.

Configuration should remain external.

---

## Mistake 3

❌ Committing Secret YAML files with real credentials to Git.

Use secure secret management solutions or encrypted secret workflows.

---

## Mistake 4

❌ Assuming Base64 encoding is encryption.

Base64 only encodes data.

It does not protect it.

---

## Mistake 5

❌ Giving every application access to every Secret.

Use Kubernetes RBAC and the principle of least privilege.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Store application configuration in ConfigMaps.

---

✅ Store sensitive information in Secrets.

---

✅ Enable Encryption at Rest for Kubernetes Secrets.

---

✅ Integrate Kubernetes with external secret management solutions such as:

- Azure Key Vault
- HashiCorp Vault
- AWS Secrets Manager

---

✅ Rotate credentials regularly.

---

✅ Restrict Secret access using RBAC.

---

✅ Keep Secrets out of application source code and Git repositories.

---

## 📌 Quick Revision

### ConfigMap Workflow

```text
ConfigMap

↓

Environment Variable

or

Mounted File

↓

Application
```

---

### Secret Workflow

```text
Secret

↓

Environment Variable

or

Mounted File

↓

Application
```

---

### ConfigMap vs Secret

```text
ConfigMap

↓

Application Settings

--------------------

Secret

↓

Sensitive Credentials
```

---

# Summary

After completing this guide, you should understand:

✅ ConfigMaps

✅ Secrets

✅ Environment Variables

✅ Volume Mounts

✅ Secret Types

✅ ConfigMap vs Secret

✅ Internal Workflow

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| Stores application configuration? | ConfigMap |
| Stores passwords and API keys? | Secret |
| Can ConfigMaps be mounted as files? | Yes |
| Can Secrets be environment variables? | Yes |
| Default Secret type? | Opaque |
| Are Kubernetes Secrets encrypted by default? | No, they are Base64 encoded unless Encryption at Rest is enabled |
| Who injects ConfigMaps and Secrets into Pods? | Kubelet |
| Best place for cloud credentials? | Secret (or an external secret manager) |
| Should credentials be committed to Git? | No |
| Recommended production secret store? | Azure Key Vault, HashiCorp Vault, AWS Secrets Manager |

---

# What's Next?

➡️ **08-volumes-storage.md**

In the next guide, you'll learn:

- What are Volumes?
- Why do we need Persistent Storage?
- emptyDir
- hostPath
- Persistent Volumes (PV)
- Persistent Volume Claims (PVC)
- Storage Classes
- Dynamic Provisioning
- 🔍 What really happens when a Pod writes data?
- Production Best Practices
- Common Interview Questions