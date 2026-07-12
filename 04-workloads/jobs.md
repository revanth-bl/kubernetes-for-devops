# Jobs

## Overview

A **Job** is a Kubernetes workload resource used to run a task **to completion**.

Unlike a Deployment, which continuously keeps Pods running, a Job creates one or more Pods, waits for them to successfully complete, and then terminates them.

Jobs are ideal for tasks that only need to run once, such as database migrations, backups, data processing, or batch workloads.

---

# Why Use a Job?

Jobs are commonly used for:

- Database migrations
- Data processing
- Backup operations
- Batch processing
- Report generation
- One-time administrative tasks

---

# How a Job Works

```text
            Job
             │
             ▼
         Create Pod
             │
             ▼
      Execute Task
             │
             ▼
    Task Completed?
        │        │
       No        Yes
       │          │
 Restart Pod   Job Complete
```

The Job controller ensures the task completes successfully.

If the Pod fails, Kubernetes creates another Pod until the task succeeds or the retry limit is reached.

---

# Features

- Runs tasks to completion
- Automatically retries failed Pods
- Tracks successful completions
- Supports parallel execution
- Suitable for batch workloads

---

# Job YAML Example

```yaml
apiVersion: batch/v1
kind: Job

metadata:
  name: hello-job

spec:
  template:
    spec:
      containers:
      - name: hello
        image: busybox
        command: ["echo", "Hello Kubernetes"]

      restartPolicy: Never

  backoffLimit: 4
```

Deploy the Job:

```bash
kubectl apply -f job.yaml
```

---

# Verify Job

List Jobs:

```bash
kubectl get jobs
```

Example:

```text
NAME         COMPLETIONS   DURATION   AGE
hello-job    1/1           8s         15s
```

View Pods created by the Job:

```bash
kubectl get pods
```

Describe the Job:

```bash
kubectl describe job hello-job
```

---

# View Job Logs

Find the Pod:

```bash
kubectl get pods
```

View logs:

```bash
kubectl logs <pod-name>
```

---

# Delete a Job

Delete using its name:

```bash
kubectl delete job hello-job
```

Delete using YAML:

```bash
kubectl delete -f job.yaml
```

---

# Parallel Jobs

A Job can execute multiple Pods simultaneously.

Example:

```yaml
spec:
  completions: 5
  parallelism: 2
```

Explanation:

- **completions** = Total successful executions required.
- **parallelism** = Number of Pods running simultaneously.

---

# Retry Policy

If a Pod fails, Kubernetes retries it automatically.

Example:

```yaml
backoffLimit: 3
```

This allows up to **3 retries** before marking the Job as failed.

---

# Job Lifecycle

```text
Job Created
      │
      ▼
Pod Started
      │
      ▼
Task Running
      │
      ▼
Task Completed
      │
      ▼
Job Completed
```

If the Pod fails:

```text
Pod Failed
     │
     ▼
Retry
     │
     ▼
Success or Failure
```

---

# Common Job Commands

Create:

```bash
kubectl apply -f job.yaml
```

View Jobs:

```bash
kubectl get jobs
```

Describe:

```bash
kubectl describe job hello-job
```

View Pods:

```bash
kubectl get pods
```

View Logs:

```bash
kubectl logs <pod-name>
```

Delete:

```bash
kubectl delete job hello-job
```

---

# CronJob

A **CronJob** is used to run Jobs on a schedule.

Example:

```yaml
apiVersion: batch/v1
kind: CronJob

metadata:
  name: backup-job

spec:
  schedule: "0 2 * * *"

  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: busybox
            command: ["echo", "Daily Backup"]

          restartPolicy: OnFailure
```

The above CronJob runs every day at **2:00 AM**.

---

# Job vs CronJob

| Feature | Job | CronJob |
|----------|------|----------|
| Runs Once | ✅ | ❌ |
| Scheduled Execution | ❌ | ✅ |
| Batch Processing | ✅ | ✅ |
| Retry Failed Tasks | ✅ | ✅ |
| Best For | One-time tasks | Repeated tasks |

---

# Job vs Deployment

| Feature | Job | Deployment |
|----------|------|------------|
| Purpose | Complete a task | Run an application continuously |
| Pod Lifecycle | Ends after completion | Runs indefinitely |
| Auto Restart | Limited by retry policy | Continuously maintained |
| Common Use | Batch processing | Web applications |

---

# Advantages

- Reliable batch execution
- Automatic retry on failure
- Parallel task execution
- Tracks task completion
- Easy integration with CI/CD pipelines

---

# Best Practices

- Use Jobs for one-time tasks.
- Use CronJobs for recurring tasks.
- Set an appropriate `backoffLimit`.
- Keep Job containers lightweight.
- Monitor Job completion and logs.
- Clean up completed Jobs if they are no longer needed.

---

# Key Takeaways

- A Job runs a task until it completes successfully.
- Kubernetes automatically retries failed Pods.
- Jobs are commonly used for backups, migrations, and batch processing.
- CronJobs extend Jobs by allowing scheduled execution.
- Jobs are not intended for continuously running applications.

---

# References

- Kubernetes Official Documentation
- Kubernetes Jobs Documentation
- Kubernetes CronJob Documentation
- CNCF Kubernetes Concepts