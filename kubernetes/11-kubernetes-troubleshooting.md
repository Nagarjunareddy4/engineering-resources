# 🚀 Kubernetes Troubleshooting - Debugging Production Issues Like a DevOps Engineer

This guide explains a systematic approach to troubleshooting Kubernetes production issues.

If you've ever wondered:

- Where should I start troubleshooting?
- Which kubectl commands should I use?
- How do I debug failing Pods?
- How do I investigate Kubernetes issues in production?
- How do experienced DevOps engineers troubleshoot clusters?

This guide is for you.

---

# 📑 Table of Contents

1. Kubernetes Troubleshooting Mindset
2. Essential kubectl Troubleshooting Commands
3. Step-by-Step Troubleshooting Workflow
4. Understanding Kubernetes Events
5. Common Interview Questions

---

# 1. Kubernetes Troubleshooting Mindset

## Answer

One of the biggest mistakes beginners make is running random commands until something works.

Experienced DevOps engineers follow a **structured troubleshooting process**.

They ask one simple question first:

> **"Where is the problem?"**

Most Kubernetes issues belong to one of these categories:

- Pod Problems
- Node Problems
- Networking Problems
- Storage Problems
- Configuration Problems

Finding the correct category makes troubleshooting much faster.

---

## Production Troubleshooting Flow

```text
Application Not Working

        │

        ▼

Is the Pod Running?

        │

────────┼────────

        │

No

↓

Pod Issue

Yes

↓

Is Service Working?

        │

────────┼────────

        │

No

↓

Networking Issue

Yes

↓

Check Application Logs
```

---

## Golden Rule

Never guess.

Always verify each layer before moving to the next.

---

## Production Example

Users report:

```text
Website is Down
```

Don't immediately restart Pods.

Instead investigate:

1. Is the Pod Running?
2. Is the Service Working?
3. Is Ingress Working?
4. Are the Nodes Healthy?
5. Are the Application Logs showing errors?

---

## Benefits

Following a structured approach:

- Reduces downtime
- Prevents unnecessary changes
- Speeds up root cause analysis
- Improves production reliability

---

## Production Tip

The first command should usually be:

```bash
kubectl get pods
```

Understand the current state before making changes.

---

## Interview Question

### Question

What is your approach to troubleshooting Kubernetes issues?

### Answer

Start by identifying the affected layer (Pod, Service, Network, Storage, or Node), verify the current state using kubectl commands, inspect events and logs, identify the root cause, and then apply the appropriate fix.

---

# 2. Essential kubectl Troubleshooting Commands

## Answer

The following commands are the most frequently used during Kubernetes troubleshooting.

---

## View Cluster Resources

```bash
kubectl get pods
```

Shows Pod status.

---

```bash
kubectl get deployments
```

Shows Deployments.

---

```bash
kubectl get services
```

Shows Services.

---

```bash
kubectl get nodes
```

Shows Worker Nodes.

---

```bash
kubectl get events
```

Shows cluster events.

---

## Inspect Resources

```bash
kubectl describe pod <pod-name>
```

Displays:

- Events
- Conditions
- Resource Limits
- Scheduling Information

---

```bash
kubectl describe node <node-name>
```

Shows:

- Node Status
- Resource Usage
- Taints
- Conditions

---

## View Logs

```bash
kubectl logs <pod-name>
```

Displays application logs.

---

For multi-container Pods:

```bash
kubectl logs <pod-name> -c <container-name>
```

---

## Access Running Containers

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

or

```bash
kubectl exec -it <pod-name> -- /bin/bash
```

Useful for:

- Checking files
- Testing connectivity
- Verifying environment variables

---

## Production Tip

Learn these five commands first:

```bash
kubectl get

kubectl describe

kubectl logs

kubectl exec

kubectl get events
```

They solve most production troubleshooting scenarios.

---

## Interview Question

### Question

Which kubectl commands do you use most during troubleshooting?

### Answer

The most common commands are:

- kubectl get
- kubectl describe
- kubectl logs
- kubectl exec
- kubectl get events

---

# 3. Step-by-Step Troubleshooting Workflow

## Answer

Experienced engineers troubleshoot Kubernetes from the outside in.

Never skip steps.

---

## Workflow

