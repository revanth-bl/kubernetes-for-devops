# ConfigMaps

## Overview

A **ConfigMap** is a Kubernetes resource used to store **non-sensitive configuration data** as key-value pairs.

Instead of hardcoding configuration values inside container images, ConfigMaps allow applications to load configuration at runtime. This makes applications more portable, reusable, and easier to manage across different environments.

ConfigMaps are commonly used to store:

- Environment variables
- Configuration files
- Command-line arguments
- Application settings

> **Note:** ConfigMaps should **not** be used for sensitive information such as passwords or API keys. Use **Secrets** for sensitive data.

---

# Why Use ConfigMaps?

Without ConfigMap:

```text
Application
      │
      ▼
Configuration Hardcoded
Inside Container Image

Changing Configuration

↓

Rebuild Image
Redeploy Application
```

---

With ConfigMap:

```text
Application
      │
      ▼
ConfigMap
      │
      ▼
Configuration Data

Update ConfigMap

↓

Restart Pod (if required)
```

The application configuration is separated from the application code.

---

# Benefits

- Separates configuration from application code
- Easy to update configuration
- Reusable across multiple Pods
- Supports environment-specific settings
- Improves application portability

---

# How ConfigMaps Work

```text
             ConfigMap
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
 Environment Variables   Mounted Files
        │                   │
        └─────────┬─────────┘
                  ▼
             Application Pod
```

ConfigMaps can be consumed as:

- Environment variables
- Command-line arguments
- Mounted configuration files

---

# ConfigMap YAML Example

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_NAME: MyApplication
  APP_ENV: production
  APP_PORT: "8080"
```

Create the ConfigMap:

```bash
kubectl apply -f configmap.yaml
```

---

# Using ConfigMap as Environment Variables

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
    - configMapRef:
        name: app-config
```

The container automatically receives:

```text
APP_NAME=MyApplication
APP_ENV=production
APP_PORT=8080
```

---

# Using ConfigMap as Individual Environment Variables

```yaml
env:
- name: APP_ENV
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_ENV
```

---

# Using ConfigMap as a Volume

```yaml
volumes:
- name: config-volume
  configMap:
    name: app-config
```

Mount the volume:

```yaml
volumeMounts:
- name: config-volume
  mountPath: /etc/config
```

The configuration values become files inside the mounted directory.

---

# Creating ConfigMaps from the Command Line

Create from literals:

```bash
kubectl create configmap app-config \
--from-literal=APP_NAME=MyApplication \
--from-literal=APP_ENV=production
```

Create from a file:

```bash
kubectl create configmap app-config \
--from-file=config.properties
```

Create from a directory:

```bash
kubectl create configmap app-config \
--from-file=./config/
```

---

# Verify ConfigMaps

List ConfigMaps:

```bash
kubectl get configmaps
```

or

```bash
kubectl get cm
```

Describe a ConfigMap:

```bash
kubectl describe configmap app-config
```

View YAML:

```bash
kubectl get configmap app-config -o yaml
```

---

# Common Commands

Create:

```bash
kubectl apply -f configmap.yaml
```

View:

```bash
kubectl get cm
```

Describe:

```bash
kubectl describe cm app-config
```

Delete:

```bash
kubectl delete configmap app-config
```

---

# ConfigMap vs Secret

| Feature | ConfigMap | Secret |
|----------|------------|---------|
| Stores Configuration | ✅ | ✅ |
| Stores Sensitive Data | ❌ | ✅ |
| Base64 Encoded | ❌ | ✅ |
| Recommended for Passwords | ❌ | ✅ |

---

# ConfigMap vs Environment Variables

| ConfigMap | Hardcoded Environment Variables |
|------------|---------------------------------|
| External configuration | Embedded in Pod manifest |
| Reusable | Not reusable |
| Easier to update | Requires manifest changes |
| Better separation of concerns | Tightly coupled |

---

# Advantages

- Decouples configuration from applications
- Easy to manage across environments
- Reusable by multiple Pods
- Supports multiple configuration formats
- Simplifies deployments

---

# Limitations

- Not suitable for sensitive information
- Pods may need to be restarted to use updated values (depending on how the application reads configuration)
- Large configuration files can become difficult to manage

---

# Best Practices

- Store only non-sensitive configuration in ConfigMaps.
- Use Secrets for passwords, API keys, and certificates.
- Keep ConfigMaps focused on a single application or component.
- Use descriptive names.
- Store ConfigMap manifests in version control.
- Avoid hardcoding configuration values inside container images.

---

# Interview Questions

### What is a ConfigMap?

A ConfigMap is a Kubernetes resource used to store non-sensitive configuration data for applications.

---

### Can ConfigMaps store passwords?

No. Passwords and other sensitive information should be stored in Kubernetes Secrets.

---

### How can a Pod use a ConfigMap?

A Pod can use a ConfigMap as:

- Environment variables
- Individual configuration values
- Mounted files inside a volume

---

### Can multiple Pods use the same ConfigMap?

Yes. A single ConfigMap can be shared by multiple Pods.

---

### What happens when a ConfigMap is updated?

Mounted ConfigMaps may be updated automatically after a short delay, but applications often need to reload the configuration or restart to use the new values. Environment variables sourced from ConfigMaps are set when the container starts and require the Pod to be recreated or restarted.

---

# Key Takeaways

- ConfigMaps store non-sensitive configuration data.
- They separate configuration from application code.
- Applications can consume ConfigMaps as environment variables or mounted files.
- ConfigMaps improve portability and simplify configuration management.
- Secrets should be used instead of ConfigMaps for confidential information.

---

# References

- Kubernetes Official Documentation
- Kubernetes ConfigMap Documentation
- Kubernetes Configuration Best Practices
- CNCF Kubernetes Documentation