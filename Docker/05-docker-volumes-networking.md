# 🐳 Docker Volumes & Networking

This guide explains everything you need to know about Docker Volumes and Docker Networking.

If you've ever wondered:

- Why does data disappear when a container is deleted?
- How can Docker persist application data?
- What are Docker Volumes?
- What are Bind Mounts?
- How do Docker containers communicate?
- What are Docker Networks?

This guide is for you.

---

# 📑 Table of Contents

1. What are Docker Volumes?
2. Why Do We Need Volumes?
3. Types of Docker Storage
4. Docker Networks Overview
5. Common Interview Questions

---

# 1. What are Docker Volumes?

## Answer

A **Docker Volume** is a persistent storage mechanism managed by Docker that allows data to exist independently of a container's lifecycle.

Normally, when a container is removed, all data stored inside the container's writable layer is also deleted.

Volumes solve this problem by storing data outside the container.

---

## Architecture

```text
Docker Container

        │

Application

        │

Docker Volume

        │

Host Storage
```

---

## Internal Workflow

```text
Container

↓

Write Data

↓

Docker Volume

↓

Host Disk
```

---

## Key Characteristics

Docker Volumes are:

- Persistent
- Managed by Docker
- Reusable
- Shareable
- Independent of Container Lifecycle

---

## Real World Example

A MySQL database stores:

- Tables
- User Data
- Transactions

inside a Docker Volume.

Even if the MySQL container is deleted, the database files remain in the volume.

---

## Benefits

Docker Volumes provide:

- Persistent Data
- Easy Backup
- Data Sharing
- Better Performance
- Simplified Storage Management

---

## Production Tip

Always store production database data inside Docker Volumes instead of relying on the container's writable layer.

---

## Interview Question

### Question

What is a Docker Volume?

### Answer

A Docker Volume is a Docker-managed persistent storage location that allows data to survive container deletion and be shared across containers.

---

# 2. Why Do We Need Volumes?

## Answer

Containers are designed to be **ephemeral**, meaning they can be created and removed at any time.

Without persistent storage:

- Uploaded files disappear.
- Database records are lost.
- Application logs are deleted.
- Configuration changes are removed.

Volumes prevent data loss.

---

## Without Volume

```text
Container

↓

Write Data

↓

Delete Container

↓

Data Lost
```

---

## With Volume

```text
Container

↓

Write Data

↓

Docker Volume

↓

Delete Container

↓

Data Still Exists
```

---

## Production Example

An e-commerce application stores customer orders in a PostgreSQL container.

The database uses a Docker Volume.

When the PostgreSQL container is upgraded, the new container attaches the existing volume and continues using the same data.

---

## Production Tip

Separate application code from persistent data. Containers should be replaceable without affecting stored information.

---

## Interview Question

### Question

Why are Docker Volumes required?

### Answer

Docker Volumes provide persistent storage so that important data survives container restarts, updates, and deletions.

---

# 3. Types of Docker Storage

## Answer

Docker supports several storage options depending on the use case.

---

## 1. Writable Layer

Every container has its own writable layer.

Characteristics:

- Temporary
- Deleted with the container
- Not suitable for persistent data

---

## 2. Named Volumes

Managed completely by Docker.

Example:

```bash
docker volume create mysql-data
```

Use:

```bash
docker run -v mysql-data:/var/lib/mysql mysql
```

---

## 3. Bind Mounts

A directory from the host machine is mounted into the container.

Example:

```bash
docker run \
-v /home/user/data:/app/data \
nginx
```

---

## Comparison

| Storage Type | Persistent | Managed by Docker | Common Use Case |
|--------------|-----------|-------------------|-----------------|
| Writable Layer | ❌ No | Yes | Temporary runtime changes |
| Named Volume | ✅ Yes | Yes | Databases, application data |
| Bind Mount | ✅ Yes | No | Development and local testing |

---

## Internal Workflow

```text
Application

↓

Writable Layer

OR

Named Volume

OR

Bind Mount

↓

Host Storage
```

---

## Production Example

A development team uses:

- Bind Mounts during local development for instant code changes.
- Named Volumes in production for persistent database storage.

---

## Production Tip

Use Named Volumes in production whenever possible. Bind Mounts are best suited for development and debugging.

---

## Interview Question

### Question

What are the different Docker storage types?

### Answer

Docker provides a writable layer, named volumes, and bind mounts for different storage requirements.

---

