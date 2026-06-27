# 🐳 Docker Compose

This guide explains everything you need to know about Docker Compose.

If you've ever wondered:

- What is Docker Compose?
- Why do we need Docker Compose?
- How do we run multiple containers together?
- What is a docker-compose.yml file?
- How does Docker Compose work?

This guide is for you.

---

# 📑 Table of Contents

1. What is Docker Compose?
2. Why Do We Need Docker Compose?
3. Docker Compose Architecture
4. Docker Compose File Structure
5. Common Interview Questions

---

# 1. What is Docker Compose?

## Answer

**Docker Compose** is a tool used to define and manage **multi-container Docker applications** using a single YAML configuration file called **`docker-compose.yml`**.

Instead of starting each container individually with multiple `docker run` commands, Docker Compose allows you to define all services, networks, and volumes in one file and start everything with a single command.

---

## Simple Architecture

```text
docker-compose.yml

        │

Docker Compose

        │

────────────────────────

│          │          │

Frontend  Backend   Database

(Container)(Container)(Container)

        │

Docker Network

        │

Docker Volumes
```

---

## Key Characteristics

Docker Compose is:

- Declarative
- Easy to Read
- Easy to Maintain
- Version Controlled
- Ideal for Multi-Container Applications

---

## Real World Example

An online shopping application consists of:

- React Frontend
- Spring Boot Backend
- PostgreSQL Database
- Redis Cache

Instead of manually starting four containers, Docker Compose starts all of them together.

---

## Benefits

Docker Compose provides:

- Simplified Deployment
- Consistent Environments
- Easy Service Management
- Built-in Networking
- Built-in Volume Support

---

## Production Tip

Docker Compose is excellent for local development, testing, and proof-of-concept environments. For large-scale production deployments, Kubernetes or Docker Swarm is generally a better choice.

---

## Interview Question

### Question

What is Docker Compose?

### Answer

Docker Compose is a tool used to define and manage multi-container Docker applications using a single `docker-compose.yml` file.

---

# 2. Why Do We Need Docker Compose?

## Answer

Modern applications rarely consist of a single container.

A typical application may require:

- Web Server
- Backend API
- Database
- Cache
- Message Broker

Managing each container individually becomes difficult.

Docker Compose automates this process.

---

## Without Docker Compose

```text
docker run frontend

↓

docker run backend

↓

docker run database

↓

docker run redis

↓

Connect Everything Manually
```

---

## With Docker Compose

```text
docker compose up

↓

Everything Starts Automatically
```

---

## Problems Docker Compose Solves

- Multiple `docker run` commands
- Manual network creation
- Manual volume creation
- Container dependency management
- Environment inconsistency

---

## Production Example

A development team uses Docker Compose to start:

- API
- PostgreSQL
- Redis
- RabbitMQ

with a single command before beginning development.

---

## Production Tip

Keep the Compose file in your application's source repository so every developer uses the same local environment.

---

## Interview Question

### Question

Why do we use Docker Compose?

### Answer

Docker Compose simplifies the management of multi-container applications by defining all services, networks, and volumes in one configuration file.

---

# 3. Docker Compose Architecture

## Answer

Docker Compose acts as an orchestration layer for Docker containers running on a single host.

It reads the `docker-compose.yml` file and creates:

- Containers
- Networks
- Volumes

based on the defined configuration.

---

## Architecture

```text
docker-compose.yml

        │

Docker Compose CLI

        │

Docker Engine

        │

────────────────────────────

│          │          │

Container  Container  Container

        │

Docker Network

        │

Docker Volume
```

---

## Internal Workflow

```text
docker-compose.yml

↓

Read Configuration

↓

Create Network

↓

Create Volumes

↓

Create Containers

↓

Start Services
```

---

## Production Example

A developer runs:

```bash
docker compose up
```

Docker Compose automatically:

1. Creates the network.
2. Creates required volumes.
3. Starts the database.
4. Starts the backend.
5. Starts the frontend.

---

## Production Tip

Allow Docker Compose to manage networking automatically instead of manually creating Docker networks for local development.

---

## Interview Question

### Question

How does Docker Compose work?

### Answer

Docker Compose reads the `docker-compose.yml` file and automatically creates the required containers, networks, and volumes before starting the application.

