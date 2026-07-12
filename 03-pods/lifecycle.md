# Pod Lifecycle

## Overview

A **Pod** is the smallest deployable unit in Kubernetes. Every Pod goes through a series of states from creation until termination. This process is known as the **Pod Lifecycle**.

Understanding the Pod lifecycle is essential for troubleshooting applications and designing reliable Kubernetes workloads.

---

# Pod Lifecycle Diagram

```text
                User Creates Pod
                       │
                       ▼
                Pending State
                       │
                       ▼
                Container Creating
                       │
                       ▼
                 Running State
                 /           \
                /             \
         Succeeded         Failed
                \             /
                 \           /
                  Terminating
                       │
                       ▼
                    Deleted
```

---

# Pod Lifecycle Phases

Kubernetes defines five primary Pod phases.

| Phase | Description |
|--------|-------------|
| Pending | Pod has been accepted but is not yet running. |
| Running | Pod is successfully running on a node. |
| Succeeded | All containers completed successfully. |
| Failed | One or more containers terminated with an error. |
| Unknown | Kubernetes cannot determine the Pod's state. |

---

# 1. Pending

The Pod has been created but is waiting to be scheduled.

Common reasons:

- No available Worker Nodes
- Image is still downloading
- Persistent Volume not available
- Scheduling constraints
- Insufficient CPU or Memory

Check status:

```bash
kubectl get pods
```

Describe Pod:

```bash
kubectl describe pod <pod-name>
```

---

# 2. Container Creating

During this stage Kubernetes prepares the Pod.

Operations include:

- Pulling container images
- Creating network interfaces
- Mounting volumes
- Initializing containers

This state appears in:

```bash
kubectl get pods
```

Example:

```text
STATUS
ContainerCreating
```

---

# 3. Running

The Pod has been successfully scheduled and all containers are running.

Example:

```text
NAME      READY   STATUS
nginx     1/1     Running
```

Verify:

```bash
kubectl get pods
```

View logs:

```bash
kubectl logs <pod-name>
```

---

# 4. Succeeded

All containers finished successfully.

Usually seen with:

- Jobs
- Batch processing
- One-time scripts

Example:

```text
STATUS
Completed
```

---

# 5. Failed

The Pod terminated because one or more containers exited with an error.

Possible causes:

- Application crash
- Missing files
- Invalid configuration
- Resource exhaustion
- Image issues

Check details:

```bash
kubectl describe pod <pod-name>
```

View logs:

```bash
kubectl logs <pod-name>
```

---

# 6. Unknown

The Kubernetes Control Plane cannot determine the Pod's status.

Possible reasons:

- Node unavailable
- Network failure
- Kubelet not responding

---

# Pod Conditions

A Pod also reports conditions that describe its health.

| Condition | Meaning |
|------------|----------|
| PodScheduled | Pod assigned to a node |
| Initialized | Init containers completed |
| ContainersReady | All containers are ready |
| Ready | Pod is ready to receive traffic |

Check conditions:

```bash
kubectl describe pod <pod-name>
```

---

# Container Restart Policy

A Pod defines how containers should restart after they exit.

## Always

Container restarts whenever it exits.

Used by:

- Deployments
- ReplicaSets

---

## OnFailure

Restart only if the container exits with an error.

Used by:

- Jobs

---

## Never

Do not restart the container.

Used for debugging or one-time tasks.

Example:

```yaml
restartPolicy: Never
```

---

# Pod Termination Process

When a Pod is deleted:

1. Kubernetes sends a SIGTERM signal.
2. The application performs graceful shutdown.
3. Kubernetes waits for the termination grace period.
4. If the container is still running, Kubernetes sends SIGKILL.
5. Resources are released.

Delete a Pod:

```bash
kubectl delete pod <pod-name>
```

---

# Common Pod Status Values

| Status | Meaning |
|---------|----------|
| Pending | Waiting to start |
| ContainerCreating | Preparing the container |
| Running | Application is running |
| Completed | Finished successfully |
| Error | Application failed |
| CrashLoopBackOff | Repeated container crashes |
| ImagePullBackOff | Unable to pull container image |
| ErrImagePull | Image download failed |
| Terminating | Pod is shutting down |

---

# Common Lifecycle Problems

## CrashLoopBackOff

Container repeatedly crashes.

Possible causes:

- Application error
- Missing environment variables
- Configuration issues
- Insufficient resources

View logs:

```bash
kubectl logs <pod-name>
```

---

## ImagePullBackOff

Kubernetes cannot download the container image.

Possible causes:

- Incorrect image name
- Missing image
- Registry authentication failure
- Network issues

---

## Pending

Possible causes:

- No available nodes
- Resource shortage
- PVC not bound
- Node selector mismatch

---

# Troubleshooting Commands

Check Pods:

```bash
kubectl get pods
```

Describe Pod:

```bash
kubectl describe pod <pod-name>
```

View logs:

```bash
kubectl logs <pod-name>
```

Watch Pod status:

```bash
kubectl get pods --watch
```

View events:

```bash
kubectl get events
```

---

# Best Practices

- Design containers to start quickly.
- Use readiness and liveness probes.
- Configure resource requests and limits.
- Monitor Pod events regularly.
- Use Deployments instead of standalone Pods for production.
- Investigate Pod failures promptly.

---

# Key Takeaways

- Every Pod moves through a defined lifecycle.
- Kubernetes continuously monitors Pod health.
- The most common phases are Pending, Running, Succeeded, Failed, and Unknown.
- Restart policies determine how containers behave after exiting.
- Understanding the Pod lifecycle is essential for debugging Kubernetes applications.

---

# References

- Kubernetes Official Documentation
- Kubernetes Pod Lifecycle Documentation
- CNCF Kubernetes Concepts