# 🚀 Kubernetes Architecture - Understanding How Kubernetes Works Internally

This guide explains the complete Kubernetes Architecture from beginner to production level.

If you've ever wondered:

- What is Kubernetes Architecture?
- What are the components of a Kubernetes Cluster?
- What is the Control Plane?
- What are Worker Nodes?
- How do Kubernetes components communicate?

This guide is for you.

---

# 📑 Table of Contents

1. What is Kubernetes Architecture?
2. Kubernetes Cluster Overview
3. Control Plane vs Worker Node
4. Kubernetes Components Overview
5. Common Interview Questions

---

# 1. What is Kubernetes Architecture?

## Answer

Kubernetes Architecture is the overall design of a Kubernetes cluster and describes how different components work together to deploy, manage, scale, and maintain containerized applications.

A Kubernetes cluster consists of two major parts:

- Control Plane
- Worker Nodes

The Control Plane manages the cluster.

The Worker Nodes run the application workloads.

Together, they ensure applications remain available and healthy.

---

## Why do we need Kubernetes Architecture?

Imagine you deploy an application.

Someone needs to:

- Accept deployment requests
- Decide where applications should run
- Monitor application health
- Restart failed applications
- Maintain the cluster state

Instead of assigning these responsibilities to a single component, Kubernetes distributes them across multiple specialized components.

This makes the platform scalable, reliable, and fault tolerant.

---

## High-Level Architecture

```text
                Kubernetes Cluster

        ┌──────────────────────────┐
        │      Control Plane       │
        └──────────────────────────┘
                    │
                    │
        ────────────┼────────────
                    │
      ┌─────────────┴─────────────┐
      │                           │
┌──────────────┐          ┌──────────────┐
│ Worker Node  │          │ Worker Node  │
└──────────────┘          └──────────────┘
      │                           │
      ▼                           ▼
    Pods                        Pods
```

---

## Real World Example

Think of Kubernetes like a logistics company.

The **Control Plane** acts as the headquarters.

It:

- Receives customer requests
- Plans deliveries
- Assigns work
- Tracks progress

The **Worker Nodes** are the delivery vehicles.

They execute the assigned work and report back to headquarters.

---

## Benefits

Kubernetes Architecture provides:

- High Availability
- Fault Tolerance
- Scalability
- Automation
- Centralized Management

---

## Production Tip

Production Kubernetes clusters typically run multiple Control Plane nodes to eliminate a single point of failure.

---

## Interview Question

### Question

What is Kubernetes Architecture?

### Answer

Kubernetes Architecture defines how the Control Plane and Worker Nodes work together to manage, schedule, and run containerized applications inside a Kubernetes cluster.

---

# 2. Kubernetes Cluster Overview

## Answer

A Kubernetes Cluster is a group of machines that work together to run containerized applications.

Every cluster contains:

- One or more Control Plane Nodes
- One or more Worker Nodes

Applications run only on Worker Nodes.

The Control Plane manages the entire cluster.

---

## Cluster Structure

```text
                Kubernetes Cluster

        ┌──────────────────────────┐
        │      Control Plane       │
        └──────────────────────────┘

              │           │

──────────────┼──────────────

              │           │

      ┌────────────┐  ┌────────────┐

      │ Worker 1   │  │ Worker 2   │

      └────────────┘  └────────────┘

           │                 │

         Pods              Pods
```

---

## Responsibilities

### Control Plane

Responsible for:

- Cluster Management
- Scheduling
- API Requests
- Monitoring Desired State

---

### Worker Nodes

Responsible for:

- Running Pods
- Running Containers
- Reporting Health
- Executing Workloads

---

## Production Example

Suppose an online banking application consists of:

- Frontend
- Backend API
- Authentication Service
- Payment Service

The Control Plane determines where each Pod should run.

Worker Nodes execute those Pods.

---

## Production Tip

A production cluster usually contains multiple Worker Nodes to improve scalability and availability.

---

## Interview Question

### Question

What is a Kubernetes Cluster?

### Answer

A Kubernetes Cluster is a collection of Control Plane and Worker Nodes that work together to deploy and manage containerized applications.

---

# 3. Control Plane vs Worker Node

## Answer

The Kubernetes cluster is divided into two major sections.

---

## Control Plane

The brain of Kubernetes.

Responsibilities include:

- Accept API Requests
- Store Cluster State
- Schedule Pods
- Monitor Resources
- Maintain Desired State

---

## Worker Node

The execution environment.

Responsibilities include:

- Run Pods
- Execute Containers
- Report Status
- Serve Applications

---

## Comparison

| Control Plane | Worker Node |
|---------------|-------------|
| Manages Cluster | Runs Applications |
| Schedules Pods | Hosts Pods |
| Stores Cluster State | Executes Containers |
| Makes Decisions | Performs Work |
| Controls the Cluster | Reports Health |

