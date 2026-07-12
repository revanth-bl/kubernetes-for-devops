# Kubernetes DNS

## Overview

**DNS (Domain Name System)** in Kubernetes enables Pods and Services to communicate using **human-readable names** instead of IP addresses.

Since Pod IP addresses are dynamic and can change when Pods are recreated, Kubernetes uses DNS to provide stable names for accessing applications and services.

Kubernetes DNS is typically provided by **CoreDNS**, which runs inside the cluster.

---

# Why Kubernetes DNS?

Without DNS:

```
Frontend Pod
      │
      ▼
10.96.45.23
```

If the backend Pod or Service changes its IP address, the frontend application stops working.

With Kubernetes DNS:

```
Frontend Pod
      │
      ▼
backend-service.default.svc.cluster.local
      │
      ▼
10.96.45.23
```

Applications always communicate using the service name, even if the underlying IP changes.

---

# How Kubernetes DNS Works

```text
                  Pod
                   │
                   ▼
         backend-service
                   │
                   ▼
              CoreDNS Server
                   │
                   ▼
          Service Cluster IP
                   │
                   ▼
                Backend Pods
```

CoreDNS resolves service names into ClusterIP addresses.

---

# CoreDNS

**CoreDNS** is the default DNS server in Kubernetes.

Responsibilities include:

- Service name resolution
- Pod name resolution
- DNS forwarding
- External DNS lookups
- Cluster service discovery

Check CoreDNS Pods:

```bash
kubectl get pods -n kube-system
```

Example:

```text
coredns-7d6b6c9f7f-abcde
coredns-7d6b6c9f7f-fghij
```

---

# DNS Naming Convention

A Service DNS name follows this format:

```text
<service-name>.<namespace>.svc.cluster.local
```

Example:

```text
backend.default.svc.cluster.local
```

Components:

| Part | Description |
|------|-------------|
| backend | Service Name |
| default | Namespace |
| svc | Indicates a Kubernetes Service |
| cluster.local | Default Cluster Domain |

---

# Service Discovery

Suppose you have:

```
Frontend Service

Backend Service
```

The frontend application can simply access:

```text
http://backend
```

If both Services are in the same namespace.

For another namespace:

```text
http://backend.production
```

Or use the fully qualified domain name (FQDN):

```text
http://backend.production.svc.cluster.local
```

---

# Pod DNS

Each Pod also receives a DNS configuration.

Inside a Pod:

```bash
cat /etc/resolv.conf
```

Example:

```text
nameserver 10.96.0.10
search default.svc.cluster.local svc.cluster.local cluster.local
```

---

# Verify DNS

Launch a temporary Pod:

```bash
kubectl run dns-test --image=busybox --restart=Never -it -- sh
```

Inside the Pod:

```bash
nslookup kubernetes
```

Example output:

```text
Name: kubernetes.default.svc.cluster.local

Address: 10.96.0.1
```

Exit:

```bash
exit
```

---

# DNS Lookup Examples

Lookup a Service:

```bash
nslookup backend
```

Lookup using FQDN:

```bash
nslookup backend.default.svc.cluster.local
```

Ping a Service:

```bash
ping backend
```

---

# Headless Service DNS

A **Headless Service** (`clusterIP: None`) does not assign a ClusterIP.

Instead, DNS returns the individual Pod IP addresses.

Example:

```yaml
spec:
  clusterIP: None
```

DNS Response:

```text
db-0.database.default.svc.cluster.local

db-1.database.default.svc.cluster.local

db-2.database.default.svc.cluster.local
```

This is commonly used with **StatefulSets**.

---

# Common DNS Commands

View Services:

```bash
kubectl get svc
```

View CoreDNS Pods:

```bash
kubectl get pods -n kube-system
```

Check DNS configuration:

```bash
cat /etc/resolv.conf
```

Lookup Service:

```bash
nslookup backend
```

Check Service Details:

```bash
kubectl describe svc backend
```

---

# Common DNS Issues

## Service Name Not Found

Possible causes:

- Incorrect Service name
- Wrong namespace
- Service does not exist

Verify:

```bash
kubectl get svc
```

---

## DNS Resolution Fails

Check CoreDNS:

```bash
kubectl get pods -n kube-system
```

View logs:

```bash
kubectl logs -n kube-system deployment/coredns
```

---

## Application Cannot Reach Service

Verify:

```bash
kubectl describe svc backend
```

Check Endpoints:

```bash
kubectl get endpoints
```

Ensure the Service selector matches the Pod labels.

---

# Best Practices

- Use Service names instead of Pod IP addresses.
- Use Fully Qualified Domain Names (FQDN) when communicating across namespaces.
- Monitor CoreDNS health.
- Keep Service names simple and meaningful.
- Use Headless Services for StatefulSets requiring direct Pod access.

---

# Key Takeaways

- Kubernetes uses **CoreDNS** for internal DNS resolution.
- Services are accessed using DNS names instead of IP addresses.
- DNS enables reliable service discovery even when Pod IPs change.
- The default Service DNS format is:
  ```
  <service-name>.<namespace>.svc.cluster.local
  ```
- Headless Services provide direct DNS records for individual Pods.

---

# References

- Kubernetes Official Documentation
- CoreDNS Documentation
- Kubernetes DNS for Services and Pods