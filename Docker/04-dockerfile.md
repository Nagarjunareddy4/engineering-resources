# 🐳 Dockerfile

This guide explains everything you need to know about Dockerfiles.

If you've ever wondered:

- What is a Dockerfile?
- Why do we need a Dockerfile?
- How does Docker build an image?
- What are Dockerfile instructions?
- What is the Docker build process?

This guide is for you.

---

# 📑 Table of Contents

1. What is a Dockerfile?
2. Why do we need a Dockerfile?
3. Docker Build Process
4. Dockerfile Structure
5. Common Interview Questions

---

# 1. What is a Dockerfile?

## Answer

A **Dockerfile** is a plain text file containing a sequence of instructions that Docker uses to build a Docker Image.

Each instruction tells Docker how to create a new layer in the image.

A Dockerfile defines:

- Base Image
- Dependencies
- Application Files
- Environment Variables
- Startup Command
- Configuration

---

## Simple Architecture

```text
Application Source Code

↓

Dockerfile

↓

docker build

↓

Docker Image

↓

Docker Container
```

---

## Example Dockerfile

```dockerfile
FROM nginx:1.27

COPY . /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

## Key Characteristics

A Dockerfile is:

- Declarative
- Version Controlled
- Repeatable
- Portable
- Easy to Automate

---

## Real World Example

A Java Spring Boot application requires:

- Java 21
- JAR file
- Environment variables

Instead of configuring every server manually, a Dockerfile defines everything needed to build the image.

---

## Benefits

Dockerfiles provide:

- Automated Image Creation
- Consistent Builds
- Version Control
- Repeatable Deployments
- CI/CD Integration

---

## Production Tip

Store Dockerfiles alongside your application source code so every build uses the correct configuration.

---

## Interview Question

### Question

What is a Dockerfile?

### Answer

A Dockerfile is a text file containing instructions that Docker uses to automatically build a Docker Image.

---

# 2. Why do we need a Dockerfile?

## Answer

Without a Dockerfile, every Docker Image would need to be created manually.

Manual image creation is:

- Slow
- Error-Prone
- Difficult to Reproduce
- Hard to Maintain

A Dockerfile automates the image creation process.

---

## Without Dockerfile

```text
Install OS

↓

Install Runtime

↓

Install Dependencies

↓

Copy Application

↓

Configure Startup

↓

Run Application
```

Manual work every time.

---

## With Dockerfile

```text
Dockerfile

↓

docker build

↓

Docker Image

↓

Run Anywhere
```

---

## Problems Solved

Dockerfiles eliminate:

- Manual Installation
- Configuration Errors
- Environment Differences
- Build Inconsistencies

---

## Production Example

An Azure DevOps pipeline automatically builds a Docker Image using the Dockerfile whenever code is merged into the `main` branch.

---

## Production Tip

Keep the Dockerfile simple and deterministic so identical source code always produces the same Docker Image.

---

## Interview Question

### Question

Why do we need a Dockerfile?

### Answer

A Dockerfile automates Docker Image creation, ensuring consistent, repeatable, and version-controlled builds.

---

# 3. Docker Build Process

## Answer

Docker builds an image by reading the Dockerfile **from top to bottom**.

Each instruction creates a new image layer.

If a layer has not changed, Docker reuses it from the cache, making future builds much faster.

---

## Build Workflow

```text
Dockerfile

↓

Read Instruction

↓

Create Layer

↓

Next Instruction

↓

Create Layer

↓

Final Docker Image
```

---

## Example

Dockerfile

```dockerfile
FROM ubuntu:24.04

RUN apt update

RUN apt install -y nginx

COPY . /app

CMD ["nginx", "-g", "daemon off;"]
```

---

## Generated Layers

```text
Layer 5

CMD

↓

Layer 4

COPY Application

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

## Production Example

A developer modifies only the application source code.

Docker reuses:

- Base Image
- Operating System Layer
- Installed Packages

and rebuilds only the application layer, significantly reducing build time.

---

## Production Tip

