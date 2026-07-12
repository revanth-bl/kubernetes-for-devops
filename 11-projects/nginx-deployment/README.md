# NGINX Deployment on Kubernetes

## Project Overview

This project demonstrates how to deploy a simple **NGINX web server** on a Kubernetes cluster.

It covers the fundamental Kubernetes resources required to run a containerized application, including Deployments, Services, Namespaces, and Ingress.

This project serves as an introduction to deploying applications on Kubernetes before moving on to more complex multi-tier and microservices-based architectures.

---

# Objectives

- Deploy an NGINX container
- Create a Kubernetes Deployment
- Expose the application using a Service
- Configure an Ingress for external access
- Verify Pods and Services
- Learn the Kubernetes deployment workflow

---

# Architecture

```text
                Internet
                    │
                    ▼
             Ingress Controller
                    │
                    ▼
              NGINX Service
                    │
                    ▼
          NGINX Deployment
                    │
                    ▼
                NGINX Pod(s)
```

---

# Technologies Used

- Kubernetes
- Docker
- NGINX
- kubectl
- Minikube / Kind (for local testing)

---

# Project Structure

```text
nginx-deployment/

├── deployment.yaml
├── service.yaml
├── namespace.yaml
├── ingress.yaml
├── README.md
└── screenshots/
```

> **Note:** The YAML manifests will be added as this project is built during the Kubernetes learning journey.

---

# Kubernetes Resources

| Resource | Purpose |
|----------|---------|
| Namespace | Isolates project resources |
| Deployment | Manages NGINX Pods |
| Service | Exposes the application inside the cluster |
| Ingress | Provides external HTTP access |

---

# Deployment Workflow

```text
Create Namespace
        │
        ▼
Deploy NGINX
        │
        ▼
Create Service
        │
        ▼
Configure Ingress
        │
        ▼
Access Application
```

---

# Commands

Create Namespace

```bash
kubectl apply -f namespace.yaml
```

Deploy NGINX

```bash
kubectl apply -f deployment.yaml
```

Create Service

```bash
kubectl apply -f service.yaml
```

Create Ingress

```bash
kubectl apply -f ingress.yaml
```

Verify Resources

```bash
kubectl get all
```

View Pods

```bash
kubectl get pods
```

View Services

```bash
kubectl get svc
```

View Ingress

```bash
kubectl get ingress
```

Describe Deployment

```bash
kubectl describe deployment nginx-deployment
```

View Logs

```bash
kubectl logs <pod-name>
```

Delete Resources

```bash
kubectl delete -f .
```

---

# Expected Outcome

After deployment:

- NGINX Pod is running.
- Deployment manages the Pod lifecycle.
- Service exposes the application inside the cluster.
- Ingress enables external browser access.
- The default NGINX welcome page is accessible.

---

# Learning Outcomes

After completing this project, you will understand:

- Kubernetes Deployments
- Pod management
- Services
- Ingress
- Namespaces
- Basic application deployment
- Troubleshooting Kubernetes workloads

---

# Screenshots

Add screenshots after completing the project.

Examples:

- Running Pods
- Services
- Ingress
- Browser showing the NGINX welcome page
- `kubectl get all` output

---

# Future Improvements

- Increase replica count
- Add Horizontal Pod Autoscaler (HPA)
- Use ConfigMaps
- Store configuration in Secrets
- Deploy using Helm
- Add monitoring with Prometheus and Grafana
- Deploy to a managed Kubernetes service (EKS, AKS, or GKE)

---

# Interview Questions

### Why is a Deployment used instead of creating a Pod directly?

A Deployment manages Pods, supports scaling, rolling updates, and self-healing, making it suitable for production workloads.

---

### What is the purpose of a Service?

A Service provides a stable network endpoint for accessing Pods.

---

### What is Ingress?

Ingress manages external HTTP and HTTPS access to services inside a Kubernetes cluster.

---

### Why use a Namespace?

Namespaces logically isolate resources within a Kubernetes cluster.

---

# References

- Kubernetes Official Documentation
- NGINX Documentation
- Kubernetes Ingress Documentation
- CNCF Kubernetes Documentation