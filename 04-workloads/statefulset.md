# StatefulSet

## Overview

A **StatefulSet** is a Kubernetes workload resource designed to manage **stateful applications**.

Unlike a Deployment, which creates interchangeable Pods, a StatefulSet provides each Pod with:

- A unique identity
- Stable network hostname
- Persistent storage
- Ordered deployment and termination

StatefulSets are commonly used for applications where data persistence and stable identities are essential.

---

# Why Use a StatefulSet?

StatefulSets are ideal for applications such as:

- MySQL
- PostgreSQL
- MongoDB
- Cassandra
- Kafka
- Elasticsearch
- Redis Cluster
- ZooKeeper

These applications require persistent data and predictable Pod identities.

---

# How StatefulSet Works

```text
                 StatefulSet
                      │
          --------------------------
          │            │           │
          ▼            ▼           ▼
       Pod-0        Pod-1       Pod-2
          │            │           │
          ▼            ▼           ▼
        PVC-0        PVC-1       PVC-2
          │            │           │
          ▼            ▼           ▼
        PV-0         PV-1        PV-2
```

Each Pod gets:

- A unique name (`pod-0`, `pod-1`, etc.)
- Its own PersistentVolumeClaim (PVC)
- Stable storage that survives Pod restarts

---

# Features

- Stable Pod names
- Stable network identity
- Persistent storage
- Ordered deployment
- Ordered scaling
- Ordered termination
- Self-healing

---

# StatefulSet YAML Example

```yaml
apiVersion: apps/v1
kind: StatefulSet

metadata:
  name: nginx-statefulset

spec:
  serviceName: "nginx"

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

        volumeMounts:
        - name: nginx-storage
          mountPath: /usr/share/nginx/html

  volumeClaimTemplates:
  - metadata:
      name: nginx-storage

    spec:
      accessModes:
      - ReadWriteOnce

      resources:
        requests:
          storage: 1Gi
```

Deploy:

```bash
kubectl apply -f statefulset.yaml
```

---

# Verify StatefulSet

View StatefulSets:

```bash
kubectl get statefulsets
```

or

```bash
kubectl get sts
```

Example:

```text
NAME                  READY   AGE
nginx-statefulset     3/3     5m
```

View Pods:

```bash
kubectl get pods
```

Example:

```text
nginx-statefulset-0
nginx-statefulset-1
nginx-statefulset-2
```

Describe StatefulSet:

```bash
kubectl describe statefulset nginx-statefulset
```

---

# Scaling a StatefulSet

Increase replicas:

```bash
kubectl scale statefulset nginx-statefulset --replicas=5
```

Reduce replicas:

```bash
kubectl scale statefulset nginx-statefulset --replicas=2
```

Pods are created and removed in order.

---

# Ordered Deployment

Pods are created sequentially.

```text
Step 1 → Pod-0

↓

Step 2 → Pod-1

↓

Step 3 → Pod-2
```

Kubernetes waits until each Pod is ready before creating the next one.

---

# Ordered Termination

Pods are deleted in reverse order.

```text
Pod-2

↓

Pod-1

↓

Pod-0
```

This prevents data corruption and ensures safe shutdown.

---

# Persistent Storage

Each Pod gets its own PersistentVolumeClaim.

Example:

```text
Pod-0 → PVC-0 → PV-0

Pod-1 → PVC-1 → PV-1

Pod-2 → PVC-2 → PV-2
```

Even if a Pod is recreated, it reconnects to its original storage.

---

# Stable Network Identity

Each Pod has a predictable DNS name.

Example:

```text
nginx-statefulset-0.nginx.default.svc.cluster.local

nginx-statefulset-1.nginx.default.svc.cluster.local

nginx-statefulset-2.nginx.default.svc.cluster.local
```

This allows applications in the cluster to reliably communicate with specific Pods.

---

# Common Commands

Create:

```bash
kubectl apply -f statefulset.yaml
```

View:

```bash
kubectl get sts
```

Describe:

```bash
kubectl describe sts nginx-statefulset
```

Scale:

```bash
kubectl scale sts nginx-statefulset --replicas=5
```

Delete:

```bash
kubectl delete sts nginx-statefulset
```

---

# StatefulSet vs Deployment

| Feature | StatefulSet | Deployment |
|----------|-------------|------------|
| Stable Pod Names | ✅ | ❌ |
| Persistent Storage | ✅ | Optional |
| Ordered Deployment | ✅ | ❌ |
| Ordered Termination | ✅ | ❌ |
| Rolling Updates | ✅ | ✅ |
| Best For | Databases | Stateless Applications |

---

# StatefulSet vs ReplicaSet

| Feature | StatefulSet | ReplicaSet |
|----------|-------------|------------|
| Stable Identity | ✅ | ❌ |
| Persistent Volumes | ✅ | ❌ |
| Ordered Operations | ✅ | ❌ |
| Self-Healing | ✅ | ✅ |
| Scaling | ✅ | ✅ |

---

# Advantages

- Stable Pod identity
- Persistent storage
- Predictable networking
- Ordered deployment
- Ordered scaling
- Suitable for databases and distributed systems

---

# Limitations

- More complex than Deployments
- Requires Persistent Volumes
- Slower scaling due to ordered operations
- Not suitable for stateless applications

---

# Best Practices

- Use StatefulSets only for stateful applications.
- Use Persistent Volumes for durable storage.
- Create a Headless Service for stable network identities.
- Regularly back up application data.
- Monitor storage utilization and Pod health.
- Use Deployments instead of StatefulSets for stateless applications.

---

# Key Takeaways

- A StatefulSet manages applications that require stable identities and persistent storage.
- Each Pod has a unique hostname and its own PersistentVolumeClaim.
- Pods are created and deleted in a predictable order.
- StatefulSets are ideal for databases, messaging systems, and distributed applications.
- Deployments should be used for stateless applications, while StatefulSets are reserved for stateful workloads.

---

# References

- Kubernetes Official Documentation
- Kubernetes StatefulSet Documentation
- CNCF Kubernetes Concepts