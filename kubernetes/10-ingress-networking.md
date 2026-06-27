# 🚀 Kubernetes Ingress & Networking - Exposing Applications to the Outside World

This guide explains everything you need to know about Kubernetes Ingress and networking.

If you've ever wondered:

- What is an Ingress?
- Why do we need Ingress?
- How is Ingress different from a Service?
- What is an Ingress Controller?
- How does external traffic reach a Kubernetes application?

This guide is for you.

---

# 📑 Table of Contents

1. What is Ingress?
2. Why do we need Ingress?
3. How Ingress Works
4. Ingress Architecture
5. Common Interview Questions

---

# 1. What is an Ingress?

## Answer

An **Ingress** is a Kubernetes resource that manages **external HTTP and HTTPS traffic** entering the cluster.

Instead of exposing every application using a separate LoadBalancer Service, an Ingress provides a **single entry point** for multiple applications.

It routes incoming requests based on:

- Host Name
- URL Path
- HTTP Rules

---

## Why do we need Ingress?

Imagine you have three applications.

- Shopping Website
- Payment API
- Admin Dashboard

Without Ingress:

```text
Shopping App

↓

LoadBalancer

↓

Public IP #1

---------------------

Payment API

↓

LoadBalancer

↓

Public IP #2

---------------------

Admin Portal

↓

LoadBalancer

↓

Public IP #3
```

Every application requires its own Load Balancer.

This increases:

- Cost
- Complexity
- Management Effort

Ingress solves this by using one Load Balancer.

---

## Ingress Overview

```text
Internet

        │

        ▼

Ingress

        │

────────────┼────────────

        │

Shopping Service

Payment Service

Admin Service
```

A single Ingress routes traffic to multiple Services.

---

## Real World Example

Suppose users access:

```text
shop.company.com

api.company.com

admin.company.com
```

A single Ingress routes requests to the correct backend Service based on the hostname.

---

## Benefits

Ingress provides:

- Single Entry Point
- HTTP/HTTPS Routing
- SSL Termination
- Host-Based Routing
- Path-Based Routing
- Reduced Infrastructure Cost

---

## Production Tip

Ingress works at **Layer 7 (Application Layer)** and is designed specifically for HTTP and HTTPS traffic.

---

## Interview Question

### Question

What is Kubernetes Ingress?

### Answer

Ingress is a Kubernetes resource that manages external HTTP/HTTPS traffic and routes requests to the appropriate Services based on rules such as hostnames and URL paths.

---

# 2. Why do we need Ingress?

## Answer

Applications often expose multiple services.

Without Ingress:

- Every application needs its own LoadBalancer.
- Every LoadBalancer receives its own public IP.
- Costs increase significantly.

Ingress allows all applications to share one external endpoint.

---

## Without Ingress

```text
Internet

↓

LoadBalancer

↓

Frontend

--------------------

Internet

↓

LoadBalancer

↓

Backend

--------------------

Internet

↓

LoadBalancer

↓

Admin
```

Three applications require three Load Balancers.

---

## With Ingress

```text
Internet

        │

        ▼

Ingress

        │

────────────┼────────────

        │

Frontend

Backend

Admin
```

One Ingress handles all incoming traffic.

---

## Why is this important?

Ingress provides:

- Lower Cloud Costs
- Centralized Routing
- Easier SSL Management
- Better Scalability
- Simplified Networking

---

## Production Example

A company hosts:

```text
company.com

↓

Frontend

api.company.com

↓

Backend API

admin.company.com

↓

Admin Portal
```

All traffic is managed through one Ingress.

---

## Production Tip

For production environments, combine an Ingress with a cloud Load Balancer to provide secure and scalable external access.

---

## Interview Question

### Question

Why do we need Ingress?

### Answer

Ingress provides a single entry point for multiple applications, reducing infrastructure costs while enabling HTTP routing and SSL termination.

---

# 3. How Ingress Works

## Answer

Ingress does not communicate with Pods directly.

Instead, it routes requests to Kubernetes Services.

The Services then forward traffic to the appropriate Pods.

---

## Internal Workflow

```text
Browser

        │

        ▼

Ingress

        │

Service

        │

────────────┼────────────

        │

Pod 1

Pod 2

Pod 3
```

---

## Request Flow

Suppose a user visits:

```text
https://shop.company.com
```

The Ingress:

1. Receives the request.
2. Matches the hostname.
3. Routes traffic to the Shopping Service.
4. The Service forwards the request to a healthy Pod.
5. The application returns a response.

