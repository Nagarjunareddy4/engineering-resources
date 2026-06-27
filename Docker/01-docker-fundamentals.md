# 🐳 Docker Fundamentals

This guide explains everything you need to know about Docker from the ground up.

If you've ever wondered:

- What is Docker?
- Why was Docker created?
- How is Docker different from Virtual Machines?
- What problems does Docker solve?
- How does Docker work?

This guide is for you.

---

# 📑 Table of Contents

1. What is Docker?
2. Why Docker?
3. Problems Before Docker
4. Virtual Machines vs Containers
5. Common Interview Questions

---

# 1. What is Docker?

## Answer

Docker is an **open-source containerization platform** that allows developers to package an application together with all of its dependencies into a lightweight, portable unit called a **container**.

A container includes:

- Application Code
- Runtime
- Libraries
- Dependencies
- Configuration Files

This ensures the application behaves the same regardless of where it is deployed.

---

## Simple Architecture

```text
Application

↓

Docker Image

↓

Docker Container

↓

Docker Engine

↓

Operating System

↓

Physical / Virtual Machine

```

---

## Key Characteristics

Docker containers are:

- Lightweight
- Portable
- Fast to Start
- Isolated
- Consistent Across Environments

---

## Real World Example

A Java application requires:

- Java 21
- Maven
- Specific libraries
- Environment variables

Instead of installing these manually on every server, the application is packaged into a Docker image.

That image can then run consistently on:

- Developer Laptop
- Test Server
- Azure Virtual Machine
- AKS Worker Node
- AWS EC2 Instance

---

## Benefits

Docker provides:

- Consistent Deployments
- Faster Delivery
- Better Resource Utilization
- Simplified Application Packaging
- Easier Scaling

---

## Production Tip

Treat Docker Images as immutable artifacts. Build them once and deploy the same image across Development, QA, Staging, and Production.

---

## Interview Question

### Question

What is Docker?

### Answer

Docker is an open-source containerization platform that packages applications and their dependencies into portable, lightweight containers that run consistently across different environments.

---

# 2. Why Docker?

## Answer

Before Docker, developers frequently encountered the problem:

```text
"It works on my machine."
```

Applications often failed after deployment because development and production environments differed.

Docker solves this by packaging everything the application needs into a container.

---

## Before Docker

```text
Developer Laptop

↓

Application Works

↓

Deploy to Server

↓

Dependency Missing

↓

Application Fails
```

---

## With Docker

```text
Developer Laptop

↓

Docker Image

↓

Docker Container

↓

Production Server

↓

Application Works
```

---

## Problems Docker Solves

- Dependency Conflicts
- Different Operating System Configurations
- Runtime Version Mismatches
- Manual Installation Errors
- Environment Inconsistencies

---

## Production Example

A Node.js application requires:

```text
Node.js 22

npm

Custom Libraries
```

The Docker image packages everything together.

Any machine capable of running Docker can execute the application without additional configuration.

---

## Production Tip

Build the Docker image once during the CI pipeline and reuse the same image across all deployment environments.

---

## Interview Question

### Question

Why do we use Docker?

### Answer

Docker provides consistent, portable, and isolated environments by packaging applications together with all required dependencies.

---

# 3. Problems Before Docker

## Answer

Before containerization, software deployment relied heavily on manual server configuration.

Each environment often had:

- Different Library Versions
- Different Runtime Versions
- Different Operating Systems
- Different Configuration Files

These differences caused deployment failures.

---

## Traditional Deployment

```text
Application

↓

Install Runtime

↓

Install Dependencies

↓

Configure Server

↓

Deploy

↓

Hope It Works
```

---

## Container-Based Deployment

```text
Application

↓

Build Docker Image

↓

Run Container

↓

Application Ready
```

---

## Common Challenges

Without Docker:

- Slow Provisioning
- Manual Configuration
- Dependency Conflicts
- Difficult Rollbacks
- Poor Scalability

---

## Production Example

A Python application works on Ubuntu 22.04 but fails on another server running a different Python version.

Packaging the application into a Docker image ensures the same Python runtime is used everywhere.

---

## Production Tip

Avoid installing application dependencies directly on production servers. Package everything inside a Docker image.

---

## Interview Question

### Question

What problems existed before Docker?

### Answer

Traditional deployments suffered from inconsistent environments, dependency conflicts, manual configuration, and difficult application portability.

---

# 4. Virtual Machines vs Containers

## Answer

Both Virtual Machines (VMs) and Containers provide isolated environments, but they do so differently.

A Virtual Machine virtualizes the **entire operating system**, while a container shares the host operating system kernel.

---

## Virtual Machine Architecture

```text
Application

↓

Guest Operating System

↓

Hypervisor

↓

Host Operating System

↓

Physical Server
```

---

## Docker Container Architecture

