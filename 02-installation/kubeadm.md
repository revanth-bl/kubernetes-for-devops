# kubeadm

## Overview

**kubeadm** is an official Kubernetes tool used to bootstrap and configure Kubernetes clusters.

Unlike **Minikube** and **KIND**, which are mainly intended for local development and testing, kubeadm is designed to create **production-ready Kubernetes clusters** on physical or virtual machines.

It simplifies the process of setting up the Kubernetes Control Plane and joining Worker Nodes to the cluster.

---

# Why Use kubeadm?

kubeadm is commonly used to:

- Create production Kubernetes clusters
- Build on-premises Kubernetes environments
- Learn Kubernetes cluster administration
- Configure multi-node clusters
- Prepare clusters for enterprise deployments

---

# How kubeadm Works

```
                Administrator
                      │
                      ▼
                  kubeadm init
                      │
                      ▼
            Creates Control Plane
                      │
          --------------------------
          │                        │
          ▼                        ▼
     Worker Node 1            Worker Node 2
          │                        │
          └────── kubeadm join ────┘
                      │
                      ▼
              Kubernetes Cluster
```

The Control Plane is initialized using `kubeadm init`, and additional Worker Nodes join the cluster using `kubeadm join`.

---

# Prerequisites

Before installing Kubernetes with kubeadm, ensure the following:

- Linux operating system
- Root or sudo privileges
- At least 2 CPUs
- Minimum 2 GB RAM
- Container Runtime (containerd or CRI-O)
- Stable network connectivity
- Swap disabled
- Unique hostname for each node

---

# Install Required Packages

Update the package index:

```bash
sudo apt update
```

Install required packages:

```bash
sudo apt install -y apt-transport-https ca-certificates curl
```

---

# Install Container Runtime

Kubernetes requires a Container Runtime.

A common choice is **containerd**.

```bash
sudo apt install -y containerd
```

Verify:

```bash
containerd --version
```

---

# Install kubeadm, kubelet, and kubectl

```bash
sudo apt update

sudo apt install -y kubelet kubeadm kubectl
```

Prevent automatic updates:

```bash
sudo apt-mark hold kubelet kubeadm kubectl
```

---

# Initialize the Control Plane

Run the following command on the Control Plane node:

```bash
sudo kubeadm init
```

Example output:

```text
Your Kubernetes control-plane has initialized successfully.
```

At the end of the installation, kubeadm displays a **join command** that Worker Nodes will use.

---

# Configure kubectl

After initialization:

```bash
mkdir -p $HOME/.kube

sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Verify:

```bash
kubectl get nodes
```

---

# Join Worker Nodes

Run the join command displayed during initialization.

Example:

```bash
sudo kubeadm join 192.168.1.10:6443 \
--token <token> \
--discovery-token-ca-cert-hash sha256:<hash>
```

After joining:

```bash
kubectl get nodes
```

---

# Verify Cluster

Check nodes:

```bash
kubectl get nodes
```

Check Pods:

```bash
kubectl get pods -A
```

Cluster information:

```bash
kubectl cluster-info
```

---

# Reset a Cluster

If needed, reset a node:

```bash
sudo kubeadm reset
```

---

# Common kubeadm Commands

Initialize cluster:

```bash
sudo kubeadm init
```

Join cluster:

```bash
sudo kubeadm join
```

Reset node:

```bash
sudo kubeadm reset
```

Upgrade cluster:

```bash
sudo kubeadm upgrade plan
```

Apply upgrade:

```bash
sudo kubeadm upgrade apply
```

---

# Advantages of kubeadm

- Official Kubernetes installation tool
- Production-ready
- Supports multi-node clusters
- Easy to bootstrap Kubernetes
- Flexible and customizable
- Widely used in enterprise environments

---

# Limitations

- Linux only
- Manual configuration required
- Higher learning curve than Minikube or KIND
- Requires networking and container runtime setup

---

# kubeadm vs KIND vs Minikube

| Feature | kubeadm | KIND | Minikube |
|----------|----------|------|-----------|
| Production Ready | ✅ Yes | ❌ No | ❌ No |
| Multi-node Support | ✅ Yes | ✅ Yes | Limited |
| Uses Docker Containers | ❌ No | ✅ Yes | Optional |
| Uses Virtual Machines | Optional | ❌ No | Usually Yes |
| Best For | Production | CI/CD & Testing | Learning |

---

# Best Practices

- Use containerd as the container runtime.
- Disable swap before installation.
- Use separate Control Plane and Worker Nodes.
- Back up etcd regularly.
- Keep Kubernetes components updated.
- Install a Container Network Interface (CNI) plugin after cluster initialization.

---

# Key Takeaways

- **kubeadm** is the official tool for creating Kubernetes clusters.
- It is designed for production and enterprise environments.
- `kubeadm init` initializes the Control Plane.
- `kubeadm join` adds Worker Nodes to the cluster.
- kubeadm simplifies Kubernetes installation while allowing flexibility for advanced configurations.

---

# References

- Kubernetes Official Documentation
- kubeadm Documentation
- CNCF Kubernetes Documentation