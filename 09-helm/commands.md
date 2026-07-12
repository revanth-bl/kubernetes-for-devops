# Helm Commands

## Overview

Helm is the package manager for Kubernetes. It simplifies application deployment, upgrades, rollbacks, and management through reusable **Helm Charts**.

This document contains the most commonly used Helm commands for daily operations.

---

# Check Helm Version

Display the installed Helm version.

```bash
helm version
```

Example Output:

```text
version.BuildInfo{Version:"v3.15.0"}
```

---

# View Help

Display all available Helm commands.

```bash
helm help
```

Help for a specific command:

```bash
helm install --help
```

---

# Add a Repository

Add a remote Helm chart repository.

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

---

# List Repositories

```bash
helm repo list
```

---

# Update Repositories

Download the latest chart information.

```bash
helm repo update
```

---

# Search for Charts

Search all configured repositories.

```bash
helm search repo nginx
```

Search the Artifact Hub.

```bash
helm search hub nginx
```

---

# Create a New Chart

Generate a new Helm Chart.

```bash
helm create my-app
```

---

# Validate a Chart

Check a chart for syntax errors and best practices.

```bash
helm lint my-app
```

---

# Package a Chart

Create a compressed chart package.

```bash
helm package my-app
```

Example Output:

```text
Successfully packaged chart and saved it to:
my-app-1.0.0.tgz
```

---

# Render Templates

Preview the generated Kubernetes manifests without deploying them.

```bash
helm template my-app
```

Render using custom values.

```bash
helm template my-app -f values.yaml
```

---

# Install a Chart

Install a Helm Chart.

```bash
helm install my-release ./my-app
```

Install from a repository.

```bash
helm install nginx bitnami/nginx
```

---

# List Installed Releases

```bash
helm list
```

Show releases in all namespaces.

```bash
helm list -A
```

---

# View Release Status

```bash
helm status my-release
```

---

# View Release History

```bash
helm history my-release
```

---

# Upgrade a Release

Upgrade an existing release.

```bash
helm upgrade my-release ./my-app
```

Upgrade using custom values.

```bash
helm upgrade my-release ./my-app -f values.yaml
```

---

# Roll Back a Release

Restore a previous release revision.

```bash
helm rollback my-release 1
```

---

# Uninstall a Release

Remove a deployed application.

```bash
helm uninstall my-release
```

---

# Show Chart Information

Display chart metadata.

```bash
helm show chart bitnami/nginx
```

---

# Show Default Values

Display the default values of a chart.

```bash
helm show values bitnami/nginx
```

---

# Show README

```bash
helm show readme bitnami/nginx
```

---

# Download a Chart

Download a chart without installing it.

```bash
helm pull bitnami/nginx
```

Download and extract.

```bash
helm pull bitnami/nginx --untar
```

---

# Verify Installed Releases

```bash
helm list
```

Example Output:

```text
NAME         NAMESPACE   STATUS     REVISION
my-release   default     deployed   1
```

---

# Common Helm Workflow

```text
Add Repository
       │
       ▼
Update Repository
       │
       ▼
Search Chart
       │
       ▼
Install Chart
       │
       ▼
Verify Deployment
       │
       ▼
Upgrade (if needed)
       │
       ▼
Rollback (if required)
       │
       ▼
Uninstall
```

---

# Frequently Used Commands

| Command | Purpose |
|----------|---------|
| `helm version` | Show Helm version |
| `helm help` | Display help |
| `helm repo add` | Add a repository |
| `helm repo list` | List repositories |
| `helm repo update` | Update repositories |
| `helm search repo` | Search installed repositories |
| `helm search hub` | Search Artifact Hub |
| `helm create` | Create a new chart |
| `helm lint` | Validate a chart |
| `helm package` | Package a chart |
| `helm template` | Render templates |
| `helm install` | Install a chart |
| `helm list` | List releases |
| `helm status` | Show release status |
| `helm history` | View release history |
| `helm upgrade` | Upgrade a release |
| `helm rollback` | Roll back a release |
| `helm uninstall` | Remove a release |
| `helm show chart` | Display chart metadata |
| `helm show values` | Show default values |
| `helm pull` | Download a chart |

---

# Best Practices

- Validate charts using `helm lint` before deployment.
- Preview resources with `helm template`.
- Use versioned Helm Charts.
- Store custom configurations in `values.yaml`.
- Keep release names meaningful.
- Review release history before rolling back.
- Regularly update chart repositories.

---

# Interview Questions

### Which command installs a Helm Chart?

```bash
helm install
```

---

### Which command upgrades an existing release?

```bash
helm upgrade
```

---

### Which command rolls back a failed deployment?

```bash
helm rollback
```

---

### Which command lists installed releases?

```bash
helm list
```

---

### Which command validates a chart?

```bash
helm lint
```

---

# Key Takeaways

- Helm simplifies Kubernetes application management.
- Charts can be installed, upgraded, rolled back, and removed using simple commands.
- `helm lint` and `helm template` are essential for testing before deployment.
- Helm repositories provide reusable application packages.
- Mastering Helm commands is important for Kubernetes administration and DevOps workflows.

---

# References

- Helm Official Documentation
- Helm CLI Reference
- Kubernetes Helm Documentation
- CNCF Helm Documentation