Place frequently changing instructions (such as `COPY . .`) near the end of the Dockerfile to maximize build cache efficiency.

---

## Interview Question

### Question

How does Docker build an image?

### Answer

Docker reads the Dockerfile from top to bottom, creating a new image layer for each instruction and reusing cached layers whenever possible.

---

# 4. Dockerfile Structure

## Answer

Most Dockerfiles follow a common structure.

---

## Typical Dockerfile

```dockerfile
FROM ubuntu:24.04

WORKDIR /app

COPY . .

RUN apt update && \
    apt install -y nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

## Structure Breakdown

```text
FROM

↓

WORKDIR

↓

COPY

↓

RUN

↓

EXPOSE

↓

CMD
```

Each instruction serves a specific purpose.

---

## Internal Workflow

```text
Dockerfile

↓

Base Image

↓

Dependencies

↓

Application Files

↓

Configuration

↓

Startup Command

↓

Docker Image
```

---

## Production Example

A Node.js application Dockerfile typically:

1. Starts with a Node base image.
2. Sets the working directory.
3. Copies dependency files.
4. Installs dependencies.
5. Copies the application code.
6. Exposes the application port.
7. Starts the application.

---

## Production Tip

Organize Dockerfile instructions logically to improve readability and maximize build cache efficiency.

---

## Interview Question

### Question

What is the typical structure of a Dockerfile?

### Answer

A Dockerfile usually defines a base image, working directory, dependencies, application files, exposed ports, and the startup command.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Dockerfile | Blueprint for building Docker Images |
| `docker build` | Creates an image from a Dockerfile |
| Build Process | Reads instructions from top to bottom |
| Image Layers | One layer per Dockerfile instruction |
| Docker Cache | Reuses unchanged layers to speed up builds |
| Dockerfile Structure | Base Image → Dependencies → Application → Startup |

---

# 5. FROM Instruction

## Answer

The **FROM** instruction specifies the **base image** for the Docker Image.

Every Dockerfile must begin with a `FROM` instruction (except in advanced multi-stage scenarios where each stage also starts with `FROM`).

Docker builds every subsequent layer on top of this base image.

---

## Syntax

```dockerfile
FROM <image>:<tag>
```

---

## Examples

```dockerfile
FROM ubuntu:24.04
```

```dockerfile
FROM nginx:1.27
```

```dockerfile
FROM eclipse-temurin:21-jdk
```

---

## Internal Workflow

```text
Base Image

↓

Add Layers

↓

Docker Image
```

---

## Production Example

A Spring Boot application starts with:

```dockerfile
FROM eclipse-temurin:21-jre
```

This provides the Java Runtime Environment required to run the application.

---

## Production Tip

Always use a specific version tag instead of `latest` to ensure predictable builds.

---

## Interview Question

### Question

What is the purpose of the `FROM` instruction?

### Answer

The `FROM` instruction specifies the base image on which the Docker Image is built.

---

# 6. RUN Instruction

## Answer

The **RUN** instruction executes commands while building the Docker Image.

Each `RUN` instruction creates a new image layer.

Typical uses include:

- Installing packages
- Updating package indexes
- Creating directories
- Downloading dependencies

---

## Syntax

```dockerfile
RUN <command>
```

---

## Example

```dockerfile
RUN apt update && \
    apt install -y nginx
```

---

## Internal Workflow

```text
Docker Build

↓

RUN Command

↓

New Layer

↓