---

# 4. Docker Compose File Structure

## Answer

The heart of Docker Compose is the **`docker-compose.yml`** file.

It describes the complete application stack.

---

## Basic Structure

```yaml
services:

  frontend:

  backend:

  database:

volumes:

networks:
```

---

## Simple Example

```yaml
version: "3.9"

services:

  nginx:

    image: nginx:1.27

    ports:

      - "8080:80"
```

---

## Main Sections

| Section | Purpose |
|----------|---------|
| `services` | Defines application containers |
| `volumes` | Defines persistent storage |
| `networks` | Defines communication between services |

---

## Internal Workflow

```text
docker-compose.yml

↓

Services

↓

Networks

↓

Volumes

↓

Application Running
```

---

## Production Example

A Compose file defines:

- `frontend`
- `backend`
- `postgres`
- `redis`

All services start together and communicate over the same Docker network.

---

## Production Tip

Use clear service names because Docker Compose automatically uses them for internal DNS resolution.

---

## Interview Question

### Question

What are the main sections of a `docker-compose.yml` file?

### Answer

The primary sections are `services`, `volumes`, and `networks`, which define containers, persistent storage, and communication respectively.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Docker Compose | Tool for managing multi-container applications |
| `docker-compose.yml` | Configuration file |
| Services | Application containers |
| Networks | Container communication |
| Volumes | Persistent storage |
| `docker compose up` | Start the complete application stack |

---

# 5. Services

## Answer

A **Service** in Docker Compose represents a container definition.

Each service specifies how a container should be built or started.

A service can define:

- Image
- Build Context
- Ports
- Environment Variables
- Volumes
- Networks
- Restart Policy

---

## Example

```yaml
services:

  backend:

    image: springboot-api:v1

  frontend:

    image: react-app:v1

  database:

    image: postgres:16
```

---

## Internal Workflow

```text
docker-compose.yml

↓

Services

↓

Containers

↓

Application Running
```

---

## Production Example

An e-commerce application defines:

- frontend
- backend
- postgres
- redis

Each service starts as its own Docker container while communicating through a shared Docker network.

---

## Production Tip

Assign meaningful service names because Docker Compose automatically uses them as hostnames for inter-service communication.

---

## Interview Question

### Question

What is a Service in Docker Compose?

### Answer

A Service defines how a Docker container should be created and managed, including its image, ports, volumes, networks, and configuration.

---

# 6. Networks

## Answer

Docker Compose automatically creates a network for all services in the project.

Every service connected to the same network can communicate using its **service name**.

No manual IP management is required.

---

## Example

```yaml
services:

  backend:

    image: backend:v1

  database:

    image: postgres:16

networks:

  app-network:
```

---

## Internal Workflow

```text
Frontend

↓

Docker Network

↓

Backend

↓

Database
```

---

## Communication Example

Instead of:

```text
192.168.1.15
```

use:

```text
database
```

Docker automatically resolves:

```text
database
```

to the correct container.

---

## Production Example

The backend connects to PostgreSQL using:

```text
Host = database
```

instead of a changing container IP address.

---

## Production Tip

Always use service names for communication between containers instead of hardcoding IP addresses.

---

## Interview Question

### Question

How do services communicate in Docker Compose?

### Answer

Services communicate using Docker's built-in DNS, where the service name acts as the hostname.

---

# 7. Volumes

## Answer

Docker Compose supports persistent storage using **Volumes**.

Volumes allow data to survive:

- Container Restart
- Container Removal
- Container Upgrade

---

## Example

```yaml
services:

  postgres:

    image: postgres:16

    volumes:

      - postgres-data:/var/lib/postgresql/data

volumes:

  postgres-data:
```

---

## Internal Workflow

```text
Container

↓

Docker Volume

↓

Host Storage

↓

Persistent Data
```

---

## Production Example

A PostgreSQL database stores all tables and transactions in a Docker Volume.

Replacing the PostgreSQL container does not affect the stored data.

---

## Production Tip

Use Docker Volumes for databases, uploaded files, and application data that must persist beyond the container lifecycle.

---

## Interview Question

### Question

Why are Volumes used in Docker Compose?

### Answer

Volumes provide persistent storage so important application data survives container restarts and replacements.