```text
User Reports Problem

        │

        ▼

Check Pods

        │

        ▼

Check Services

        │

        ▼

Check Ingress

        │

        ▼

Check Events

        │

        ▼

Check Logs

        │

        ▼

Check Nodes

        │

        ▼

Identify Root Cause

        │

        ▼

Fix Issue
```

---

## Example Scenario

A customer reports:

```text
Cannot access the website.
```

Step 1

```bash
kubectl get pods
```

Pods are Running.

---

Step 2

```bash
kubectl get svc
```

Service exists.

---

Step 3

```bash
kubectl get ingress
```

Ingress exists.

---

Step 4

```bash
kubectl logs frontend
```

Logs show:

```text
Database Connection Refused
```

Root Cause:

Database is unavailable.

---

## Production Tip

Never restart resources before understanding why they failed.

Restarting may temporarily hide the real problem.

---

## Interview Question

### Question

How would you troubleshoot an application that is inaccessible?

### Answer

Verify Pods, Services, Ingress, Events, Logs, and Nodes in a structured order. Identify the root cause before making any changes.

---

# 4. Understanding Kubernetes Events

## Answer

Kubernetes records important cluster activities as **Events**.

Events explain why resources are behaving unexpectedly.

Examples include:

- Failed Scheduling
- Image Pull Errors
- Failed Mounts
- Health Probe Failures
- Container Restarts

---

## View Events

```bash
kubectl get events
```

---

## Sort Events by Time

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## Example Output

```text
FailedScheduling

ImagePullBackOff

BackOff

FailedMount

OOMKilled
```

These events often point directly to the root cause.

---

## Production Tip

Always review Kubernetes Events before deleting or restarting Pods.

Events often contain the first clue to what went wrong.

---

## Interview Question

### Question

Why are Kubernetes Events useful?

### Answer

Events provide detailed information about cluster activities and failures, helping engineers quickly identify the root cause of issues.

---

# 5. CrashLoopBackOff

## Answer

A **CrashLoopBackOff** occurs when a container starts, crashes, and Kubernetes repeatedly tries to restart it.

Instead of continuously restarting immediately, Kubernetes waits for increasing intervals before trying again.

---

## Internal Workflow

```text
Container Starts

        │

        ▼

Application Crashes

        │

        ▼

Kubelet Restarts Container

        │

        ▼

Application Crashes Again

        │

        ▼

Backoff Timer Increases

        │

        ▼

CrashLoopBackOff
```

---

## Common Causes

- Application Bug
- Missing Environment Variables
- Database Connection Failure
- Invalid Configuration
- Port Already in Use
- Missing ConfigMap or Secret

---

## Troubleshooting Commands

Check Pod Status

```bash
kubectl get pods
```

View Logs

```bash
kubectl logs <pod-name>
```

View Previous Logs

```bash
kubectl logs <pod-name> --previous
```

Describe Pod

```bash
kubectl describe pod <pod-name>
```

---

## Production Example

A Spring Boot application fails because the database is unavailable.

Logs show:

```text
Connection Refused
```

The Pod repeatedly restarts until the database becomes available.

---

## Production Tip

Always inspect application logs before restarting a CrashLoopBackOff Pod.

---

## Interview Question

### Question

What is CrashLoopBackOff?

### Answer

CrashLoopBackOff means the container repeatedly starts, crashes, and Kubernetes delays each restart using an exponential backoff strategy.

---

# 6. ImagePullBackOff & ErrImagePull

## Answer

These errors occur when Kubernetes cannot download the container image.

---

## Internal Workflow

```text
Pod Created

        │

        ▼

Kubelet Requests Image

        │

────────────┼────────────

        │

Image Found

↓

Container Starts

Image Not Found

↓

ImagePullBackOff
```

---

## Common Causes

- Incorrect Image Name
- Invalid Image Tag
- Private Registry Authentication Failure
- Registry Unavailable
- Network Connectivity Issues

---

## Troubleshooting Commands

Describe Pod

```bash
kubectl describe pod <pod-name>
```

Check Events

```bash
kubectl get events
```

Verify Image Name

```bash
kubectl get pod <pod-name> -o yaml
```

---

## Production Example

Deployment:

```yaml
image: nginx:lates
```

The tag is misspelled.

Kubernetes cannot download the image.

Pod Status:

```text
ImagePullBackOff
```

---

## Production Tip

Verify image names and registry credentials before deploying.

---

## Interview Question

### Question

What is the difference between ErrImagePull and ImagePullBackOff?

### Answer

