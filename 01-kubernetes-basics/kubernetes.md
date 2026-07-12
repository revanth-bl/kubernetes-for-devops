# Kubernetes

## Overview

Kubernetes (commonly abbreviated as **K8s**) is an open-source container orchestration platform designed to automate the deployment, scaling, networking, and management of containerized applications.

It provides a consistent platform for running applications across on-premises data centers, public clouds, private clouds, and hybrid environments.

Kubernetes abstracts the underlying infrastructure, allowing developers to focus on building applications while Kubernetes manages their lifecycle.

---

# What is Container Orchestration?

Container orchestration is the automated management of containerized applications.

Instead of manually starting, stopping, monitoring, and scaling containers, Kubernetes performs these tasks automatically.

Container orchestration includes:

- Deploying applications
- Scheduling containers
- Scaling workloads
- Load balancing
- Service discovery
- Self-healing
- Rolling updates
- Rollbacks
- Storage management

---

# Kubernetes Goals

Kubernetes is designed to:

- Automate application deployment
- Maintain high availability
- Scale applications automatically
- Recover from failures
- Efficiently utilize computing resources
- Simplify infrastructure management

---

# Kubernetes Principles

Kubernetes follows several important principles.

## Declarative Configuration

Users define the **desired state** of the application using YAML or JSON files.

Example:

```yaml
replicas: 3
image: nginx:latest
```

Kubernetes continuously works to ensure the cluster matches this desired state.

---

## Desired State Management

Instead of telling Kubernetes **how** to perform every action, you specify **what** you want.

Example:

```
Desired State

3 Pods Running
```

If one Pod crashes, Kubernetes automatically creates a replacement.

---

## Self-Healing

Kubernetes constantly monitors workloads.

If a Pod fails, Kubernetes can:

- Restart it
- Replace it
- Move it to another node if necessary

This improves application reliability.

---

## Automatic Scheduling

When a Pod is created, Kubernetes automatically selects the most appropriate Worker Node based on available resources and scheduling rules.

Scheduling decisions consider:

- CPU
- Memory
- Node health
- Affinity rules
- Taints and tolerations

---

## Horizontal Scaling

Applications can scale by increasing or decreasing the number of Pods.

Example:

```
Traffic Increases

3 Pods

↓

10 Pods
```

---

# Kubernetes Objects

Everything in Kubernetes is represented as an object.

Common Kubernetes objects include:

| Object | Purpose |
|---------|---------|
| Pod | Smallest deployable unit |
| Deployment | Manages Pods |
| ReplicaSet | Maintains the desired number of Pods |
| Service | Provides network access |
| Namespace | Logical separation |
| ConfigMap | Stores configuration |
| Secret | Stores sensitive data |
| Volume | Provides persistent storage |
| Ingress | Manages external access |

---

# Kubernetes Resource Workflow

```
Developer

↓

YAML Manifest

↓

kubectl

↓

API Server

↓

Scheduler

↓

Worker Node

↓

Pod Running
```

---

# Desired State vs Actual State

One of Kubernetes' core concepts is maintaining the desired state.

Example:

Desired State

```
Replicas = 3
```

Actual State

```
Replicas = 2
```

Kubernetes detects the difference and creates another Pod until the desired state is restored.

---

# Kubernetes Features

- Container orchestration
- Declarative deployments
- Automated scheduling
- Self-healing
- Horizontal scaling
- Rolling updates
- Rollbacks
- Service discovery
- Load balancing
- Secret management
- Configuration management
- Persistent storage
- Resource monitoring

---

# Kubernetes Architecture Overview

A Kubernetes cluster consists of:

```
Control Plane
        │
        ▼
API Server
Scheduler
Controller Manager
etcd
        │
        ▼
Worker Nodes
        │
        ▼
Pods
```

---

# Kubernetes Deployment Process

1. Write a YAML manifest.
2. Apply it using `kubectl apply`.
3. API Server validates the request.
4. Scheduler selects a Worker Node.
5. Kubelet starts the Pod.
6. Container Runtime creates the container.
7. Kube Proxy configures networking.
8. The application becomes available.

---

# Advantages of Kubernetes

- Highly scalable
- Self-healing
- Portable across cloud providers
- Efficient resource utilization
- High availability
- Automated deployments
- Production ready
- Strong ecosystem
- Open source
- Cloud agnostic

---

# Limitations

Although Kubernetes is powerful, it also has challenges:

- Steep learning curve
- Complex initial setup
- Requires monitoring and maintenance
- Networking can be difficult for beginners
- Additional tools are often needed for logging, monitoring, and security

---

# When Should You Use Kubernetes?

Kubernetes is a good choice when:

- Running multiple containerized applications
- Building microservices
- Requiring automatic scaling
- Deploying applications across multiple servers
- Needing high availability
- Managing production workloads

For small applications or simple development environments, Docker alone may be sufficient.

---

# Key Takeaways

- Kubernetes is an open-source container orchestration platform.
- It automates deployment, scaling, and management of containerized applications.
- Kubernetes continuously maintains the desired state of the cluster.
- It provides self-healing, load balancing, rolling updates, and automatic scaling.
- Kubernetes has become the industry standard for managing cloud-native applications.

---

# References

- Kubernetes Official Documentation
- Cloud Native Computing Foundation (CNCF)
- Kubernetes Concepts Documentation