# 4. Docker Networks Overview

## Answer

Docker Networking enables containers to communicate with:

- Other Containers
- The Host Machine
- External Systems
- The Internet

Without networking, containers would be isolated and unable to exchange data.

---

## Architecture

```text
Container A

        │

Docker Network

        │

Container B

        │

Internet
```

---

## Internal Workflow

```text
Container

↓

Docker Network

↓

Another Container

↓

External Services
```

---

## Why Docker Networking?

Networking enables:

- Microservices Communication
- API Calls
- Database Connections
- Internet Access
- Service Discovery

---

## Production Example

An online shopping application consists of:

- Frontend Container
- Backend API Container
- MySQL Database Container

All three containers communicate securely using a Docker Network.

---

## Production Tip

Instead of connecting containers using IP addresses, use container names. Docker provides built-in DNS resolution for containers on the same user-defined network.

---

## Interview Question

### Question

Why is Docker Networking important?

### Answer

Docker Networking allows containers to communicate with each other, the host, and external services, making multi-container applications possible.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Docker Volume | Persistent Docker-managed storage |
| Writable Layer | Temporary container storage |
| Named Volume | Persistent storage managed by Docker |
| Bind Mount | Host directory mounted into a container |
| Docker Network | Enables communication between containers |
| Container Communication | Uses Docker Networking |

---

# 5. Creating and Managing Docker Volumes

## Answer

Docker provides commands to create, inspect, list, and remove volumes.

Volumes are managed independently of containers, allowing data to persist even after containers are deleted.

---

## Create a Volume

```bash
docker volume create mysql-data
```

---

## List Volumes

```bash
docker volume ls
```

Example Output

```text
DRIVER    VOLUME NAME

local     mysql-data

local     nginx-logs
```

---

## Inspect a Volume

```bash
docker volume inspect mysql-data
```

---

## Remove a Volume

```bash
docker volume rm mysql-data
```

---

## Remove Unused Volumes

```bash
docker volume prune
```

---

## Internal Workflow

```text
Create Volume

↓

Store Data

↓

Attach to Container

↓

Container Deleted

↓

Volume Still Exists
```

---

## Production Example

A PostgreSQL container stores its database files in a Docker Volume named:

```text
postgres-data
```

When the container is upgraded, the new container mounts the same volume and continues using the existing database.

---

## Production Tip

Regularly remove unused volumes to reclaim storage space, but verify that no important data is stored before deleting them.

---

## Interview Question

### Question

How do you create a Docker Volume?

### Answer

Use the `docker volume create <volume-name>` command.

---

# 6. Using Named Volumes

## Answer

Named Volumes are Docker-managed storage locations that provide persistent data storage.

---

## Mount a Named Volume

```bash
docker run -d \
-v mysql-data:/var/lib/mysql \
mysql:8
```

Explanation:

- `mysql-data` → Docker Volume
- `/var/lib/mysql` → Container directory

---

## Internal Workflow

```text
Container

↓

Named Volume

↓

Host Storage

↓

Persistent Data
```

---

## Production Example

A MySQL container stores all database files in a Named Volume.

If the container is recreated, customer data remains available.

---

## Production Tip

Use Named Volumes for databases, uploaded files, and other application data that must survive container replacement.

---

## Interview Question

### Question

Why are Named Volumes preferred in production?

### Answer

Named Volumes provide Docker-managed persistent storage that survives container removal and simplifies storage management.

---

# 7. Bind Mounts

## Answer

A Bind Mount maps a directory or file from the host machine directly into a container.

Unlike Named Volumes, Docker does not manage the host path.

---

## Example

```bash
docker run -d \
-v /home/user/project:/app \
node:22
```

---

## Internal Workflow

```text
Host Directory

↓

Bind Mount

↓

Container

↓

Application
```

---

## Production Example

During local development, a developer mounts the project directory into a container.

Any file changes on the host are immediately visible inside the running container without rebuilding the image.

---

## Comparison

| Named Volume | Bind Mount |
|--------------|------------|
| Docker-managed | Host-managed |
| Ideal for Production | Ideal for Development |
| Portable | Depends on host path |

---

## Production Tip

Avoid Bind Mounts for critical production data because they rely on specific host filesystem paths.

---

## Interview Question

### Question

What is a Bind Mount?

### Answer

A Bind Mount maps a host directory or file into a Docker container, allowing both the host and the container to access the same data.

---

# 8. Bridge Network