---

## Production Example

Request:

```text
https://api.company.com/orders
```

Ingress forwards the request to:

```text
api-service
```

The Service load balances traffic across API Pods.

---

## Production Tip

Ingress routes traffic to **Services**, not directly to Pods.

---

## Interview Question

### Question

How does Kubernetes Ingress work?

### Answer

Ingress receives external HTTP/HTTPS requests, evaluates routing rules, forwards traffic to a Kubernetes Service, and the Service delivers the request to a healthy Pod.

---

# 4. Ingress Architecture

## Answer

Ingress sits between external users and Kubernetes Services.

---

## Architecture

```text
Internet

        │

        ▼

Cloud Load Balancer

        │

        ▼

Ingress Controller

        │

        ▼

Ingress Resource

        │

────────────┼────────────

        │

Frontend Service

Backend Service

Admin Service

        │

Pods
```

---

## Component Responsibilities

### Cloud Load Balancer

- Receives external traffic
- Forwards requests to the Ingress Controller

---

### Ingress Controller

- Reads Ingress resources
- Configures routing rules
- Forwards traffic to Services

---

### Ingress Resource

- Defines routing rules
- Maps domains and paths to Services

---

### Service

- Load balances requests
- Routes traffic to healthy Pods

---

## Production Tip

An Ingress resource alone does **nothing**.

It requires an **Ingress Controller** (such as NGINX Ingress Controller or Traefik) to process and enforce the routing rules.

---

## Interview Question

### Question

What is the difference between an Ingress and an Ingress Controller?

### Answer

An Ingress is a Kubernetes resource that defines routing rules, while an Ingress Controller is the component that reads those rules and routes traffic accordingly.

---

# 5. What is an Ingress Controller?

## Answer

An **Ingress Controller** is the component that implements the rules defined in an Ingress resource.

An Ingress resource only contains routing rules.

It does not process network traffic by itself.

The Ingress Controller continuously watches the Kubernetes API for Ingress resources and configures itself accordingly.

---

## Why do we need an Ingress Controller?

Imagine creating an Ingress resource.

```text
Ingress

↓

Host Rules

↓

Path Rules
```

Without an Ingress Controller, Kubernetes has no component responsible for enforcing those rules.

The Ingress Controller performs that work.

---

## Internal Workflow

```text
Ingress Resource

        │

Routing Rules

        │

        ▼

Ingress Controller

        │

Configures Routing

        │

        ▼

Services

        │

Pods
```

---

## Popular Ingress Controllers

- NGINX Ingress Controller
- Traefik
- HAProxy
- Kong
- AWS Load Balancer Controller
- Azure Application Gateway Ingress Controller (AGIC)

---

## Production Example

An AKS cluster uses the **NGINX Ingress Controller**.

Whenever a new Ingress resource is created, the controller automatically updates its routing configuration.

No manual configuration is required.

---

## Production Tip

An Ingress resource without an Ingress Controller has no effect.

Always deploy a supported Ingress Controller.

---

## Interview Question

### Question

What is an Ingress Controller?

### Answer

An Ingress Controller watches Kubernetes Ingress resources and implements the routing rules by forwarding external traffic to the appropriate Services.

---

# 6. Path-Based Routing

## Answer

Path-Based Routing allows multiple applications to share the same domain while routing requests based on the URL path.

---

## Example

```text
company.com/shop

↓

Shopping Service

--------------------

company.com/api

↓

API Service

--------------------

company.com/admin

↓

Admin Service
```

---

## Internal Workflow

```text
Browser

        │

company.com/shop

        │

        ▼

Ingress

        │

────────────┼────────────

        │

/shop

↓

Shopping Service

/api

↓

API Service

/admin

↓

Admin Service
```

---

## Example YAML

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: company-ingress

spec:

  rules:

  - host: company.com

    http:

      paths:

      - path: /shop

        pathType: Prefix

        backend:

          service:

            name: shopping-service

            port:

              number: 80
```

---

## Production Example

A company hosts:

- Shopping Portal
- REST API
- Admin Dashboard

All applications are available using the same domain with different paths.

---

## Production Tip

Use Path-Based Routing when multiple applications belong to the same website.

---

## Interview Question

### Question

What is Path-Based Routing?

### Answer

Path-Based Routing forwards requests to different Services based on the requested URL path.

---

# 7. Host-Based Routing

## Answer

Host-Based Routing routes requests according to the requested domain name.

Each application has its own hostname.

---

## Example

```text
shop.company.com

