# Metrics Server

## Overview

**Metrics Server** is a lightweight Kubernetes component that collects **resource usage metrics** from Nodes and Pods.

It gathers CPU and memory usage data from each node's **Kubelet** and exposes these metrics through the Kubernetes Metrics API.

Metrics Server is primarily used for:

- `kubectl top`
- Horizontal Pod Autoscaler (HPA)
- Vertical Pod Autoscaler (VPA)
- Resource monitoring

> **Note:** Metrics Server is **not** a long-term monitoring solution. It provides real-time resource usage and does not store historical metrics. For long-term monitoring, tools like **Prometheus** are commonly used.

---

# Why Use Metrics Server?

Without Metrics Server:

```text
Kubernetes Cluster

↓

No Resource Metrics

↓

kubectl top Fails

↓

Autoscaling Doesn't Work
```

Problems:

- Cannot monitor CPU and memory usage
- HPA cannot make scaling decisions
- No quick visibility into resource consumption

---

With Metrics Server:

```text
Kubernetes Nodes

↓

Kubelet

↓

Metrics Server

↓

Metrics API

↓

kubectl top
HPA
VPA
```

Metrics become available throughout the cluster.

---

# Benefits

- Lightweight
- Easy installation
- Real-time CPU and memory metrics
- Enables Horizontal Pod Autoscaler
- Enables Vertical Pod Autoscaler
- Simple resource monitoring

---

# Metrics Server Architecture

```text
             Kubernetes Cluster
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
     Node 1                  Node 2
        │                       │
     Kubelet                 Kubelet
        │                       │
        └───────────┬───────────┘
                    ▼
            Metrics Server
                    │
                    ▼
             Metrics API
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
   kubectl top     HPA         VPA
```

---

# How Metrics Server Works

1. Kubelets collect CPU and memory usage from running Pods.
2. Metrics Server periodically retrieves these metrics from each Kubelet.
3. Metrics are exposed through the Kubernetes Metrics API.
4. Tools like `kubectl top` and the Horizontal Pod Autoscaler use these metrics.

---

# Install Metrics Server

Apply the official manifest:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Verify installation:

```bash
kubectl get pods -n kube-system
```

---

# Verify Metrics Server

View the Metrics Server Pod:

```bash
kubectl get deployment metrics-server -n kube-system
```

Check available APIs:

```bash
kubectl api-resources | grep metrics
```

---

# View Node Metrics

```bash
kubectl top nodes
```

Example Output:

```text
NAME       CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
node-1     250m         12%    1450Mi          42%
```

---

# View Pod Metrics

```bash
kubectl top pods
```

Example Output:

```text
NAME           CPU(cores)   MEMORY(bytes)
nginx-pod      5m           28Mi
redis-pod      12m          80Mi
```

View metrics in a Namespace:

```bash
kubectl top pods -n development
```

---

# Common Commands

View Node Metrics:

```bash
kubectl top nodes
```

View Pod Metrics:

```bash
kubectl top pods
```

View Namespace Metrics:

```bash
kubectl top pods -n default
```

View Metrics Server:

```bash
kubectl get deployment metrics-server -n kube-system
```

Delete Metrics Server:

```bash
kubectl delete -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

---

# Metrics Server vs Prometheus

| Metrics Server | Prometheus |
|----------------|------------|
| Real-time metrics | Long-term monitoring |
| CPU & Memory only | Many types of metrics |
| No historical storage | Stores historical data |
| Supports HPA/VPA | Advanced monitoring and alerting |
| Lightweight | Feature-rich |

---

# Metrics Server vs kube-state-metrics

| Metrics Server | kube-state-metrics |
|----------------|--------------------|
| Resource usage (CPU/Memory) | Kubernetes object state |
| Uses Kubelet metrics | Uses Kubernetes API |
| Enables autoscaling | Enables operational monitoring |

---

# Advantages

- Lightweight and easy to deploy
- Provides real-time resource usage
- Required for Horizontal Pod Autoscaler
- Integrates with `kubectl top`
- Low resource consumption

---

# Limitations

- Does not store historical metrics.
- Collects only CPU and memory metrics.
- Does not provide dashboards or alerting.
- Not intended to replace Prometheus or other monitoring systems.

---

# Best Practices

- Install Metrics Server in every Kubernetes cluster.
- Use it with Horizontal Pod Autoscaler for automatic scaling.
- Combine it with Prometheus and Grafana for comprehensive monitoring.
- Set appropriate CPU and memory requests for workloads to improve autoscaling decisions.
- Regularly verify that metrics are being collected successfully.

---

# Interview Questions

### What is Metrics Server?

Metrics Server is a Kubernetes component that collects real-time CPU and memory usage from Nodes and Pods.

---

### Which command displays Node resource usage?

```bash
kubectl top nodes
```

---

### Which command displays Pod resource usage?

```bash
kubectl top pods
```

---

### Does Metrics Server store historical data?

No. It provides only current resource usage metrics.

---

### Which Kubernetes feature depends on Metrics Server?

**Horizontal Pod Autoscaler (HPA)**

---

# Key Takeaways

- Metrics Server provides real-time CPU and memory usage metrics.
- It enables `kubectl top`, HPA, and VPA.
- It is lightweight and easy to install.
- It does not store historical metrics.
- Metrics Server is commonly used alongside Prometheus and Grafana for complete Kubernetes observability.

---

# References

- Kubernetes Official Documentation
- Metrics Server Documentation
- Kubernetes Horizontal Pod Autoscaler Documentation
- Kubernetes Resource Metrics Pipeline
- CNCF Kubernetes Documentation