---

# 8. Environment Variables

## Answer

Environment variables allow configuration values to be passed into containers without modifying application code.

They are commonly used for:

- Database Credentials
- Application Ports
- Runtime Profiles
- API Endpoints

---

## Example

```yaml
services:

  backend:

    image: backend:v1

    environment:

      SPRING_PROFILES_ACTIVE: production

      DB_HOST: database
```

---

## Internal Workflow

```text
docker-compose.yml

↓

Environment Variables

↓

Container

↓

Application Reads Configuration
```

---

## Production Example

A Spring Boot application reads:

```text
SPRING_PROFILES_ACTIVE=production
```

to load production-specific configuration.

---

## Production Tip

Avoid storing sensitive information directly inside the `docker-compose.yml` file. Use environment files (`.env`) or secret management solutions for credentials.

---

## Interview Question

### Question

Why are environment variables used in Docker Compose?

### Answer

Environment variables provide runtime configuration without requiring changes to the application code or Docker image.

---

# 9. depends_on & Application Lifecycle

## Answer

The **`depends_on`** option specifies startup dependencies between services.

It ensures that dependent containers are started before the current service.

> **Note:** `depends_on` controls startup order but **does not guarantee that the dependent service is fully ready**. Applications should implement health checks or retry logic when required.

---

## Example

```yaml
services:

  backend:

    image: backend:v1

    depends_on:

      - database

  database:

    image: postgres:16
```

---

## Start All Services

```bash
docker compose up
```

---

## Start in Detached Mode

```bash
docker compose up -d
```

---

## Stop All Services

```bash
docker compose down
```

---

## Internal Workflow

```text
Read Compose File

↓

Create Network

↓

Create Volumes

↓

Start Database

↓

Start Backend

↓

Start Frontend
```

---

## Production Example

A developer starts the complete application stack using:

```bash
docker compose up -d
```

Within seconds, the frontend, backend, database, and Redis services are running.

---

## Production Tip

Combine `depends_on` with application health checks to ensure dependent services are actually ready before accepting requests.

---

## Interview Question

### Question

What is the purpose of `depends_on` in Docker Compose?

### Answer

`depends_on` defines the startup order between services, ensuring dependent containers are started before the current service, though it does not guarantee application readiness.

---

## 📌 Quick Revision

| Feature | Purpose |
|----------|---------|
| Services | Define application containers |
| Networks | Enable communication between services |
| Volumes | Provide persistent storage |
| Environment Variables | Supply runtime configuration |
| `depends_on` | Define startup order between services |
| `docker compose up` | Start the application stack |
| `docker compose down` | Stop and remove the application stack |

---

# 10. Scaling Services

## Answer

Docker Compose allows you to run multiple instances of the same service.

Scaling is useful for:

- Load Testing
- High Availability (Development)
- Simulating Production Environments

> **Note:** Modern Docker Compose supports scaling with the `--scale` option. The older `docker-compose scale` command is deprecated.

---

## Scale a Service

```bash
docker compose up -d --scale backend=3
```

This creates:

```text
backend_1

backend_2

backend_3
```

---

## Internal Workflow

```text
Backend Service

↓

Scale = 3

↓

Container 1

Container 2

Container 3
```

---

## Production Example

During performance testing, the backend service is scaled to three instances to simulate multiple application servers.

---

## Production Tip

Docker Compose scaling is suitable for development and testing. For production auto-scaling and advanced orchestration, use Kubernetes or Docker Swarm.

---

## Interview Question

### Question

How do you scale a service in Docker Compose?

### Answer

Use:

```bash
docker compose up -d --scale <service-name>=<number>
```

to start multiple instances of a service.

---

# 11. Common Docker Compose Commands

## Start Services

```bash
docker compose up
```

---

## Start in Background

```bash
docker compose up -d
```

---

## Stop Services

```bash
docker compose stop
```

---

## Remove Services

```bash
docker compose down
```

---

## Restart Services

```bash
docker compose restart
```

---

## View Running Services

```bash
docker compose ps
```

---

## View Logs

```bash
docker compose logs
```

---

## Follow Logs

```bash
docker compose logs -f
```

---

## Internal Workflow

```text
Compose File

↓

Docker Compose

↓

Containers

↓

Manage Services
```

