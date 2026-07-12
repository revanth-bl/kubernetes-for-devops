# Secrets

## Overview

A **Secret** is a Kubernetes resource used to securely store and manage **sensitive information**, such as:

- Passwords
- API Keys
- Database Credentials
- SSH Keys
- TLS Certificates
- OAuth Tokens

Instead of hardcoding sensitive data into application code or container images, Kubernetes Secrets allow applications to securely access confidential information at runtime.

> **Note:** Secrets are **Base64 encoded**, not encrypted by default. For production environments, enable **Encryption at Rest** and implement proper RBAC policies.

---

# Why Use Secrets?

Without Secrets:

```text
Application

↓

Password Hardcoded

↓

Container Image

↓

Git Repository
```

Problems:

- Credentials exposed
- Difficult to rotate
- Security risks

---

With Secrets:

```text
Application
      │
      ▼
Kubernetes Secret
      │
      ▼
Environment Variable
or Mounted File
```

Sensitive data is separated from application code.

---

# Benefits

- Secure credential management
- Separate secrets from application code
- Easy credential rotation
- Supports environment variables and volumes
- Works with RBAC for access control

---

# How Secrets Work

```text
             Secret
                │
      ┌─────────┴─────────┐
      ▼                   ▼
Environment Variables   Mounted Files
      │                   │
      └─────────┬─────────┘
                ▼
           Application Pod
```

Applications can consume Secrets as:

- Environment Variables
- Mounted Volumes
- Individual Secret Keys

---

# Secret Types

| Type | Purpose |
|------|---------|
| Opaque | Generic user-defined secret |
| kubernetes.io/tls | TLS certificates |
| kubernetes.io/dockerconfigjson | Docker registry credentials |
| kubernetes.io/basic-auth | Username and password |
| kubernetes.io/ssh-auth | SSH private keys |
| kubernetes.io/service-account-token | Service Account tokens |

---

# Secret YAML Example

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: app-secret

type: Opaque

stringData:
  USERNAME: admin
  PASSWORD: mypassword
```

Create the Secret:

```bash
kubectl apply -f secret.yaml
```

---

# Using Secrets as Environment Variables

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod

spec:
  containers:
  - name: nginx
    image: nginx

    envFrom:
    - secretRef:
        name: app-secret
```

The container receives:

```text
USERNAME=admin

PASSWORD=mypassword
```

---

# Using Individual Secret Values

```yaml
env:
- name: PASSWORD
  valueFrom:
    secretKeyRef:
      name: app-secret
      key: PASSWORD
```

---

# Using Secrets as Volumes

```yaml
volumes:
- name: secret-volume
  secret:
    secretName: app-secret
```

Mount the volume:

```yaml
volumeMounts:
- name: secret-volume
  mountPath: /etc/secrets
  readOnly: true
```

Each Secret key becomes a file inside `/etc/secrets`.

---

# Creating Secrets from the Command Line

Create from literals:

```bash
kubectl create secret generic app-secret \
--from-literal=USERNAME=admin \
--from-literal=PASSWORD=mypassword
```

Create from a file:

```bash
kubectl create secret generic app-secret \
--from-file=config.txt
```

Create a TLS Secret:

```bash
kubectl create secret tls tls-secret \
--cert=certificate.crt \
--key=private.key
```

---

# Verify Secrets

List Secrets:

```bash
kubectl get secrets
```

Describe a Secret:

```bash
kubectl describe secret app-secret
```

View Secret YAML:

```bash
kubectl get secret app-secret -o yaml
```

Decode a Secret:

```bash
kubectl get secret app-secret \
-o jsonpath="{.data.PASSWORD}" | base64 --decode
```

---

# Common Commands

Create:

```bash
kubectl apply -f secret.yaml
```

View:

```bash
kubectl get secrets
```

Describe:

```bash
kubectl describe secret app-secret
```

Delete:

```bash
kubectl delete secret app-secret
```

---

# Secrets vs ConfigMaps

| Feature | Secret | ConfigMap |
|---------|--------|-----------|
| Sensitive Data | ✅ | ❌ |
| Base64 Encoded | ✅ | ❌ |
| Stores Passwords | ✅ | ❌ |
| Stores Configuration | Limited | ✅ |

---

# Secrets vs Environment Variables

| Secret | Environment Variable |
|---------|----------------------|
| Kubernetes resource | Runtime configuration |
| Stores confidential data | Consumes Secret values |
| Managed by Kubernetes | Used by applications |

---

# Advantages

- Keeps sensitive data out of application code
- Easy credential rotation
- Supports environment variables and mounted files
- Integrates with RBAC
- Supports multiple secret types

---

# Limitations

- Base64 encoding is **not encryption**.
- Requires proper RBAC permissions.
- Secrets exposed as environment variables remain visible to processes inside the container.
- Additional security measures are recommended for production.

---

# Security Best Practices

- Never store passwords in ConfigMaps.
- Enable **Encryption at Rest** in Kubernetes.
- Apply **Role-Based Access Control (RBAC)** to limit Secret access.
- Rotate credentials regularly.
- Mount Secrets as read-only volumes when possible.
- Avoid committing Secret manifests containing real credentials to Git repositories.
- Use external secret managers (such as HashiCorp Vault or cloud secret services) for production environments when appropriate.

---

# Interview Questions

### What is a Kubernetes Secret?

A Secret is a Kubernetes resource used to securely store sensitive information such as passwords, API keys, and certificates.

---

### Are Kubernetes Secrets encrypted?

By default, they are **Base64 encoded**, not encrypted. Encryption at Rest should be enabled for production clusters.

---

### How can a Pod use a Secret?

- As environment variables
- As mounted files (volumes)
- As individual key-value pairs

---

### What is the default Secret type?

**Opaque**

---

### Should passwords be stored in ConfigMaps?

No. Passwords and other confidential information should always be stored in **Secrets**.

---

# Key Takeaways

- Secrets securely store sensitive application data.
- Applications can access Secrets through environment variables or mounted volumes.
- Secrets are Base64 encoded and should be protected using RBAC and Encryption at Rest.
- Use Secrets for passwords, tokens, certificates, and API keys.
- Never hardcode sensitive information into container images or source code.

---

# References

- Kubernetes Official Documentation
- Kubernetes Secrets Documentation
- Kubernetes Security Best Practices
- CNCF Kubernetes Documentation