# Environment Variables

## Overview

Environment Variables are **key-value pairs** that provide configuration values to containers at runtime.

Instead of hardcoding configuration values inside an application, Kubernetes allows environment variables to be defined in Pod specifications or sourced from **ConfigMaps** and **Secrets**.

Environment variables make applications portable, configurable, and easier to manage across different environments.

---

# Why Use Environment Variables?

Without Environment Variables:

```text
Application

↓

Configuration Hardcoded

↓

Rebuild Image for Every Change
```

Problems:

- Difficult to maintain
- Requires rebuilding images
- Environment-specific values are hardcoded

---

With Environment Variables:

```text
Application

↓

Environment Variables

↓

Configuration Changes

↓

No Image Rebuild Required
```

Configuration is separated from application code.

---

# Benefits

- Easy configuration management
- Environment-specific settings
- Better portability
- No application code changes
- Supports ConfigMaps and Secrets

---

# How Environment Variables Work

```text
            ConfigMap / Secret
                    │
                    ▼
          Environment Variables
                    │
                    ▼
              Container Runtime
                    │
                    ▼
               Application
```

Environment variables are injected into the container when it starts.

---

# Basic Environment Variable Example

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod

spec:
  containers:
  - name: nginx
    image: nginx

    env:
    - name: APP_ENV
      value: production

    - name: APP_PORT
      value: "8080"
```

Create the Pod:

```bash
kubectl apply -f pod.yaml
```

---

# Using ConfigMap

ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_ENV: production
  APP_PORT: "8080"
```

Pod:

```yaml
envFrom:
- configMapRef:
    name: app-config
```

The container automatically receives:

```text
APP_ENV=production

APP_PORT=8080
```

---

# Using Secret

Secret:

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: app-secret

type: Opaque

stringData:
  PASSWORD: mypassword
```

Pod:

```yaml
envFrom:
- secretRef:
    name: app-secret
```

The Secret is injected as an environment variable.

---

# Using Individual Values

ConfigMap:

```yaml
data:
  APP_ENV: production
```

Pod:

```yaml
env:
- name: APP_ENV
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_ENV
```

---

# Using Secret Key

```yaml
env:
- name: PASSWORD
  valueFrom:
    secretKeyRef:
      name: app-secret
      key: PASSWORD
```

---

# Viewing Environment Variables

Access the container:

```bash
kubectl exec -it nginx-pod -- sh
```

View variables:

```bash
env
```

Or:

```bash
printenv
```

Example:

```text
APP_ENV=production

APP_PORT=8080
```

---

# Common Commands

Create Pod:

```bash
kubectl apply -f pod.yaml
```

View Pods:

```bash
kubectl get pods
```

Access Pod:

```bash
kubectl exec -it nginx-pod -- sh
```

View Environment Variables:

```bash
env
```

or

```bash
printenv
```

Delete Pod:

```bash
kubectl delete pod nginx-pod
```

---

# Environment Variables vs ConfigMap

| Environment Variables | ConfigMap |
|------------------------|-----------|
| Used by application | Stores configuration |
| Inside container | Kubernetes resource |
| Temporary | Reusable |
| Can use ConfigMap values | Stores configuration data |

---

# Environment Variables vs Secret

| Environment Variables | Secret |
|------------------------|--------|
| Runtime variables | Stores sensitive data |
| Can contain ConfigMap or Secret values | Kubernetes resource |
| Used by applications | Used to inject confidential information |

---

# Advantages

- Keeps configuration separate from application code
- Easy to update for different environments
- Works with ConfigMaps and Secrets
- No need to rebuild container images
- Supported by nearly all programming languages

---

# Limitations

- Values are available only after the container starts.
- Environment variables do not automatically update inside running containers.
- Large amounts of configuration are better managed using mounted files from ConfigMaps or Secrets.

---

# Best Practices

- Store non-sensitive values in ConfigMaps.
- Store passwords, API keys, and certificates in Secrets.
- Use descriptive variable names.
- Avoid hardcoding environment-specific values.
- Keep sensitive information out of source code.
- Use mounted configuration files for large configuration data.

---

# Interview Questions

### What are Environment Variables in Kubernetes?

Environment Variables are key-value pairs passed to containers at runtime to configure application behavior.

---

### Where can Environment Variables come from?

- Pod manifest
- ConfigMap
- Secret

---

### Can Environment Variables be updated without restarting a Pod?

No. Environment variables are loaded when the container starts. Changes require the Pod (or container) to be restarted or recreated.

---

### Which resource should store passwords?

**Secret**

---

### Which resource should store application configuration?

**ConfigMap**

---

# Key Takeaways

- Environment Variables provide runtime configuration to containers.
- They separate configuration from application code.
- Values can come directly from the Pod manifest, ConfigMaps, or Secrets.
- Secrets should be used for confidential information.
- Environment Variables are one of the most common ways to configure Kubernetes applications.

---

# References

- Kubernetes Official Documentation
- Kubernetes Environment Variables Documentation
- Kubernetes ConfigMap Documentation
- Kubernetes Secret Documentation
- CNCF Kubernetes Documentation