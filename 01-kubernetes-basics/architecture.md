# Kubernetes Architecture

## Overview

Kubernetes follows a **master-worker architecture**, officially referred to as the **Control Plane and Worker Nodes** architecture.

The Control Plane manages the overall state of the cluster, while Worker Nodes run the containerized applications.

```
                +---------------------------+
                |       Control Plane       |
                |---------------------------|
                | API Server                |
                | Scheduler                 |
                | Controller Manager        |
                | etcd                      |
                +-------------+-------------+
                              |
               ---------------------------------
               |                               |
      +--------+--------+             +--------+--------+
      |   Worker Node 1 |             |   Worker Node 2 |
      |-----------------|             |-----------------|
      | Kubelet         |             | Kubelet         |
      | Kube Proxy      |             | Kube Proxy      |
      | Container Runtime|            | Container Runtime|
      | Pods            |             | Pods            |
      +-----------------+             +-----------------+
```

---

# Architecture Components

## 1. Control Plane

The Control Plane is responsible for managing the Kubernetes cluster.

It makes scheduling decisions, maintains the desired state of the cluster, and responds to cluster events.

### Components

- API Server
- Scheduler
- Controller Manager
- etcd

---

## 2. API Server

The API Server is the entry point for all communication with the Kubernetes cluster.

Every request from users, kubectl, CI/CD pipelines, or internal components passes through the API Server.

### Responsibilities

- Validates requests
- Authenticates users
- Authorizes access
- Stores cluster data in etcd
- Exposes the Kubernetes REST API

---

## 3. etcd

etcd is Kubernetes' distributed key-value database.

It stores the entire cluster state, including:

- Nodes
- Pods
- Deployments
- Secrets
- ConfigMaps
- Services

Without etcd, Kubernetes cannot remember the state of the cluster.

---

## 4. Scheduler

The Scheduler decides where Pods should run.

When a Pod is created, the Scheduler selects the most suitable Worker Node based on available resources and scheduling policies.

Scheduling considers factors such as:

- CPU availability
- Memory availability
- Node affinity
- Taints and tolerations
- Resource requests and limits

---

## 5. Controller Manager

The Controller Manager continuously monitors the cluster.

It compares the **desired state** with the **actual state** and performs corrective actions when necessary.

Examples:

- Restart failed Pods
- Maintain replica count
- Replace unhealthy nodes
- Manage endpoints

---

# Worker Node

Worker Nodes execute containerized applications.

Each Worker Node contains several components that communicate with the Control Plane.

---

## 1. Kubelet

Kubelet is the primary agent running on every Worker Node.

Responsibilities:

- Receives instructions from the API Server
- Starts Pods
- Stops Pods
- Monitors container health
- Reports node status

---

## 2. Kube Proxy

Kube Proxy handles networking between Pods and Services.

Responsibilities:

- Maintains networking rules
- Load balances traffic
- Enables Service communication
- Routes requests to Pods

---

## 3. Container Runtime

The Container Runtime is responsible for running containers.

Supported runtimes include:

- containerd
- CRI-O

Docker was previously supported through Dockershim, but modern Kubernetes versions use CRI-compatible runtimes.

---

## 4. Pods

Pods are the smallest deployable units in Kubernetes.

A Pod can contain:

- One container
- Multiple tightly coupled containers

Containers inside the same Pod share:

- Network namespace
- Storage volumes
- IP address

---

# Request Flow

The following sequence illustrates how Kubernetes processes a deployment request:

1. User executes a kubectl command.
2. kubectl sends the request to the API Server.
3. API Server validates and stores the desired state in etcd.
4. Scheduler selects the best Worker Node.
5. Kubelet receives instructions.
6. Container Runtime creates the container.
7. Kube Proxy configures networking.
8. The Pod becomes available.

---

# Kubernetes Architecture Workflow

```
User
   │
   ▼
kubectl
   │
   ▼
API Server
   │
   ▼
etcd
   │
   ▼
Scheduler
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
```

---

# Advantages of Kubernetes Architecture

- Highly scalable
- Self-healing
- Automated scheduling
- High availability
- Declarative configuration
- Rolling updates
- Efficient resource utilization
- Fault tolerance

---

# Key Takeaways

- Kubernetes consists of a **Control Plane** and multiple **Worker Nodes**.
- The API Server is the central communication hub.
- etcd stores the cluster state.
- Scheduler determines Pod placement.
- Controller Manager maintains the desired state.
- Kubelet manages Pods on Worker Nodes.
- Kube Proxy handles networking.
- Pods are the smallest deployable units.

---

# References

- Kubernetes Official Documentation
- Kubernetes Architecture Guide
- CNCF Documentation