---

## Internal Workflow

```text
User

        │

kubectl apply

        │

        ▼

Control Plane

        │

Schedules Pod

        │

        ▼

Worker Node

        │

Creates Pod

        │

Application Running
```

---

## Production Tip

Applications should never be scheduled directly on the Control Plane in production environments.

Dedicated Worker Nodes should handle application workloads.

---

## Interview Question

### Question

What is the difference between the Control Plane and Worker Nodes?

### Answer

The Control Plane manages the Kubernetes cluster and makes scheduling decisions, while Worker Nodes execute application workloads by running Pods and containers.

---

# 4. Kubernetes Components Overview

## Answer

The Kubernetes Architecture consists of several components that work together.

---

## Control Plane Components

- API Server
- Scheduler
- Controller Manager
- etcd

---

## Worker Node Components

- Kubelet
- Kube Proxy
- Container Runtime

---

## High-Level Component Diagram

```text
Control Plane

│

├── API Server

├── Scheduler

├── Controller Manager

└── etcd

        │

────────┼────────

        │

Worker Node

│

├── Kubelet

├── Kube Proxy

└── Container Runtime

        │

        ▼

      Pods
```

Each component has a specific responsibility.

Together, they ensure the cluster operates correctly.

---

## Production Tip

Understanding the role of each component is essential for troubleshooting Kubernetes clusters and performing well in interviews.

---

## Interview Question

### Question

What are the main components of Kubernetes Architecture?

### Answer

The main components are the API Server, Scheduler, Controller Manager, etcd, Kubelet, Kube Proxy, Container Runtime, and Worker Nodes.

---

# 5. What is the Kubernetes API Server?

## Answer

The **API Server** is the front door of the Kubernetes Control Plane.

Every request to the Kubernetes cluster goes through the API Server.

Whether you're:

- Creating a Deployment
- Deleting a Pod
- Scaling an Application
- Viewing Cluster Information

the request is always received by the API Server first.

Without the API Server, no component can communicate with the Kubernetes cluster.

---

## Why do we need the API Server?

Imagine a company where every employee directly accesses the database.

It would create:

- Security Issues
- Data Corruption
- No Validation
- No Authentication

Instead, everyone communicates through a central system.

Similarly, every Kubernetes component communicates through the API Server.

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

Authentication

Authorization

Validation

        │

        ▼

Store in etcd

        │

Notify Other Components
```

---

## Production Example

A DevOps engineer executes:

```bash
kubectl apply -f deployment.yaml
```

The API Server:

1. Receives the request.
2. Authenticates the user.
3. Authorizes the action.
4. Validates the YAML.
5. Stores the desired state in etcd.
6. Notifies the Scheduler and Controller Manager.

---

## Production Tip

Every Kubernetes component communicates through the API Server.

No component talks directly to etcd.

---

## Interview Question

### Question

What is the Kubernetes API Server?

### Answer

The API Server is the central communication component of Kubernetes. It receives, validates, authenticates, authorizes, and processes all requests to the Kubernetes cluster.

---

# 6. What is etcd?

## Answer

**etcd** is the distributed key-value database used by Kubernetes.

It stores the complete state of the cluster.

Examples include:

- Pods
- Nodes
- Deployments
- Services
- Secrets
- ConfigMaps
- Namespaces

Everything about the cluster is stored in etcd.

---

## Why do we need etcd?

Suppose the Control Plane restarts.

How does Kubernetes remember:

- Running Pods?
- Existing Nodes?
- Deployments?
- Services?

The answer is etcd.

---

## Internal Workflow

```text
API Server

        │

        ▼

Store Cluster State

        │

        ▼

etcd

        │

        ▼

Persistent Data
```

---

## Production Example

Suppose you create:

```bash
kubectl create deployment nginx
```

The API Server stores the Deployment information inside etcd.

Even if the API Server restarts, the Deployment information remains available.

---

## Production Tip

Always back up etcd regularly.

If etcd is lost and no backup exists, recovering the cluster becomes extremely difficult.

---

## Interview Question

### Question

What is etcd?

### Answer

etcd is Kubernetes' distributed key-value database that stores the complete desired and current state of the cluster.

---

# 7. What is the Kubernetes Scheduler?

## Answer

The Scheduler decides **where new Pods should run**.

When a Pod is created, it has not yet been assigned to any Worker Node.

The Scheduler evaluates all available Worker Nodes and selects the most suitable one.

---

## How does the Scheduler decide?

The Scheduler considers:

- Available CPU
- Available Memory
- Node Health
- Labels
- Taints & Tolerations
- Affinity Rules
- Resource Requests

---

## Internal Workflow

```text
New Pod

        │