ErrImagePull is the initial image download failure, while ImagePullBackOff indicates Kubernetes is repeatedly retrying the image pull with increasing delays.

---

# 7. Pending Pods

## Answer

A Pod remains in the **Pending** state when Kubernetes cannot schedule it onto a Worker Node.

---

## Internal Workflow

```text
Pod Created

        │

        ▼

Scheduler

        │

────────────┼────────────

        │

Resources Available

↓

Running

Resources Unavailable

↓

Pending
```

---

## Common Causes

- Insufficient CPU
- Insufficient Memory
- No Matching Node Selector
- Unsatisfied Taints & Tolerations
- Unbound PersistentVolumeClaim

---

## Troubleshooting Commands

Describe Pod

```bash
kubectl describe pod <pod-name>
```

Check Nodes

```bash
kubectl get nodes
```

View Node Resources

```bash
kubectl describe node <node-name>
```

---

## Production Example

A Pod requests:

```text
16 CPU
```

Cluster Nodes provide only:

```text
8 CPU
```

The Pod remains Pending until sufficient resources become available.

---

## Production Tip

Always check Scheduling Events in the Pod description.

---

## Interview Question

### Question

Why does a Pod remain in the Pending state?

### Answer

A Pod remains Pending because Kubernetes cannot find a suitable Worker Node due to resource, scheduling, or storage constraints.

---

# 8. OOMKilled

## Answer

**OOMKilled** stands for **Out Of Memory Killed**.

It means the container exceeded its configured memory limit.

The Linux Kernel terminated the process to protect the system.

---

## Internal Workflow

```text
Application Running

        │

Memory Usage Increases

        │

Memory Limit Exceeded

        │

        ▼

Linux OOM Killer

        │

Container Terminated

        │

Pod Restarted
```

---

## Common Causes

- Memory Leak
- Low Memory Limit
- Large Data Processing
- Inefficient Application Code

---

## Troubleshooting Commands

Describe Pod

```bash
kubectl describe pod <pod-name>
```

Check Resource Limits

```bash
kubectl get pod <pod-name> -o yaml
```

View Logs

```bash
kubectl logs <pod-name>
```

---

## Production Example

A Java application has:

```text
Memory Limit

512Mi
```

Application requires:

```text
1Gi
```

Result:

```text
OOMKilled
```

---

## Production Tip

Always configure realistic CPU and Memory Requests & Limits.

Monitor application memory usage using Prometheus and Grafana.

---

## Interview Question

### Question

What does OOMKilled mean?

### Answer

OOMKilled indicates that the container exceeded its memory limit and was terminated by the Linux Out Of Memory Killer.

---

# 9. ContainerCreating

## Answer

The **ContainerCreating** state means Kubernetes is preparing the container before it starts.

---

## Internal Workflow

```text
Pod Scheduled

        │

        ▼

Pull Image

        │

Mount Volumes

        │

Create Network

        │

        ▼

Start Container
```

---

## Common Causes of Delays

- Large Container Image
- Slow Image Registry
- Volume Mount Issues
- Persistent Volume Problems
- CNI Network Delays

---

## Troubleshooting Commands

Describe Pod

```bash
kubectl describe pod <pod-name>
```

Check Events

```bash
kubectl get events
```

---

## Production Example

A container image is:

```text
8 GB
```

The image download takes several minutes.

The Pod remains in:

```text
ContainerCreating
```

until the download completes.

---

## Production Tip

Use smaller container images such as Alpine-based images whenever appropriate to improve startup times.

---

## Interview Question

### Question

Why does a Pod remain in the ContainerCreating state?

### Answer

A Pod remains in ContainerCreating while Kubernetes prepares the container by pulling images, mounting volumes, and configuring networking.

---

# 10. 🔍 What really happens when a Pod fails?

## Answer

This is one of the most frequently asked Kubernetes interview questions.

Let's understand the complete recovery process.

Suppose a Pod suddenly crashes.

---

## Internal Workflow

```text
Application Crash

        │

        ▼

Container Exits

        │

        ▼

Kubelet Detects Failure

        │

        ▼

Pod Status Updated

(API Server)

        │

        ▼

ReplicaSet Detects

Desired State

        │

        ▼

Scheduler Selects Node

        │

        ▼

Kubelet Creates New Pod

        │

        ▼

Container Runtime Pulls Image

        │

        ▼

Application Running Again
```

---

## Step-by-Step Flow

