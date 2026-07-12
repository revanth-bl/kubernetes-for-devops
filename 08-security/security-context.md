# Security Context

## Overview

A **Security Context** is a Kubernetes configuration that defines **security settings** for a Pod or Container.

It controls how containers run by specifying permissions, Linux capabilities, user IDs, file system access, privilege levels, and other security-related settings.

Using Security Contexts helps reduce security risks by enforcing the **Principle of Least Privilege**, ensuring containers run with only the permissions they need.

---

# Why Use Security Context?

Without Security Context:

```text
Container

↓

Runs as Root

↓

Full System Access

↓

Higher Security Risk
```

Problems:

- Containers run as root
- Privilege escalation
- Unauthorized filesystem access
- Increased attack surface

---

With Security Context:

```text
Container

↓

Security Context

↓

Restricted Permissions

↓

Least Privilege

↓

Improved Security
```

Containers run with controlled permissions.

---

# Benefits

- Prevents running containers as root
- Controls Linux capabilities
- Restricts filesystem access
- Prevents privilege escalation
- Improves container security
- Supports compliance requirements

---

# How Security Context Works

```text
             Pod
              │
      ┌───────┴────────┐
      ▼                ▼
Security Context   Container
      │
      ▼
Linux Security Features

- User ID
- Group ID
- Capabilities
- Privileges
- Filesystem Access
```

Security Context settings can be applied at the **Pod** or **Container** level.

---

# Pod-Level Security Context

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: secure-pod

spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000

  containers:
  - name: nginx
    image: nginx
```

This applies the security settings to all containers in the Pod unless overridden.

---

# Container-Level Security Context

```yaml
containers:
- name: nginx
  image: nginx

  securityContext:
    runAsUser: 1000
    allowPrivilegeEscalation: false
```

Container-level settings override Pod-level settings for that container.

---

# Common Security Context Options

## runAsUser

Runs the container as a specific Linux user.

```yaml
securityContext:
  runAsUser: 1000
```

---

## runAsGroup

Runs the container with a specific Linux group.

```yaml
securityContext:
  runAsGroup: 1000
```

---

## fsGroup

Sets group ownership for mounted volumes.

```yaml
securityContext:
  fsGroup: 2000
```

---

## runAsNonRoot

Prevents containers from running as the root user.

```yaml
securityContext:
  runAsNonRoot: true
```

---

## readOnlyRootFilesystem

Makes the root filesystem read-only.

```yaml
securityContext:
  readOnlyRootFilesystem: true
```

---

## allowPrivilegeEscalation

Prevents processes from gaining more privileges.

```yaml
securityContext:
  allowPrivilegeEscalation: false
```

---

## privileged

Runs the container with elevated privileges.

```yaml
securityContext:
  privileged: true
```

> **Warning:** Avoid `privileged: true` unless absolutely necessary.

---

## Linux Capabilities

Instead of granting full root access, add or remove specific Linux capabilities.

Example:

```yaml
securityContext:
  capabilities:
    drop:
    - ALL

    add:
    - NET_BIND_SERVICE
```

This removes all capabilities and only allows binding to privileged network ports.

---

# Complete Example

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: secure-nginx

spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 1000
    fsGroup: 2000

  containers:
  - name: nginx
    image: nginx

    securityContext:
      runAsNonRoot: true
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false

      capabilities:
        drop:
        - ALL
```

---

# Common Commands

Create Pod:

```bash
kubectl apply -f secure-pod.yaml
```

View Pod:

```bash
kubectl get pods
```

Describe Pod:

```bash
kubectl describe pod secure-nginx
```

Delete Pod:

```bash
kubectl delete pod secure-nginx
```

---

# Security Context vs RBAC

| Security Context | RBAC |
|------------------|------|
| Secures containers | Secures Kubernetes API access |
| Controls Linux permissions | Controls user permissions |
| Works inside Pods | Works at cluster level |

---

# Security Context vs Service Account

| Security Context | Service Account |
|------------------|-----------------|
| Controls container privileges | Provides Kubernetes API identity |
| Linux-level security | Kubernetes authentication |

---

# Advantages

- Reduces attack surface
- Prevents root execution
- Protects the filesystem
- Restricts Linux capabilities
- Improves compliance with security standards

---

# Limitations

- Some applications require elevated privileges.
- Incorrect configuration may prevent containers from starting.
- Security Context only controls container runtime security; additional mechanisms like RBAC and Network Policies are still required.

---

# Best Practices

- Always use `runAsNonRoot: true` when possible.
- Avoid running containers as the root user.
- Set `allowPrivilegeEscalation: false`.
- Use `readOnlyRootFilesystem: true` for stateless applications.
- Drop unnecessary Linux capabilities.
- Avoid `privileged: true` unless absolutely required.
- Combine Security Contexts with RBAC, Network Policies, and Pod Security Admission for defense in depth.

---

# Interview Questions

### What is a Security Context?

A Security Context defines security-related settings for Pods and Containers, such as user IDs, privileges, and filesystem permissions.

---

### Why should containers avoid running as root?

Running as root increases the attack surface and can allow an attacker to gain elevated privileges if the container is compromised.

---

### What does `runAsNonRoot` do?

It ensures that the container cannot run as the root user.

---

### What does `readOnlyRootFilesystem` do?

It mounts the container's root filesystem as read-only, preventing unauthorized modifications.

---

### What is the purpose of Linux Capabilities?

Linux Capabilities provide fine-grained privileges, allowing containers to perform only specific operations instead of granting full root access.

---

# Key Takeaways

- Security Context controls how Pods and Containers run.
- It helps enforce the Principle of Least Privilege.
- Common settings include `runAsUser`, `runAsNonRoot`, `fsGroup`, and `allowPrivilegeEscalation`.
- Dropping unnecessary Linux capabilities significantly improves security.
- Security Contexts are a core part of Kubernetes workload hardening.

---

# References

- Kubernetes Official Documentation
- Kubernetes Security Context Documentation
- Kubernetes Security Best Practices
- Pod Security Standards Documentation
- CNCF Kubernetes Documentation