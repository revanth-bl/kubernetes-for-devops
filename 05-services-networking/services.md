# Kubernetes Services

## Overview

A **Service** is a Kubernetes resource that provides a **stable network endpoint** for accessing one or more Pods.

Since Pods are **ephemeral** (their IP addresses can change when they are recreated), Services provide a fixed IP address and DNS name, allowing applications to communicate reliably.

Services use **label selectors** to identify and route traffic to the correct Pods.

---

# Why Do We Need Services?

Without a Service:

```text
Client
   │
   ▼
Pod (10.244.0.15)

Pod Crashes

New Pod (10.244.0.22)
```

The Pod's IP address changes, breaking communication.

---

With a Service:

```text
            Client
               │
               ▼
        Kubernetes Service
          (Stable ClusterIP)
               │
      ┌────────┴────────┐
      ▼                 ▼
   Pod A             Pod B
```

The Service provides a stable endpoint, while Kubernetes automatically routes traffic to healthy Pods.

---

# How Services Work

```text
          Client
             │
             ▼
        Kubernetes Service
             │
      Label Selector
             │
      ┌──────┴──────┐
      ▼             ▼
    Pod 1         Pod 2
```

The Service continuously discovers Pods using labels and load balances requests across them.

---

# Features

- Stable IP Address
- Automatic Load Balancing
- Service Discovery
- Decouples Clients from Pods
- Supports Internal and External Access
- Integrates with Kubernetes DNS

---

# Types of Services

## 1. ClusterIP (Default)

Accessible only from within the Kubernetes cluster.

```text
Pod
 │
 ▼
ClusterIP Service
 │
 ▼
Backend Pods
```

Best for:

- Internal APIs
- Databases
- Microservices

---

## 2. NodePort

Exposes the Service on a static port on every worker node.

```text
Internet
     │
     ▼
Node IP:30080
     │
     ▼
NodePort Service
     │
     ▼
Pods
```

Default Port Range:

```
30000 - 32767
```

Best for:

- Development
- Testing
- Small clusters

---

## 3. LoadBalancer

Creates an external cloud load balancer.

```text
Internet
     │
     ▼
Cloud Load Balancer
     │
     ▼
Service
     │
     ▼
Pods
```

Supported by:

- AWS
- Azure
- Google Cloud

Best for:

- Production Applications
- Public APIs
- Websites

---

## 4. ExternalName

Maps a Kubernetes Service to an external DNS name.

Example:

```
database.example.com
```

Useful for connecting to services outside the Kubernetes cluster.

---

# ClusterIP Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80

  type: ClusterIP
```

Deploy:

```bash
kubectl apply -f service.yaml
```

---

# NodePort Example

```yaml
spec:
  type: NodePort

  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```

Access:

```
http://<Node-IP>:30080
```

---

# LoadBalancer Example

```yaml
spec:
  type: LoadBalancer

  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80
```

Cloud providers automatically provision an external load balancer.

---

# ExternalName Example

```yaml
apiVersion: v1
kind: Service

metadata:
  name: external-db

spec:
  type: ExternalName

  externalName: database.example.com
```

---

# Common Commands

Create Service:

```bash
kubectl apply -f service.yaml
```

View Services:

```bash
kubectl get services
```

or

```bash
kubectl get svc
```

Describe Service:

```bash
kubectl describe service nginx-service
```

Delete Service:

```bash
kubectl delete service nginx-service
```

View Endpoints:

```bash
kubectl get endpoints
```

---

# Service Discovery

Every Service automatically receives a DNS name.

Example:

```
nginx-service.default.svc.cluster.local
```

Applications in the same namespace can simply use:

```
http://nginx-service
```

---

# Service Types Comparison

| Service Type | Internal | External | Use Case |
|--------------|----------|----------|----------|
| ClusterIP | ✅ | ❌ | Internal communication |
| NodePort | ✅ | ✅ | Development & Testing |
| LoadBalancer | ✅ | ✅ | Production Applications |
| ExternalName | External Resource | External | External Services |

---

# Service vs Pod

| Pod | Service |
|-----|----------|
| Temporary IP | Stable IP |
| Can Fail | Always Available |
| Single Instance | Load Balances Multiple Pods |
| Not Discoverable | DNS Enabled |

---

# Service vs Ingress

| Feature | Service | Ingress |
|----------|----------|----------|
| Load Balancing | ✅ | ✅ |
| Internal Access | ✅ | ❌ |
| External HTTP/HTTPS | Limited | ✅ |
| Path Routing | ❌ | ✅ |
| Host Routing | ❌ | ✅ |

---

# Advantages

- Stable networking
- Automatic load balancing
- Built-in service discovery
- Works seamlessly with DNS
- Supports multiple exposure methods
- Decouples applications from Pod lifecycle

---

# Best Practices

- Use **ClusterIP** for internal services.
- Use **LoadBalancer** for production internet-facing applications.
- Use **NodePort** only for development or testing.
- Use meaningful labels for Pod selection.
- Verify Service selectors match Pod labels.
- Monitor Service endpoints regularly.

---

# Interview Questions

### What is a Kubernetes Service?

A Service provides a stable network endpoint that allows clients to access one or more Pods.

---

### Why are Services required?

Because Pod IP addresses are temporary and change whenever Pods are recreated.

---

### Which Service type is the default?

**ClusterIP**

---

### Which Service type is used in cloud environments?

**LoadBalancer**

---

### What is the difference between NodePort and LoadBalancer?

NodePort exposes the application through a port on each worker node, while LoadBalancer provisions a cloud load balancer with a public IP.

---

# Key Takeaways

- Services provide stable networking for Pods.
- They use label selectors to route traffic.
- Kubernetes supports four Service types:
  - ClusterIP
  - NodePort
  - LoadBalancer
  - ExternalName
- Services integrate with Kubernetes DNS.
- Every production Kubernetes application relies on Services.

---

# References

- Kubernetes Official Documentation
- Kubernetes Services Documentation
- Kubernetes Networking Concepts
- CNCF Kubernetes Documentation