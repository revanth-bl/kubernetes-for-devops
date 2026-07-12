# DaemonSet

## Overview

A **DaemonSet** is a Kubernetes workload resource that ensures **exactly one Pod runs on every eligible node** in a cluster.

Whenever a new node joins the cluster, Kubernetes automatically schedules a DaemonSet Pod on that node. Likewise, when a node is removed, the corresponding Pod is automatically deleted.

DaemonSets are commonly used for running **system-level services** on every node.

---

# Why Use a DaemonSet?

DaemonSets are ideal for workloads that must run on **every node**, such as:

- Log collection
- Monitoring agents
- Security scanners
- Storage daemons
- Network plugins

Unlike Deployments, you do **not** specify the number of replicas.

---

# How DaemonSet Works

```text
                 Kubernetes Cluster
        -------------------------------------

        Control Plane

              │
              ▼

          DaemonSet

              │
     -------------------------
     │          │            │
     ▼          ▼            ▼

 Worker 1   Worker 2    Worker 3
     │          │            │
     ▼          ▼            ▼

 Log Agent  Log Agent   Log Agent
```

Every Worker Node automatically receives one DaemonSet Pod.

---

# Features

- One Pod per node
- Automatic scheduling on new nodes
- Automatic removal from deleted nodes
- No replica management required
- Ideal for infrastructure services

---

# DaemonSet YAML Example

```yaml
apiVersion: apps/v1
kind: DaemonSet

metadata:
  name: fluentd-daemonset

spec:
  selector:
    matchLabels:
      app: fluentd

  template:
    metadata:
      labels:
        app: fluentd

    spec:
      containers:
      - name: fluentd
        image: fluent/fluentd:v1.16
```

Deploy:

```bash
kubectl apply -f daemonset.yaml
```

---

# Verify DaemonSet

View DaemonSets:

```bash
kubectl get daemonsets
```

Example:

```text
NAME                 DESIRED   CURRENT   READY
fluentd-daemonset       3         3         3
```

---

View Pods:

```bash
kubectl get pods
```

---

Describe DaemonSet:

```bash
kubectl describe daemonset fluentd-daemonset
```

---

# Common Commands

Create:

```bash
kubectl apply -f daemonset.yaml
```

List:

```bash
kubectl get daemonsets
```

Describe:

```bash
kubectl describe daemonset fluentd-daemonset
```

Delete:

```bash
kubectl delete daemonset fluentd-daemonset
```

Edit:

```bash
kubectl edit daemonset fluentd-daemonset
```

---

# Real-World Use Cases

## Log Collection

Install Fluentd or Fluent Bit on every node.

```
Node 1 → Fluentd
Node 2 → Fluentd
Node 3 → Fluentd
```

---

## Monitoring

Run monitoring agents such as:

- Prometheus Node Exporter
- Datadog Agent
- New Relic Agent

---

## Networking

Deploy network plugins like:

- Calico
- Cilium
- Weave Net

---

## Security

Run security tools such as:

- Falco
- CrowdStrike
- Security scanners

---

# DaemonSet vs Deployment

| Feature | DaemonSet | Deployment |
|----------|-----------|------------|
| Pod per Node | Yes | No |
| Replica Count | Automatic | User-defined |
| Scaling | With Nodes | With Replicas |
| Best For | Infrastructure Services | Applications |
| Production Applications | Rarely | Yes |

---

# Advantages

- Automatic deployment on every node
- Easy management of node-level services
- Scales automatically as nodes are added or removed
- No manual replica management
- Highly suitable for infrastructure workloads

---

# Limitations

- Not intended for regular application deployments
- Cannot control the number of Pods with replicas
- Every eligible node runs one Pod, which may consume additional resources

---

# Best Practices

- Use DaemonSets only for node-level services.
- Keep DaemonSet containers lightweight.
- Monitor resource usage on each node.
- Use labels and node selectors when targeting specific nodes.
- Combine DaemonSets with tolerations if Pods should run on control plane nodes.

---

# Key Takeaways

- A DaemonSet ensures one Pod runs on every eligible node.
- New nodes automatically receive a DaemonSet Pod.
- Removing a node removes its corresponding Pod.
- DaemonSets are commonly used for logging, monitoring, networking, and security.
- They are not a replacement for Deployments.

---

# References

- Kubernetes Official Documentation
- Kubernetes DaemonSet Documentation
- CNCF Kubernetes Concepts