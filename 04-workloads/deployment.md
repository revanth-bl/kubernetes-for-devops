# Deployment

## Overview

A **Deployment** is a Kubernetes workload resource used to manage stateless applications.

It provides declarative updates for Pods and ReplicaSets, ensuring that the desired number of application instances are running at all times.

Deployments simplify application lifecycle management by supporting rolling updates, rollbacks, scaling, and self-healing.

---

# Why Use Deployments?

Deployments are the recommended way to deploy applications in Kubernetes because they provide:

- Automatic Pod management
- Self-healing
- Horizontal scaling
- Rolling updates
- Rollbacks
- Zero or minimal downtime deployments

---

# How Deployment Works

```text
                Deployment
                     │
                     ▼
               ReplicaSet
                     │
          -----------------------
          │          │          │
          ▼          ▼          ▼
        Pod 1      Pod 2      Pod 3
```

The Deployment manages a ReplicaSet, and the ReplicaSet ensures the desired number of Pods are running.

---

# Features

- Declarative application management
- Automatic ReplicaSet creation
- Rolling updates
- Rollbacks
- Self-healing
- Horizontal scaling
- Version management

---

# Deployment YAML Example

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment

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

Deploy:

```bash
kubectl apply -f deployment.yaml
```

---

# Verify Deployment

View Deployments:

```bash
kubectl get deployments
```

Example:

```text
NAME                READY   UP-TO-DATE   AVAILABLE
nginx-deployment    3/3     3            3
```

View ReplicaSets:

```bash
kubectl get replicasets
```

View Pods:

```bash
kubectl get pods
```

Describe Deployment:

```bash
kubectl describe deployment nginx-deployment
```

---

# Scaling a Deployment

Increase replicas:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Reduce replicas:

```bash
kubectl scale deployment nginx-deployment --replicas=2
```

Verify:

```bash
kubectl get deployment
```

---

# Updating a Deployment

Update the container image:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.27
```

Check rollout progress:

```bash
kubectl rollout status deployment nginx-deployment
```

---

# Rolling Updates

During a rolling update, Kubernetes gradually replaces old Pods with new ones without bringing down the application.

```text
Old Pods
│
├── Pod v1
├── Pod v1
├── Pod v1

↓

Rolling Update

↓

New Pods
├── Pod v2
├── Pod v2
├── Pod v2
```

Benefits:

- No downtime
- Continuous availability
- Safe application upgrades

---

# Rollback

View rollout history:

```bash
kubectl rollout history deployment nginx-deployment
```

Rollback to the previous version:

```bash
kubectl rollout undo deployment nginx-deployment
```

Rollback to a specific revision:

```bash
kubectl rollout undo deployment nginx-deployment --to-revision=2
```

---

# Common Deployment Commands

Create:

```bash
kubectl apply -f deployment.yaml
```

List:

```bash
kubectl get deployments
```

Describe:

```bash
kubectl describe deployment nginx-deployment
```

Scale:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Restart:

```bash
kubectl rollout restart deployment nginx-deployment
```

Delete:

```bash
kubectl delete deployment nginx-deployment
```

---

# Deployment Strategy

## RollingUpdate (Default)

Pods are updated gradually.

Advantages:

- Minimal downtime
- Safe upgrades
- Easy rollback

---

## Recreate

Deletes all existing Pods before creating new ones.

Advantages:

- Simple strategy

Disadvantage:

- Causes application downtime.

Example:

```yaml
strategy:
  type: Recreate
```

---

# Deployment vs ReplicaSet

| Feature | Deployment | ReplicaSet |
|----------|------------|------------|
| Manages Pods | ✅ | ✅ |
| Creates ReplicaSets | ✅ | ❌ |
| Rolling Updates | ✅ | ❌ |
| Rollbacks | ✅ | ❌ |
| Version History | ✅ | ❌ |
| Recommended for Applications | ✅ | ❌ |

---

# Advantages

- Self-healing
- Easy scaling
- Rolling updates
- Rollbacks
- High availability
- Declarative management
- Production ready

---

# Best Practices

- Use Deployments instead of standalone Pods.
- Store Deployment YAML files in Git.
- Define CPU and memory requests.
- Use readiness and liveness probes.
- Use labels consistently.
- Monitor rollout status after updates.

---

# Key Takeaways

- A Deployment is the recommended way to run stateless applications in Kubernetes.
- Deployments manage ReplicaSets, which in turn manage Pods.
- They support scaling, rolling updates, rollbacks, and self-healing.
- Rolling updates help deploy new versions with little or no downtime.
- Deployments are one of the most commonly used Kubernetes resources in production.

---

# References

- Kubernetes Official Documentation
- Kubernetes Deployment Documentation
- CNCF Kubernetes Concepts