1. The application crashes.
2. The Kubelet detects the failure.
3. The API Server updates the Pod status.
4. The ReplicaSet notices the missing Pod.
5. The Scheduler selects a healthy Worker Node.
6. The Kubelet creates a replacement Pod.
7. The Container Runtime starts the container.
8. The application becomes available again.

---

## Production Example

An API Pod crashes because of an application bug.

The Deployment requires:

```text
Replicas = 3
```

ReplicaSet automatically creates another Pod.

Users continue accessing the application while the failed Pod is replaced.

---

## Production Tip

Kubernetes focuses on restoring the **desired state**, not repairing the failed Pod.

---

## Interview Question

### Question

What happens internally when a Kubernetes Pod fails?

### Answer

The Kubelet detects the failure, the API Server updates the cluster state, the ReplicaSet creates a replacement Pod, the Scheduler selects a Worker Node, and the new Pod starts automatically.

---

# 11. Node NotReady

## Answer

A Worker Node enters the **NotReady** state when it stops communicating with the Kubernetes Control Plane.

Pods already running on that node may become unavailable.

---

## Common Causes

- Network Failure
- Kubelet Stopped
- Disk Pressure
- Memory Pressure
- CPU Pressure
- Node Shutdown

---

## Troubleshooting Commands

Check Nodes

```bash
kubectl get nodes
```

Describe Node

```bash
kubectl describe node <node-name>
```

View Node Events

```bash
kubectl get events
```

---

## Production Example

A virtual machine hosting a Worker Node loses network connectivity.

The Control Plane marks the node as:

```text
NotReady
```

Workloads are rescheduled onto healthy nodes if managed by a Deployment.

---

## Production Tip

Always investigate why the node became NotReady before restarting services.

---

## Interview Question

### Question

What does Node NotReady mean?

### Answer

Node NotReady indicates that the Worker Node is no longer communicating properly with the Kubernetes Control Plane.

---

# 12. Service Connectivity Issues

## Answer

Sometimes Pods are running successfully, but the application is still unreachable.

The issue may be with the Kubernetes Service.

---

## Common Causes

- Incorrect Labels
- Incorrect Selectors
- Wrong Target Port
- Service Missing
- Network Policy Restrictions

---

## Troubleshooting Commands

Check Services

```bash
kubectl get svc
```

Describe Service

```bash
kubectl describe svc <service-name>
```

View Endpoints

```bash
kubectl get endpoints
```

---

## Production Example

Service Selector

```yaml
app: frontend
```

Pod Label

```yaml
app: web
```

Since the labels do not match, the Service has no endpoints and cannot forward traffic.

---

## Production Tip

Always verify that Service selectors exactly match Pod labels.

---

## Interview Question

### Question

How do you troubleshoot a Service that is not routing traffic?

### Answer

Verify the Service exists, confirm its selectors match the Pod labels, check the endpoints, and ensure the target port is correct.

---

# 13. DNS Resolution Issues

## Answer

Pods communicate using Kubernetes DNS.

If DNS fails, applications cannot locate Services.

---

## Common Causes

- CoreDNS Failure
- Incorrect Service Name
- Namespace Mismatch
- Network Policy Restrictions

---

## Troubleshooting Commands

Verify CoreDNS Pods

```bash
kubectl get pods -n kube-system
```

Test DNS

```bash
kubectl exec -it <pod-name> -- nslookup backend-service
```

Check Service

```bash
kubectl get svc
```

---

## Production Example

The application attempts to connect to:

```text
backend-api
```

The actual Service is named:

```text
backend-service
```

DNS resolution fails because the Service name is incorrect.

---

## Production Tip

Always use Kubernetes Service names rather than hardcoded IP addresses.

---

## Interview Question

### Question

How do you troubleshoot Kubernetes DNS issues?

### Answer

Verify CoreDNS is running, confirm the Service exists, validate the Service name, and test DNS resolution from inside a Pod.

---

# 14. PV/PVC Binding Issues

## Answer

Sometimes a Pod remains Pending because its Persistent Volume Claim cannot bind to a Persistent Volume.

---

## Common Causes

- No Available PV
- StorageClass Missing
- Storage Size Mismatch
- Access Mode Mismatch

---

## Troubleshooting Commands

View PVC

```bash
kubectl get pvc
```

Describe PVC

```bash
kubectl describe pvc <pvc-name>
```

