# Role-Based Access Control (RBAC)

## Overview

**Role-Based Access Control (RBAC)** is Kubernetes' authorization mechanism that controls **who can perform which actions on which resources**.

RBAC allows administrators to define permissions for users, groups, or Service Accounts based on their responsibilities, following the **Principle of Least Privilege**.

Instead of giving every user full administrative access, RBAC grants only the permissions required to perform specific tasks.

---

# Why Use RBAC?

Without RBAC:

```text
User

↓

Full Cluster Access

↓

Can Modify Everything
```

Problems:

- Security risks
- Accidental changes
- Unauthorized access
- Difficult auditing

---

With RBAC:

```text
             Kubernetes Cluster

        ┌──────────────┐
        │    Admin     │
        └──────┬───────┘
               │
        Full Cluster Access

        ┌──────────────┐
        │   Developer  │
        └──────┬───────┘
               │
      Manage Pods & Services

        ┌──────────────┐
        │    Viewer    │
        └──────┬───────┘
               │
         Read-Only Access
```

Each user receives only the permissions they need.

---

# RBAC Components

RBAC consists of four primary resources:

- Role
- ClusterRole
- RoleBinding
- ClusterRoleBinding

---

# How RBAC Works

```text
          User / Service Account
                    │
                    ▼
              RoleBinding
                    │
                    ▼
                 Role
                    │
                    ▼
        Allowed Kubernetes Resources
```

---

# Role

A **Role** defines permissions **within a single Namespace**.

Example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role

metadata:
  namespace: development
  name: pod-reader

rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get","list","watch"]
```

This Role allows viewing Pods in the **development** namespace.

---

# ClusterRole

A **ClusterRole** defines permissions across the **entire Kubernetes cluster**.

Example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole

metadata:
  name: pod-reader

rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get","list","watch"]
```

ClusterRoles are used for:

- Cluster-wide permissions
- Nodes
- PersistentVolumes
- StorageClasses
- Namespaces

---

# RoleBinding

A **RoleBinding** grants a Role to a user, group, or Service Account within a Namespace.

Example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding

metadata:
  name: read-pods
  namespace: development

subjects:
- kind: User
  name: developer

roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

---

# ClusterRoleBinding

A **ClusterRoleBinding** grants a ClusterRole across the entire cluster.

Example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding

metadata:
  name: cluster-admin-binding

subjects:
- kind: User
  name: admin

roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
```

---

# Common Verbs

| Verb | Description |
|------|-------------|
| get | View a resource |
| list | List resources |
| watch | Watch for changes |
| create | Create resources |
| update | Modify resources |
| patch | Update part of a resource |
| delete | Remove resources |
| deletecollection | Delete multiple resources |

---

# Common Resources

- Pods
- Deployments
- Services
- ConfigMaps
- Secrets
- Namespaces
- Jobs
- StatefulSets
- DaemonSets

---

# Common Commands

View Roles:

```bash
kubectl get roles
```

View ClusterRoles:

```bash
kubectl get clusterroles
```

View RoleBindings:

```bash
kubectl get rolebindings
```

View ClusterRoleBindings:

```bash
kubectl get clusterrolebindings
```

Describe a Role:

```bash
kubectl describe role pod-reader
```

Apply RBAC configuration:

```bash
kubectl apply -f rbac.yaml
```

Delete a Role:

```bash
kubectl delete role pod-reader
```

---

# Testing Permissions

Check if a user can perform an action:

```bash
kubectl auth can-i create pods
```

Example:

```text
yes
```

Check another user's permission:

```bash
kubectl auth can-i delete pods --as=developer
```

---

# Role vs ClusterRole

| Role | ClusterRole |
|------|-------------|
| Namespace-scoped | Cluster-wide |
| Used within one Namespace | Used across all Namespaces |
| Cannot manage cluster resources | Can manage cluster resources |

---

# RoleBinding vs ClusterRoleBinding

| RoleBinding | ClusterRoleBinding |
|-------------|--------------------|
| Grants a Role in one Namespace | Grants a ClusterRole cluster-wide |
| Namespace-scoped | Cluster-scoped |

---

# Advantages

- Fine-grained access control
- Improves cluster security
- Supports least privilege
- Easy to audit permissions
- Supports multi-team environments

---

# Limitations

- Complex configurations in large clusters
- Incorrect permissions may block applications
- Requires careful planning and maintenance

---

# Best Practices

- Follow the **Principle of Least Privilege**.
- Avoid granting `cluster-admin` unless absolutely necessary.
- Use Roles instead of ClusterRoles whenever possible.
- Grant permissions to **Groups** or **Service Accounts** rather than individual users.
- Regularly audit RBAC permissions.
- Store RBAC manifests in version control.

---

# Interview Questions

### What is RBAC?

RBAC (Role-Based Access Control) is Kubernetes' authorization system that controls access to cluster resources.

---

### What is the difference between a Role and a ClusterRole?

A Role applies to a single Namespace, while a ClusterRole applies across the entire cluster.

---

### What is the purpose of a RoleBinding?

A RoleBinding assigns a Role to a user, group, or Service Account within a Namespace.

---

### Which command checks permissions?

```bash
kubectl auth can-i
```

---

### What security principle should RBAC follow?

The **Principle of Least Privilege**, granting only the permissions required to perform specific tasks.

---

# Key Takeaways

- RBAC controls who can access Kubernetes resources.
- Roles define permissions, while RoleBindings assign them.
- ClusterRoles and ClusterRoleBindings provide cluster-wide access.
- RBAC is essential for securing Kubernetes environments.
- Always grant the minimum permissions required.

---

# References

- Kubernetes Official Documentation
- Kubernetes RBAC Documentation
- Kubernetes Authorization Concepts
- CNCF Kubernetes Security Best Practices