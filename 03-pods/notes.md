# Pod Notes

## Quick Revision

- A **Pod** is the smallest deployable unit in Kubernetes.
- A Pod can contain **one or more containers**.
- Containers inside a Pod share:
  - Network namespace
  - IP Address
  - Storage Volumes
- Pods are **ephemeral**, meaning they can be recreated if they fail.
- Pods are usually managed by **Deployments**, not created directly in production.

---

# Pod Lifecycle

```
Pending
    │
    ▼
ContainerCreating
    │
    ▼
Running
   ├────────► Succeeded
   └────────► Failed
          │
          ▼
     Terminating
          │
          ▼
       Deleted
```

---

# Pod Phases

| Phase | Description |
|--------|-------------|
| Pending | Waiting to be scheduled |
| Running | Pod is running successfully |
| Succeeded | All containers completed successfully |
| Failed | One or more containers failed |
| Unknown | Kubernetes cannot determine Pod status |

---

# Restart Policies

| Policy | Description |
|---------|-------------|
| Always | Restart container whenever it exits |
| OnFailure | Restart only if the container fails |
| Never | Never restart the container |

---

# Basic Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
```

Deploy:

```bash
kubectl apply -f pod.yaml
```

---

# Common kubectl Commands

Create Pod

```bash
kubectl apply -f pod.yaml
```

View Pods

```bash
kubectl get pods
```

Detailed Information

```bash
kubectl describe pod <pod-name>
```

View Logs

```bash
kubectl logs <pod-name>
```

Execute Command

```bash
kubectl exec -it <pod-name> -- bash
```

Delete Pod

```bash
kubectl delete pod <pod-name>
```

Port Forward

```bash
kubectl port-forward pod/<pod-name> 8080:80
```

Watch Pods

```bash
kubectl get pods --watch
```

---

# Useful Output Options

Wide Output

```bash
kubectl get pods -o wide
```

YAML Output

```bash
kubectl get pod <pod-name> -o yaml
```

JSON Output

```bash
kubectl get pod <pod-name> -o json
```

---

# Common Pod Status

| Status | Meaning |
|---------|----------|
| Running | Application is running |
| Pending | Waiting for resources |
| ContainerCreating | Image is being prepared |
| Completed | Job finished successfully |
| CrashLoopBackOff | Container repeatedly crashes |
| ImagePullBackOff | Unable to pull image |
| ErrImagePull | Image download failed |
| Terminating | Pod is shutting down |

---

# Common Errors

## CrashLoopBackOff

Possible causes:

- Application crashes
- Missing environment variables
- Incorrect configuration
- Insufficient resources

Check:

```bash
kubectl logs <pod-name>
kubectl describe pod <pod-name>
```

---

## ImagePullBackOff

Possible causes:

- Incorrect image name
- Image does not exist
- Private registry authentication failure
- Network issue

Check:

```bash
kubectl describe pod <pod-name>
```

---

## Pending

Possible causes:

- No available Worker Nodes
- CPU/Memory shortage
- PVC not bound
- Node selector mismatch

Check:

```bash
kubectl get events
```

---

# Pod Best Practices

- Keep one main application per Pod.
- Use Deployments instead of standalone Pods.
- Define CPU and memory requests/limits.
- Add readiness and liveness probes.
- Use labels for grouping resources.
- Store Pod manifests in version control.
- Avoid editing Pods directly in production.

---

# Interview Questions

### What is a Pod?

A Pod is the smallest deployable unit in Kubernetes that can contain one or more containers.

---

### Can a Pod contain multiple containers?

Yes. Multiple containers can run inside the same Pod and share networking and storage.

---

### Why are Pods considered ephemeral?

Pods are temporary. If a Pod fails, Kubernetes replaces it instead of repairing it.

---

### What is the difference between a Pod and a Container?

| Pod | Container |
|-----|-----------|
| Kubernetes object | Application runtime |
| Can contain multiple containers | Runs a single application/process |
| Shares networking and storage | Isolated execution environment |

---

### Why shouldn't standalone Pods be used in production?

Because they are not automatically recreated if deleted or if a node fails. Deployments and ReplicaSets provide self-healing and scaling.

---

# Learning Progress

- [x] Pod Basics
- [x] Pod Lifecycle
- [x] Pod YAML
- [x] kubectl Commands
- [ ] ReplicaSets
- [ ] Deployments
- [ ] Services
- [ ] ConfigMaps
- [ ] Secrets

---

# Personal Notes

> Use this section to record commands, troubleshooting tips, interview questions, and observations as you continue learning Kubernetes.