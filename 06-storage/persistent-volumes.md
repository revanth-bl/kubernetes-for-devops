# Persistent Volumes (PV)

## Overview

A **Persistent Volume (PV)** is a cluster-wide storage resource in Kubernetes that provides **persistent storage** for applications.

Unlike regular Pod volumes, a Persistent Volume **continues to exist even if the Pod is deleted or restarted**, allowing applications to retain their data.

Persistent Volumes are managed by the cluster administrator or dynamically provisioned using a **StorageClass**.

---

# Why Use Persistent Volumes?

Without Persistent Volumes:

```text
Pod
 │
 ▼
Temporary Volume

Pod Deleted

↓

Data Lost
```

With Persistent Volumes:

```text
Pod
 │
 ▼
Persistent Volume Claim (PVC)
 │
 ▼
Persistent Volume (PV)
 │
 ▼
Physical Storage
```

Even if the Pod is recreated, the data remains available.

---

# Benefits

- Persistent storage
- Data survives Pod restarts
- Independent of Pod lifecycle
- Supports multiple storage providers
- Can be reused by different Pods

---

# How Persistent Volumes Work

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
(AWS EBS, Azure Disk, NFS, etc.)
```

The Pod never accesses the Persistent Volume directly—it uses a **PersistentVolumeClaim (PVC)**.

---

# Persistent Volume Lifecycle

```text
Create PV
     │
     ▼
Available
     │
     ▼
Bound to PVC
     │
     ▼
Used by Pod
     │
     ▼
Released
     │
     ▼
Reclaimed or Deleted
```

---

# Persistent Volume YAML Example

```yaml
apiVersion: v1
kind: PersistentVolume

metadata:
  name: pv-storage

spec:
  capacity:
    storage: 5Gi

  accessModes:
  - ReadWriteOnce

  persistentVolumeReclaimPolicy: Retain

  hostPath:
    path: /data/pv
```

Create the PV:

```bash
kubectl apply -f persistent-volume.yaml
```

---

# Verify Persistent Volumes

List Persistent Volumes:

```bash
kubectl get pv
```

Example:

```text
NAME         CAPACITY   ACCESS MODES   STATUS
pv-storage   5Gi        RWO            Available
```

Describe a Persistent Volume:

```bash
kubectl describe pv pv-storage
```

---

# Access Modes

| Access Mode | Description |
|--------------|-------------|
| ReadWriteOnce (RWO) | Mounted as read-write by a single node |
| ReadOnlyMany (ROX) | Mounted as read-only by multiple nodes |
| ReadWriteMany (RWX) | Mounted as read-write by multiple nodes |
| ReadWriteOncePod (RWOP) | Mounted as read-write by only one Pod |

---

# Reclaim Policies

## Retain

The Persistent Volume is preserved after the PVC is deleted.

```yaml
persistentVolumeReclaimPolicy: Retain
```

Use when data must not be lost.

---

## Delete

The storage resource is automatically deleted when the PVC is removed.

```yaml
persistentVolumeReclaimPolicy: Delete
```

Commonly used with cloud storage.

---

## Recycle (Deprecated)

Previously erased data and reused the volume.

> This policy is deprecated and should not be used.

---

# Storage Providers

Persistent Volumes can use various storage backends, including:

- AWS Elastic Block Store (EBS)
- Azure Managed Disks
- Google Persistent Disk
- NFS
- Ceph
- iSCSI
- Local Storage

---

# Common Commands

Create a PV:

```bash
kubectl apply -f persistent-volume.yaml
```

View PVs:

```bash
kubectl get pv
```

Describe a PV:

```bash
kubectl describe pv pv-storage
```

Delete a PV:

```bash
kubectl delete pv pv-storage
```

---

# Persistent Volume vs Volume

| Feature | Volume | Persistent Volume |
|----------|--------|-------------------|
| Lifetime | Pod Lifetime | Independent of Pod |
| Data Persistence | ❌ No | ✅ Yes |
| Managed By | Pod | Kubernetes Cluster |
| Reusable | ❌ | ✅ |

---

# Persistent Volume vs PersistentVolumeClaim

| Persistent Volume | PersistentVolumeClaim |
|-------------------|-----------------------|
| Actual Storage Resource | Request for Storage |
| Created by Admin or StorageClass | Created by User/Application |
| Supplies Storage | Consumes Storage |

---

# Advantages

- Data survives Pod deletion
- Supports multiple storage backends
- Independent of Pod lifecycle
- Enables persistent databases
- Easy integration with cloud storage

---

# Limitations

- Requires proper storage configuration
- Some access modes depend on the storage provider
- Static provisioning requires manual management

---

# Best Practices

- Use PersistentVolumeClaims instead of directly referencing Persistent Volumes.
- Select appropriate access modes based on application requirements.
- Use dynamic provisioning with StorageClasses whenever possible.
- Choose an appropriate reclaim policy.
- Regularly back up persistent data.
- Monitor storage usage and capacity.

---

# Interview Questions

### What is a Persistent Volume?

A Persistent Volume is a cluster-managed storage resource that exists independently of Pods.

---

### Why use a Persistent Volume?

To ensure application data survives Pod restarts, failures, and recreation.

---

### What is the default way applications access a PV?

Applications access Persistent Volumes through a **PersistentVolumeClaim (PVC)**.

---

### What are the common access modes?

- ReadWriteOnce (RWO)
- ReadOnlyMany (ROX)
- ReadWriteMany (RWX)
- ReadWriteOncePod (RWOP)

---

### What is the difference between Retain and Delete reclaim policies?

- **Retain:** Keeps the storage after the PVC is deleted.
- **Delete:** Removes the storage resource automatically.

---

# Key Takeaways

- Persistent Volumes provide long-term storage in Kubernetes.
- They are independent of Pod lifecycles.
- Applications use PVs through PersistentVolumeClaims.
- Persistent Volumes support multiple storage providers.
- StorageClasses enable automatic creation of Persistent Volumes.

---

# References

- Kubernetes Official Documentation
- Kubernetes Persistent Volumes Documentation
- Kubernetes Storage Concepts
- CNCF Kubernetes Documentation