Continue Build
```

---

## Production Example

A Python application installs dependencies during the build:

```dockerfile
RUN pip install -r requirements.txt
```

---

## Production Tip

Combine related commands into a single `RUN` instruction to reduce the number of image layers.

---

## Interview Question

### Question

What does the `RUN` instruction do?

### Answer

The `RUN` instruction executes commands during the image build process and creates a new image layer.

---

# 7. COPY and ADD Instructions

## COPY

### Answer

The **COPY** instruction copies files or directories from the build context into the Docker Image.

---

## Syntax

```dockerfile
COPY <source> <destination>
```

---

## Example

```dockerfile
COPY . /app
```

---

## ADD

### Answer

The **ADD** instruction performs everything `COPY` does and also supports:

- Extracting local compressed archives (such as `.tar`)
- Downloading files from URLs (although this is generally discouraged in production)

---

## Example

```dockerfile
ADD app.tar.gz /app
```

---

## Comparison

| COPY | ADD |
|------|-----|
| Copies files | Copies files |
| Simple and predictable | Can extract archives |
| Preferred in most cases | Supports additional features |

---

## Internal Workflow

```text
Application Files

↓

COPY / ADD

↓

Docker Image
```

---

## Production Example

A Node.js application copies:

```dockerfile
COPY package.json .
```

before installing dependencies to improve build cache reuse.

---

## Production Tip

Prefer `COPY` over `ADD` unless you specifically require archive extraction or other `ADD` functionality.

---

## Interview Question

### Question

What is the difference between `COPY` and `ADD`?

### Answer

`COPY` copies files into the image, while `ADD` can also extract local archives and supports additional features. In most cases, `COPY` is the recommended choice.

---

# 8. WORKDIR Instruction

## Answer

The **WORKDIR** instruction sets the working directory for all subsequent instructions.

If the directory does not exist, Docker creates it automatically.

---

## Syntax

```dockerfile
WORKDIR /app
```

---

## Example

```dockerfile
FROM node:22

WORKDIR /app

COPY . .

RUN npm install
```

---

## Internal Workflow

```text
Create Directory

↓

Set Working Directory

↓

Execute Remaining Instructions
```

---

## Production Example

A Java application stores all source code under:

```text
/app
```

making the Dockerfile clean and consistent.

---

## Production Tip

Always use `WORKDIR` instead of repeatedly using `cd` inside `RUN` instructions.

---

## Interview Question

### Question

What is the purpose of `WORKDIR`?

### Answer

`WORKDIR` sets the default working directory for all subsequent Dockerfile instructions.

---

# 9. CMD vs ENTRYPOINT

## CMD

### Answer

The **CMD** instruction specifies the **default command** that runs when the container starts.

It can be overridden at runtime.

---

## Example

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

---

## ENTRYPOINT

### Answer

The **ENTRYPOINT** instruction defines the primary executable for the container.

It is intended to make the container behave like a dedicated executable and is not easily replaced.

---

## Example

```dockerfile
ENTRYPOINT ["python"]
```

---

## Comparison

| CMD | ENTRYPOINT |
|-----|------------|
| Default command | Main executable |
| Can be overridden easily | Designed as the fixed executable |
| Provides default arguments | Defines container behavior |

---

## Internal Workflow

```text
Container Starts

↓

ENTRYPOINT

↓

CMD Arguments

↓

Application Runs
```

---

## Production Example

A Python utility image uses:

```dockerfile
ENTRYPOINT ["python"]

CMD ["app.py"]
```

Running:

```bash
docker run my-app
```

executes:

```text
python app.py
```

---

## Production Tip

Use `ENTRYPOINT` for the main executable and `CMD` to provide default arguments that users can override if needed.

---

## Interview Question

### Question

What is the difference between `CMD` and `ENTRYPOINT`?

### Answer

`CMD` provides the default command or arguments for a container, while `ENTRYPOINT` defines the primary executable that runs when the container starts.

---

## 📌 Quick Revision

| Instruction | Purpose |
|-------------|---------|
| `FROM` | Specifies the base image |
| `RUN` | Executes commands during build |
| `COPY` | Copies files into the image |
| `ADD` | Copies files and can extract local archives |
| `WORKDIR` | Sets the working directory |
| `CMD` | Defines the default startup command |
| `ENTRYPOINT` | Defines the main executable |

---

# 10. ENV Instruction

## Answer

The **ENV** instruction defines environment variables that are available during image runtime.

These variables can be accessed by the application running inside the container.

---

## Syntax

```dockerfile
ENV <key>=<value>
```

---

## Example

```dockerfile
ENV APP_ENV=production

