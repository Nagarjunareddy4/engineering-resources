# 🐳 Docker Registry

This guide explains everything you need to know about Docker Registries.

If you've ever wondered:

- What is a Docker Registry?
- Why do we need a Docker Registry?
- What is Docker Hub?
- What is the difference between Public and Private Registries?
- How do DevOps teams share Docker Images?

This guide is for you.

---

# 📑 Table of Contents

1. What is a Docker Registry?
2. Why Do We Need a Docker Registry?
3. Public vs Private Registries
4. Docker Hub
5. Common Interview Questions

---

# 1. What is a Docker Registry?

## Answer

A **Docker Registry** is a centralized repository used to **store, manage, version, and distribute Docker Images**.

Instead of copying image files manually between systems, developers push images to a registry, and other systems pull the required image whenever needed.

Docker Registries make Docker Images available across:

- Developer Machines
- CI/CD Pipelines
- Test Environments
- Kubernetes Clusters
- Production Servers

---

## Architecture

```text
Developer

↓

Build Docker Image

↓

Docker Registry

↓

Pull Image

↓

Docker Host

↓

Run Container
```

---

## Internal Workflow

```text
Docker Image

↓

Push

↓

Docker Registry

↓

Pull

↓

Docker Container
```

---

## Key Characteristics

Docker Registries provide:

- Centralized Storage
- Image Versioning
- Secure Distribution
- Team Collaboration
- Image Reusability

---

## Real World Example

A DevOps engineer builds:

```text
shopping-api:v2.5
```

The image is pushed to **Azure Container Registry (ACR)**.

AKS clusters in Development, QA, and Production all pull the exact same image, ensuring deployment consistency.

---

## Benefits

Docker Registries provide:

- Centralized Image Management
- Faster Deployments
- Image Version Control
- Secure Sharing
- CI/CD Integration

---

## Production Tip

Never copy Docker Images manually between servers. Always use a Docker Registry as the central source of truth for container images.

---

## Interview Question

### Question

What is a Docker Registry?

### Answer

A Docker Registry is a centralized repository that stores, manages, versions, and distributes Docker Images.

---

# 2. Why Do We Need a Docker Registry?

## Answer

Without a Docker Registry, sharing Docker Images becomes difficult.

Every image would need to be exported, copied, and imported manually.

A Docker Registry eliminates this process.

---

## Without Registry

```text
Build Image

↓

Save Image

↓

Copy File

↓

Transfer Server

↓

Load Image

↓

Run Container
```

---

## With Registry

```text
Build Image

↓

Push Registry

↓

Pull Registry

↓

Run Container
```

---

## Problems Solved

Docker Registries eliminate:

- Manual Image Transfers
- Version Confusion
- Image Duplication
- Deployment Inconsistencies
- Slow Distribution

---

## Production Example

A GitHub Actions pipeline:

1. Builds a Docker Image.
2. Pushes it to Azure Container Registry.
3. Argo CD deploys the application to AKS using the image stored in ACR.

---

## Production Tip

Build images once in your CI pipeline and distribute them through a registry instead of rebuilding them in every environment.

---

## Interview Question

### Question

Why do we need a Docker Registry?

### Answer

A Docker Registry provides centralized image storage and distribution, enabling consistent, secure, and version-controlled deployments.

---

# 3. Public vs Private Registries

## Answer

Docker Registries are generally classified as:

- Public Registries
- Private Registries

---

## Public Registry

Public registries allow anyone to pull publicly available images.

Examples:

- Docker Hub
- GitHub Container Registry (Public Images)

Typical use cases:

- Open-source software
- Community images
- Learning and testing

---

## Private Registry

Private registries require authentication.

They are commonly used for:

- Enterprise Applications
- Internal Services
- Proprietary Software
- Secure Deployments

Examples:

- Azure Container Registry (ACR)
- Amazon Elastic Container Registry (ECR)
- Google Artifact Registry (GAR)
- Harbor

---

## Comparison

| Public Registry | Private Registry |
|-----------------|------------------|
| Public Access | Authentication Required |
| Community Images | Enterprise Images |
| Internet Accessible | Restricted Access |
| Free/Public Content | Secure Internal Content |

---

## Internal Workflow

```text
Developer

↓

Push Image

↓

Private Registry

↓

Authenticated Pull

↓

Production
```

---

## Production Example

An organization stores:

- Internal APIs
- Financial Applications
- Banking Services

inside Azure Container Registry because only authorized users and Kubernetes clusters can access the images.

---

## Production Tip

