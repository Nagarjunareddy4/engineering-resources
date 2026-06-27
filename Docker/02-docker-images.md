# 🐳 Docker Images

This guide explains everything you need to know about Docker Images.

If you've ever wondered:

- What is a Docker Image?
- How is a Docker Image different from a Container?
- What are Image Layers?
- Why are Docker Images lightweight?
- How do Docker Images work?

This guide is for you.

---

# 📑 Table of Contents

1. What is a Docker Image?
2. Docker Image vs Docker Container
3. Image Layers
4. Union File System
5. Common Interview Questions

---

# 1. What is a Docker Image?

## Answer

A **Docker Image** is a **read-only template** used to create Docker Containers.

It contains everything required to run an application, including:

- Application Code
- Runtime
- Libraries
- Dependencies
- Configuration Files
- Environment Variables

Think of a Docker Image as a blueprint.

Every time you run an image, Docker creates a new container from that blueprint.

---

## Simple Architecture

```text
Application Code

↓

Docker Image

↓

Docker Container

↓

Running Application
```

---

## Key Characteristics

Docker Images are:

- Immutable (cannot be modified after creation)
- Portable
- Lightweight
- Versioned
- Reusable

---

## Real World Example

An organization builds:

```text
shopping-api:v1.0
```

The same image is used in:

- Development
- QA
- Staging
- Production

This ensures consistent deployments across all environments.

---

## Benefits

Docker Images provide:

- Consistency
- Portability
- Version Control
- Fast Deployment
- Easy Rollbacks

---

## Production Tip

Build an image once and deploy the exact same image everywhere. Avoid rebuilding images for different environments.

---

## Interview Question

### Question

What is a Docker Image?

### Answer

A Docker Image is a read-only template that packages an application and all of its dependencies, serving as the blueprint for creating Docker Containers.

---

# 2. Docker Image vs Docker Container

## Answer

Many beginners confuse Images and Containers.

A Docker Image is a **template**.

A Docker Container is a **running instance** of that template.

---

## Relationship

```text
Docker Image

↓

Create

↓

Docker Container

↓

Running Application
```

---

## Example

Image

```text
nginx:1.27
```

Run:

```bash
docker run nginx:1.27
```

Result

```text
Container 1

Container 2

Container 3
```

One image can create many containers.

---

## Comparison

| Docker Image | Docker Container |
|--------------|------------------|
| Read-only | Running instance |
| Template | Executing application |
| Immutable | Can change while running |
| Stored in Registry | Runs on Docker Engine |

---

## Production Example

An e-commerce platform uses one image:

```text
shopping-api:v2.0
```

to create:

- 5 containers in QA
- 20 containers in Production

All containers originate from the same image.

---

## Production Tip

Never modify running containers manually. Update the Docker Image, then recreate the containers.

---

## Interview Question

### Question

What is the difference between a Docker Image and a Docker Container?

### Answer

A Docker Image is a read-only template, while a Docker Container is a running instance created from that image.

---

# 3. Image Layers

## Answer

Docker Images are built using **layers**.

Each instruction in a Dockerfile creates a new layer.

These layers are stacked together to form the final image.

---

## Layer Example

Dockerfile

```dockerfile
FROM ubuntu:24.04

RUN apt update

RUN apt install -y nginx

COPY app/ /var/www/html

CMD ["nginx", "-g", "daemon off;"]
```

---

## Generated Layers

```text
Layer 5

CMD

↓

Layer 4

COPY

↓

Layer 3

Install NGINX

↓

Layer 2

apt update

↓

Layer 1

Ubuntu Base Image
```

---

## Benefits of Layers

- Faster Builds
- Layer Caching
- Smaller Downloads
- Reusability
- Efficient Storage

---

## Production Example

If only the application code changes, Docker reuses the existing operating system and dependency layers, rebuilding only the affected layer.

This significantly reduces build time.

---

## Production Tip

Order Dockerfile instructions carefully. Place frequently changing instructions (such as `COPY . .`) near the end to maximize layer cache usage.

---

## Interview Question

### Question

What are Docker Image Layers?

### Answer

Docker Image Layers are read-only filesystem layers created by each Dockerfile instruction. Together they form the complete Docker Image.

---

# 4. Union File System (UnionFS)

## Answer

