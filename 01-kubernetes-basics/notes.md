# Kubernetes Notes

## Quick Revision

- Kubernetes is an open-source container orchestration platform.
- It automates deployment, scaling, networking, and management of containers.
- Kubernetes is commonly abbreviated as **K8s**.
- Originally developed by Google and maintained by the **Cloud Native Computing Foundation (CNCF)**.
- Uses a **Control Plane** and one or more **Worker Nodes**.
- The smallest deployable unit is a **Pod**.

---

# Core Components

## Control Plane

- API Server
- Scheduler
- Controller Manager
- etcd

## Worker Node

- Kubelet
- Kube Proxy
- Container Runtime
- Pods

---

# Important Concepts

- Container Orchestration
- Desired State
- Declarative Configuration
- Self-Healing
- Automatic Scheduling
- Horizontal Scaling
- Rolling Updates
- Rollbacks
- Service Discovery
- Load Balancing

---

# Kubernetes Objects

- Pod
- ReplicaSet
- Deployment
- Service
- Namespace
- ConfigMap
- Secret
- Persistent Volume (PV)
- Persistent Volume Claim (PVC)
- Ingress

---

# Common kubectl Commands

## Cluster

```bash
kubectl cluster-info
kubectl get nodes
kubectl version
```

## Pods

```bash
kubectl get pods
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl delete pod <pod-name>
```

## Deployments

```bash
kubectl get deployments
kubectl create deployment nginx --image=nginx
kubectl scale deployment nginx --replicas=3
kubectl rollout restart deployment nginx
kubectl rollout undo deployment nginx
```

## Services

```bash
kubectl get svc
kubectl describe svc <service-name>
```

## Namespaces

```bash
kubectl get ns
kubectl create ns dev
kubectl delete ns dev
```

## Apply YAML

```bash
kubectl apply -f deployment.yaml
kubectl delete -f deployment.yaml
```

---

# Interview Questions

### What is Kubernetes?

An open-source container orchestration platform used to automate deployment, scaling, and management of containerized applications.

---

### What is a Pod?

The smallest deployable unit in Kubernetes that can contain one or more containers.

---

### What is a Node?

A machine (physical or virtual) that runs Kubernetes workloads.

---

### What is a Cluster?

A group of Control Plane and Worker Nodes working together.

---

### What is etcd?

A distributed key-value database that stores the cluster state and configuration.

---

### What is Kubelet?

An agent running on each Worker Node that manages Pods and communicates with the API Server.

---

### What is Kube Proxy?

A networking component that routes traffic and provides load balancing for Services.

---

### What is the API Server?

The central communication hub of Kubernetes.

---

### Difference between Docker and Kubernetes?

| Docker | Kubernetes |
|---------|------------|
| Creates and runs containers | Orchestrates containers |
| Single host | Multi-node cluster |
| Basic scaling | Automatic scaling |
| No built-in self-healing | Self-healing |
| Container runtime | Container orchestration platform |

---

# Best Practices

- Use Deployments instead of standalone Pods.
- Define resource requests and limits.
- Use namespaces to organize workloads.
- Store YAML files in Git.
- Back up etcd regularly.
- Use RBAC for access control.
- Monitor cluster health continuously.

---

# Useful Resources

- Kubernetes Official Documentation
- CNCF Documentation
- kubectl Command Reference
- Kubernetes API Reference

---

# Learning Progress

- [x] Introduction
- [x] Kubernetes Concepts
- [x] Architecture
- [x] Cluster
- [x] Components
- [x] kubectl
- [ ] Pods
- [ ] Deployments
- [ ] Services
- [ ] Storage
- [ ] Security
- [ ] Helm
- [ ] Monitoring
- [ ] Projects

---

# Personal Notes

> Add your own observations, troubleshooting tips, interview questions, and commands here as you continue learning Kubernetes.