# Three-Tier Application Deployment on Kubernetes

## Project Overview

This project demonstrates the deployment of a **three-tier web application architecture** on Kubernetes.

A three-tier architecture separates an application into three independent layers:

1. **Frontend Layer** - User interface
2. **Backend Layer** - Application logic and APIs
3. **Database Layer** - Data storage

Each layer is deployed independently using Kubernetes resources such as Deployments, Services, Persistent Volumes, Secrets, ConfigMaps, and Ingress.

This project demonstrates how Kubernetes manages real-world multi-tier applications.

---

# Architecture

```text
                    Users
                      │
                      ▼
              Ingress Controller
                      │
                      ▼
              Frontend Service
                      │
                      ▼
              Frontend Pods
                      │
                      ▼
              Backend Service
                      │
                      ▼
              Backend Pods
                      │
                      ▼
              Database Service
                      │
                      ▼
              MySQL Database
                      │
                      ▼
          Persistent Volume Storage
```

---

# Application Flow

```text
User Request

      │

Frontend Layer
(React / Angular / Web UI)

      │

Backend Layer
(Node.js / Java / Python API)

      │

Database Layer
(MySQL / PostgreSQL)

      │

Persistent Storage
```

---

# Objectives

- Deploy a complete three-tier application on Kubernetes
- Understand communication between application layers
- Configure internal Kubernetes networking
- Manage database persistence
- Secure application credentials
- Expose the application using Ingress
- Understand production application architecture

---

# Technologies Used

- Kubernetes
- Docker
- kubectl
- NGINX Ingress Controller
- MySQL Database
- Persistent Volumes
- ConfigMaps
- Secrets
- Services
- Deployments

---

# Project Structure

```text
three-tier-app/

├── frontend-deployment.yaml
├── frontend-service.yaml
│
├── backend-deployment.yaml
├── backend-service.yaml
│
├── mysql-deployment.yaml
├── mysql-service.yaml
│
├── pvc.yaml
├── secret.yaml
├── ingress.yaml
│
├── README.md
└── screenshots/
```

> YAML files will be added while building the project.

---

# Kubernetes Components Used

| Component | Purpose |
|----------|---------|
| Namespace | Resource isolation |
| Deployment | Manages application Pods |
| Service | Enables communication between tiers |
| Ingress | External application access |
| ConfigMap | Stores application configuration |
| Secret | Stores sensitive information |
| Persistent Volume | Provides database storage |
| PVC | Requests storage resources |

---

# Three-Tier Components

## Frontend Tier

Responsibilities:

- Provides user interface
- Communicates with backend APIs
- Handles user interactions

Kubernetes Resources:

```
Frontend Deployment
        |
        |
Frontend Service
```

---

## Backend Tier

Responsibilities:

- Application business logic
- API processing
- Database communication

Kubernetes Resources:

```
Backend Deployment
        |
        |
Backend Service
```

---

## Database Tier

Responsibilities:

- Stores application data
- Provides persistent storage

Kubernetes Resources:

```
MySQL Deployment
        |
        |
MySQL Service
        |
        |
Persistent Volume
```

---

# Deployment Workflow

```text
Create Namespace

        ↓

Deploy Database

        ↓

Create Persistent Storage

        ↓

Deploy Backend

        ↓

Deploy Frontend

        ↓

Configure Ingress

        ↓

Access Application
```

---

# Deployment Commands

Create Namespace:

```bash
kubectl apply -f namespace.yaml
```

Deploy Database:

```bash
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml
```

Create Storage:

```bash
kubectl apply -f pvc.yaml
```

Deploy Backend:

```bash
kubectl apply -f backend-deployment.yaml
kubectl apply -f backend-service.yaml
```

Deploy Frontend:

```bash
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
```

Deploy Ingress:

```bash
kubectl apply -f ingress.yaml
```

---

# Verification Commands

Check all resources:

```bash
kubectl get all
```

Check Pods:

```bash
kubectl get pods
```

Check Services:

```bash
kubectl get svc
```

Check Persistent Volumes:

```bash
kubectl get pv
```

Check Persistent Volume Claims:

```bash
kubectl get pvc
```

View Application Logs:

```bash
kubectl logs <pod-name>
```

---

# Expected Outcome

After deployment:

- Frontend is accessible through Ingress.
- Frontend communicates with backend APIs.
- Backend connects to MySQL database.
- Database data persists using Persistent Volume.
- Each tier can be scaled independently.

---

# Scaling

Scale frontend:

```bash
kubectl scale deployment frontend --replicas=3
```

Scale backend:

```bash
kubectl scale deployment backend --replicas=3
```

Check resources:

```bash
kubectl get deployments
```

---

# Security Implementation

This project uses:

- Kubernetes Secrets for database credentials
- ConfigMaps for application configuration
- Namespace isolation
- Internal Services for backend communication
- Least privilege access principles

---

# Monitoring

Future monitoring integration:

- Prometheus for metrics collection
- Grafana for dashboards
- Loki for centralized logging

---

# Learning Outcomes

After completing this project, you will understand:

- Multi-tier Kubernetes architecture
- Application networking
- Service discovery
- Persistent storage
- Database deployment
- Configuration management
- Secret management
- Ingress routing
- Production deployment patterns

---

# Possible Improvements

- Add Horizontal Pod Autoscaler
- Convert manifests into Helm Charts
- Add CI/CD pipeline
- Deploy on AWS EKS
- Add TLS certificates using cert-manager
- Implement Kubernetes RBAC
- Add monitoring and alerting

---

# Interview Questions

### What is a three-tier architecture?

A three-tier architecture separates an application into presentation, application logic, and database layers.

---

### Why separate frontend and backend deployments?

It allows independent scaling, development, and maintenance of each application layer.

---

### How do Pods communicate with each other?

Pods communicate through Kubernetes Services using stable DNS names.

---

### How is database data preserved?

Using Persistent Volumes and Persistent Volume Claims.

---

### Why use Secrets instead of ConfigMaps for passwords?

Secrets are designed to store sensitive information such as credentials and tokens.

---

# References

- Kubernetes Official Documentation
- Kubernetes Networking Documentation
- Kubernetes Storage Documentation
- Docker Documentation
- CNCF Kubernetes Documentation