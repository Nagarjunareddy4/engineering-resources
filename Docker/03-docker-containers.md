# 🐳 Docker Containers

This guide explains everything you need to know about Docker Containers.

If you've ever wondered:

- What is a Docker Container?
- How is a Container created?
- What is the Container Lifecycle?
- How do Containers work?
- Why are Containers lightweight?

This guide is for you.

---

# 📑 Table of Contents

1. What is a Docker Container?
2. How Docker Containers Work
3. Docker Container Lifecycle
4. Container States
5. Common Interview Questions

---

# 1. What is a Docker Container?

## Answer

A **Docker Container** is a lightweight, isolated, and executable instance of a Docker Image.

When you run a Docker Image, Docker creates a Container by adding a **writable layer** on top of the image.

A container includes:

- Application
- Runtime
- Libraries
- Dependencies
- Configuration
- Writable Layer

Unlike a Docker Image, a Container is **running and interactive**.

---

## Architecture

```text
Docker Image

↓

Create Container

↓

Docker Container

↓

Running Application
```

---

## Key Characteristics

Docker Containers are:

- Lightweight
- Portable
- Fast to Start
- Isolated
- Temporary by Default
- Easy to Scale

---

## Real World Example

A company has an image:

```text
shopping-api:v2.0
```

From this single image, Docker creates:

```text
Container 1

Container 2

Container 3

Container 4
```

Each container runs independently while sharing the same image.

---

## Benefits

Docker Containers provide:

- Fast Startup
- Efficient Resource Usage
- Application Isolation
- Consistent Runtime
- Easy Scaling

---

## Production Tip

Treat containers as **ephemeral**. If a container fails, replace it with a new one instead of repairing it manually.

---

## Interview Question

### Question

What is a Docker Container?

### Answer

A Docker Container is a lightweight, isolated, running instance of a Docker Image that contains the application and its runtime environment.

---

# 2. How Docker Containers Work

## Answer

Docker creates a container from an image by adding a **thin writable layer** on top of the image's read-only layers.

When an application writes data:

- Image layers remain unchanged.
- Changes are stored only in the writable container layer.

---

## Internal Workflow

```text
Docker Image

↓

Read-Only Layers

↓

Writable Layer

↓

Running Container
```

---

## Container Architecture

```text
Application

↓

Writable Layer

↓

Read-Only Image Layers

↓

Docker Engine

↓

Host Operating System

↓

Physical / Virtual Machine
```

---

## Production Example

A developer runs:

```bash
docker run nginx:1.27
```

Docker:

1. Checks if the image exists locally.
2. Pulls the image if necessary.
3. Creates a writable layer.
4. Starts the container.
5. Runs the NGINX process.

---

## Production Tip

Any changes made inside a container are lost when the container is removed unless persistent storage (Volumes or Bind Mounts) is used.

---

## Interview Question

### Question

How does a Docker Container work?

### Answer

Docker creates a writable layer on top of a read-only image and runs the application inside an isolated container managed by the Docker Engine.

---

# 3. Docker Container Lifecycle

## Answer

Every Docker Container follows a lifecycle from creation to removal.

Understanding this lifecycle is essential for managing containers effectively.

---

## Lifecycle

```text
Image

↓

Create

↓

Start

↓

Running

↓

Stop

↓

Restart

↓

Remove
```

---

## Lifecycle Commands

### Create a Container

```bash
docker create nginx
```

Creates a container but does not start it.

---

### Create and Start a Container

```bash
docker run nginx
```

---

### Stop a Container

```bash
docker stop <container-id>
```

---

### Restart a Container

```bash
docker restart <container-id>
```

---

### Remove a Container

```bash
docker rm <container-id>
```

---

## Internal Workflow

```text
Docker Image

↓

Container Created

↓

Container Started

↓

Running

↓

Stopped

↓

Removed
```

---

## Production Example

A CI/CD pipeline deploys a new application version by:

1. Creating a new container.
2. Starting the new container.
3. Verifying health checks.
4. Stopping the old container.
5. Removing the old container.

---

## Production Tip

Automate the container lifecycle using orchestration platforms like Kubernetes instead of manually managing containers.

---

## Interview Question

### Question

What is the Docker Container Lifecycle?

### Answer

The Docker Container Lifecycle includes creating, starting, running, stopping, restarting, and removing containers.

---

# 4. Container States

## Answer

A Docker Container moves through different states during its lifecycle.

