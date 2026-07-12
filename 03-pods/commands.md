# Pod Commands Cheat Sheet

## Overview

This document contains commonly used **kubectl** commands for creating, managing, debugging, and deleting Kubernetes Pods.

---

# Create Pods

Create a Pod using a YAML file:

```bash
kubectl apply -f pod.yaml
```

Create a Pod directly from an image:

```bash
kubectl run nginx --image=nginx
```

Create a Pod in a specific namespace:

```bash
kubectl run nginx --image=nginx -n dev
```

---

# View Pods

List Pods in the current namespace:

```bash
kubectl get pods
```

List Pods with additional details:

```bash
kubectl get pods -o wide
```

List Pods in all namespaces:

```bash
kubectl get pods -A
```

Watch Pods in real time:

```bash
kubectl get pods --watch
```

---

# Describe Pods

Display detailed information about a Pod:

```bash
kubectl describe pod <pod-name>
```

Example:

```bash
kubectl describe pod nginx
```

---

# View Logs

View Pod logs:

```bash
kubectl logs <pod-name>
```

Follow logs in real time:

```bash
kubectl logs -f <pod-name>
```

View logs from a specific container:

```bash
kubectl logs <pod-name> -c <container-name>
```

---

# Execute Commands Inside a Pod

Open a Bash shell:

```bash
kubectl exec -it <pod-name> -- /bin/bash
```

If Bash is unavailable:

```bash
kubectl exec -it <pod-name> -- sh
```

Run a single command:

```bash
kubectl exec <pod-name> -- ls /
```

---

# Copy Files

Copy a file from your computer to a Pod:

```bash
kubectl cp file.txt <pod-name>:/tmp/
```

Copy a file from a Pod:

```bash
kubectl cp <pod-name>:/tmp/file.txt .
```

---

# Delete Pods

Delete a specific Pod:

```bash
kubectl delete pod <pod-name>
```

Delete using a YAML file:

```bash
kubectl delete -f pod.yaml
```

Delete all Pods in the current namespace:

```bash
kubectl delete pods --all
```

Force delete a Pod:

```bash
kubectl delete pod <pod-name> --force --grace-period=0
```

---

# Pod Labels

Show Pod labels:

```bash
kubectl get pods --show-labels
```

Add a label:

```bash
kubectl label pod <pod-name> app=web
```

Remove a label:

```bash
kubectl label pod <pod-name> app-
```

---

# Pod Annotations

Add an annotation:

```bash
kubectl annotate pod <pod-name> owner=devops
```

Remove an annotation:

```bash
kubectl annotate pod <pod-name> owner-
```

---

# Resource Information

View CPU and memory usage:

```bash
kubectl top pod
```

View Pod YAML:

```bash
kubectl get pod <pod-name> -o yaml
```

View Pod JSON:

```bash
kubectl get pod <pod-name> -o json
```

---

# Port Forwarding

Access a Pod locally:

```bash
kubectl port-forward pod/<pod-name> 8080:80
```

Example:

```bash
kubectl port-forward pod/nginx 8080:80
```

---

# Debugging

View Pod events:

```bash
kubectl describe pod <pod-name>
```

Check Pod status:

```bash
kubectl get pod <pod-name>
```

View recent events:

```bash
kubectl get events
```

View logs:

```bash
kubectl logs <pod-name>
```

---

# Useful Commands

Count running Pods:

```bash
kubectl get pods
```

Delete completed Pods:

```bash
kubectl delete pod --field-selector=status.phase==Succeeded
```

Delete failed Pods:

```bash
kubectl delete pod --field-selector=status.phase==Failed
```

---

# Common kubectl Output Options

Wide output:

```bash
kubectl get pods -o wide
```

YAML output:

```bash
kubectl get pod <pod-name> -o yaml
```

JSON output:

```bash
kubectl get pod <pod-name> -o json
```

Custom columns:

```bash
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase
```

---

# Troubleshooting Commands

Check cluster information:

```bash
kubectl cluster-info
```

Check nodes:

```bash
kubectl get nodes
```

View namespaces:

```bash
kubectl get namespaces
```

View events:

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

Check API resources:

```bash
kubectl api-resources
```

---

# Most Frequently Used Commands

```bash
kubectl apply -f pod.yaml
kubectl get pods
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- bash
kubectl delete pod <pod-name>
kubectl port-forward pod/<pod-name> 8080:80
kubectl get pod <pod-name> -o yaml
kubectl get events
```

---

# Best Practices

- Use YAML manifests instead of imperative commands whenever possible.
- Assign meaningful labels to Pods.
- Use `kubectl describe` before troubleshooting logs.
- Keep Pods lightweight and focused on a single responsibility.
- Use Deployments instead of standalone Pods for production workloads.
- Monitor Pod resource usage regularly.

---

# References

- Kubernetes Official Documentation
- kubectl Command Reference
- Kubernetes Pods Documentation