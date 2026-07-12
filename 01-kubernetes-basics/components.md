# Kubernetes Components

## Overview

A Kubernetes cluster consists of several components that work together to deploy, manage, scale, and monitor containerized applications.

These components are divided into two categories:

- **Control Plane Components**
- **Worker Node Components**

Understanding each component is essential for troubleshooting and designing Kubernetes clusters.

---

# Kubernetes Components Diagram

```
                    Kubernetes Cluster

         +---------------------------------------+
         |           Control Plane               |
         |---------------------------------------|
         | • API Server                          |
         | • etcd                               |
         | • Scheduler                          |
         | • Controller Manager                 |
         +------------------+--------------------+
                            |
          ----------------------------------------
          |                                      |
+--------------------------+        +--------------------------+
|      Worker Node         |        |      Worker Node         |
|--------------------------|        |--------------------------|
| • Kubelet                |        | • Kubelet                |
| • Kube Proxy             |        | • Kube Proxy             |
| • Container Runtime      |        | • Container Runtime      |
| • Pods                   |        | • Pods                   |
+--------------------------+        +--------------------------+
```

---

# Control Plane Components

The Control Plane manages the entire Kubernetes cluster.

It receives requests, schedules workloads, maintains the cluster state, and ensures applications continue running as expected.

---

# 1. API Server (kube-apiserver)

The API Server is the central communication hub of Kubernetes.

Every request from users, kubectl, CI/CD tools, or internal Kubernetes components passes through the API Server.

### Responsibilities

- Validates API requests
- Authenticates users
- Authorizes access
- Exposes the Kubernetes REST API
- Stores cluster information in etcd
- Communicates with all cluster components

### Example

When you run:

```bash
kubectl get pods
```

The request is sent to the API Server, which retrieves Pod information and returns it to the client.

---

# 2. etcd

etcd is a distributed key-value database used to store the cluster's configuration and state.

It acts as Kubernetes' source of truth.

### Stores

- Pods
- Deployments
- Services
- Nodes
- Secrets
- ConfigMaps
- Namespaces
- Cluster configuration

### Features

- Highly available
- Consistent
- Distributed
- Fault tolerant

> **Important:** Always back up etcd regularly in production environments.

---

# 3. Scheduler (kube-scheduler)

The Scheduler determines which Worker Node should run a newly created Pod.

It evaluates all available nodes and selects the most suitable one.

### Scheduling Factors

- CPU availability
- Memory availability
- Resource requests
- Node affinity
- Taints and tolerations
- Pod affinity/anti-affinity

The Scheduler only decides **where** the Pod should run; it does not create the Pod.

---

# 4. Controller Manager (kube-controller-manager)

The Controller Manager continuously monitors the cluster and ensures the actual state matches the desired state.

If something changes unexpectedly, it takes corrective action.

### Examples

- Restart failed Pods
- Replace terminated Pods
- Maintain replica count
- Detect node failures
- Create endpoints

### Common Controllers

- Deployment Controller
- ReplicaSet Controller
- Node Controller
- Namespace Controller
- Job Controller

---

# Worker Node Components

Worker Nodes execute containerized applications.

Each Worker Node contains several components responsible for running and managing Pods.

---

# 5. Kubelet

Kubelet is the primary agent running on every Worker Node.

It communicates with the API Server and ensures Pods are running correctly.

### Responsibilities

- Creates Pods
- Starts containers
- Stops containers
- Performs health checks
- Reports node status
- Restarts failed containers

Without Kubelet, a Worker Node cannot participate in the cluster.

---

# 6. Kube Proxy

Kube Proxy manages networking on Worker Nodes.

It enables communication between Pods and Services.

### Responsibilities

- Maintains networking rules
- Load balances traffic
- Routes requests to Pods
- Enables Service discovery

Kube Proxy ensures users can access applications reliably.

---

# 7. Container Runtime

The Container Runtime is responsible for running containers.

Supported runtimes include:

- containerd
- CRI-O

Docker was previously supported through Dockershim, but Kubernetes now uses Container Runtime Interface (CRI)-compatible runtimes.

### Responsibilities

- Pull container images
- Create containers
- Start containers
- Stop containers
- Remove containers

---

# 8. Pods

Pods are the smallest deployable units in Kubernetes.

A Pod may contain:

- One container
- Multiple tightly coupled containers

Containers inside the same Pod share:

- IP address
- Network namespace
- Storage volumes

Pods are ephemeral and can be recreated automatically if they fail.

---

# Component Interaction Workflow

```
User
   │
   ▼
kubectl
   │
   ▼
API Server
   │
   ├────────► etcd
   │
   ├────────► Scheduler
   │
   ▼
Worker Node
   │
   ▼
Kubelet
   │
   ▼
Container Runtime
   │
   ▼
Pod Running
   │
   ▼
Kube Proxy
```

---

# Summary Table

| Component | Purpose |
|-----------|---------|
| API Server | Entry point for all Kubernetes requests |
| etcd | Stores the cluster state and configuration |
| Scheduler | Assigns Pods to Worker Nodes |
| Controller Manager | Maintains the desired state of the cluster |
| Kubelet | Runs and monitors Pods on Worker Nodes |
| Kube Proxy | Manages networking and load balancing |
| Container Runtime | Runs containerized applications |
| Pods | Smallest deployable unit in Kubernetes |

---

# Best Practices

- Protect the API Server with authentication and RBAC.
- Regularly back up etcd.
- Monitor Control Plane components.
- Keep Kubernetes components updated.
- Define resource requests and limits for workloads.
- Use multiple Control Plane nodes in production for high availability.

---

# Key Takeaways

- Kubernetes is built from multiple components working together.
- The Control Plane manages the cluster.
- Worker Nodes run applications.
- The API Server is the central communication hub.
- etcd stores the cluster state.
- Kubelet manages Pods on each Worker Node.
- Kube Proxy enables networking between Pods and Services.
- The Container Runtime executes containers.

---

# References

- Kubernetes Official Documentation
- CNCF Kubernetes Documentation
- Kubernetes Components Guide