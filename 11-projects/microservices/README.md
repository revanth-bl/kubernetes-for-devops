# Microservices Deployment on Kubernetes

## Project Overview

This project demonstrates how to deploy a **microservices-based application** on Kubernetes.

Unlike a monolithic application, a microservices architecture divides an application into multiple independent services. Each service is deployed, scaled, and managed separately while communicating with other services over the network.

This project showcases common Kubernetes resources such as Deployments, Services, ConfigMaps, Secrets, Ingress, and Horizontal Pod Autoscalers in a production-style setup.

---

# Project Architecture

```text
                    Internet
                        │
                        ▼
                  Ingress Controller
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
   Frontend        User Service     Product Service
        │               │               │
        └───────────────┼───────────────┘
                        ▼
                 Order Service
                        │
                        ▼
                   MySQL Database
```

---

# Technologies Used

- Kubernetes
- Docker
- Helm (Optional)
- NGINX Ingress Controller
- ConfigMaps
- Secrets
- Persistent Volumes
- Horizontal Pod Autoscaler
- Prometheus
- Grafana

---

# Project Structure

```text
microservices/

├── frontend/
│   ├── deployment.yaml
│   └── service.yaml
│
├── user-service/
│   ├── deployment.yaml
│   └── service.yaml
│
├── product-service/
│   ├── deployment.yaml
│   └── service.yaml
│
├── order-service/
│   ├── deployment.yaml
│   └── service.yaml
│
├── database/
│   ├── mysql-deployment.yaml
│   ├── pvc.yaml
│   └── service.yaml
│
├── ingress.yaml
├── configmap.yaml
├── secret.yaml
├── hpa.yaml
└── README.md
```

---

# Kubernetes Resources Used

| Resource | Purpose |
|----------|---------|
| Deployment | Deploy application containers |
| Service | Internal communication |
| Ingress | External access |
| ConfigMap | Application configuration |
| Secret | Store passwords and credentials |
| PersistentVolumeClaim | Database storage |
| HorizontalPodAutoscaler | Automatic scaling |
| Namespace | Resource isolation |

---

# Deployment Steps

## 1. Create Namespace

```bash
kubectl create namespace microservices
```

---

## 2. Deploy Database

```bash
kubectl apply -f database/
```

---

## 3. Deploy ConfigMap

```bash
kubectl apply -f configmap.yaml
```

---

## 4. Deploy Secrets

```bash
kubectl apply -f secret.yaml
```

---

## 5. Deploy Backend Services

```bash
kubectl apply -f user-service/

kubectl apply -f product-service/

kubectl apply -f order-service/
```

---

## 6. Deploy Frontend

```bash
kubectl apply -f frontend/
```

---

## 7. Deploy Ingress

```bash
kubectl apply -f ingress.yaml
```

---

## 8. Verify Deployment

```bash
kubectl get all -n microservices
```

---

# Scaling

Scale a deployment manually:

```bash
kubectl scale deployment frontend --replicas=5
```

View Horizontal Pod Autoscaler:

```bash
kubectl get hpa
```

---

# Monitoring

View Pods:

```bash
kubectl get pods
```

View Services:

```bash
kubectl get svc
```

View Ingress:

```bash
kubectl get ingress
```

View Logs:

```bash
kubectl logs <pod-name>
```

---

# Project Features

- Microservices architecture
- Independent service deployment
- Internal service communication
- Persistent database storage
- External access through Ingress
- Automatic scaling
- Secure configuration using Secrets
- Environment configuration using ConfigMaps
- Monitoring with Prometheus and Grafana

---

# Learning Outcomes

After completing this project, you should understand:

- Kubernetes application deployment
- Service discovery
- Inter-service communication
- Storage management
- Configuration management
- Secret management
- Ingress configuration
- Autoscaling
- Monitoring and observability
- Production deployment workflow

---

# Possible Improvements

- Deploy with Helm Charts
- Add CI/CD using Jenkins or GitHub Actions
- Implement GitOps using Argo CD
- Add Service Mesh with Istio
- Enable TLS using cert-manager
- Integrate centralized logging using Loki
- Deploy on AWS EKS, Azure AKS, or Google GKE

---

# Interview Questions

### Why use a microservices architecture?

It allows services to be developed, deployed, scaled, and maintained independently, improving flexibility and fault isolation.

---

### Which Kubernetes resource exposes services externally?

**Ingress**

---

### Which resource stores application configuration?

**ConfigMap**

---

### Which resource stores sensitive information?

**Secret**

---

### Which Kubernetes resource enables automatic scaling?

**Horizontal Pod Autoscaler (HPA)**

---

# References

- Kubernetes Official Documentation
- CNCF Kubernetes Documentation
- Helm Documentation
- Prometheus Documentation
- Grafana Documentation