ENV PORT=8080
```

---

## Internal Workflow

```text
Dockerfile

↓

ENV Variables

↓

Docker Image

↓

Container

↓

Application Reads Variables
```

---

## Production Example

A Spring Boot application uses:

```dockerfile
ENV JAVA_OPTS="-Xms512m -Xmx1024m"
```

to define default JVM options.

---

## Production Tip

Use `ENV` for non-sensitive configuration. Never store passwords, API keys, or secrets using `ENV`.

---

## Interview Question

### Question

What is the purpose of the `ENV` instruction?

### Answer

The `ENV` instruction defines environment variables that are available to the application running inside the container.

---

# 11. ARG Instruction

## Answer

The **ARG** instruction defines variables that are available **only during the image build process**.

Unlike `ENV`, ARG values are not automatically available at container runtime.

---

## Syntax

```dockerfile
ARG VERSION=1.0
```

---

## Example

```dockerfile
ARG APP_VERSION=2.1

FROM nginx:1.27
```

Build command:

```bash
docker build \
--build-arg APP_VERSION=2.2 \
-t web-app .
```

---

## ENV vs ARG

| ENV | ARG |
|------|-----|
| Runtime variable | Build-time variable |
| Available inside container | Available only during build |
| Stored in image metadata | Used while building the image |

---

## Production Example

A CI pipeline passes the application version using:

```bash
--build-arg BUILD_NUMBER=245
```

during image creation.

---

## Production Tip

Use `ARG` for build-time customization and `ENV` for runtime configuration.

---

## Interview Question

### Question

What is the difference between `ENV` and `ARG`?

### Answer

`ENV` defines runtime environment variables, while `ARG` defines build-time variables that are available only during image creation.

---

# 12. EXPOSE and USER Instructions

## EXPOSE

### Answer

The **EXPOSE** instruction documents the network port the application listens on inside the container.

It does **not** publish the port automatically.

---

## Example

```dockerfile
EXPOSE 8080
```

Port publishing still requires:

```bash
docker run -p 8080:8080 my-app
```

---

## USER

### Answer

The **USER** instruction specifies which user the container should run as.

By default, containers often run as the **root** user, which is not recommended for production.

---

## Example

```dockerfile
RUN useradd appuser

USER appuser
```

---

## Internal Workflow

```text
Dockerfile

↓

Create User

↓

Switch User

↓

Start Container
```

---

## Production Example

A financial application runs as a non-root user to reduce the impact of potential security vulnerabilities.

---

## Production Tip

Always run containers as a non-root user unless absolutely necessary.

---

## Interview Question

### Question

Why should the `USER` instruction be used?

### Answer

The `USER` instruction improves container security by running applications with a non-root user instead of the default root account.

---

# 13. Multi-stage Builds

## Answer

A **Multi-stage Build** uses multiple `FROM` instructions in a single Dockerfile.

This separates the build environment from the runtime environment, resulting in much smaller production images.

---

## Example

```dockerfile
# Build Stage
FROM maven:3.9-eclipse-temurin-21 AS builder

WORKDIR /app

COPY . .

RUN mvn clean package

# Runtime Stage
FROM eclipse-temurin:21-jre

WORKDIR /app

COPY --from=builder \
/app/target/app.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
```

---

## Internal Workflow

```text
Source Code

↓

Build Stage

↓

Compiled Artifact

↓

Runtime Stage

↓