Never store proprietary or confidential application images in public registries.

---

## Interview Question

### Question

What is the difference between a Public and a Private Docker Registry?

### Answer

A Public Registry allows anyone to access public images, while a Private Registry restricts access through authentication and authorization.

---

# 4. Docker Hub

## Answer

**Docker Hub** is the default public Docker Registry provided by Docker.

It hosts millions of Docker Images, including official images maintained by software vendors.

Popular Official Images include:

- NGINX
- Ubuntu
- Redis
- PostgreSQL
- MySQL
- Jenkins
- Node.js

---

## Docker Hub Workflow

```text
Developer

↓

docker pull nginx

↓

Docker Hub

↓

Download Image

↓

Run Container
```

---

## Example Commands

Pull an image:

```bash
docker pull nginx
```

Search for an image:

```bash
docker search redis
```

List local images:

```bash
docker images
```

---

## Production Example

During local development, engineers pull official images from Docker Hub to quickly start databases, web servers, and caching services.

---

## Production Tip

Always verify that you are using **Official Images** or trusted publishers to reduce the risk of using compromised or outdated images.

---

## Interview Question

### Question

What is Docker Hub?

### Answer

Docker Hub is Docker's default public registry used to store and distribute Docker Images, including official images maintained by software vendors.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Docker Registry | Centralized image repository |
| Public Registry | Publicly accessible images |
| Private Registry | Authenticated image storage |
| Docker Hub | Default public Docker Registry |
| Push | Upload image to registry |
| Pull | Download image from registry |

---

# 5. Azure Container Registry (ACR)

## Answer

**Azure Container Registry (ACR)** is Microsoft's fully managed private Docker Registry service.

It securely stores Docker Images and OCI artifacts for Azure-based applications.

ACR integrates seamlessly with:

- Azure Kubernetes Service (AKS)
- Azure DevOps
- GitHub Actions
- Azure Container Apps

---

## Architecture

```text
Developer

↓

Build Docker Image

↓

Azure Container Registry

↓

Azure Kubernetes Service

↓

Run Containers
```

---

## Benefits

ACR provides:

- Private Image Storage
- Azure AD Authentication
- High Availability
- Geo-Replication
- Vulnerability Scanning (with integrated security services)
- CI/CD Integration

---

## Production Example

An Azure DevOps pipeline:

1. Builds the Docker Image.
2. Pushes it to ACR.
3. AKS pulls the image during deployment.

This keeps all production images inside Azure.

---

## Production Tip

Use Managed Identities or Service Principals for AKS and CI/CD authentication instead of storing registry credentials.

---

## Interview Question

### Question

What is Azure Container Registry?

### Answer

Azure Container Registry is Microsoft's private container registry service used to securely store and manage Docker Images and OCI artifacts.

---

# 6. Docker Login

## Answer

Before pushing or pulling images from a **private registry**, Docker must authenticate.

The `docker login` command stores authentication credentials locally.

---

## Login to Docker Hub

```bash
docker login
```

---

## Login to Azure Container Registry

```bash
docker login myregistry.azurecr.io
```

---

## Verify Login

Docker displays:

```text
Login Succeeded
```

---

## Internal Workflow

```text
Developer

↓

docker login

↓

Registry Authentication

↓

Push / Pull Allowed
```

---

## Production Example

A GitHub Actions workflow authenticates to Azure Container Registry before pushing a newly built application image.

---

## Production Tip

Avoid using interactive logins in automated pipelines. Use service principals, managed identities, or secure access tokens.

---

## Interview Question

### Question

Why is `docker login` required?

### Answer

`docker login` authenticates the Docker client with a registry, allowing secure access to private repositories.

---

# 7. Tagging Images for Registries

## Answer

Before pushing an image to a registry, it must be tagged with the correct repository name.

The tag tells Docker where the image should be stored.

---

## Syntax

```bash
docker tag SOURCE_IMAGE TARGET_IMAGE
```

---

## Example

```bash
docker tag shopping-api:v1 \
myregistry.azurecr.io/shopping-api:v1
```

---

## Repository Format

```text
<registry>/<repository>:<tag>
```

Example:

```text
myregistry.azurecr.io/shopping-api:v2.0
```

---

## Internal Workflow

```text
Local Image

↓

Tag Image

↓

Registry Repository

↓

Ready to Push
```

---

## Production Example

A CI pipeline tags an application image with:

```text
companyacr.azurecr.io/payment-api:2.4.1
```

before uploading it to ACR.

---

## Production Tip

