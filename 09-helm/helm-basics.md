# Helm Basics

## Overview

**Helm** is the **package manager for Kubernetes**. It simplifies the deployment, configuration, and management of Kubernetes applications by packaging all required Kubernetes resources into reusable **Helm Charts**.

Instead of manually creating and maintaining multiple YAML files, Helm allows you to install, upgrade, rollback, and uninstall applications with simple commands.

Helm is one of the most widely used tools in Kubernetes and is considered an essential skill for DevOps Engineers, Cloud Engineers, and Kubernetes Administrators.

---

# Why Helm?

Without Helm:

```text
Application

↓

deployment.yaml
service.yaml
configmap.yaml
secret.yaml
ingress.yaml
pvc.yaml

↓

kubectl apply -f
```

Problems:

- Large number of YAML files
- Difficult upgrades
- Manual version tracking
- Hard to reuse configurations

---

With Helm:

```text
Application

↓

Helm Chart

↓

helm install

↓

Kubernetes Cluster
```

One command deploys the complete application.

---

# Benefits of Helm

- Simplifies Kubernetes deployments
- Reusable application packages
- Easy upgrades
- Built-in rollbacks
- Version management
- Environment-specific configurations
- Dependency management

---

# Helm Architecture

```text
              Developer
                  │
                  ▼
            Helm CLI
                  │
                  ▼
          Kubernetes API Server
                  │
                  ▼
          Kubernetes Cluster
                  │
                  ▼
      Deployments, Services,
      ConfigMaps, Secrets,
      Ingress, PVCs, etc.
```

The Helm CLI communicates with the Kubernetes API Server to deploy resources.

---

# Core Helm Concepts

## Chart

A **Chart** is a package containing Kubernetes resource templates and configuration files.

Example:

```text
nginx-chart/

Chart.yaml
values.yaml
templates/
charts/
```

---

## Release

A **Release** is a running instance of a Helm Chart installed in a Kubernetes cluster.

Example:

```text
Chart

↓

helm install my-nginx

↓

Release: my-nginx
```

Multiple releases can be created from the same chart.

---

## Repository

A **Repository** stores Helm Charts and makes them available for installation.

Examples:

- Bitnami
- Artifact Hub
- Private Helm Repositories

---

## Values

The `values.yaml` file stores configuration values used by chart templates.

Example:

```yaml
replicaCount: 3

image:
  repository: nginx
  tag: latest

service:
  type: ClusterIP
```

These values can be customized during installation.

---

## Templates

Templates are Kubernetes YAML files that contain placeholders.

Example:

```yaml
replicas: {{ .Values.replicaCount }}

image:
  repository: {{ .Values.image.repository }}
```

Helm replaces these placeholders with actual values from `values.yaml`.

---

# Helm Workflow

```text
Create Chart

↓

Edit values.yaml

↓

Render Templates

↓

Install Release

↓

Upgrade

↓

Rollback (if required)

↓

Uninstall
```

---

# Install Helm

### Linux

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

### macOS

```bash
brew install helm
```

### Windows (Chocolatey)

```powershell
choco install kubernetes-helm
```

Verify installation:

```bash
helm version
```

---

# Basic Helm Commands

Install a chart:

```bash
helm install my-release bitnami/nginx
```

List installed releases:

```bash
helm list
```

Upgrade a release:

```bash
helm upgrade my-release bitnami/nginx
```

Rollback:

```bash
helm rollback my-release 1
```

Uninstall:

```bash
helm uninstall my-release
```

---

# Helm Lifecycle

```text
Chart

↓

Install

↓

Release

↓

Upgrade

↓

Rollback

↓

Uninstall
```

---

# Helm vs kubectl

| Helm | kubectl |
|------|----------|
| Package manager | Kubernetes CLI |
| Uses Charts | Uses YAML manifests |
| Supports templates | Static configuration |
| Version management | Manual |
| Rollback support | Limited/manual |
| Dependency management | Yes |

---

# Helm vs Docker

| Helm | Docker |
|------|---------|
| Deploys applications | Packages applications |
| Kubernetes-focused | Container-focused |
| Uses Charts | Uses Images |
| Manages Kubernetes resources | Builds and runs containers |

---

# Advantages

- Faster deployments
- Reusable templates
- Easy upgrades
- Built-in rollback support
- Simplified application management
- Supports application versioning
- Large ecosystem of community charts

---

# Limitations

- Requires learning Helm templating.
- Complex charts can be difficult to maintain.
- Incorrect values may cause deployment failures.
- Additional abstraction compared to plain Kubernetes manifests.

---

# Best Practices

- Keep charts modular and reusable.
- Store configurable values in `values.yaml`.
- Validate charts using `helm lint`.
- Preview resources with `helm template`.
- Use semantic versioning for charts.
- Store Helm Charts in a repository.
- Test upgrades before deploying to production.

---

# Interview Questions

### What is Helm?

Helm is the package manager for Kubernetes that simplifies deploying and managing applications using reusable Helm Charts.

---

### What is a Helm Chart?

A Helm Chart is a package containing Kubernetes resource templates and configuration files.

---

### What is a Release?

A Release is an installed instance of a Helm Chart running in a Kubernetes cluster.

---

### What is `values.yaml` used for?

It stores configurable values used by Helm templates during deployment.

---

### What are the main advantages of Helm?

- Simplified deployments
- Reusable templates
- Easy upgrades
- Rollbacks
- Version management

---

# Key Takeaways

- Helm is the standard package manager for Kubernetes.
- Helm Charts package Kubernetes applications into reusable units.
- Releases are deployed instances of Helm Charts.
- Templates and `values.yaml` make applications configurable.
- Helm simplifies deployments, upgrades, and rollbacks while reducing manual YAML management.

---

# References

- Helm Official Documentation
- Helm Chart Best Practices Guide
- Kubernetes Helm Documentation
- Artifact Hub Documentation
- CNCF Helm Documentation