```text
Application

↓

Libraries

↓

Docker Engine

↓

Host Operating System

↓

Physical Server
```

---

## Comparison

| Virtual Machine | Docker Container |
|-----------------|------------------|
| Includes full Guest OS | Shares Host OS Kernel |
| Larger Size (GBs) | Smaller Size (MBs) |
| Slower Startup | Starts in Seconds |
| Higher Resource Usage | Lightweight |
| Strong Isolation | Process-Level Isolation |

---

## Production Example

A single virtual machine may take several minutes to boot.

A Docker container typically starts in just a few seconds, making it ideal for microservices and autoscaling.

---

## Production Tip

Use Virtual Machines for infrastructure isolation and Docker containers for packaging and running applications.

---

## Interview Question

### Question

What is the difference between a Virtual Machine and a Docker Container?

### Answer

A Virtual Machine includes a complete guest operating system, while a Docker container shares the host operating system kernel, making containers smaller, faster, and more resource-efficient.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Docker | Containerization Platform |
| Container | Lightweight application package |
| Docker Image | Blueprint for creating containers |
| Docker Engine | Runs and manages containers |
| Virtual Machine | Full operating system virtualization |
| Container | Process-level isolation using the host kernel |

---

# 5. Docker Architecture

## Answer

Docker follows a **client-server architecture**.

The Docker Client communicates with the Docker Daemon, which performs operations such as building images, creating containers, and managing networks and volumes.

---

## Docker Architecture

```text
+----------------------+
|    Docker Client     |
| (docker commands)    |
+----------+-----------+
           |
           | Docker API
           |
+----------v-----------+
|    Docker Daemon     |
| (dockerd)            |
+----------+-----------+
           |
   +-------+--------+
   |       |        |
 Images  Containers Networks
   |       |        |
   +-------+--------+
           |
     Docker Host
           |
   Docker Registry
(Docker Hub / ACR / ECR)
```

---

## Architecture Flow

```text
Developer

↓

Docker CLI

↓

Docker Daemon

↓

Docker Engine

↓

Images

↓

Containers
```

---

## Production Example

A DevOps engineer runs:

```bash
docker run nginx
```

The Docker Client sends the request to the Docker Daemon.

The Daemon:

1. Checks for the image locally.
2. Downloads it if necessary.
3. Creates the container.
4. Starts the container.

---

## Production Tip

Remember that the Docker Client never creates containers directly. All operations are performed by the Docker Daemon.

---

## Interview Question

### Question

Explain Docker Architecture.

### Answer

Docker follows a client-server architecture where the Docker Client communicates with the Docker Daemon using the Docker API. The Daemon manages images, containers, networks, and volumes.

---

# 6. Docker Components

## Answer

Docker consists of several core components that work together.

---

## Main Components

### Docker Client

Used to execute Docker commands.

Example:

```bash
docker run nginx
```

---

### Docker Daemon

Runs in the background and manages Docker objects.

Responsible for:

- Building Images
- Running Containers
- Managing Networks
- Managing Volumes

---

### Docker Engine

The complete runtime responsible for container management.

Includes:

- Docker Daemon
- REST API
- Docker CLI Support

---

### Docker Registry

Stores Docker Images.

Examples:

- Docker Hub
- Azure Container Registry (ACR)
- Amazon Elastic Container Registry (ECR)
- Google Artifact Registry

---

### Docker Images

Blueprints used to create containers.

---

### Docker Containers

Running instances of Docker Images.

---

## Internal Workflow

```text
Docker Client

↓

Docker Daemon

↓

Docker Engine

↓

Image

↓

Container
```

---

## Production Example

A CI pipeline:

1. Builds a Docker Image.
2. Pushes it to Azure Container Registry.
3. Kubernetes pulls the image from the registry and starts containers.

---

## Production Tip

Understand the responsibility of each component before troubleshooting Docker-related issues.

---

## Interview Question

### Question

What are the main Docker components?

### Answer

The main Docker components are the Docker Client, Docker Daemon, Docker Engine, Docker Registry, Docker Images, and Docker Containers.

---

# 7. Docker Engine

## Answer

Docker Engine is the core runtime that enables containerization.

It consists of:

- Docker Daemon (`dockerd`)
- Docker REST API
- Docker CLI

---

## Responsibilities

Docker Engine:

- Builds Images
- Runs Containers
- Creates Networks
- Creates Volumes
- Manages Container Lifecycle

---

## Internal Workflow

```text
Docker CLI

↓

REST API

↓

Docker Daemon

↓

Container Created
```

---

## Production Example

When Kubernetes starts a Pod using Docker (or historically Docker as the container runtime), the Docker Engine manages the container lifecycle on the node.

> **Note:** Modern Kubernetes commonly uses container runtimes such as **containerd** or **CRI-O** through the Container Runtime Interface (CRI), although understanding Docker Engine remains important for building and testing container images.

