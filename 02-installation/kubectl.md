# Installing kubectl

## Overview

**kubectl** is the official command-line tool used to communicate with Kubernetes clusters.

It allows users to deploy applications, inspect cluster resources, troubleshoot workloads, and manage Kubernetes objects.

After installation, `kubectl` communicates with the Kubernetes **API Server** using a configuration file called **kubeconfig**.

---

# Prerequisites

Before installing `kubectl`, ensure that:

- You have administrator/root privileges.
- Internet connectivity is available.
- A Kubernetes cluster is available (optional for installation but required for cluster operations).

---

# Installing kubectl on Windows

## Using Chocolatey

```powershell
choco install kubernetes-cli
```

---

## Using Scoop

```powershell
scoop install kubectl
```

---

## Using Winget

```powershell
winget install -e --id Kubernetes.kubectl
```

---

# Installing kubectl on macOS

Using Homebrew:

```bash
brew install kubectl
```

Verify installation:

```bash
kubectl version --client
```

---

# Installing kubectl on Ubuntu/Debian

Update package index:

```bash
sudo apt update
```

Install kubectl:

```bash
sudo apt install -y kubectl
```

Verify installation:

```bash
kubectl version --client
```

---

# Verify Installation

Check the installed version:

```bash
kubectl version --client
```

Example output:

```text
Client Version: v1.30.x
Kustomize Version: v5.x.x
```

---

# Configure kubectl

After installing kubectl, configure it to communicate with a Kubernetes cluster.

The configuration is stored in a **kubeconfig** file.

---

# Default kubeconfig Location

### Linux/macOS

```text
~/.kube/config
```

### Windows

```text
C:\Users\<username>\.kube\config
```

---

# Configure Access

Create the configuration directory:

```bash
mkdir -p ~/.kube
```

Copy the cluster configuration:

```bash
cp /etc/kubernetes/admin.conf ~/.kube/config
```

Adjust permissions:

```bash
chmod 600 ~/.kube/config
```

> **Note:** On Windows, the kubeconfig file is typically copied to:
>
> `C:\Users\<username>\.kube\config`

---

# Test Cluster Connectivity

Check cluster information:

```bash
kubectl cluster-info
```

View nodes:

```bash
kubectl get nodes
```

Expected output:

```text
NAME              STATUS   ROLES           AGE
control-plane     Ready    control-plane   10m
worker-1          Ready    <none>          8m
```

---

# Managing Contexts

View current context:

```bash
kubectl config current-context
```

List all contexts:

```bash
kubectl config get-contexts
```

Switch context:

```bash
kubectl config use-context <context-name>
```

View configuration:

```bash
kubectl config view
```

---

# Environment Variables

Specify a custom kubeconfig file:

### Linux/macOS

```bash
export KUBECONFIG=/path/to/config
```

### Windows PowerShell

```powershell
$env:KUBECONFIG="C:\path\to\config"
```

---

# Common Verification Commands

Cluster information:

```bash
kubectl cluster-info
```

List nodes:

```bash
kubectl get nodes
```

List namespaces:

```bash
kubectl get namespaces
```

List Pods:

```bash
kubectl get pods -A
```

---

# Troubleshooting

## kubectl: command not found

Check installation:

```bash
kubectl version --client
```

Verify that kubectl is included in the system PATH.

---

## Unable to connect to the server

Check cluster status:

```bash
kubectl cluster-info
```

Verify the kubeconfig file:

```bash
kubectl config view
```

Confirm the current context:

```bash
kubectl config current-context
```

---

## No resources found

Verify the namespace:

```bash
kubectl get pods -A
```

---

# Best Practices

- Keep kubectl updated to a version compatible with your Kubernetes cluster.
- Store kubeconfig files securely.
- Avoid sharing kubeconfig files publicly.
- Use contexts when working with multiple clusters.
- Verify the active context before making changes.

---

# Key Takeaways

- `kubectl` is the official Kubernetes command-line tool.
- It communicates with the Kubernetes API Server.
- The kubeconfig file stores cluster connection details.
- Multiple clusters can be managed using contexts.
- Proper configuration is essential for cluster administration.

---

# References

- Kubernetes Official Documentation
- kubectl Installation Guide
- Kubernetes Command Reference