---

## Common States

| State | Description |
|--------|-------------|
| Created | Container has been created but not started |
| Running | Application is actively running |
| Paused | Processes are temporarily suspended |
| Exited | Application has stopped |
| Restarting | Container is restarting |
| Dead | Container failed and cannot be recovered automatically |
| Removed | Container has been deleted |

---

## State Transition

```text
Created

↓

Running

↓

Paused

↓

Running

↓

Exited

↓

Removed
```

---

## View Running Containers

```bash
docker ps
```

---

## View All Containers

```bash
docker ps -a
```

---

## Production Example

A web server container enters the **Exited** state because the main application process crashes.

Monitoring tools detect the failure and Kubernetes starts a replacement container.

---

## Production Tip

Monitor containers continuously and investigate unexpected **Exited** or **Dead** states promptly.

---

## Interview Question

### Question

What are the common Docker Container states?

### Answer

Common states include Created, Running, Paused, Exited, Restarting, Dead, and Removed.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Docker Container | Running instance of an image |
| Writable Layer | Stores runtime changes |
| Container Lifecycle | Create → Start → Run → Stop → Remove |
| Running | Application is active |
| Exited | Application has stopped |
| Removed | Container deleted |

---

# 5. Running Docker Containers

## Answer

The `docker run` command is used to create and start a container from a Docker Image.

If the image is not available locally, Docker automatically downloads it from the configured registry.

---

## Basic Syntax

```bash
docker run <image-name>
```

Example:

```bash
docker run nginx
```

---

## Run in Detached Mode

Detached mode runs the container in the background.

```bash
docker run -d nginx
```

---

## Run with Port Mapping

```bash
docker run -d -p 8080:80 nginx
```

Explanation:

- `-d` → Run in background
- `-p 8080:80` → Map host port 8080 to container port 80

Access the application:

```text
http://localhost:8080
```

---

## Run with a Custom Name

```bash
docker run -d --name web-server nginx
```

---

## Internal Workflow

```text
Docker Image

↓

docker run

↓

Create Container

↓

Start Container

↓

Running Application
```

---

## Production Example

A DevOps engineer deploys an NGINX container as a reverse proxy using:

```bash
docker run -d \
-p 80:80 \
--name nginx-proxy \
nginx:1.27
```

---

## Production Tip

Always assign meaningful container names instead of relying on randomly generated names.

---

## Interview Question

### Question

What does the `docker run` command do?

### Answer

The `docker run` command creates a new container from an image and starts it.

---

# 6. Starting, Stopping & Restarting Containers

## Answer

Docker allows containers to be started, stopped, and restarted without recreating them.

---

## Start a Stopped Container

```bash
docker start web-server
```

---

## Stop a Running Container

```bash
docker stop web-server
```

Docker sends a graceful termination signal before stopping the container.

---

## Force Stop a Container

```bash
docker kill web-server
```

This immediately terminates the container.

---

## Restart a Container

```bash
docker restart web-server
```

---

## Internal Workflow

```text
Created

↓

Running

↓

Stopped

↓

Started

↓

Restarted
```

---

## Production Example

After updating a configuration file mounted into a container, a DevOps engineer restarts the container to apply the changes.

---

## Production Tip

Use `docker stop` whenever possible. Reserve `docker kill` for situations where a container does not respond to a graceful shutdown.

---

## Interview Question

### Question

What is the difference between `docker stop` and `docker kill`?

### Answer

`docker stop` attempts a graceful shutdown, while `docker kill` immediately terminates the container.

---

# 7. Executing Commands Inside Containers

## Answer

The `docker exec` command allows you to execute commands inside a running container.

This is commonly used for troubleshooting and debugging.

---

## Execute a Command

```bash
docker exec web-server ls
```

---

## Open an Interactive Shell

For Linux containers:

```bash
docker exec -it web-server bash
```

If Bash is unavailable:

```bash
docker exec -it web-server sh
```

---

## Internal Workflow

```text
Running Container

↓

docker exec

↓

Execute Command

↓

Display Output
```

---

## Production Example

A DevOps engineer connects to a running application container to verify log files or configuration settings.

---

## Production Tip

Avoid making permanent changes inside running containers. Update the Docker Image and redeploy instead.

---

## Interview Question

### Question

What is the purpose of `docker exec`?

### Answer

The `docker exec` command runs commands inside an already running container.

---