View PV

```bash
kubectl get pv
```

---

## Production Example

PVC requests:

```text
100Gi
```

Available PV:

```text
50Gi
```

The PVC remains Pending because no matching Persistent Volume exists.

---

## Production Tip

Always verify StorageClass, requested size, and access modes when troubleshooting storage issues.

---

## Interview Question

### Question

Why would a PVC remain Pending?

### Answer

A PVC remains Pending when Kubernetes cannot find or provision a matching Persistent Volume.

---

# 15. Common Beginner Mistakes

## Mistake 1

❌ Restarting Pods before checking logs.

Always investigate first.

---

## Mistake 2

❌ Ignoring Kubernetes Events.

Events often reveal the root cause immediately.

---

## Mistake 3

❌ Checking only Pod status.

Healthy Pods do not guarantee a healthy application.

---

## Mistake 4

❌ Forgetting to verify Services and Ingress.

Networking problems are common production issues.

---

## Mistake 5

❌ Assuming Kubernetes fixes application bugs.

Kubernetes restarts containers but cannot fix faulty application code.

---

# 16. Production Best Practices

Always follow these recommendations.

---

✅ Follow a structured troubleshooting workflow.

---

✅ Check Events before restarting workloads.

---

✅ Read application logs carefully.

---

✅ Monitor CPU and Memory usage.

---

✅ Configure Liveness and Readiness Probes.

---

✅ Use Prometheus and Grafana for monitoring.

---

✅ Centralize logs using ELK or Loki.

---

✅ Document Root Cause Analysis (RCA) after every production incident.

---

## 📌 Quick Revision

### Troubleshooting Workflow

```text
Application Issue

↓

Check Pods

↓

Check Services

↓

Check Ingress

↓

Check Events

↓

Check Logs

↓

Check Nodes

↓

Check Storage

↓

Identify Root Cause

↓

Apply Fix
```

---

### Most Common Pod Status

```text
Running

Pending

ContainerCreating

CrashLoopBackOff

ImagePullBackOff

OOMKilled
```

---

### Essential Commands

```bash
kubectl get pods

kubectl describe pod <pod>

kubectl logs <pod>

kubectl exec -it <pod> -- /bin/sh

kubectl get events

kubectl get svc

kubectl get nodes

kubectl get pvc
```

---

# Summary

After completing this guide, you should understand:

✅ Kubernetes Troubleshooting Mindset

✅ kubectl Troubleshooting Commands

✅ CrashLoopBackOff

✅ ImagePullBackOff

✅ Pending Pods

✅ OOMKilled

✅ Node NotReady

✅ Service Connectivity Issues

✅ DNS Resolution Problems

✅ PV/PVC Binding Issues

✅ Production Troubleshooting Workflow

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| First troubleshooting command? | `kubectl get pods` |
| View Pod details? | `kubectl describe pod` |
| View application logs? | `kubectl logs` |
| Access a running container? | `kubectl exec` |
| Why CrashLoopBackOff? | Application repeatedly crashes |
| Why ImagePullBackOff? | Image cannot be downloaded |
| Why Pending? | Scheduler cannot place the Pod |
| What is OOMKilled? | Container exceeded its memory limit |
| Why Node NotReady? | Node lost communication with the Control Plane |
| Best troubleshooting approach? | Follow a structured workflow and identify the root cause before making changes |

---

# 🎉 Congratulations!

You have now completed the **Kubernetes Learning Roadmap**.

You now understand:

✅ Kubernetes Fundamentals

✅ Kubernetes Architecture

✅ Pods

✅ ReplicaSets

✅ Deployments

✅ Services

✅ ConfigMaps & Secrets

✅ Volumes & Storage

✅ DaemonSets

✅ StatefulSets

✅ Jobs

✅ CronJobs

✅ Ingress & Networking

✅ Production Troubleshooting

---

# 🚀 What's Next?

You're now ready to move into advanced Kubernetes topics such as:

- Helm
- Kubernetes Security (RBAC, Network Policies, Pod Security)
- GitOps with Argo CD & Flux
- Operators & Custom Resources (CRDs)
- Kubernetes Monitoring (Prometheus & Grafana)
- Service Mesh (Istio & Linkerd)
- Kubernetes CI/CD Pipelines
- Kubernetes Autoscaling (HPA, VPA, Cluster Autoscaler)
- Production AKS/EKS/GKE Best Practices