---

## Production Example

A developer checks logs for all services using:

```bash
docker compose logs -f
```

to troubleshoot communication issues between the backend and database.

---

## Production Tip

Use `docker compose ps` and `docker compose logs` as the first steps when troubleshooting Compose-based applications.

---

## Interview Question

### Question

Which command displays the status of Docker Compose services?

### Answer

Use:

```bash
docker compose ps
```

to display the status of all services.

---

# 12. Common Beginner Mistakes

## Mistake 1

❌ Using container IP addresses.

Use service names instead.

Example:

```text
database
```

instead of:

```text
172.18.0.5
```

---

## Mistake 2

❌ Storing database data inside containers.

Use Docker Volumes.

---

## Mistake 3

❌ Hardcoding passwords in the Compose file.

Use:

- `.env` files
- Docker Secrets
- Kubernetes Secrets (for Kubernetes deployments)

---

## Mistake 4

❌ Ignoring service dependencies.

Use:

```yaml
depends_on:
```

and implement health checks or retry logic where necessary.

---

## Mistake 5

❌ Using Docker Compose for large-scale production orchestration.

Use Kubernetes or Docker Swarm for production environments requiring advanced scheduling, self-healing, and auto-scaling.

---

# 13. Production Best Practices

Always follow these recommendations.

---

✅ Keep the `docker-compose.yml` file under version control.

---

✅ Use Named Volumes for persistent data.

---

✅ Use service names for internal communication.

---

✅ Store configuration using environment variables.

---

✅ Separate development and production Compose files when configurations differ.

Example:

```text
docker-compose.yml

docker-compose.prod.yml
```

---

✅ Monitor container logs regularly.

---

✅ Keep images versioned instead of using the `latest` tag.

---

✅ Validate the Compose configuration before deployment.

```bash
docker compose config
```

---

## 📌 Quick Revision

### Docker Compose Workflow

```text
docker-compose.yml

↓

Docker Compose

↓

Create Network

↓

Create Volumes

↓

Start Services

↓

Application Running
```

---

### Multi-Container Architecture

```text
Frontend

↓

Backend

↓

Database

↓

Redis
```

---

### Common Commands

```bash
docker compose up

docker compose up -d

docker compose down

docker compose stop

docker compose restart

docker compose ps

docker compose logs

docker compose config
```

---

# Summary

After completing this guide, you should understand:

✅ Docker Compose

✅ Multi-Container Applications

✅ docker-compose.yml

✅ Services

✅ Networks

✅ Volumes

✅ Environment Variables

✅ depends_on

✅ Scaling Services

✅ Docker Compose Commands

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Docker Compose? | A tool for defining and managing multi-container Docker applications |
| What file does Docker Compose use? | `docker-compose.yml` |
| What is a Service? | A definition of how a container should be created and managed |
| How do services communicate? | Through Docker's built-in DNS using service names |
| Why are Volumes used? | To provide persistent storage |
| What is `depends_on`? | It defines startup order between services |
| How do you start all services? | `docker compose up` |
| How do you stop and remove services? | `docker compose down` |
| How do you scale a service? | `docker compose up -d --scale <service>=<count>` |
| Why isn't Docker Compose the preferred choice for large production deployments? | It lacks advanced orchestration features such as self-healing, automatic scheduling, and auto-scaling that Kubernetes provides |

---

# 🎉 Docker Compose Module Completed

Congratulations! You have completed **Docker Compose**, including:

- ✅ Docker Compose Fundamentals
- ✅ Multi-Container Architecture
- ✅ docker-compose.yml
- ✅ Services
- ✅ Networks
- ✅ Volumes
- ✅ Environment Variables
- ✅ depends_on
- ✅ Scaling Services
- ✅ Docker Compose Commands
- ✅ Production Best Practices

You now have a strong understanding of how to build and manage multi-container applications using Docker Compose.

---

# 🚀 What's Next?

➡️ **07-docker-registry.md**

In the next guide, you'll learn:

- What is a Docker Registry?
- Docker Hub
- Private Registries
- Azure Container Registry (ACR)
- Pushing Images
- Pulling Images
- Image Authentication
- Image Scanning
- Image Signing
- Production Best Practices
- Common Interview Questions