No Worker Node

        │

        ▼

Scheduler

        │

Checks Cluster

        │

────────┼────────

        │

Worker 1

Worker 2

Worker 3

        │

        ▼

Best Node Selected
```

---

## Production Example

Suppose:

Worker 1

```text
CPU: 90%
Memory: 95%
```

Worker 2

```text
CPU: 20%
Memory: 30%
```

The Scheduler selects Worker 2 because it has more available resources.

---

## Production Tip

The Scheduler does **not** run containers.

It only selects the most appropriate Worker Node.

---

## Interview Question

### Question

What is the responsibility of the Kubernetes Scheduler?

### Answer

The Scheduler selects the most suitable Worker Node for newly created Pods based on available resources and scheduling rules.

---

# 8. What is the Controller Manager?

## Answer

The Controller Manager continuously monitors the cluster and ensures the actual state matches the desired state.

If something changes unexpectedly, the Controller Manager works to restore the desired state.

This is one of Kubernetes' self-healing capabilities.

---

## Example

Desired State:

```text
3 Pods
```

Current State:

```text
2 Pods
```

The Controller Manager detects the difference.

It automatically creates a new Pod.

---

## Internal Workflow

```text
Desired State

3 Pods

        │

        ▼

Controller Manager

        │

Compares

        │

Current State

2 Pods

        │

        ▼

Creates Missing Pod

        │

Desired State Restored
```

---

## Common Controllers

- Deployment Controller
- ReplicaSet Controller
- Node Controller
- Job Controller
- Endpoint Controller

Each controller is responsible for a specific type of Kubernetes resource.

---

## Production Example

A Pod crashes unexpectedly.

The Controller Manager detects the failure and creates a replacement Pod automatically.

Users often don't notice the failure because Kubernetes restores the desired state.

---

## Production Tip

The Controller Manager is responsible for maintaining the desired state—not for scheduling Pods.

Scheduling is handled by the Scheduler.

---

## Interview Question

### Question

What is the role of the Controller Manager?

### Answer

The Controller Manager continuously monitors the Kubernetes cluster and ensures that the actual state matches the desired state by creating, updating, or replacing resources when necessary.

---

# 9. What is Kubelet?

## Answer

The **Kubelet** is an agent that runs on every Worker Node.

Its primary responsibility is to communicate with the Kubernetes Control Plane and ensure that Pods are running correctly on its node.

Think of the Kubelet as the **manager of a Worker Node**.

---

## Responsibilities

The Kubelet:

- Registers the Worker Node with the cluster
- Receives Pod instructions from the API Server
- Starts Pods using the Container Runtime
- Monitors Pod health
- Reports Node and Pod status back to the Control Plane

---

## Internal Workflow

```text
API Server

        │

Assign Pod

        │

        ▼

Kubelet

        │

        ▼

Container Runtime

        │

        ▼

Pod Running
```

---

## Production Example

Suppose the Scheduler assigns a Pod to **Worker Node 2**.

The Kubelet on Worker Node 2:

1. Receives the Pod specification.
2. Pulls the container image (if needed).
3. Starts the container.
4. Monitors the Pod.
5. Reports the Pod status to the API Server.

---

## Production Tip

If the Kubelet stops running, Kubernetes cannot manage Pods on that Worker Node.

---

## Interview Question

### Question

What is Kubelet?

### Answer

Kubelet is the Kubernetes agent running on every Worker Node that ensures Pods are created, monitored, and kept in their desired state.

---

# 10. What is Kube Proxy?

## Answer

**Kube Proxy** is the networking component that runs on every Worker Node.

It manages network rules and enables communication between Pods and Services.

Without Kube Proxy, applications running in different Pods would not communicate reliably.

---

## Responsibilities

Kube Proxy:

- Routes network traffic
- Implements Service networking
- Performs Load Balancing
- Maintains network rules

---

## Internal Workflow

```text
User Request

        │

        ▼

Kubernetes Service

        │

        ▼

Kube Proxy

        │

────────┼────────

        │

Pod 1

Pod 2

Pod 3
```

Traffic is distributed across healthy Pods.

---

## Production Example

A Service exposes three application Pods.

Kube Proxy automatically balances incoming requests among them.

If one Pod fails, traffic is routed only to the remaining healthy Pods.

---

## Production Tip

Kube Proxy does not create Pods.

Its responsibility is networking and traffic routing.

---

## Interview Question

### Question

What is Kube Proxy?

### Answer

Kube Proxy is the Kubernetes networking component that manages Service networking and routes traffic to the appropriate Pods.

---

# 11. What is the Container Runtime?

## Answer

The **Container Runtime** is responsible for running containers on Worker Nodes.

When the Kubelet receives instructions to start a Pod, it communicates with the Container Runtime.

The Container Runtime then:

- Pulls container images
- Creates containers
- Starts containers
- Stops containers
- Removes containers

---

## Common Container Runtimes

- containerd
- CRI-O

*(Earlier Kubernetes versions also supported Docker through Dockershim, but modern Kubernetes primarily uses CRI-compatible runtimes such as containerd and CRI-O.)*

---

## Internal Workflow

```text
Kubelet

        │

        ▼

