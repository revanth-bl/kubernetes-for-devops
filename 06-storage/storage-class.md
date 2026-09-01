# StorageClass

## Overview

A **StorageClass** is a Kubernetes resource that enables **dynamic provisioning** of Persistent Volumes (PVs).

Instead of requiring an administrator to manually create Persistent Volumes, a StorageClass allows Kubernetes to automatically provision storage when a **PersistentVolumeClaim (PVC)** requests it.

StorageClasses simplify storage management and are the recommended approach for modern Kubernetes clusters.

---

# Why Use a StorageClass?

Without StorageClass:

```text
Administrator
      │
      ▼
Create Persistent Volume
      │
      ▼
PersistentVolumeClaim
      │
      ▼
Application
```

Every Persistent Volume must be created manually.

---

With StorageClass:

```text
Application
      │
      ▼
PersistentVolumeClaim
      │
      ▼
StorageClass
      │
      ▼
Automatically Creates
Persistent Volume
      │
      ▼
Storage Backend
```

Kubernetes provisions storage automatically when needed.

---

# Benefits

- Automatic storage provisioning
- Simplifies storage management
- Supports cloud-native environments
- Reduces manual administration
- Works with multiple storage providers

---

# How StorageClass Works

```text
Application Pod
        │
        ▼
Persistent Volume Claim
        │
        ▼
StorageClass
        │
        ▼
Dynamic Provisioner
        │
        ▼
Persistent Volume
        │
        ▼
Storage Backend
```

The application requests storage through a PVC, and the StorageClass automatically creates an appropriate Persistent Volume.

---

# StorageClass YAML Example

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass

metadata:
  name: standard

provisioner: kubernetes.io/aws-ebs

parameters:
  type: gp3

reclaimPolicy: Delete

volumeBindingMode: WaitForFirstConsumer
```

Create the StorageClass:

```bash
kubectl apply -f storage-class.yaml
```

---

# Using a StorageClass with a PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: app-pvc

spec:
  accessModes:
  - ReadWriteOnce

  storageClassName: standard

  resources:
    requests:
      storage: 5Gi
```

When this PVC is created, Kubernetes automatically provisions a matching Persistent Volume.

---

# Verify StorageClasses

List StorageClasses:

```bash
kubectl get storageclass
```

or

```bash
kubectl get sc
```

Example:

```text
NAME                 PROVISIONER                  RECLAIMPOLICY
standard (default)   kubernetes.io/aws-ebs       Delete
```

Describe a StorageClass:

```bash
kubectl describe storageclass standard
```

---

# Common Provisioners

| Cloud Provider | Provisioner |
|---------------|-------------|
| AWS | ebs.csi.aws.com |
| Azure | disk.csi.azure.com |
| Google Cloud | pd.csi.storage.gke.io |
| NFS | nfs.csi.k8s.io |
| Ceph | rook-ceph.rbd.csi.ceph.com |

> Modern Kubernetes clusters typically use **CSI (Container Storage Interface)** drivers instead of older in-tree provisioners.

---

# Reclaim Policies

## Delete

Deletes the Persistent Volume and underlying storage when the PVC is removed.

```yaml
reclaimPolicy: Delete
```

Best for temporary workloads.

---

## Retain

Preserves the Persistent Volume after the PVC is deleted.

```yaml
reclaimPolicy: Retain
```

Best for important application data.

---

# Volume Binding Modes

## Immediate

The Persistent Volume is created as soon as the PVC is created.

```yaml
volumeBindingMode: Immediate
```

---

## WaitForFirstConsumer

The Persistent Volume is created only after a Pod uses the PVC.

```yaml
volumeBindingMode: WaitForFirstConsumer
```

This helps Kubernetes choose the most appropriate storage location.

---

# Common Commands

Create a StorageClass:

```bash
kubectl apply -f storage-class.yaml
```

View StorageClasses:

```bash
kubectl get sc
```

Describe a StorageClass:

```bash
kubectl describe sc standard
```

Delete a StorageClass:

```bash
kubectl delete sc standard
```

---

# StorageClass vs Persistent Volume

| StorageClass | Persistent Volume |
|--------------|-------------------|
| Defines how storage is created | Actual storage resource |
| Enables dynamic provisioning | Stores application data |
| Uses provisioners | Used by Pods through PVCs |

---

# StorageClass vs PersistentVolumeClaim

| StorageClass | PersistentVolumeClaim |
|--------------|-----------------------|
| Defines storage type | Requests storage |
| Creates Persistent Volumes | Consumes Persistent Volumes |
| Created by administrators | Created by applications/users |

---

# Advantages

- Dynamic storage provisioning
- Less manual administration
- Cloud-native integration
- Supports multiple storage backends
- Simplifies Kubernetes storage management

---

# Limitations

- Requires a supported storage provisioner
- Features depend on the storage backend
- Incorrect configuration may prevent volume provisioning

---

# Best Practices

- Use CSI drivers whenever possible.
- Create separate StorageClasses for different performance tiers (e.g., SSD and HDD).
- Set a default StorageClass for the cluster.
- Use `WaitForFirstConsumer` when supported.
- Choose the appropriate reclaim policy based on application requirements.
- Monitor storage capacity and provisioner health.

---

# Interview Questions

### What is a StorageClass?

A StorageClass defines how Kubernetes dynamically provisions Persistent Volumes.

---

### Why is StorageClass used?

It eliminates the need to manually create Persistent Volumes.

---

### Which Kubernetes resource uses a StorageClass?

A **PersistentVolumeClaim (PVC)** references a StorageClass using the `storageClassName` field.

---

### What is dynamic provisioning?

Dynamic provisioning is the automatic creation of Persistent Volumes when a PVC requests storage.

---

### What is the default StorageClass?

The StorageClass marked with **(default)** is automatically used when a PVC does not specify one.

---

# Key Takeaways

- StorageClasses enable dynamic provisioning of Persistent Volumes.
- PVCs request storage from a StorageClass.
- Kubernetes automatically creates matching Persistent Volumes.
- StorageClasses simplify storage management in production clusters.
- CSI drivers are the recommended method for storage provisioning.

---

# References

- Kubernetes Official Documentation
- Kubernetes StorageClass Documentation
- Container Storage Interface (CSI) Documentation 
- CNCF Kubernetes Storage Concepts ,1 2 3 4 5 6 7 8 9