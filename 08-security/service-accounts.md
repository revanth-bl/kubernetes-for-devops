# Service Accounts

## Overview

A **Service Account** is a Kubernetes identity used by **Pods and applications** to authenticate with the Kubernetes API.

Unlike regular user accounts, which represent humans, Service Accounts are designed for workloads running inside the cluster. They allow applications to securely interact with Kubernetes resources based on the permissions granted through **Role-Based Access Control (RBAC)**.

Every Namespace automatically contains a **default Service Account**, which Pods use unless another Service Account is specified.

---

# Why Use Service Accounts?

Without a Service Account:

```text
Application

↓

No Kubernetes Identity

↓

Cannot Access Kubernetes API
```

Problems:

- No authentication
- No authorization
- Applications cannot manage cluster resources

---

With a Service Account:

```text
Application Pod
        │
        ▼
Service Account
        │
        ▼
Authentication
        │
        ▼
RBAC Permissions
        │
        ▼
Kubernetes API Server
```

The application can securely communicate with the Kubernetes API.

---

# Benefits

- Provides identity for Pods
- Secure Kubernetes API authentication
- Integrates with RBAC
- Supports least privilege
- Namespace-specific by default

---

# How Service Accounts Work

```text
Application Pod
        │
        ▼
Service Account
        │
        ▼
Authentication Token
        │
        ▼
API Server
        │
        ▼
RBAC Authorization
        │
        ▼
Allowed Kubernetes Resources
```

The API Server authenticates the Service Account and checks its permissions through RBAC.

---

# Default Service Account

Every Namespace contains a default Service Account.

View it:

```bash
kubectl get serviceaccounts
```

or

```bash
kubectl get sa
```

Example:

```text
NAME      SECRETS   AGE
default   0         20d
```

> Kubernetes automatically assigns the `default` Service Account to Pods unless another Service Account is specified.

---

# Create a Service Account

YAML Example:

```yaml
apiVersion: v1
kind: ServiceAccount

metadata:
  name: app-service-account
```

Create it:

```bash
kubectl apply -f service-account.yaml
```

Or directly:

```bash
kubectl create serviceaccount app-service-account
```

---

# Using a Service Account in a Pod

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx

spec:
  serviceAccountName: app-service-account

  containers:
  - name: nginx
    image: nginx
```

The Pod now authenticates as `app-service-account`.

---

# Grant Permissions with RBAC

Service Accounts gain permissions through Roles and RoleBindings.

Example RoleBinding:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding

metadata:
  name: app-binding

subjects:
- kind: ServiceAccount
  name: app-service-account
  namespace: default

roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

This allows the Service Account to use the permissions defined in the `pod-reader` Role.

---

# Verify Service Accounts

List Service Accounts:

```bash
kubectl get sa
```

Describe a Service Account:

```bash
kubectl describe sa app-service-account
```

View YAML:

```bash
kubectl get sa app-service-account -o yaml
```

---

# Common Commands

Create:

```bash
kubectl create serviceaccount app-service-account
```

View:

```bash
kubectl get sa
```

Describe:

```bash
kubectl describe sa app-service-account
```

Delete:

```bash
kubectl delete sa app-service-account
```

---

# Service Account vs User Account

| Service Account | User Account |
|-----------------|--------------|
| Used by applications | Used by humans |
| Namespace-scoped | External to Kubernetes |
| Managed by Kubernetes | Managed by external identity providers |
| Authenticates Pods | Authenticates administrators and users |

---

# Service Account vs RBAC

| Service Account | RBAC |
|-----------------|------|
| Provides identity | Provides permissions |
| Authentication | Authorization |
| Used by Pods | Used to define access rules |

---

# Advantages

- Secure authentication for applications
- Works seamlessly with RBAC
- Supports least privilege
- Namespace isolation
- Easy to manage and automate

---

# Limitations

- A Service Account alone does not grant permissions.
- Excessive RBAC permissions can create security risks.
- Service Accounts should be reviewed and audited regularly.

---

# Best Practices

- Create dedicated Service Accounts for applications instead of relying on the `default` Service Account.
- Grant only the minimum required permissions using RBAC.
- Avoid assigning `cluster-admin` unless absolutely necessary.
- Regularly audit Service Accounts and RoleBindings.
- Use separate Service Accounts for different workloads.
- Disable automatic Service Account token mounting for Pods that do not need to access the Kubernetes API by setting:

```yaml
automountServiceAccountToken: false
```

---

# Interview Questions

### What is a Service Account?

A Service Account is a Kubernetes identity used by Pods and applications to authenticate with the Kubernetes API.

---

### Does a Service Account provide permissions by itself?

No. A Service Account only provides an identity. Permissions are granted through RBAC (Roles and RoleBindings).

---

### What Service Account is assigned to a Pod by default?

The **default** Service Account in the Pod's Namespace.

---

### Why should you avoid using the default Service Account for production applications?

Because it may have broader permissions than necessary and makes it harder to follow the Principle of Least Privilege.

---

### How do you assign a Service Account to a Pod?

By specifying the `serviceAccountName` field in the Pod specification.

---

# Key Takeaways

- Service Accounts provide identities for Pods and applications.
- They authenticate to the Kubernetes API.
- RBAC determines what a Service Account is allowed to do.
- Every Namespace has a default Service Account.
- Using dedicated Service Accounts with least-privilege RBAC is a Kubernetes security best practice.

---

# References

- Kubernetes Official Documentation
- Kubernetes Service Accounts Documentation
- Kubernetes Authentication Documentation
- Kubernetes RBAC Documentation
- CNCF Kubernetes Documentation