Adopt consistent tagging conventions, such as semantic versions (`v2.4.1`) or immutable build numbers, rather than using `latest`.

---

## Interview Question

### Question

Why must an image be tagged before pushing it to a registry?

### Answer

The tag specifies the target registry, repository, and version so Docker knows where to store the image.

---

# 8. Pushing & Pulling Images

## Answer

Once authenticated and tagged, images can be uploaded to or downloaded from a registry.

---

## Push an Image

```bash
docker push myregistry.azurecr.io/shopping-api:v1
```

---

## Pull an Image

```bash
docker pull myregistry.azurecr.io/shopping-api:v1
```

---

## Internal Workflow

```text
Build Image

↓

Tag Image

↓

Push Registry

↓

Pull Registry

↓

Run Container
```

---

## Production Example

A deployment pipeline pushes a versioned image to ACR.

During deployment, AKS pulls the same image, ensuring every environment runs an identical artifact.

---

## Production Tip

Always pull versioned images in production to ensure predictable deployments and simplify rollback procedures.

---

## Interview Question

### Question

What is the difference between `docker push` and `docker pull`?

### Answer

`docker push` uploads a Docker Image to a registry, while `docker pull` downloads an image from a registry to a local system.

---

# 9. Repository Naming Best Practices

## Answer

A consistent repository naming strategy makes images easier to identify and manage.

---

## Recommended Format

```text
<registry>/<application>:<version>
```

Examples:

```text
companyacr.azurecr.io/frontend:v1.2.0

companyacr.azurecr.io/backend:v3.5.1

companyacr.azurecr.io/payment-api:v2.0.4
```

---

## Naming Guidelines

- Use lowercase names.
- Use meaningful application names.
- Use immutable version tags.
- Keep naming consistent across teams.

---

## Production Example

A company standardizes all repositories using:

```text
companyacr.azurecr.io/<service-name>:<version>
```

This simplifies automation, monitoring, and troubleshooting.

---

## Production Tip

Avoid using random or inconsistent repository names. Standard naming conventions improve maintainability across CI/CD pipelines and Kubernetes deployments.

---

## Interview Question

### Question

What is a good Docker repository naming convention?

### Answer

A recommended convention is:

```text
<registry>/<application>:<version>
```

using meaningful names and immutable version tags.

---

## 📌 Quick Revision

| Feature | Purpose |
|----------|---------|
| Azure Container Registry | Private Azure registry |
| `docker login` | Authenticate with a registry |
| `docker tag` | Prepare an image for a registry |
| `docker push` | Upload an image |
| `docker pull` | Download an image |
| Repository Naming | Organize images consistently |

---

# 10. Image Authentication

## Answer

Authentication ensures that only authorized users, services, or applications can push or pull images from a private Docker Registry.

This protects container images from unauthorized access.

---

## Authentication Methods

Common authentication methods include:

- Username & Password
- Personal Access Tokens (PAT)
- Service Principals (Azure)
- Managed Identities (Azure)
- IAM Roles (AWS)
- Service Accounts (Kubernetes)

---

## Authentication Workflow

```text
Developer / CI Pipeline

↓

Authenticate

↓

Docker Registry

↓

Authorized?

↓

Push / Pull Image
```

---

## Production Example

An Azure DevOps pipeline authenticates to Azure Container Registry using a **Service Principal** before pushing application images.

An AKS cluster uses a **Managed Identity** to securely pull images from ACR without storing credentials.

---

## Production Tip

Avoid storing registry usernames and passwords in scripts or source code. Use identity-based authentication whenever possible.

---

## Interview Question

### Question

Why is registry authentication important?

### Answer

Registry authentication ensures that only authorized users and systems can access private Docker Images, improving security.

---

# 11. Image Scanning

## Answer

Image Scanning analyzes Docker Images for:

- Known Vulnerabilities (CVEs)
- Outdated Packages
- Security Misconfigurations
- Unsupported Libraries

Scanning should be integrated into the CI/CD pipeline before deployment.

---

## Workflow

```text
Build Image

↓

Scan Image

↓

No Critical Issues?

↓

Push Registry

↓

Deploy
```

---

## Common Scanning Tools

- Microsoft Defender for Cloud (ACR integration)
- Docker Scout
- Trivy
- Snyk
- Clair

---

## Production Example

A CI pipeline scans an image after building it.

If a critical vulnerability is detected, the pipeline fails and the image is not pushed to the registry.

---

## Production Tip

Block deployments of images with critical vulnerabilities until they are remediated.

---

## Interview Question

### Question

