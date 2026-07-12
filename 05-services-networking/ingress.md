# Ingress

## Overview

An **Ingress** is a Kubernetes API object that manages **external HTTP and HTTPS access** to services running inside a Kubernetes cluster.

Instead of exposing every application using a separate **LoadBalancer** or **NodePort**, Ingress provides a **single entry point** that routes traffic to different Services based on rules such as hostnames or URL paths.

An **Ingress Controller** (such as NGINX Ingress Controller or Traefik) is required for Ingress resources to function.

---

# Why Use Ingress?

Without Ingress:

```text
Internet
    │
    ├────────► LoadBalancer → Service A
    │
    ├────────► LoadBalancer → Service B
    │
    └────────► LoadBalancer → Service C
```

Problems:

- Multiple Load Balancers
- Higher cloud costs
- Difficult management

---

With Ingress:

```text
                Internet
                    │
                    ▼
          Ingress Controller
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
   Service A    Service B    Service C
        │           │           │
      Pods        Pods        Pods
```

One public endpoint can route traffic to multiple services.

---

# Components

- Ingress Resource
- Ingress Controller
- Services
- Backend Pods

---

# How Ingress Works

```text
User Request
      │
      ▼
example.com/api
      │
      ▼
Ingress
      │
      ▼
Ingress Controller
      │
      ▼
Kubernetes Service
      │
      ▼
Application Pods
```

---

# Features

- HTTP Routing
- HTTPS Support
- SSL/TLS Termination
- Host-Based Routing
- Path-Based Routing
- Load Balancing
- URL Rewriting (Controller-dependent)

---

# Ingress YAML Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: web-ingress

spec:
  rules:
  - host: example.com

    http:
      paths:
      - path: /
        pathType: Prefix

        backend:
          service:
            name: web-service

            port:
              number: 80
```

Deploy:

```bash
kubectl apply -f ingress.yaml
```

---

# Verify Ingress

View Ingress resources:

```bash
kubectl get ingress
```

Example:

```text
NAME          CLASS   HOSTS
web-ingress   nginx   example.com
```

Describe:

```bash
kubectl describe ingress web-ingress
```

---

# Host-Based Routing

Different domains route to different Services.

```text
app.example.com
        │
        ▼
   App Service

api.example.com
        │
        ▼
   API Service
```

Example:

```yaml
rules:
- host: app.example.com
  http:
    paths:
    - path: /
      backend:
        service:
          name: frontend

- host: api.example.com
  http:
    paths:
    - path: /
      backend:
        service:
          name: backend
```

---

# Path-Based Routing

Different URL paths route to different Services.

```text
example.com/app
        │
        ▼
Frontend Service

example.com/api
        │
        ▼
Backend Service
```

Example:

```yaml
rules:
- host: example.com

  http:
    paths:
    - path: /app
      pathType: Prefix

      backend:
        service:
          name: frontend

    - path: /api
      pathType: Prefix

      backend:
        service:
          name: backend
```

---

# TLS / HTTPS

Ingress supports HTTPS using TLS certificates.

Example:

```yaml
tls:
- hosts:
  - example.com

  secretName: tls-secret
```

Create a TLS secret:

```bash
kubectl create secret tls tls-secret \
--cert=certificate.crt \
--key=private.key
```

---

# Ingress Controller

An Ingress resource only defines routing rules.

The actual routing is performed by an **Ingress Controller**.

Popular controllers:

- NGINX Ingress Controller
- Traefik
- HAProxy Ingress
- Kong Ingress
- AWS Load Balancer Controller
- Istio Gateway

---

# Common Commands

Create Ingress:

```bash
kubectl apply -f ingress.yaml
```

View Ingress:

```bash
kubectl get ingress
```

Describe Ingress:

```bash
kubectl describe ingress web-ingress
```

Delete Ingress:

```bash
kubectl delete ingress web-ingress
```

View Services:

```bash
kubectl get svc
```

---

# Ingress vs Service

| Feature | Ingress | Service |
|----------|----------|----------|
| External HTTP/HTTPS Access | ✅ | Limited |
| Load Balancing | ✅ | ✅ |
| Host-Based Routing | ✅ | ❌ |
| Path-Based Routing | ✅ | ❌ |
| TLS Termination | ✅ | ❌ |
| Requires Controller | ✅ | ❌ |

---

# Ingress vs LoadBalancer

| Feature | Ingress | LoadBalancer |
|----------|----------|--------------|
| One IP for Multiple Apps | ✅ | ❌ |
| HTTP Routing | ✅ | ❌ |
| HTTPS Support | ✅ | Limited |
| Lower Cloud Cost | ✅ | ❌ |
| Best For | Multiple Applications | Single Service |

---

# Advantages

- Single entry point for applications
- Supports HTTPS
- Reduces cloud infrastructure costs
- Simplifies traffic management
- Enables host and path-based routing
- Improves scalability

---

# Limitations

- Requires an Ingress Controller
- Primarily supports HTTP and HTTPS traffic
- Additional configuration may be needed for advanced routing

---

# Best Practices

- Use HTTPS for production workloads.
- Install a reliable Ingress Controller such as NGINX.
- Use meaningful hostnames and paths.
- Monitor Ingress Controller logs.
- Store Ingress manifests in version control.
- Combine Ingress with TLS certificates for secure communication.

---

# Key Takeaways

- Ingress manages external HTTP and HTTPS traffic into a Kubernetes cluster.
- It routes requests to Services based on hostnames or URL paths.
- An Ingress Controller is required to process Ingress resources.
- Ingress reduces the need for multiple LoadBalancer Services.
- It is the preferred solution for exposing multiple web applications through a single entry point.

---

# References

- Kubernetes Official Documentation
- Kubernetes Ingress Documentation
- NGINX Ingress Controller Documentation
- CNCF Kubernetes Networking Concepts