↓

Shopping Service

--------------------

api.company.com

↓

API Service

--------------------

admin.company.com

↓

Admin Service
```

---

## Internal Workflow

```text
Browser

        │

api.company.com

        │

        ▼

Ingress

        │

────────────┼────────────

        │

shop.company.com

↓

Shopping Service

api.company.com

↓

API Service

admin.company.com

↓

Admin Service
```

---

## Example YAML

```yaml
rules:

- host: api.company.com

  http:

    paths:

    - path: /

      pathType: Prefix

      backend:

        service:

          name: api-service

          port:

            number: 80
```

---

## Production Example

A SaaS company hosts:

```text
portal.company.com

api.company.com

docs.company.com
```

One Ingress routes requests based on the hostname.

---

## Production Tip

Host-Based Routing is commonly used for microservices and multi-tenant applications.

---

## Interview Question

### Question

What is Host-Based Routing?

### Answer

Host-Based Routing directs requests to different Services based on the requested hostname.

---

# 8. TLS & HTTPS

## Answer

Ingress supports HTTPS using TLS certificates.

Instead of each application managing certificates individually, the Ingress Controller terminates TLS and forwards HTTP traffic internally.

---

## Internal Workflow

```text
Browser

HTTPS

        │

        ▼

Ingress Controller

TLS Termination

        │

HTTP

        ▼

Service

        │

Pod
```

---

## Example YAML

```yaml
tls:

- hosts:

  - shop.company.com

  secretName: shop-tls-secret
```

The TLS certificate is stored inside a Kubernetes Secret.

---

## Production Example

Users access:

```text
https://shop.company.com
```

The Ingress Controller decrypts the HTTPS request and forwards it securely to the backend Service.

---

## Production Tip

Never store TLS certificates inside container images.

Store them as Kubernetes Secrets and reference them from the Ingress resource.

---

## Interview Question

### Question

How does TLS work with Kubernetes Ingress?

### Answer

The Ingress Controller terminates HTTPS using a TLS certificate stored in a Kubernetes Secret and forwards the request to the backend Service.

---

# 9. Complete Ingress YAML Example

## Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: company-ingress

spec:

  ingressClassName: nginx

  tls:

  - hosts:

    - shop.company.com

    secretName: shop-tls

  rules:

  - host: shop.company.com

    http:

      paths:

      - path: /

        pathType: Prefix

        backend:

          service:

            name: shopping-service

            port:

              number: 80
```

---

## Production Tip

Always specify the correct `ingressClassName` so Kubernetes knows which Ingress Controller should manage the resource.

---

## Interview Question

### Question

What are the key fields in an Ingress resource?

### Answer

The most important fields are:

- ingressClassName
- rules
- host
- paths
- backend
- tls

---

## 📌 Quick Comparison

| Feature | Path-Based Routing | Host-Based Routing |
|----------|--------------------|--------------------|
| Routing Method | URL Path | Hostname |
| Example | `/api` | `api.company.com` |
| Best Use Case | Multiple apps under one domain | Multiple domains or subdomains |

---

# 10. 🔍 What really happens when a user opens your application in a browser?

## Answer

This is one of the most frequently asked Kubernetes networking interview questions.

Suppose a user opens:

```text
https://shop.company.com
```

Let's understand the complete request flow.

---

## Internal Workflow

```text
Browser

        │

HTTPS Request

        │

        ▼

DNS

        │

Resolves Domain

        │

        ▼

Cloud Load Balancer

        │

        ▼

Ingress Controller

        │

Reads Ingress Rules

        │

        ▼

Ingress Resource

        │

Matches Host & Path

        │

        ▼

Kubernetes Service

        │

Kube Proxy

        │

        ▼

Healthy Pod

        │

Application Processes Request

        │

        ▼

Response Returned
```

---

## Step-by-Step Flow

1. The user enters the application URL.
2. DNS resolves the domain to the Load Balancer's public IP.
3. The Cloud Load Balancer forwards the request to the Ingress Controller.
4. The Ingress Controller evaluates the Ingress rules.
5. The matching Service is identified.
6. Kube Proxy routes the request to a healthy Pod.
7. The application processes the request.
8. The response is sent back to the user.

---

## Production Example

User opens:

```text
https://api.company.com/orders
```

The request flows through:

```text
Internet

↓

DNS

↓

Azure Load Balancer

↓

NGINX Ingress Controller

↓

api-service

↓

Backend Pod

↓

Response
```

