# Minikube

## Overview

**Minikube** is an open-source tool that allows you to run a **single-node Kubernetes cluster** on your local machine.

It is designed for learning Kubernetes, developing applications, and testing deployments without requiring a full production cluster.

Minikube creates a local Kubernetes environment using a virtualization driver or container runtime, making it one of the easiest ways to get started with Kubernetes.

---

# Why Use Minikube?

Minikube is commonly used for:

- Learning Kubernetes
- Local application development
- Testing Kubernetes manifests
- Experimenting with Kubernetes features
- Demonstrating Kubernetes concepts

---

# How Minikube Works

```
                 Developer
                      │
                      ▼
               Minikube CLI
                      │
                      ▼
        ----------------------------
        |                          |
 Virtual Machine              Docker Driver
        |                          |
        -------- Kubernetes --------
                 Cluster
                      │
                      ▼
             Control Plane + Worker
```

By default, Minikube creates a **single-node cluster**, where the Control Plane and Worker Node run on the same machine.

---

# Prerequisites

Before installing Minikube, ensure you have:

- Docker, Hyper-V, VirtualBox, or another supported driver
- kubectl installed
- Internet connection
- At least:
  - 2 CPUs
  - 2 GB RAM
  - 20 GB free disk space

---

# Installing Minikube

## Windows

Using Chocolatey:

```powershell
choco install minikube
```

Using Winget:

```powershell
winget install Kubernetes.minikube
```

---

## macOS

Using Homebrew:

```bash
brew install minikube
```

---

## Ubuntu/Debian

Download the latest package:

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

---

# Verify Installation

```bash
minikube version
```

Example:

```text
minikube version: v1.35.x
```

---

# Start a Cluster

Start Minikube using the default driver:

```bash
minikube start
```

Start using Docker:

```bash
minikube start --driver=docker
```

Example output:

```text
😄  minikube v1.35.x
✨  Using the docker driver
🏄  Done! kubectl is now configured.
```

---

# Check Cluster Status

```bash
minikube status
```

Example:

```text
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

---

# Verify Kubernetes Cluster

View cluster information:

```bash
kubectl cluster-info
```

View nodes:

```bash
kubectl get nodes
```

Expected output:

```text
NAME        STATUS   ROLES           AGE
minikube    Ready    control-plane   5m
```

---

# Stop the Cluster

```bash
minikube stop
```

---

# Restart the Cluster

```bash
minikube start
```

---

# Delete the Cluster

```bash
minikube delete
```

---

# Access the Kubernetes Dashboard

Enable the dashboard:

```bash
minikube dashboard
```

This command launches the Kubernetes Dashboard in your default web browser.

---

# SSH into the Minikube Node

```bash
minikube ssh
```

Exit the SSH session:

```bash
exit
```

---

# Enable Add-ons

List available add-ons:

```bash
minikube addons list
```

Enable an add-on:

```bash
minikube addons enable metrics-server
```

Disable an add-on:

```bash
minikube addons disable metrics-server
```

Popular add-ons include:

- Dashboard
- Metrics Server
- Ingress
- Registry
- Storage Provisioner

---

# Common Minikube Commands

Start cluster:

```bash
minikube start
```

Stop cluster:

```bash
minikube stop
```

Delete cluster:

```bash
minikube delete
```

Check status:

```bash
minikube status
```

View logs:

```bash
minikube logs
```

Open dashboard:

```bash
minikube dashboard
```

SSH into node:

```bash
minikube ssh
```

List add-ons:

```bash
minikube addons list
```

---

# Advantages

- Easy to install
- Beginner-friendly
- Ideal for learning Kubernetes
- Supports multiple drivers
- Includes useful add-ons
- Lightweight compared to production clusters
- Cross-platform support

---

# Limitations

- Primarily designed for development and testing
- Default setup is a single-node cluster
- Not intended for production workloads
- Performance depends on local machine resources

---

# Minikube vs KIND vs kubeadm

| Feature | Minikube | KIND | kubeadm |
|----------|-----------|------|----------|
| Primary Use | Learning & Development | Testing & CI/CD | Production |
| Cluster Type | Single-node (default) | Single/Multi-node | Multi-node |
| Uses Docker | Optional | Yes | No |
| Uses Virtual Machine | Yes (or Docker) | No | No |
| Production Ready | No | No | Yes |

---

# Best Practices

- Use Minikube for local development and learning.
- Allocate sufficient CPU and memory for better performance.
- Enable only the add-ons you need.
- Stop the cluster when not in use to conserve system resources.
- Keep Minikube and kubectl updated.

---

# Key Takeaways

- Minikube is a lightweight tool for running Kubernetes locally.
- It creates a single-node Kubernetes cluster by default.
- It is ideal for learning, testing, and application development.
- It integrates seamlessly with kubectl.
- Minikube provides an easy way to explore Kubernetes without requiring multiple machines.

---

# References

- Kubernetes Official Documentation
- Minikube Official Documentation
- CNCF Kubernetes Documentation