Container Runtime

        │

Pull Image

        │

Create Container

        │

Start Container

        │

Pod Running
```

---

## Production Tip

The Container Runtime only runs containers.

It does not make scheduling or scaling decisions.

---

## Interview Question

### Question

What is the Container Runtime?

### Answer

The Container Runtime is the software responsible for pulling container images and creating, running, and stopping containers on Worker Nodes.

---

# 12. What really happens when you run `kubectl apply -f deployment.yaml`?

## Answer

This is one of the most frequently asked Kubernetes interview questions.

The complete workflow is:

```text
Developer

        │

kubectl apply -f deployment.yaml

        │

        ▼

API Server

(Authentication & Validation)

        │

        ▼

etcd

(Store Desired State)

        │

        ▼

Controller Manager

(Create ReplicaSet)

        │

        ▼

Scheduler

(Choose Worker Node)

        │

        ▼

Kubelet

(Create Pod)

        │

        ▼

Container Runtime

(Pull Image & Start Container)

        │

        ▼

Pod Running
```

---

## Step-by-Step Flow

1. The developer runs `kubectl apply`.
2. The request reaches the API Server.
3. The API Server authenticates and validates the request.
4. The desired state is stored in etcd.
5. The Controller Manager creates the required resources.
6. The Scheduler selects the best Worker Node.
7. The Kubelet receives the Pod assignment.
8. The Container Runtime pulls the image and starts the container.
9. The Pod becomes available.

---

## Production Tip

Understanding this workflow helps troubleshoot deployment issues quickly.

For example:

- Authentication issue → API Server
- Scheduling issue → Scheduler
- Pod creation issue → Kubelet
- Image pull issue → Container Runtime

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Thinking the Scheduler starts containers.

The Scheduler only selects the Worker Node.

---

## Mistake 2

❌ Thinking the API Server stores cluster data.

The API Server communicates with **etcd**, which stores the data.

---

## Mistake 3

❌ Assuming the Kubelet schedules Pods.

Scheduling is handled by the Scheduler.

---

## Mistake 4

❌ Confusing Kube Proxy with Ingress.

Kube Proxy manages internal Service networking.

Ingress manages external HTTP/HTTPS traffic.

---

## Mistake 5

❌ Assuming the Control Plane runs application Pods.

Application Pods should run on Worker Nodes.

---

# 14. Production Best Practices

Always follow these recommendations.

---

✅ Deploy multiple Control Plane nodes for High Availability.

---

✅ Monitor the health of Kubelet on every Worker Node.

---

✅ Back up etcd regularly.

---

✅ Secure API Server access using authentication and authorization.

---

✅ Monitor cluster networking.

---

✅ Use dedicated Worker Nodes for production workloads.

---

## 📌 Quick Revision

### Control Plane

- API Server
- Scheduler
- Controller Manager
- etcd

---

### Worker Node

- Kubelet
- Kube Proxy
- Container Runtime

---

### Kubernetes Request Flow

```text
kubectl apply

↓

API Server

↓

etcd

↓

Controller Manager

↓

Scheduler

↓

Kubelet

↓

Container Runtime

↓

Pod Running
```

---

# Summary

After completing this guide, you should understand:

✅ Kubernetes Architecture

✅ Control Plane

✅ Worker Nodes

✅ API Server

✅ etcd

✅ Scheduler

✅ Controller Manager

✅ Kubelet

✅ Kube Proxy

✅ Container Runtime

✅ Complete Deployment Workflow

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| Front door of Kubernetes? | API Server |
| Cluster database? | etcd |
| Chooses Worker Node? | Scheduler |
| Maintains desired state? | Controller Manager |
| Runs on every Worker Node? | Kubelet |
| Manages networking? | Kube Proxy |
| Runs containers? | Container Runtime |
| Smallest deployable unit? | Pod |
| Stores cluster state? | etcd |
| Starts with `kubectl apply`? | API Server |

---

# What's Next?

➡️ **03-pods.md**

In the next guide, you'll learn:

- What is a Pod?
- Why do we need Pods?
- Pod Lifecycle
- Multi-Container Pods
- Init Containers
- Static Pods
- Pod Networking
- Production Best Practices
- Common Interview Questions