# Persistent Volume Claims (PVC)

## Overview

A **PersistentVolumeClaim (PVC)** is a request for persistent storage made by an application.

Instead of directly using a **Persistent Volume (PV)**, Pods request storage through a PVC. Kubernetes automatically binds the PVC to a suitable Persistent Volume that satisfies the requested size and access mode.

PVCs decouple applications from the underlying storage infrastructure, making storage management more flexible and portable.

---

# Why Use PVC?

Without a PVC:

```text
Application
      │
      ▼
Persistent Volume
```

The application must know which Persistent Volume to use.

---

With a PVC:

```text
Application
      │
      ▼
Persistent Volume Claim
      │
      ▼
Persistent Volume
      │
      ▼
Storage Backend
```

Applications request storage without needing to know where or how it is provided.

---

# Benefits

- Simplifies storage management
- Decouples applications from storage
- Supports dynamic provisioning
- Portable across environments
- Easy to resize (if supported)

---

# How PVC Works

```text
Application Pod
        │
        ▼
Persistent Volume Claim
        │
        ▼
Persistent Volume
        │
        ▼
Storage Backend
```

The Pod mounts the PVC, and Kubernetes connects it to a matching Persistent Volume.

---

# PVC Lifecycle

```text
Create PVC
     │
     ▼
Pending
     │
     ▼
Bound to PV
     │
     ▼
Mounted by Pod
     │
     ▼
Released
```

---

# PVC YAML Example

```yaml
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: nginx-pvc

spec:
  accessModes:
  - ReadWriteOnce

  resources:
    requests:
      storage: 2Gi
```

Create the PVC:

```bash
kubectl apply -f pvc.yaml
```

---

# Using PVC in a Pod

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod

spec:
  containers:
  - name: nginx
    image: nginx

    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: storage

  volumes:
  - name: storage
    persistentVolumeClaim:
      claimName: nginx-pvc
```

The Pod mounts the storage requested by `nginx-pvc`.

---

# Verify PVC

List PVCs:

```bash
kubectl get pvc
```

Example:

```text
NAME         STATUS   VOLUME        CAPACITY
nginx-pvc    Bound    pv-storage    2Gi
```

Describe a PVC:

```bash
kubectl describe pvc nginx-pvc
```

---

# PVC Status

| Status | Description |
|---------|-------------|
| Pending | Waiting for a matching PV |
| Bound | Successfully connected to a PV |
| Lost | The bound PV is unavailable |

---

# Access Modes

| Mode | Description |
|------|-------------|
| ReadWriteOnce (RWO) | Mounted as read-write by one node |
| ReadOnlyMany (ROX) | Mounted as read-only by many nodes |
| ReadWriteMany (RWX) | Mounted as read-write by many nodes |
| ReadWriteOncePod (RWOP) | Mounted as read-write by one Pod |

---

# Storage Requests

Specify the required storage size:

```yaml
resources:
  requests:
    storage: 5Gi
```

Kubernetes searches for a matching Persistent Volume.

---

# Common Commands

Create PVC:

```bash
kubectl apply -f pvc.yaml
```

View PVCs:

```bash
kubectl get pvc
```

Describe PVC:

```bash
kubectl describe pvc nginx-pvc
```

Delete PVC:

```bash
kubectl delete pvc nginx-pvc
```

---

# PVC vs PV

| Persistent Volume | Persistent Volume Claim |
|-------------------|-------------------------|
| Actual storage resource | Request for storage |
| Created by administrator or StorageClass | Created by users or applications |
| Supplies storage | Consumes storage |

---

# PVC vs Volume

| Volume | PVC |
|--------|-----|
| Temporary storage | Persistent storage request |
| Exists only with the Pod | Independent of Pod lifecycle |
| Not reusable | Can be reused by Pods |

---

# Advantages

- Simplifies storage allocation
- Supports dynamic provisioning
- Portable across clusters
- Independent of storage implementation
- Enables persistent applications

---

# Limitations

- Requires an available Persistent Volume or StorageClass
- Storage features depend on the underlying storage provider
- Some storage types support limited access modes

---

# Best Practices

- Use PVCs instead of referencing Persistent Volumes directly.
- Request only the storage your application requires.
- Use StorageClasses for dynamic provisioning.
- Monitor PVC status regularly.
- Back up persistent application data.
- Use meaningful names for PVC resources.

---

# Interview Questions

### What is a PersistentVolumeClaim?

A PersistentVolumeClaim is a request for persistent storage made by a Kubernetes application.

---

### Why use a PVC instead of directly using a PV?

PVCs abstract the underlying storage, making applications portable and simplifying storage management.

---

### What happens if no matching Persistent Volume exists?

The PVC remains in the **Pending** state until a suitable PV is available or dynamically provisioned.

---

### Which resource does a Pod mount?

A Pod mounts a **PersistentVolumeClaim (PVC)**, not a Persistent Volume directly.

---

### Can PVCs be dynamically provisioned?

Yes. When used with a **StorageClass**, Kubernetes can automatically create a matching Persistent Volume.

---

# Key Takeaways

- PVCs are requests for persistent storage.
- Pods use PVCs to access Persistent Volumes.
- Kubernetes automatically binds compatible PVs to PVCs.
- PVCs simplify storage management and improve portability.
- StorageClasses enable automatic PV creation for PVCs.

---

# References

- Kubernetes Official Documentation
- Kubernetes PersistentVolumeClaim Documentation
- Kubernetes Storage Concepts
- CNCF Kubernetes Documentation