# 8. Viewing Container Logs

## Answer

The `docker logs` command displays the output generated by the application's standard output (stdout) and standard error (stderr).

Logs are essential for monitoring and troubleshooting applications.

---

## View Logs

```bash
docker logs web-server
```

---

## Follow Logs in Real Time

```bash
docker logs -f web-server
```

---

## View the Last 100 Lines

```bash
docker logs --tail 100 web-server
```

---

## Internal Workflow

```text
Application

↓

stdout / stderr

↓

Docker Logs

↓

Developer
```

---

## Production Example

An application fails during startup.

The engineer runs:

```bash
docker logs web-server
```

to identify the root cause.

---

## Production Tip

Configure centralized log collection using tools such as ELK, Grafana Loki, or Azure Monitor instead of relying only on local Docker logs.

---

## Interview Question

### Question

How do you view logs for a Docker Container?

### Answer

Use the `docker logs <container-name>` command.

---

# 9. Inspecting Containers

## Answer

The `docker inspect` command provides detailed information about a container.

It returns configuration details in JSON format.

---

## Inspect a Container

```bash
docker inspect web-server
```

---

## Information Available

- Container ID
- Image
- IP Address
- Network Configuration
- Mounted Volumes
- Environment Variables
- Port Mappings
- Restart Policy

---

## Internal Workflow

```text
Container

↓

docker inspect

↓

Configuration Details

↓

JSON Output
```

---

## Production Example

A DevOps engineer verifies the IP address and mounted volumes of a container while troubleshooting connectivity issues.

---

## Production Tip

Use `docker inspect` to verify runtime configuration instead of making assumptions about how a container was started.

---

## Interview Question

### Question

What is the purpose of the `docker inspect` command?

### Answer

The `docker inspect` command displays detailed configuration and runtime information about Docker objects such as containers, images, networks, and volumes.

---

## 📌 Quick Revision

| Command | Purpose |
|----------|---------|
| `docker run` | Create and start a container |
| `docker start` | Start a stopped container |
| `docker stop` | Gracefully stop a container |
| `docker kill` | Forcefully terminate a container |
| `docker restart` | Restart a container |
| `docker exec` | Execute commands inside a running container |
| `docker logs` | View container logs |
| `docker inspect` | Display detailed container information |

---

# 10. Removing Docker Containers

## Answer

Containers that are no longer needed should be removed to free up system resources.

Removing a container deletes its writable layer, but the Docker Image remains available unless explicitly removed.

---

## Remove a Stopped Container

```bash
docker rm web-server
```

---

## Force Remove a Running Container

```bash
docker rm -f web-server
```

---

## Remove Multiple Containers

```bash
docker rm container1 container2 container3
```

---

## Remove All Stopped Containers

```bash
docker container prune
```

Example Output

```text
Deleted Containers:

abc123

def456

ghi789
```

---

## Internal Workflow

```text
Running Container

↓

Stop

↓

Remove

↓

Writable Layer Deleted

↓

Image Remains
```

---

## Production Example

After deploying a new application version, the old containers are removed automatically to free disk space and avoid resource waste.

---

## Production Tip

Regularly clean up unused containers on development machines and CI/CD agents to maintain healthy Docker hosts.

---

## Interview Question

### Question

How do you remove a Docker Container?

### Answer

Use `docker rm <container-name>` to remove a stopped container or `docker rm -f <container-name>` to forcefully remove a running container.

---

# 11. Restart Policies

## Answer

Restart policies define how Docker should behave when a container stops unexpectedly.

These policies improve application availability.

---

## Available Restart Policies

| Policy | Description |
|---------|-------------|
| `no` | Never restart the container (default) |
| `on-failure` | Restart only if the container exits with an error |
| `always` | Always restart the container |
| `unless-stopped` | Restart automatically unless manually stopped |

---

## Example

```bash
docker run -d \
--restart unless-stopped \
nginx
```

---

## Internal Workflow

```text
Container Stops

↓

Restart Policy

↓

Restart?

↓

Container Running
```

---

## Production Example

A web server is started with:

```bash
docker run -d \
--restart always \
nginx
```

If the Docker host reboots, the container starts automatically.

---

## Production Tip

Use `unless-stopped` or `always` for long-running production services.

---

## Interview Question

### Question

Why are restart policies important?

### Answer

Restart policies automatically recover containers after failures or host reboots, improving application availability.

---

# 12. Container Resource Limits