Small Docker Image
```

---

## Benefits

- Smaller Images
- Faster Deployments
- Reduced Attack Surface
- Cleaner Runtime Environment

---

## Production Example

A Java application is compiled in the builder stage using Maven, but the final runtime image contains only the JAR file and the Java Runtime Environment.

---

## Production Tip

Use multi-stage builds for Java, .NET, Go, Node.js, and other compiled applications to remove unnecessary build tools from production images.

---

## Interview Question

### Question

What is a Multi-stage Build?

### Answer

A Multi-stage Build uses multiple build stages to separate compilation from runtime, creating smaller and more secure Docker Images.

---

# 14. Common Beginner Mistakes

## Mistake 1

❌ Using the `latest` tag in the `FROM` instruction.

Use versioned images:

```dockerfile
FROM nginx:1.27
```

---

## Mistake 2

❌ Creating too many image layers.

Combine related `RUN` instructions whenever appropriate.

---

## Mistake 3

❌ Copying unnecessary files into the image.

Use a `.dockerignore` file to exclude:

- `.git`
- `node_modules`
- Log files
- Temporary files
- Build artifacts

---

## Mistake 4

❌ Running containers as the root user.

Create and use a dedicated non-root user.

---

## Mistake 5

❌ Storing secrets inside Dockerfiles.

Never include:

- Passwords
- API Keys
- Access Tokens
- Certificates

Use Docker Secrets, Kubernetes Secrets, or an external secret manager instead.

---

# 15. Production Best Practices

Always follow these recommendations.

---

✅ Use official base images.

---

✅ Pin image versions instead of using `latest`.

---

✅ Keep Dockerfiles simple and readable.

---

✅ Use `.dockerignore` to reduce build context.

---

✅ Use multi-stage builds.

---

✅ Run containers as non-root users.

---

✅ Store secrets outside Docker Images.

---

✅ Optimize layer caching by ordering instructions carefully.

---

## 📌 Quick Revision

### Dockerfile Workflow

```text
Source Code

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

### Common Dockerfile Instructions

```text
FROM

↓

WORKDIR

↓

COPY

↓

RUN

↓

ENV

↓

EXPOSE

↓

USER

↓

CMD / ENTRYPOINT
```

---

### Multi-stage Build

```text
Build Stage

↓

Compile Application

↓

Runtime Stage

↓

Small Production Image
```

---

# Summary

After completing this guide, you should understand:

✅ Dockerfile Basics

✅ Build Process

✅ FROM

✅ RUN

✅ COPY

✅ ADD

✅ WORKDIR

✅ CMD

✅ ENTRYPOINT

✅ ENV

✅ ARG

✅ EXPOSE

✅ USER

✅ Multi-stage Builds

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is a Dockerfile? | A text file containing instructions to build a Docker Image |
| What does `FROM` do? | Specifies the base image |
| What does `RUN` do? | Executes commands during image build |
| What is the difference between `COPY` and `ADD`? | `COPY` copies files, while `ADD` can also extract local archives |
| What does `WORKDIR` do? | Sets the working directory for subsequent instructions |
| What is the difference between `CMD` and `ENTRYPOINT`? | `CMD` provides default commands or arguments, while `ENTRYPOINT` defines the main executable |
| What is the difference between `ENV` and `ARG`? | `ENV` is for runtime variables; `ARG` is for build-time variables |
| What does `EXPOSE` do? | Documents the container's listening port |
| Why should `USER` be used? | To run containers as a non-root user for better security |
| What is a Multi-stage Build? | A technique that separates build and runtime stages to produce smaller, more secure images |

---

# 🎉 Dockerfile Module Completed

Congratulations! You have completed **Dockerfile**, including:

- ✅ Dockerfile Fundamentals
- ✅ Docker Build Process
- ✅ FROM
- ✅ RUN
- ✅ COPY & ADD
- ✅ WORKDIR
- ✅ CMD & ENTRYPOINT
- ✅ ENV & ARG
- ✅ EXPOSE & USER
- ✅ Multi-stage Builds
- ✅ Production Best Practices

You now have the knowledge to create efficient, secure, and production-ready Docker Images.

---

# 🚀 What's Next?

➡️ **05-docker-volumes-networking.md**

In the next guide, you'll learn:

- What are Docker Volumes?
- Bind Mounts vs Named Volumes
- Volume Management
- Docker Networks
- Bridge Network
- Host Network
- None Network
- Overlay Network
- Port Mapping
- DNS Resolution
- Production Best Practices
- Common Interview Questions