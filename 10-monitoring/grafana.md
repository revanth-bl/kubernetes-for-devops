# Grafana

## Overview

**Grafana** is an open-source visualization and monitoring platform used to create **interactive dashboards** from metrics collected by various data sources.

In Kubernetes, Grafana is commonly used with **Prometheus** to visualize cluster performance, application health, resource utilization, and infrastructure metrics.

Grafana transforms raw monitoring data into easy-to-understand charts, graphs, and alerts, making it an essential tool for DevOps and Site Reliability Engineering (SRE).

---

# Why Grafana?

Without Grafana:

```text
Prometheus Metrics

↓

Raw Numerical Data

↓

Difficult to Analyze
```

Problems:

- Hard to understand trends
- No dashboards
- Difficult troubleshooting
- Poor visibility

---

With Grafana:

```text
Prometheus

↓

Grafana

↓

Dashboards

↓

Real-Time Monitoring
```

Administrators can monitor the health of their Kubernetes cluster visually.

---

# Benefits

- Interactive dashboards
- Real-time monitoring
- Supports multiple data sources
- Customizable visualizations
- Alerting capabilities
- Easy troubleshooting

---

# Grafana Architecture

```text
          Kubernetes Cluster
                 │
                 ▼
            Prometheus
                 │
                 ▼
             Grafana
                 │
        ┌────────┼────────┐
        ▼        ▼        ▼
   Dashboards  Alerts   Reports
```

Grafana queries Prometheus for metrics and displays them in dashboards.

---

# How Grafana Works

1. Applications and Kubernetes components expose metrics.
2. Prometheus collects and stores those metrics.
3. Grafana connects to Prometheus as a data source.
4. Grafana displays the metrics using dashboards and visualizations.
5. Alerts can be triggered based on configured thresholds.

---

# Common Dashboards

Grafana dashboards typically display:

- CPU Usage
- Memory Usage
- Disk Usage
- Network Traffic
- Pod Status
- Node Health
- Deployment Status
- Request Latency
- Error Rates
- Kubernetes Events

---

# Install Grafana

Using Helm:

```bash
helm repo add grafana https://grafana.github.io/helm-charts

helm repo update

helm install grafana grafana/grafana
```

Verify installation:

```bash
kubectl get pods
```

---

# Access Grafana

Port-forward the Grafana service:

```bash
kubectl port-forward svc/grafana 3000:80
```

Open your browser:

```text
http://localhost:3000
```

---

# Get Admin Password

Retrieve the initial admin password:

```bash
kubectl get secret grafana \
-o jsonpath="{.data.admin-password}" \
| base64 --decode
```

Default username:

```text
admin
```

---

# Add Prometheus as a Data Source

1. Log in to Grafana.
2. Navigate to **Connections → Data Sources**.
3. Select **Prometheus**.
4. Enter the Prometheus service URL.
5. Save and test the connection.

Example URL:

```text
http://prometheus-server
```

---

# Import a Dashboard

1. Click **Dashboards**.
2. Select **Import**.
3. Enter a dashboard ID from the Grafana Dashboard Library.
4. Choose the Prometheus data source.
5. Import the dashboard.

Popular dashboard examples:

- Kubernetes Cluster Monitoring
- Node Exporter Full
- Kubernetes Pods
- Kubernetes API Server

---

# Common Commands

View Pods:

```bash
kubectl get pods
```

View Services:

```bash
kubectl get svc
```

Port Forward:

```bash
kubectl port-forward svc/grafana 3000:80
```

Delete Grafana:

```bash
helm uninstall grafana
```

---

# Grafana vs Prometheus

| Grafana | Prometheus |
|----------|------------|
| Visualization platform | Metrics collection system |
| Creates dashboards | Stores metrics |
| Supports alerts | Scrapes metrics |
| Supports multiple data sources | Primarily a metrics database |

---

# Supported Data Sources

Grafana supports many data sources, including:

- Prometheus
- Loki
- Elasticsearch
- InfluxDB
- MySQL
- PostgreSQL
- Microsoft SQL Server
- AWS CloudWatch
- Azure Monitor
- Google Cloud Monitoring

---

# Advantages

- Rich visual dashboards
- Real-time monitoring
- Easy integration with Prometheus
- Custom alerts
- Large community dashboard library
- Supports many data sources

---

# Limitations

- Depends on external data sources for metrics.
- Dashboard design can become complex in large environments.
- Large deployments may require additional resources.

---

# Best Practices

- Use Prometheus as the primary data source for Kubernetes monitoring.
- Organize dashboards by application or environment.
- Create meaningful alerts for critical metrics.
- Import community dashboards to save time.
- Restrict dashboard editing using role-based permissions.
- Regularly back up Grafana dashboards and configurations.

---

# Interview Questions

### What is Grafana?

Grafana is an open-source visualization platform used to create dashboards from monitoring data.

---

### Does Grafana collect metrics?

No. Grafana visualizes data collected by systems such as Prometheus.

---

### Which monitoring tool is most commonly paired with Grafana?

**Prometheus**

---

### What is a Grafana Dashboard?

A dashboard is a collection of panels that display metrics using charts, graphs, gauges, and tables.

---

### Which port does Grafana use by default?

**3000**

---

# Key Takeaways

- Grafana provides dashboards and visualization for monitoring data.
- It is commonly integrated with Prometheus in Kubernetes environments.
- Grafana supports alerts, reports, and multiple data sources.
- Dashboards help administrators quickly identify performance issues.
- Grafana is a core component of modern Kubernetes observability stacks.

---

# References

- Grafana Official Documentation
- Grafana Dashboard Library
- Prometheus Documentation
- Kubernetes Monitoring Documentation
- CNCF Observability Documentation