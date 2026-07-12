# Namespaces

## Overview

A **Namespace** is a Kubernetes resource that provides **logical isolation** for resources within a cluster.

Namespaces allow multiple teams, projects, or environments to share the same Kubernetes cluster while keeping their resources separate and organized.

Instead of creating multiple Kubernetes clusters, organizations commonly use **Namespaces** to divide workloads.

---

# Why Use Namespaces?

Without Namespaces:

```text
Kubernetes Cluster

├── Pod A
├── Pod B
├── Service A
├── Service B
├── Deployment A
└── Deployment B
```

Problems:

- Resource name conflicts
- Difficult management
- Poor organization
- No logical isolation

---

With Namespaces:

```text
Kubernetes Cluster

├── Development Namespace
│     ├── Pods
│     ├── Services
│     └── Deployments
│
├── Testing Namespace
│     ├── Pods
│     ├── Services
│     └── Deployments
│
└── Production Namespace
      ├── Pods
      ├── Services
      └── Deployments
```

Each environment is isolated while sharing the same cluster.

---

# Benefits

- Logical isolation
- Multi-tenancy
- Better resource organization
- Easier access control
- Resource quotas
- Environment separation

---

# How Namespaces Work

```text
             Kubernetes Cluster
                    │
     ┌──────────────┼──────────────┐
     ▼              ▼              ▼
 Development     Testing      Production
 Namespace       Namespace     Namespace
     │              │              │
  Pods, SVCs     Pods, SVCs     Pods, SVCs
```

Resources inside one Namespace are isolated from resources in another Namespace.

---

# Default Namespaces

Kubernetes creates several Namespaces by default.

| Namespace | Purpose |
|------------|---------|
| default | Default location for user workloads |
| kube-system | Kubernetes system components |
| kube-public | Public resources accessible by all users |
| kube-node-lease | Stores node heartbeat information |

View Namespaces:

```bash
kubectl get namespaces
```

or

```bash
kubectl get ns
```

---

# Create a Namespace

YAML Example:

```yaml
apiVersion: v1
kind: Namespace

metadata:
  name: development
```

Create:

```bash
kubectl apply -f namespace.yaml
```

Or directly:

```bash
kubectl create namespace development
```

---

# Deploy Resources to a Namespace

Example Deployment:

```yaml
metadata:
  name: nginx

  namespace: development
```

Or specify using kubectl:

```bash
kubectl apply -f deployment.yaml -n development
```

---

# View Resources in a Namespace

Pods:

```bash
kubectl get pods -n development
```

Services:

```bash
kubectl get svc -n development
```

Deployments:

```bash
kubectl get deployments -n development
```

---

# Switch Default Namespace

View current context:

```bash
kubectl config view --minify
```

Set default namespace:

```bash
kubectl config set-context --current --namespace=development
```

Now commands like:

```bash
kubectl get pods
```

will automatically use the **development** namespace.

---

# Delete a Namespace

```bash
kubectl delete namespace development
```

> **Warning:** Deleting a Namespace deletes all resources within it.

---

# Common Commands

Create Namespace:

```bash
kubectl create namespace development
```

View Namespaces:

```bash
kubectl get ns
```

Describe Namespace:

```bash
kubectl describe namespace development
```

Delete Namespace:

```bash
kubectl delete namespace development
```

View Resources:

```bash
kubectl get all -n development
```

---

# Namespace vs Cluster

| Namespace | Cluster |
|------------|---------|
| Logical isolation | Physical Kubernetes environment |
| Shares cluster resources | Contains all Namespaces |
| Lightweight | More resource intensive |

---

# Namespace vs Label

| Namespace | Label |
|------------|-------|
| Separates resources | Categorizes resources |
| Used for isolation | Used for selection |
| One per resource | Multiple labels allowed |

---

# Advantages

- Better organization
- Resource isolation
- Supports multi-team environments
- Enables RBAC and Resource Quotas
- Reduces naming conflicts

---

# Limitations

- Namespaces do not provide complete security isolation.
- Some resources (such as Nodes and PersistentVolumes) are cluster-wide.
- Additional security mechanisms like RBAC and Network Policies are still required.

---

# Best Practices

- Create separate Namespaces for development, testing, and production.
- Use meaningful Namespace names.
- Apply Resource Quotas to each Namespace.
- Use RBAC to control Namespace access.
- Combine Namespaces with Network Policies for better security.
- Avoid deploying production workloads in the **default** Namespace.

---

# Interview Questions

### What is a Namespace in Kubernetes?

A Namespace is a logical partition within a Kubernetes cluster that isolates resources.

---

### Why are Namespaces used?

To organize resources, support multiple teams, and separate environments such as development, testing, and production.

---

### Are Namespaces physical clusters?

No. Namespaces provide logical isolation within the same Kubernetes cluster.

---

### Which Namespace is used by default?

**default**

---

### Can resources in different Namespaces have the same name?

Yes. Resource names only need to be unique within a Namespace.

---

# Key Takeaways

- Namespaces provide logical isolation in Kubernetes.
- They help organize workloads and support multi-tenancy.
- Kubernetes includes several built-in Namespaces.
- RBAC, Resource Quotas, and Network Policies are commonly applied at the Namespace level.
- Using separate Namespaces for different environments is a Kubernetes best practice.

---

# References

- Kubernetes Official Documentation
- Kubernetes Namespaces Documentation
- Kubernetes Multi-Tenancy Guide
- CNCF Kubernetes Documentation