# Helm Charts

## Overview

A **Helm Chart** is a packaged collection of Kubernetes resource definitions that describes how to deploy an application.

Think of a Helm Chart as a **template** for Kubernetes applications. Instead of manually creating multiple YAML files, a Helm Chart packages everything together into a reusable and versioned application package.

A typical Helm Chart includes:

- Deployments
- Services
- ConfigMaps
- Secrets
- Ingress
- PersistentVolumeClaims
- Other Kubernetes resources

---

# Why Use Helm Charts?

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

Apply Each File Individually
```

Problems:

- Large number of YAML files
- Difficult updates
- Repetitive configurations
- Hard to maintain

---

With Helm:

```text
Application

↓

Helm Chart

↓

helm install

↓

All Kubernetes Resources
```

One command deploys the complete application.

---

# Benefits

- Package Kubernetes applications
- Reusable templates
- Version control
- Easy upgrades
- Rollbacks
- Environment-specific configuration

---

# Helm Chart Structure

A typical Helm Chart looks like:

```text
my-chart/

├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── configmap.yaml
│   └── _helpers.tpl
└── .helmignore
```

---

# Chart Components

## Chart.yaml

Contains chart metadata.

Example:

```yaml
apiVersion: v2
name: my-app
description: A sample Helm chart
version: 1.0.0
appVersion: "1.0"
```

---

## values.yaml

Stores default configuration values.

Example:

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: latest

service:
  type: ClusterIP
  port: 80
```

These values can be overridden during installation.

---

## templates/

Contains Kubernetes resource templates.

Example:

```text
templates/

deployment.yaml
service.yaml
ingress.yaml
configmap.yaml
```

Helm converts these templates into Kubernetes manifests.

---

## charts/

Contains dependent Helm Charts.

Example:

```text
charts/

redis/
mysql/
```

Useful when one application depends on another.

---

## .helmignore

Specifies files that should not be included in the packaged chart.

Similar to `.gitignore`.

---

# Template Example

Deployment template:

```yaml
replicas: {{ .Values.replicaCount }}

containers:
- image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

Helm replaces placeholders using values from `values.yaml`.

Rendered output:

```yaml
replicas: 2

containers:
- image: nginx:latest
```

---

# Creating a Chart

Create a new chart:

```bash
helm create my-app
```

Generated structure:

```text
my-app/

Chart.yaml
values.yaml
charts/
templates/
```

---

# Install a Chart

```bash
helm install my-release ./my-app
```

---

# Upgrade a Chart

```bash
helm upgrade my-release ./my-app
```

---

# Package a Chart

```bash
helm package my-app
```

Output:

```text
my-app-1.0.0.tgz
```

---

# Lint a Chart

Validate chart syntax:

```bash
helm lint my-app
```

---

# Render Templates

Preview the generated Kubernetes manifests:

```bash
helm template my-app
```

---

# Common Commands

Create Chart:

```bash
helm create my-app
```

Install Chart:

```bash
helm install my-release ./my-app
```

Upgrade Chart:

```bash
helm upgrade my-release ./my-app
```

Rollback Release:

```bash
helm rollback my-release 1
```

Package Chart:

```bash
helm package my-app
```

Lint Chart:

```bash
helm lint my-app
```

Render Templates:

```bash
helm template my-app
```

List Releases:

```bash
helm list
```

---

# Chart Lifecycle

```text
Create Chart

↓

Configure values.yaml

↓

Install

↓

Upgrade

↓

Rollback (if needed)

↓

Uninstall
```

---

# Helm Chart vs Kubernetes YAML

| Helm Chart | Kubernetes YAML |
|------------|-----------------|
| Templates | Static manifests |
| Reusable | Repetitive |
| Parameterized | Hardcoded |
| Versioned | Manual management |
| Easy upgrades | Manual updates |

---

# Helm Chart vs Docker Image

| Helm Chart | Docker Image |
|------------|--------------|
| Deploys applications | Packages applications |
| Kubernetes resource definitions | Application binaries and runtime |
| YAML templates | Container filesystem |

---

# Advantages

- Simplifies Kubernetes deployments
- Supports reusable templates
- Easy version management
- Environment-specific configuration
- Built-in rollback support
- Easy dependency management

---

# Limitations

- Learning Helm templating takes time.
- Poorly designed charts can become difficult to maintain.
- Managing many chart dependencies can add complexity.

---

# Best Practices

- Keep charts small and focused.
- Use `values.yaml` for configurable settings.
- Avoid hardcoding values in templates.
- Version charts properly using semantic versioning.
- Validate charts with `helm lint` before deployment.
- Test rendered templates using `helm template`.
- Store charts in a Helm repository for reuse.

---

# Interview Questions

### What is a Helm Chart?

A Helm Chart is a packaged collection of Kubernetes resource templates used to deploy and manage applications.

---

### What is `Chart.yaml`?

It contains metadata about the Helm Chart, including its name, version, and description.

---

### What is `values.yaml`?

It stores the default configuration values used by chart templates.

---

### Which directory contains Kubernetes manifests?

The **templates/** directory.

---

### Which command creates a new Helm Chart?

```bash
helm create my-app
```

---

# Key Takeaways

- Helm Charts package Kubernetes applications.
- Templates allow dynamic and reusable configurations.
- `Chart.yaml` defines chart metadata.
- `values.yaml` stores configurable values.
- Helm Charts simplify deployment, upgrades, and rollbacks.

---

# References

- Helm Official Documentation
- Kubernetes Helm Documentation
- Helm Chart Best Practices Guide
- CNCF Helm Documentation