Why should Docker Images be scanned?

### Answer

Image scanning identifies security vulnerabilities and misconfigurations before images are deployed to production.

---

# 12. Image Signing

## Answer

Image Signing verifies that an image:

- Was created by a trusted source
- Has not been modified after publication
- Maintains its integrity

Signing helps prevent supply chain attacks.

---

## Workflow

```text
Build Image

↓

Sign Image

↓

Push Registry

↓

Verify Signature

↓

Deploy
```

---

## Common Image Signing Tools

- Cosign (Sigstore)
- Docker Content Trust (Notary v1, legacy)
- Notation (OCI artifact signing)

---

## Production Example

A Kubernetes admission policy verifies that only images signed by the organization's trusted signing key can be deployed.

---

## Production Tip

Sign production images and verify signatures before deployment to ensure image authenticity.

---

## Interview Question

### Question

What is the purpose of image signing?

### Answer

Image signing verifies the authenticity and integrity of Docker Images, ensuring they have not been tampered with.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Using the `latest` tag for production deployments.

Instead use:

```text
shopping-api:v2.4.1
```

---

## Mistake 2

❌ Pushing proprietary application images to public registries.

Use private registries such as:

- Azure Container Registry
- Amazon ECR
- Google Artifact Registry
- Harbor

---

## Mistake 3

❌ Skipping vulnerability scanning.

Always scan images before publishing them.

---

## Mistake 4

❌ Sharing registry credentials.

Use secure identity-based authentication instead.

---

## Mistake 5

❌ Leaving unused image versions in the registry.

Implement image retention and cleanup policies to reduce storage costs and improve manageability.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Use private registries for enterprise applications.

---

✅ Authenticate using Managed Identities, Service Principals, IAM Roles, or other secure identity mechanisms.

---

✅ Tag every image with immutable versions.

---

✅ Scan images before deployment.

---

✅ Sign production images.

---

✅ Apply Role-Based Access Control (RBAC) to registry access.

---

✅ Enable image retention and lifecycle policies.

---

✅ Monitor registry usage, storage, and access logs.

---

## 📌 Quick Revision

### Registry Workflow

```text
Source Code

↓

Build Image

↓

Tag Image

↓

Authenticate

↓

Push Registry

↓

Pull Image

↓

Run Container
```

---

### Secure Image Pipeline

```text
Build

↓

Scan

↓

Sign

↓

Push

↓

Verify

↓

Deploy
```

---

### Common Commands

```bash
docker login

docker tag

docker push

docker pull

docker logout
```

---

# Summary

After completing this guide, you should understand:

✅ Docker Registries

✅ Public vs Private Registries

✅ Docker Hub

✅ Azure Container Registry (ACR)

✅ Docker Login

✅ Image Tagging

✅ Pushing Images

✅ Pulling Images

✅ Repository Naming

✅ Image Authentication

✅ Image Scanning

✅ Image Signing

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is a Docker Registry? | A centralized repository for storing and distributing Docker Images |
| Why do we need a Docker Registry? | To securely store, version, and share container images |
| What is Docker Hub? | Docker's default public image registry |
| What is Azure Container Registry (ACR)? | Azure's managed private container registry |
| Why is `docker login` required? | To authenticate with a private registry |
| Why must images be tagged before pushing? | To specify the target registry, repository, and version |
| What is the difference between `docker push` and `docker pull`? | `push` uploads images; `pull` downloads images |
| Why is image scanning important? | It identifies vulnerabilities before deployment |
| Why is image signing important? | It verifies image authenticity and integrity |
| Why should private registries be used? | To securely store proprietary enterprise images |

---

# 🎉 Docker Registry Module Completed

Congratulations! You have completed **Docker Registry**, including:

- ✅ Docker Registry Fundamentals
- ✅ Public vs Private Registries
- ✅ Docker Hub
- ✅ Azure Container Registry (ACR)
- ✅ Docker Login
- ✅ Image Tagging
- ✅ Pushing & Pulling Images
- ✅ Image Authentication
- ✅ Image Scanning
- ✅ Image Signing
- ✅ Production Best Practices

You now have a solid understanding of how Docker Images are securely stored, versioned, distributed, and protected in enterprise environments.

---

# 🚀 What's Next?

➡️ **08-docker-security-best-practices.md**

In the next guide, you'll learn:

- What is Docker Security?
- Container Security
- Image Security
- Rootless Containers
- Docker Secrets
- Resource Limits
- Logging & Monitoring
- Security Hardening
- Production Best Practices
- Common Interview Questions