Docker combines multiple image layers into a single unified filesystem using a **Union File System (UnionFS)**.

This allows Docker to reuse common layers across multiple images, improving storage efficiency.

---

## Architecture

```text
Application Layer

↓

Dependency Layer

↓

Runtime Layer

↓

Operating System Layer

↓

Unified File System

↓

Container
```

---

## Internal Workflow

```text
Multiple Layers

↓

Union File System

↓

Single Image View

↓

Container
```

---

## Production Example

Suppose three different images all use:

```text
ubuntu:24.04
```

The Ubuntu base layer is stored only once on the host.

All images reuse that shared layer, reducing disk usage.

---

## Production Tip

Choose lightweight base images such as Alpine Linux when appropriate to reduce image size and improve deployment speed.

---

## Interview Question

### Question

What is the purpose of Union File System in Docker?

### Answer

UnionFS combines multiple image layers into a single unified filesystem while allowing layers to be shared and reused efficiently.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Docker Image | Read-only application template |
| Docker Container | Running instance of an image |
| Image Layers | Read-only filesystem layers |
| UnionFS | Combines layers into one filesystem |
| Image | Can create multiple containers |
| Layers | Improve caching and storage efficiency |

---

# 5. Docker Hub

## Answer

**Docker Hub** is the default public Docker Registry used to store and distribute Docker Images.

It contains millions of ready-to-use images for popular software.

Examples include:

- NGINX
- Ubuntu
- Redis
- MySQL
- PostgreSQL
- Jenkins

---

## Docker Hub Workflow

```text
Developer

↓

Docker Hub

↓

Pull Image

↓

Run Container
```

---

## Search for an Image

```bash
docker search nginx
```

---

## Pull an Image

```bash
docker pull nginx
```

---

## Production Example

A developer pulls the official NGINX image from Docker Hub to quickly deploy a web server for testing.

---

## Production Tip

Always use **Official Images** or images from trusted publishers to reduce security risks.

---

## Interview Question

### Question

What is Docker Hub?

### Answer

Docker Hub is the default public registry used to store, share, and distribute Docker Images.

---

# 6. Docker Image Registries

## Answer

A **Docker Registry** is a repository where Docker Images are stored.

Registries can be:

- Public
- Private

---

## Popular Registries

| Registry | Provider |
|-----------|----------|
| Docker Hub | Docker |
| Azure Container Registry (ACR) | Microsoft Azure |
| Amazon Elastic Container Registry (ECR) | AWS |
| Google Artifact Registry | Google Cloud |
| Harbor | Self-hosted |

---

## Registry Workflow

```text
Build Image

↓

Push Image

↓

Registry

↓

Pull Image

↓

Run Container
```

---

## Production Example

A company stores all production images in **Azure Container Registry (ACR)** instead of Docker Hub for better security and access control.

---

## Production Tip

Use private registries for enterprise applications and enable vulnerability scanning for stored images.

---

## Interview Question

### Question

What is the difference between Docker Hub and a Docker Registry?

### Answer

Docker Hub is a public Docker Registry provided by Docker, while a Docker Registry is the general service used to store and distribute Docker Images.

---

# 7. Pulling, Listing & Removing Images

## Pull an Image

```bash
docker pull nginx
```

---

## Pull a Specific Version

```bash
docker pull nginx:1.27
```

---

## List Local Images

```bash
docker images
```

Example Output

```text
REPOSITORY   TAG     IMAGE ID

nginx        1.27    a12bc345

ubuntu       24.04   d56ef789
```

---

## Remove an Image

```bash
docker rmi nginx:1.27
```

---

## Remove Multiple Images

```bash
docker rmi nginx ubuntu redis
```

---

## Internal Workflow

```text
Docker Registry

↓

Pull Image

↓

Local Image Store

↓

Run Container

↓

Remove Image
```

---

## Production Example

A CI/CD pipeline pulls the latest approved application image from Azure Container Registry before deploying it to AKS.

---

## Production Tip

Regularly remove unused images to reclaim disk space on build agents and Docker hosts.

---

## Interview Question

### Question

How do you list all Docker Images?

### Answer

Use the `docker images` command.

---

# 8. Docker Image Tagging

## Answer

Tags identify different versions of the same Docker Image.

Without a specified tag, Docker uses:

