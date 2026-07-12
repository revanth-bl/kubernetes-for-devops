# Network Policies

## Overview

A **NetworkPolicy** is a Kubernetes resource used to control **network communication between Pods** within a cluster.

By default, Kubernetes allows **all Pods to communicate with each other**. Network Policies introduce rules that define **which Pods can send or receive traffic**, improving cluster security.

Network Policies work only if your cluster uses a **Container Network Interface (CNI)** plugin that supports them, such as **Calico**, **Cilium**, or **Weave Net**.

---

# Why Use Network Policies?

Without Network Policies:

```text
            Kubernetes Cluster

     Pod A  ─────────────► Pod B
       │                    ▲
       │                    │
       └────────────► Pod C
```

Every Pod can communicate with every other Pod.

---

With Network Policies:

```text
            Kubernetes Cluster

     Pod A ─────────────► Pod B

     Pod C     ✖────────► Pod B
```

Only authorized Pods are allowed to communicate.

---

# Benefits

- Improve cluster security
- Restrict unnecessary communication
- Implement Zero Trust networking
- Protect sensitive applications
- Control ingress and egress traffic

---

# How Network Policies Work

```text
                Network Policy
                      │
          -------------------------
          │                       │
      Ingress Rules          Egress Rules
          │                       │
          ▼                       ▼
     Incoming Traffic      Outgoing Traffic
```

Network Policies define:

- Which Pods can receive traffic (**Ingress**)
- Which Pods can send traffic (**Egress**)

---

# Types of Policies

## Ingress Policy

Controls incoming traffic to a Pod.

```
Allowed Pods
      │
      ▼
Target Pod
```

---

## Egress Policy

Controls outgoing traffic from a Pod.

```
Source Pod
     │
     ▼
Allowed Destination
```

---

## Ingress + Egress

A single NetworkPolicy can manage both inbound and outbound traffic.

---

# Basic NetworkPolicy Example

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy

metadata:
  name: allow-frontend

spec:
  podSelector:
    matchLabels:
      app: backend

  policyTypes:
  - Ingress

  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
```

Deploy:

```bash
kubectl apply -f network-policy.yaml
```

This policy allows only Pods labeled **app=frontend** to access Pods labeled **app=backend**.

---

# Deny All Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy

metadata:
  name: deny-all

spec:
  podSelector: {}

  policyTypes:
  - Ingress
```

This blocks all incoming traffic to all Pods in the namespace.

---

# Allow Specific Namespace

```yaml
ingress:
- from:
  - namespaceSelector:
      matchLabels:
        environment: production
```

Only Pods in namespaces labeled **environment=production** can connect.

---

# Allow Specific Port

```yaml
ingress:
- ports:
  - protocol: TCP
    port: 80
```

Only TCP traffic on port **80** is allowed.

---

# Egress Example

```yaml
policyTypes:
- Egress

egress:
- to:
  - podSelector:
      matchLabels:
        app: database
```

Allows communication only with Pods labeled **app=database**.

---

# Verify Network Policies

List policies:

```bash
kubectl get networkpolicy
```

or

```bash
kubectl get netpol
```

Describe a policy:

```bash
kubectl describe networkpolicy allow-frontend
```

Delete a policy:

```bash
kubectl delete networkpolicy allow-frontend
```

---

# Common Commands

Create:

```bash
kubectl apply -f network-policy.yaml
```

View:

```bash
kubectl get netpol
```

Describe:

```bash
kubectl describe netpol allow-frontend
```

Delete:

```bash
kubectl delete netpol allow-frontend
```

---

# NetworkPolicy Components

| Component | Purpose |
|-----------|---------|
| podSelector | Selects the Pods affected by the policy |
| policyTypes | Defines Ingress and/or Egress rules |
| ingress | Controls incoming traffic |
| egress | Controls outgoing traffic |
| namespaceSelector | Selects namespaces |
| ports | Restricts traffic to specific ports |

---

# NetworkPolicy vs Service

| Feature | NetworkPolicy | Service |
|----------|---------------|----------|
| Controls Traffic | ✅ | ❌ |
| Load Balancing | ❌ | ✅ |
| Service Discovery | ❌ | ✅ |
| Security | ✅ | ❌ |

---

# NetworkPolicy vs Firewall

| Feature | NetworkPolicy | Traditional Firewall |
|----------|---------------|----------------------|
| Kubernetes Aware | ✅ | ❌ |
| Pod-Based Rules | ✅ | ❌ |
| Namespace Support | ✅ | ❌ |
| Cluster Internal | ✅ | Limited |

---

# Advantages

- Improves application security
- Limits lateral movement within the cluster
- Supports Zero Trust networking
- Fine-grained traffic control
- Namespace-level isolation
- Works with labels for flexible policies

---

# Limitations

- Requires a compatible CNI plugin
- Can become complex in large environments
- Incorrect rules may block legitimate traffic

---

# Best Practices

- Apply a **default deny** policy before allowing required traffic.
- Use labels consistently for Pod selection.
- Keep policies simple and easy to understand.
- Test policies in development before production.
- Document all NetworkPolicies.
- Regularly review and audit network rules.

---

# Key Takeaways

- Network Policies control Pod-to-Pod communication.
- By default, Kubernetes allows unrestricted communication.
- Policies define **Ingress**, **Egress**, or both.
- Network Policies require a supported CNI plugin.
- They are essential for securing production Kubernetes clusters.

---

# References

- Kubernetes Official Documentation
- Kubernetes NetworkPolicy Documentation
- Calico Documentation
- Cilium Documentation
- CNCF Kubernetes Networking Concepts