## Answer

The **Bridge Network** is Docker's default network for standalone containers.

Containers connected to the same Bridge Network can communicate with each other.

---

## Create a Bridge Network

```bash
docker network create app-network
```

---

## Run a Container on the Network

```bash
docker run -d \
--network app-network \
--name backend \
nginx
```

---

## Internal Workflow

```text
Container A

↓

Bridge Network

↓

Container B
```

---

## Production Example

A backend container communicates with a Redis container over the same Bridge Network using:

```text
redis
```

as the hostname instead of an IP address.

---

## Production Tip

Use user-defined Bridge Networks rather than the default bridge network to take advantage of built-in DNS resolution and better isolation.

---

## Interview Question

### Question

What is a Bridge Network in Docker?

### Answer

A Bridge Network is Docker's default network type that enables communication between containers running on the same Docker host.

---

# 9. Host Network & None Network

## Host Network

### Answer

The **Host Network** allows a container to use the host machine's network stack directly.

No network isolation exists between the host and the container.

---

## Example

```bash
docker run --network host nginx
```

---

## Advantages

- Better network performance
- No port mapping required

---

## Limitations

- Reduced isolation
- Potential port conflicts

---

## None Network

### Answer

The **None Network** disables all networking for a container.

The container has:

- No Internet
- No External Connectivity
- No Communication with Other Containers

---

## Example

```bash
docker run --network none alpine
```

---

## Internal Workflow

```text
Container

↓

None Network

↓

No Connectivity
```

---

## Production Example

A security-sensitive batch job is started with the `none` network to ensure it cannot communicate with any external systems.

---

## Production Tip

Use the Host Network only when direct host networking is required. Use the None Network for highly isolated workloads.

---

## Interview Question

### Question

What is the difference between the Bridge, Host, and None networks?

### Answer

- **Bridge** provides isolated container networking on a Docker host.
- **Host** shares the host's network stack with the container.
- **None** disables networking entirely.

---

## 📌 Quick Revision

| Feature | Purpose |
|----------|---------|
| `docker volume create` | Create a persistent volume |
| Named Volume | Docker-managed persistent storage |
| Bind Mount | Mount a host directory into a container |
| Bridge Network | Connect containers on the same host |
| Host Network | Share the host network stack |
| None Network | Disable container networking |

---

# 10. Overlay Network

## Answer

An **Overlay Network** allows containers running on **different Docker hosts** to communicate securely.

It is primarily used with:

- Docker Swarm
- Multi-host deployments
- Distributed applications

Unlike a Bridge Network, an Overlay Network spans multiple Docker hosts.

---

## Architecture

```text
Host 1

Container A

        │

======================

Overlay Network

======================

        │

Container B

Host 2
```

---

## Internal Workflow

```text
Container

↓

Overlay Network

↓

Remote Container

↓

Application Communication
```

---

## Production Example

A Docker Swarm cluster contains three worker nodes.

Containers running on different nodes communicate through an Overlay Network without requiring manual routing.

---

## Production Tip

Overlay Networks are designed for multi-host environments. For a single Docker host, a user-defined Bridge Network is usually sufficient.

---

## Interview Question

### Question

What is an Overlay Network?

### Answer

An Overlay Network connects containers running on multiple Docker hosts, enabling secure communication across distributed environments.

---

# 11. Port Mapping

## Answer

Containers run in isolated network namespaces.

Port Mapping exposes a container's internal port to the host machine so external users can access the application.

---

## Syntax

```bash
docker run -p HOST_PORT:CONTAINER_PORT IMAGE
```

---

## Example

```bash
docker run -d \
-p 8080:80 \
nginx
```

Explanation:

- Host Port → **8080**
- Container Port → **80**

Access:

```text
http://localhost:8080
```

---

## Internal Workflow

```text
Client

↓

Host Port 8080

↓

Docker Port Mapping

↓

Container Port 80

↓

Application
```

---

## Production Example

A web application listens on port **8080** inside the container.

The DevOps engineer maps it to port **80** on the host:

```bash
docker run -d \
-p 80:8080 \
shopping-api
```

Users access the application through the standard HTTP port.

---

## Production Tip

Expose only the ports that are required. Avoid publishing unnecessary ports to reduce the attack surface.

---

## Interview Question

### Question

Why is Port Mapping required?

### Answer

Port Mapping allows external clients to access services running inside Docker containers by mapping host ports to container ports.

---