```text
latest
```

---

## Tag Syntax

```text
repository:tag
```

Example

```text
shopping-api:v1.0

shopping-api:v2.0

shopping-api:latest
```

---

## Tag an Image

```bash
docker tag shopping-api:v1 shopping-api:production
```

---

## Push Tagged Image

```bash
docker push shopping-api:production
```

---

## Internal Workflow

```text
Build Image

↓

Assign Tag

↓

Push Registry

↓

Deploy
```

---

## Production Example

Instead of using:

```text
latest
```

an organization uses:

```text
shopping-api:2.5.1
```

This makes deployments predictable and simplifies rollbacks.

---

## Production Tip

Avoid using the `latest` tag in production. Use semantic versioning (for example, `v1.2.3`) or immutable build numbers.

---

## Interview Question

### Question

Why are Docker Image tags important?

### Answer

Image tags uniquely identify image versions, making deployments predictable and enabling easy rollbacks.

---

# 9. Docker Image History

## Answer

Docker records every layer used to build an image.

The history command helps understand:

- Build Steps
- Layer Sizes
- Dockerfile Instructions

---

## View Image History

```bash
docker history nginx
```

Example Output

```text
IMAGE       CREATED        SIZE

Layer 5     2 days ago     0B

Layer 4     2 days ago     10MB

Layer 3     2 days ago     25MB
```

---

## Internal Workflow

```text
Dockerfile

↓

Build

↓

Image Layers

↓

History
```

---

## Production Example

A DevOps engineer analyzes image history to identify large layers that increase image size and slow deployments.

---

## Production Tip

Review image history regularly to optimize Dockerfiles and reduce unnecessary image layers.

---

## Interview Question

### Question

What is the purpose of the `docker history` command?

### Answer

The `docker history` command displays the layers and build history of a Docker Image, helping analyze how the image was created.

---

## 📌 Quick Revision

| Feature | Purpose |
|----------|---------|
| Docker Hub | Public image registry |
| Docker Registry | Stores Docker Images |
| `docker pull` | Download an image |
| `docker images` | List local images |
| `docker rmi` | Remove images |
| `docker tag` | Assign version tags |
| `docker history` | View image build history |

---

# 10. Building Docker Images

## Answer

Docker Images are created using a **Dockerfile**, which contains a series of instructions describing how to package an application.

Docker reads the Dockerfile step by step and creates an image layer for each instruction.

---

## Build Workflow

```text
Application Source Code

↓

Dockerfile

↓

docker build

↓

Docker Image

↓

Docker Registry

↓

Docker Container
```

---

## Build Command

```bash
docker build -t shopping-api:v1 .
```

### Command Breakdown

- `docker build` → Builds an image
- `-t` → Assigns a name and tag
- `shopping-api:v1` → Image name and version
- `.` → Current directory containing the Dockerfile

---

## Verify the Image

```bash
docker images
```

---

## Production Example

A CI/CD pipeline:

1. Builds the application.
2. Executes:

```bash
docker build -t shopping-api:2.1.0 .
```

3. Pushes the image to Azure Container Registry.
4. Argo CD deploys the image to AKS.

---

## Production Tip

Automate image builds in your CI pipeline instead of building images manually on developer machines.

---

## Interview Question

### Question

How do you build a Docker Image?

### Answer

Use the `docker build` command with a Dockerfile to create a versioned Docker Image.

---

# 11. Docker Image Optimization

## Answer

Smaller Docker Images are:

- Faster to Build
- Faster to Download
- Faster to Deploy
- More Secure
- Easier to Maintain

---

## Optimization Techniques

### Use Small Base Images

Instead of:

```dockerfile
FROM ubuntu:24.04
```

Use:

```dockerfile
FROM alpine:3.21
```

or other lightweight images when appropriate.

---

### Use Multi-stage Builds

Separate the build environment from the runtime environment.

Example:

```dockerfile
FROM maven:3.9 AS builder

# Build application

FROM eclipse-temurin:21-jre

# Copy compiled application
```

---

### Remove Temporary Files

Delete package caches and temporary files during image creation.

---

### Combine RUN Instructions

Instead of:

```dockerfile
RUN apt update

RUN apt install nginx
```

Use:

```dockerfile
RUN apt update && \
    apt install -y nginx
```