The user never communicates directly with the Pod.

---

## Production Tip

Every external request should pass through:

- DNS
- Load Balancer
- Ingress Controller
- Service

Pods should never be exposed directly to the internet.

---

## Interview Question

### Question

What happens internally when a user accesses a Kubernetes application?

### Answer

The request passes through DNS, the cloud Load Balancer, the Ingress Controller, the Kubernetes Service, Kube Proxy, and finally reaches a healthy Pod.

---

# 11. Default Backend

## Answer

Sometimes a request does not match any Ingress rule.

For example:

```text
https://unknown.company.com
```

or

```text
/company/unknown-page
```

In these situations, the request is sent to the **Default Backend**.

---

## Internal Workflow

```text
Browser

        │

Unknown Request

        │

        ▼

Ingress Controller

        │

No Matching Rule

        │

        ▼

Default Backend

        │

404 Response
```

---

## Production Example

A user enters an incorrect URL.

Instead of returning an error from the application, the Ingress Controller forwards the request to the Default Backend.

---

## Production Tip

Configure a custom Default Backend to return a branded error page instead of a generic 404 response.

---

## Interview Question

### Question

What is the Default Backend in Kubernetes Ingress?

### Answer

The Default Backend handles requests that do not match any Ingress routing rule.

---

# 12. Common Beginner Mistakes

## Mistake 1

❌ Assuming Ingress works without an Ingress Controller.

An Ingress resource requires an Ingress Controller to process its rules.

---

## Mistake 2

❌ Using a LoadBalancer Service for every application.

Use one Ingress with multiple routing rules whenever possible.

---

## Mistake 3

❌ Exposing Pods directly to the internet.

Expose applications through Services and Ingress.

---

## Mistake 4

❌ Forgetting TLS configuration.

Production applications should use HTTPS with valid TLS certificates.

---

## Mistake 5

❌ Incorrect Host or Path rules.

Always verify that Ingress rules match your application's domains and URL paths.

---

# 13. Production Best Practices

Always follow these recommendations.

---

✅ Use an Ingress Controller such as NGINX, Traefik, or Azure Application Gateway Ingress Controller.

---

✅ Configure HTTPS using TLS certificates stored as Kubernetes Secrets.

---

✅ Use Host-Based Routing for multiple applications or subdomains.

---

✅ Use Path-Based Routing when applications share the same domain.

---

✅ Protect applications with authentication, rate limiting, and Web Application Firewalls (WAF) where appropriate.

---

✅ Monitor Ingress Controller logs and metrics for troubleshooting.

---

## 📌 Quick Revision

### Complete Request Flow

```text
Browser

↓

DNS

↓

Cloud Load Balancer

↓

Ingress Controller

↓

Ingress Resource

↓

Service

↓

Kube Proxy

↓

Pod

↓

Application Response
```

---

### Ingress Responsibilities

- Single Entry Point
- Host-Based Routing
- Path-Based Routing
- TLS Termination
- HTTPS Support
- Traffic Routing

---

### Routing Types

```text
Host-Based Routing

↓

api.company.com

---------------------

Path-Based Routing

↓

company.com/api
```

---

# Summary

After completing this guide, you should understand:

✅ Ingress

✅ Ingress Controller

✅ Path-Based Routing

✅ Host-Based Routing

✅ TLS & HTTPS

✅ Default Backend

✅ Complete Request Flow

✅ Production Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What exposes multiple applications through one entry point? | Ingress |
| Does Ingress route directly to Pods? | No, it routes to Services |
| Which component implements Ingress rules? | Ingress Controller |
| Default Ingress protocol? | HTTP/HTTPS |
| Where are TLS certificates stored? | Kubernetes Secrets |
| What is Host-Based Routing? | Routing based on the hostname |
| What is Path-Based Routing? | Routing based on the URL path |
| What handles unmatched requests? | Default Backend |
| Can Ingress replace Services? | No, Ingress routes traffic to Services |
| Should production applications use HTTPS? | Yes |

---

# What's Next?

➡️ **11-kubernetes-troubleshooting.md**

In the next guide, you'll learn:

- `kubectl get`, `describe`, `logs`, and `exec`
- Debugging CrashLoopBackOff
- ImagePullBackOff
- Pending Pods
- OOMKilled
- Node NotReady
- Service Connectivity Issues
- DNS Resolution Problems
- PV/PVC Binding Failures
- 🔍 What really happens when a Pod fails?
- Production Troubleshooting Workflow
- Common Interview Scenarios