---

## Production Tip

Ensure the Docker Engine is running before executing Docker commands.

---

## Interview Question

### Question

What is Docker Engine?

### Answer

Docker Engine is the runtime responsible for building, running, and managing Docker containers through the Docker Daemon and Docker API.

---

# 8. Docker Client & Docker Daemon

## Docker Client

### Answer

The Docker Client is the command-line interface used to communicate with Docker.

Common commands include:

```bash
docker build

docker run

docker pull

docker push

docker ps
```

---

## Docker Daemon

### Answer

The Docker Daemon (`dockerd`) runs as a background service.

It listens for Docker API requests and performs operations such as:

- Pulling Images
- Creating Containers
- Starting Containers
- Removing Containers

---

## Internal Workflow

```text
docker run nginx

↓

Docker Client

↓

Docker API

↓

Docker Daemon

↓

Create Container

↓

Run Container
```

---

## Production Example

A developer executes:

```bash
docker build -t shopping-api:v1 .
```

The Docker Client sends the build request to the Docker Daemon, which builds the image layer by layer.

---

## Production Tip

If Docker commands fail, verify that the Docker Daemon service is running before troubleshooting further.

---

## Interview Question

### Question

What is the difference between the Docker Client and the Docker Daemon?

### Answer

The Docker Client sends commands, while the Docker Daemon performs the actual work of managing Docker objects.

---

# 9. Docker Registry

## Answer

A Docker Registry is a repository for storing and distributing Docker Images.

Registries allow teams to share images across environments.

---

## Popular Registries

- Docker Hub
- Azure Container Registry (ACR)
- Amazon Elastic Container Registry (ECR)
- Google Artifact Registry (GAR)
- Harbor (Private Registry)

---

## Workflow

```text
Developer

↓

Build Image

↓

Push Image

↓

Docker Registry

↓

Pull Image

↓

Run Container
```

---

## Common Commands

### Pull an Image

```bash
docker pull nginx
```

---

### Push an Image

```bash
docker push myrepo/shopping-api:v1
```

---

## Production Example

An Azure DevOps pipeline:

1. Builds a Docker image.
2. Pushes it to Azure Container Registry.
3. AKS pulls the image during deployment.

---

## Production Tip

Use private registries for enterprise applications and enable image scanning to detect security vulnerabilities before deployment.

---

## Interview Question

### Question

What is a Docker Registry?

### Answer

A Docker Registry stores and distributes Docker Images, allowing applications to be shared and deployed consistently across environments.

---

## 📌 Quick Revision

| Component | Purpose |
|-----------|---------|
| Docker Client | Sends Docker commands |
| Docker Daemon | Manages Docker operations |
| Docker Engine | Container runtime |
| Docker Registry | Stores Docker Images |
| Docker Image | Template for containers |
| Docker Container | Running instance of an image |

---

# 10. Docker Workflow

## Answer

A typical Docker workflow follows a simple lifecycle from writing code to running a container.

---

## Complete Workflow

```text
Write Application

↓

Create Dockerfile

↓

Build Docker Image

↓

Push Image to Registry

↓

Pull Image

↓

Run Docker Container
```

---

## Internal Workflow

```text
Developer

        │

Source Code

        │

Docker Build

        │

Docker Image

        │

Docker Registry

        │

Docker Pull

        │

Docker Run

        ▼

Running Container
```

---

## Production Example

A CI/CD pipeline performs the following steps:

1. Build the application.
2. Build the Docker image.
3. Push the image to Azure Container Registry (ACR).
4. Kubernetes pulls the image.
5. Deploy the application to AKS.

---

## Production Tip

Build the image once and promote the same image through Development, QA, Staging, and Production. Avoid rebuilding images for each environment.

---

## Interview Question

### Question

Explain the Docker workflow.

### Answer

The Docker workflow involves building an image from source code, storing it in a registry, pulling it to the target environment, and running it as a container.

---

# 11. Installing Docker

## Answer

Docker can be installed on:

- Windows
- Linux
- macOS

After installation, verify that Docker is working correctly.

---

## Verify Installation

```bash
docker --version
```

Example Output

```text
Docker version 28.x.x
```

---

## Verify Docker Engine

```bash
docker info
```

---

## Verify Running Containers

```bash
docker ps
```

---

## Internal Workflow

```text
Install Docker

↓

Start Docker Engine

↓

Verify Installation

↓

Ready to Run Containers
```

---

## Production Example

Before deploying applications, a DevOps engineer verifies that the Docker Engine is running and accessible.

---

## Production Tip

Keep Docker Engine updated with supported releases to receive security fixes and performance improvements.

---

## Interview Question

### Question

How do you verify that Docker is installed correctly?

### Answer

Run `docker --version`, `docker info`, and `docker ps` to verify the Docker CLI and Engine are functioning correctly.

