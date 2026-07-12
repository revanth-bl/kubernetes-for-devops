# kubectl

## Overview

**kubectl** is the official command-line tool for interacting with Kubernetes clusters.

It allows users to deploy applications, inspect resources, manage workloads, troubleshoot issues, and administer Kubernetes clusters.

Every command executed using `kubectl` communicates with the **Kubernetes API Server**, which processes the request and updates the cluster state.

---

# How kubectl Works

```
User
   │
   ▼
kubectl
   │
   ▼
Kubernetes API Server
   │
   ▼
Control Plane
   │
   ▼
Worker Nodes
```

---

# Features of kubectl

- Deploy applications
- Create Kubernetes resources
- Update existing resources
- Delete resources
- View cluster information
- Debug applications
- Scale workloads
- Roll back deployments
- Execute commands inside containers
- View logs

---

# Installing kubectl

## Windows

Using Chocolatey:

```powershell
choco install kubernetes-cli
```

Using Scoop:

```powershell
scoop install kubectl
```

---

## macOS

Using Homebrew:

```bash
brew install kubectl
```

---

## Ubuntu/Debian

```bash
sudo apt update
sudo apt install kubectl
```

---

# Verify Installation

```bash
kubectl version --client
```

Example output:

```text
Client Version: v1.30.x
Kustomize Version: v5.x.x
```

---

# Kubernetes Configuration File

kubectl uses a configuration file called **kubeconfig**.

Default location:

### Linux/macOS

```text
~/.kube/config
```

### Windows

```text
C:\Users\<username>\.kube\config
```

The kubeconfig file stores:

- Cluster information
- User credentials
- Authentication details
- Contexts

---

# Common kubectl Commands

## Cluster Information

View cluster details:

```bash
kubectl cluster-info
```

---

View Kubernetes version:

```bash
kubectl version
```

---

View all nodes:

```bash
kubectl get nodes
```

---

## Pod Commands

List Pods:

```bash
kubectl get pods
```

List Pods with more details:

```bash
kubectl get pods -o wide
```

Describe a Pod:

```bash
kubectl describe pod <pod-name>
```

Delete a Pod:

```bash
kubectl delete pod <pod-name>
```

---

## Deployment Commands

List Deployments:

```bash
kubectl get deployments
```

Create a Deployment:

```bash
kubectl create deployment nginx --image=nginx
```

Scale a Deployment:

```bash
kubectl scale deployment nginx --replicas=3
```

Restart a Deployment:

```bash
kubectl rollout restart deployment nginx
```

Check rollout status:

```bash
kubectl rollout status deployment nginx
```

Undo a rollout:

```bash
kubectl rollout undo deployment nginx
```

---

## Service Commands

View Services:

```bash
kubectl get services
```

Describe a Service:

```bash
kubectl describe service <service-name>
```

Delete a Service:

```bash
kubectl delete service <service-name>
```

---

## Namespace Commands

View namespaces:

```bash
kubectl get namespaces
```

Create a namespace:

```bash
kubectl create namespace dev
```

Delete a namespace:

```bash
kubectl delete namespace dev
```

---

## Logs

View Pod logs:

```bash
kubectl logs <pod-name>
```

Follow logs in real time:

```bash
kubectl logs -f <pod-name>
```

---

## Execute Commands Inside a Pod

```bash
kubectl exec -it <pod-name> -- /bin/bash
```

If Bash is unavailable:

```bash
kubectl exec -it <pod-name> -- sh
```

---

## Apply Configuration Files

Create resources:

```bash
kubectl apply -f deployment.yaml
```

Apply all YAML files in a folder:

```bash
kubectl apply -f .
```

---

## Delete Resources

Delete using a YAML file:

```bash
kubectl delete -f deployment.yaml
```

Delete all Pods:

```bash
kubectl delete pods --all
```

---

# Output Formats

Default output:

```bash
kubectl get pods
```

Wide output:

```bash
kubectl get pods -o wide
```

YAML output:

```bash
kubectl get pod nginx -o yaml
```

JSON output:

```bash
kubectl get pod nginx -o json
```

---

# Useful Flags

| Flag | Description |
|------|-------------|
| `-A` | All namespaces |
| `-n` | Specify namespace |
| `-o` | Output format |
| `-f` | Read from file |
| `--watch` | Watch changes |
| `--dry-run=client` | Preview without creating resources |
| `--force` | Force deletion |
| `--selector` | Filter by label |

Example:

```bash
kubectl get pods -A
```

---

# Best Practices

- Use `kubectl apply` for declarative resource management.
- Store YAML manifests in version control (Git).
- Use namespaces to organize workloads.
- Verify changes before applying them.
- Use labels and selectors consistently.
- Avoid deleting resources directly in production without verification.

---

# Troubleshooting

### Cannot connect to the cluster

```bash
kubectl cluster-info
```

---

### Check current context

```bash
kubectl config current-context
```

---

### View available contexts

```bash
kubectl config get-contexts
```

---

### Switch context

```bash
kubectl config use-context <context-name>
```

---

### View cluster configuration

```bash
kubectl config view
```

---

# Key Takeaways

- `kubectl` is the primary command-line tool for Kubernetes.
- It communicates with the Kubernetes API Server.
- It is used to create, manage, inspect, and delete Kubernetes resources.
- It supports declarative management using YAML manifests.
- Understanding `kubectl` is essential for Kubernetes administration and troubleshooting.

---

# References

- Kubernetes Official Documentation
- kubectl Command Reference
- Kubernetes Concepts Guide