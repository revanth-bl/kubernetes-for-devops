# ReplicaSet

## Overview

A **ReplicaSet** is a Kubernetes workload resource responsible for ensuring that a specified number of identical Pod replicas are running at all times.

If a Pod fails, crashes, or is deleted, the ReplicaSet automatically creates a new Pod to maintain the desired state.

Although ReplicaSets can be used directly, they are most commonly managed by **Deployments** in production environments.

---

# Why Use a ReplicaSet?

ReplicaSets help achieve:

- High Availability
- Self-Healing
- Automatic Pod Replacement
- Consistent Number of Running Pods
- Fault Tolerance

---

# How ReplicaSet Works

```text
              ReplicaSet
                   │
      Desired Replicas = 3
                   │
       -------------------------
       │           │           │
       ▼           ▼           ▼
     Pod 1       Pod 2       Pod 3

If Pod 2 fails...

              ReplicaSet
                   │
                   ▼
           Creates New Pod

       -------------------------
       │           │           │
       ▼           ▼           ▼
     Pod 1      New Pod      Pod 3
```

ReplicaSet continuously compares the **desired state** with the **current state** and creates or removes Pods as needed.

---

# Features

- Maintains a desired number of Pods
- Automatically replaces failed Pods
- Supports scaling
- Uses label selectors
- Provides self-healing

---

# ReplicaSet YAML Example

```yaml
apiVersion: apps/v1
kind: ReplicaSet

metadata:
  name: nginx-replicaset

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

Deploy the ReplicaSet:

```bash
kubectl apply -f replicaset.yaml
```

---

# Verify ReplicaSet

View ReplicaSets:

```bash
kubectl get replicasets
```

Example:

```text
NAME                 DESIRED   CURRENT   READY
nginx-replicaset         3         3         3
```

View Pods:

```bash
kubectl get pods
```

Describe ReplicaSet:

```bash
kubectl describe replicaset nginx-replicaset
```

---

# Scaling a ReplicaSet

Increase replicas:

```bash
kubectl scale replicaset nginx-replicaset --replicas=5
```

Decrease replicas:

```bash
kubectl scale replicaset nginx-replicaset --replicas=2
```

Verify:

```bash
kubectl get replicasets
```

---

# Self-Healing

If a Pod is deleted manually:

```bash
kubectl delete pod <pod-name>
```

The ReplicaSet automatically creates a replacement Pod.

Verify:

```bash
kubectl get pods
```

---

# Label Selectors

ReplicaSets identify Pods using labels.

Example:

```yaml
selector:
  matchLabels:
    app: nginx
```

Pods with the matching label become part of the ReplicaSet.

---

# Common ReplicaSet Commands

Create:

```bash
kubectl apply -f replicaset.yaml
```

View:

```bash
kubectl get replicasets
```

Describe:

```bash
kubectl describe replicaset nginx-replicaset
```

Scale:

```bash
kubectl scale replicaset nginx-replicaset --replicas=5
```

Delete:

```bash
kubectl delete replicaset nginx-replicaset
```

Edit:

```bash
kubectl edit replicaset nginx-replicaset
```

---

# ReplicaSet vs ReplicationController

| Feature | ReplicaSet | ReplicationController |
|----------|------------|-----------------------|
| Label Selectors | Set-based & Equality-based | Equality-based only |
| Current Standard | ✅ Yes | ❌ Legacy |
| Used in Deployments | ✅ Yes | ❌ No |
| Recommended | ✅ Yes | ❌ No |

---

# ReplicaSet vs Deployment

| Feature | ReplicaSet | Deployment |
|----------|------------|------------|
| Maintains Pod Replicas | ✅ | ✅ |
| Rolling Updates | ❌ | ✅ |
| Rollbacks | ❌ | ✅ |
| Version History | ❌ | ✅ |
| Production Recommendation | Rarely | ✅ |

ReplicaSets are primarily managed by Deployments rather than being created directly.

---

# Advantages

- Automatic Pod recovery
- High availability
- Easy horizontal scaling
- Ensures desired state
- Improves application reliability

---

# Limitations

- No rolling updates
- No rollback support
- No deployment history
- Usually replaced by Deployments in production

---

# Best Practices

- Use Deployments instead of creating ReplicaSets directly.
- Apply meaningful labels to Pods.
- Monitor ReplicaSet health regularly.
- Store YAML manifests in version control.
- Scale ReplicaSets based on application demand.

---

# Key Takeaways

- A ReplicaSet ensures a specified number of Pod replicas are always running.
- It automatically replaces failed or deleted Pods.
- ReplicaSets use label selectors to manage Pods.
- They provide self-healing and scaling capabilities.
- In production, ReplicaSets are typically created and managed by Deployments.

---

# References

- Kubernetes Official Documentation
- Kubernetes ReplicaSet Documentation
- CNCF Kubernetes Concepts