---

# 12. Running Your First Container

## Answer

The easiest way to understand Docker is by running an existing image from Docker Hub.

---

## Run an NGINX Container

```bash
docker run nginx
```

If the image is not available locally, Docker automatically downloads it.

---

## Run in Detached Mode

```bash
docker run -d nginx
```

---

## Run with Port Mapping

```bash
docker run -d -p 8080:80 nginx
```

Now open:

```text
http://localhost:8080
```

You should see the default NGINX welcome page.

---

## View Running Containers

```bash
docker ps
```

---

## Stop the Container

```bash
docker stop <container-id>
```

---

## Internal Workflow

```text
docker run nginx

↓

Check Local Image

↓

Image Exists?

────────────┼────────────

        │

No

↓

Pull Image

↓

Create Container

↓

Start Container

Yes

↓

Create Container

↓

Start Container
```

---

## Production Example

A developer quickly starts an NGINX container to test reverse proxy configurations before deploying them to Kubernetes.

---

## Production Tip

Always specify an image tag such as `nginx:1.27` instead of relying on the `latest` tag to ensure predictable deployments.

---

## Interview Question

### Question

What happens when you run `docker run nginx`?

### Answer

Docker checks whether the image exists locally. If not, it downloads the image from the registry, creates a container, and starts it.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Using the `latest` image tag in production.

Use explicit version tags.

Example:

```text
nginx:1.27
```

---

## Mistake 2

❌ Installing application dependencies directly on the server.

Package all dependencies inside the Docker image.

---

## Mistake 3

❌ Confusing Images and Containers.

Remember:

```text
Image

↓

Template

↓

Container

↓

Running Instance
```

---

## Mistake 4

❌ Modifying running containers manually.

Rebuild the Docker image and redeploy instead of making manual changes.

---

## Mistake 5

❌ Running multiple unrelated applications inside a single container.

Follow the principle of one primary application per container whenever practical.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Use official base images whenever possible.

---

✅ Tag images with explicit versions.

---

✅ Build images once and reuse them across environments.

---

✅ Store images in a secure private registry.

---

✅ Keep Docker Engine updated.

---

✅ Monitor running containers and resource usage.

---

✅ Treat containers as immutable.

---

## 📌 Quick Revision

### Docker Workflow

```text
Application

↓

Dockerfile

↓

Docker Build

↓

Docker Image

↓

Docker Registry

↓

Docker Run

↓

Container
```

---

### Docker Architecture

```text
Docker Client

↓

Docker Daemon

↓

Docker Engine

↓

Image

↓

Container
```

---

### Common Commands

```bash
docker --version

docker info

docker run nginx

docker run -d nginx

docker run -d -p 8080:80 nginx

docker ps

docker stop <container-id>
```

---

# Summary

After completing this guide, you should understand:

✅ What is Docker?

✅ Why Docker?

✅ Problems Before Docker

✅ Virtual Machines vs Containers

✅ Docker Architecture

✅ Docker Components

✅ Docker Engine

✅ Docker Client

✅ Docker Daemon

✅ Docker Registry

✅ Docker Workflow

✅ Docker Installation

✅ Running Your First Container

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Docker? | An open-source containerization platform for packaging applications and their dependencies |
| Why do we use Docker? | To provide consistent, portable, and isolated application environments |
| What problem does Docker solve? | Environment inconsistency and dependency conflicts |
| What is the difference between a VM and a Container? | A VM includes a guest OS, while a container shares the host OS kernel |
| What is Docker Engine? | The runtime responsible for building and managing containers |
| What is Docker Daemon? | The background service that performs Docker operations |
| What is Docker Client? | The command-line interface used to communicate with Docker |
| What is a Docker Registry? | A repository for storing and distributing Docker images |
| What happens during `docker run`? | Docker pulls the image if needed, creates a container, and starts it |
| Why should images be immutable? | To ensure consistent, repeatable, and reliable deployments |

---

# 🎉 Docker Fundamentals Module Completed

Congratulations! You have completed **Docker Fundamentals**, including:

- ✅ Docker Basics
- ✅ Why Docker
- ✅ Virtual Machines vs Containers
- ✅ Docker Architecture
- ✅ Docker Components
- ✅ Docker Engine
- ✅ Docker Client & Daemon
- ✅ Docker Registry
- ✅ Docker Workflow
- ✅ Installing Docker
- ✅ Running Your First Container
- ✅ Production Best Practices

You now have a solid foundation to move into building, managing, and optimizing Docker images.

---

# 🚀 What's Next?

➡️ **02-docker-images.md**

In the next guide, you'll learn:

- What is a Docker Image?
- Image Layers
- Union File System
- Docker Hub
- Pulling Images
- Building Images
- Image Tagging
- Image History
- Image Optimization
- Production Best Practices
- Common Interview Questions