# 12. Docker DNS Resolution

## Answer

Docker provides **built-in DNS resolution** for containers connected to the same **user-defined network**.

Instead of using IP addresses, containers can communicate using container names.

---

## Example

Create a network:

```bash
docker network create app-network
```

Run backend:

```bash
docker run -d \
--network app-network \
--name backend \
nginx
```

Run frontend:

```bash
docker run -d \
--network app-network \
--name frontend \
alpine
```

The frontend container can connect using:

```text
http://backend
```

instead of an IP address.

---

## Internal Workflow

```text
Frontend

↓

DNS Lookup

↓

Docker DNS

↓

Backend Container
```

---

## Production Example

A microservices application contains:

- frontend
- orders-api
- payments-api
- redis

Each service communicates using container names, eliminating the need to manage changing IP addresses.

---

## Production Tip

Always use user-defined Bridge Networks to benefit from Docker's automatic DNS resolution.

---

## Interview Question

### Question

How do Docker containers resolve each other's names?

### Answer

Containers on the same user-defined Docker network use Docker's built-in DNS service to resolve container names automatically.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Storing database data inside the container's writable layer.

Use Docker Volumes for persistent data.

---

## Mistake 2

❌ Using Bind Mounts for critical production workloads.

Prefer Docker-managed Named Volumes.

---

## Mistake 3

❌ Communicating between containers using IP addresses.

Use container names and Docker DNS instead.

---

## Mistake 4

❌ Publishing every container port.

Expose only the ports required by the application.

---

## Mistake 5

❌ Deleting Docker Volumes without verifying their contents.

Always ensure important data has been backed up before removing volumes.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Store persistent application data in Docker Volumes.

---

✅ Use Bind Mounts primarily for development environments.

---

✅ Create user-defined Bridge Networks for container communication.

---

✅ Use container names instead of IP addresses.

---

✅ Publish only required ports.

---

✅ Back up important Docker Volumes regularly.

---

✅ Monitor storage usage and network connectivity.

---

✅ Separate application containers from database containers.

---

## 📌 Quick Revision

### Storage Architecture

```text
Application

↓

Docker Container

↓

Named Volume

↓

Host Storage
```

---

### Networking Architecture

```text
Frontend

↓

Bridge Network

↓

Backend

↓

Database
```

---

### Network Types

```text
Bridge

↓

Host

↓

None

↓

Overlay
```

---

# Summary

After completing this guide, you should understand:

✅ Docker Volumes

✅ Persistent Storage

✅ Writable Layer

✅ Named Volumes

✅ Bind Mounts

✅ Docker Networks

✅ Bridge Network

✅ Host Network

✅ None Network

✅ Overlay Network

✅ Port Mapping

✅ Docker DNS Resolution

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is a Docker Volume? | A Docker-managed persistent storage location independent of the container lifecycle |
| Why are Docker Volumes needed? | To preserve data after containers are restarted or deleted |
| What is a Bind Mount? | A host directory or file mounted directly into a container |
| What is a Bridge Network? | The default Docker network for communication between containers on the same host |
| What is a Host Network? | A network mode where the container shares the host's network stack |
| What is a None Network? | A network mode that disables all networking for the container |
| What is an Overlay Network? | A network that connects containers across multiple Docker hosts |
| Why is Port Mapping required? | To expose container services to external clients |
| How does Docker DNS work? | Docker resolves container names automatically on user-defined networks |
| Why should container names be used instead of IP addresses? | Container names remain consistent, while IP addresses may change |

---

# 🎉 Docker Volumes & Networking Module Completed

Congratulations! You have completed **Docker Volumes & Networking**, including:

- ✅ Docker Volumes
- ✅ Persistent Storage
- ✅ Writable Layer
- ✅ Named Volumes
- ✅ Bind Mounts
- ✅ Bridge Network
- ✅ Host Network
- ✅ None Network
- ✅ Overlay Network
- ✅ Port Mapping
- ✅ Docker DNS Resolution
- ✅ Production Best Practices

You now understand how to manage persistent data and enable secure communication between containers in production environments.

---

# 🚀 What's Next?

➡️ **06-docker-compose.md**

In the next guide, you'll learn:

- What is Docker Compose?
- Why Docker Compose?
- `docker-compose.yml` Structure
- Managing Multi-Container Applications
- Services
- Networks
- Volumes
- Environment Variables
- Scaling Services
- Production Best Practices
- Common Interview Questions