This reduces the number of image layers.

---

## Production Example

Optimizing a Java application reduces the image size from **1.2 GB** to **250 MB**, improving deployment speed and reducing storage costs.

---

## Production Tip

Optimize Dockerfiles regularly to reduce image size and improve build performance.

---

## Interview Question

### Question

How can Docker Images be optimized?

### Answer

Use lightweight base images, multi-stage builds, combined RUN instructions, and remove unnecessary files to create smaller and more efficient Docker Images.

---

# 12. Common Beginner Mistakes

## Mistake 1

❌ Using the `latest` tag.

Instead use:

```text
shopping-api:v2.3.1
```

---

## Mistake 2

❌ Creating unnecessarily large images.

Remove unused packages and files.

---

## Mistake 3

❌ Installing unnecessary software.

Only include what the application needs.

---

## Mistake 4

❌ Rebuilding identical dependency layers repeatedly.

Structure the Dockerfile to maximize layer caching.

---

## Mistake 5

❌ Storing secrets inside Docker Images.

Never include:

- Passwords
- API Keys
- Tokens
- Certificates

Inject secrets at runtime using Kubernetes Secrets, Docker Secrets, or an external secret manager.

---

# 13. Production Best Practices

Always follow these recommendations.

---

✅ Use Official Images whenever possible.

---

✅ Use versioned image tags.

---

✅ Build images in CI pipelines.

---

✅ Scan images for vulnerabilities before deployment.

---

✅ Use multi-stage builds.

---

✅ Keep images small.

---

✅ Never store secrets inside images.

---

✅ Regularly update base images with security patches.

---

## 📌 Quick Revision

### Image Lifecycle

```text
Application

↓

Dockerfile

↓

Build Image

↓

Tag Image

↓

Push Registry

↓

Pull Image

↓

Run Container
```

---

### Image Optimization

```text
Small Base Image

↓

Multi-stage Build

↓

Fewer Layers

↓

Smaller Image

↓

Faster Deployment
```

---

### Common Commands

```bash
docker build -t shopping-api:v1 .

docker images

docker tag shopping-api:v1 shopping-api:production

docker push shopping-api:v1

docker pull shopping-api:v1

docker history shopping-api
```

---

# Summary

After completing this guide, you should understand:

✅ What is a Docker Image?

✅ Docker Image vs Container

✅ Image Layers

✅ Union File System

✅ Docker Hub

✅ Docker Registries

✅ Pulling Images

✅ Listing Images

✅ Removing Images

✅ Image Tagging

✅ Image History

✅ Building Images

✅ Image Optimization

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is a Docker Image? | A read-only template used to create Docker Containers |
| What is the difference between an Image and a Container? | An Image is a template; a Container is a running instance |
| What are Docker Image Layers? | Read-only filesystem layers created by Dockerfile instructions |
| What is UnionFS? | A filesystem that combines image layers into one unified view |
| What is Docker Hub? | The default public Docker Registry |
| What is a Docker Registry? | A service that stores and distributes Docker Images |
| How do you build an image? | `docker build -t <image-name>:<tag> .` |
| Why are image tags important? | They identify image versions and support predictable deployments |
| How can Docker Images be optimized? | Use lightweight base images, multi-stage builds, and fewer layers |
| Why shouldn't secrets be stored in images? | Because anyone with access to the image could extract them, creating a security risk |

---

# 🎉 Docker Images Module Completed

Congratulations! You have completed **Docker Images**, including:

- ✅ Docker Images
- ✅ Image vs Container
- ✅ Image Layers
- ✅ Union File System
- ✅ Docker Hub
- ✅ Docker Registries
- ✅ Pulling & Removing Images
- ✅ Image Tagging
- ✅ Image History
- ✅ Building Images
- ✅ Image Optimization
- ✅ Production Best Practices

You now have a strong understanding of how Docker Images are created, optimized, versioned, and managed in production environments.

---

# 🚀 What's Next?

➡️ **03-docker-containers.md**

In the next guide, you'll learn:

- What is a Docker Container?
- Container Lifecycle
- Creating Containers
- Starting, Stopping & Restarting Containers
- Executing Commands Inside Containers
- Viewing Logs
- Inspecting Containers
- Removing Containers
- Production Best Practices
- Common Interview Questions