## Answer

Docker allows CPU and memory limits to prevent a single container from consuming excessive host resources.

This improves stability on shared systems.

---

## Limit Memory

```bash
docker run -d \
--memory="512m" \
nginx
```

---

## Limit CPU

```bash
docker run -d \
--cpus="1.5" \
nginx
```

---

## Limit Both CPU and Memory

```bash
docker run -d \
--memory="1g" \
--cpus="2" \
nginx
```

---

## Internal Workflow

```text
Container

↓

Resource Limits

↓

CPU

Memory

↓

Controlled Resource Usage
```

---

## Production Example

A Java application is limited to:

- 2 CPUs
- 2 GB Memory

to prevent it from affecting other workloads on the same host.

---

## Production Tip

Always define resource limits for production containers to prevent resource exhaustion.

---

## Interview Question

### Question

Why should CPU and memory limits be configured?

### Answer

Resource limits prevent containers from consuming excessive system resources and improve workload stability.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Running containers without meaningful names.

Instead use:

```bash
docker run --name shopping-api
```

---

## Mistake 2

❌ Making configuration changes directly inside running containers.

Rebuild the image and recreate the container instead.

---

## Mistake 3

❌ Forgetting restart policies.

Critical applications should restart automatically after failures.

---

## Mistake 4

❌ Running production containers without resource limits.

Always define CPU and memory limits.

---

## Mistake 5

❌ Storing important data inside containers.

Use Docker Volumes or Bind Mounts for persistent storage.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Use meaningful container names.

---

✅ Configure restart policies.

---

✅ Set CPU and memory limits.

---

✅ Store persistent data in volumes.

---

✅ Monitor container logs and health.

---

✅ Remove unused containers regularly.

---

✅ Treat containers as immutable.

---

## 📌 Quick Revision

### Container Lifecycle

```text
Image

↓

Create

↓

Start

↓

Running

↓

Stop

↓

Restart

↓

Remove
```

---

### Common Commands

```bash
docker run

docker start

docker stop

docker restart

docker exec

docker logs

docker inspect

docker rm

docker container prune
```

---

### Production Workflow

```text
Build Image

↓

Run Container

↓

Monitor

↓

Restart if Needed

↓

Scale

↓

Replace

↓

Remove Old Container
```

---

# Summary

After completing this guide, you should understand:

✅ Docker Containers

✅ Container Architecture

✅ Container Lifecycle

✅ Container States

✅ Running Containers

✅ Starting, Stopping & Restarting

✅ Executing Commands

✅ Viewing Logs

✅ Inspecting Containers

✅ Removing Containers

✅ Restart Policies

✅ Resource Limits

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is a Docker Container? | A running instance of a Docker Image |
| How does a container work? | Docker adds a writable layer to a read-only image and runs the application |
| What is the container lifecycle? | Create → Start → Run → Stop → Restart → Remove |
| How do you execute commands inside a running container? | `docker exec -it <container-name> bash` |
| How do you view container logs? | `docker logs <container-name>` |
| What is the purpose of `docker inspect`? | To display detailed runtime configuration and metadata |
| How do you remove a container? | `docker rm <container-name>` |
| Why are restart policies important? | They automatically recover containers after failures |
| Why configure resource limits? | To prevent containers from exhausting host resources |
| Why should containers be immutable? | Updates should be made by rebuilding images and replacing containers rather than modifying running instances |

---

# 🎉 Docker Containers Module Completed

Congratulations! You have completed **Docker Containers**, including:

- ✅ Docker Containers
- ✅ Container Architecture
- ✅ Container Lifecycle
- ✅ Container States
- ✅ Running Containers
- ✅ Starting, Stopping & Restarting
- ✅ Executing Commands
- ✅ Viewing Logs
- ✅ Inspecting Containers
- ✅ Removing Containers
- ✅ Restart Policies
- ✅ Resource Limits
- ✅ Production Best Practices

You now have a strong understanding of how Docker containers are created, managed, monitored, and optimized in production.

---

# 🚀 What's Next?

➡️ **04-dockerfile.md**

In the next guide, you'll learn:

- What is a Dockerfile?
- Dockerfile Syntax
- FROM
- RUN
- COPY
- ADD
- WORKDIR
- CMD
- ENTRYPOINT
- ENV
- ARG
- EXPOSE
- USER
- Multi-stage Builds
- Production Best Practices
- Common Interview Questions