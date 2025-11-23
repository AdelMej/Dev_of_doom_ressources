# Monitoring God Squad Architecture

## 1. Full Architecture

    [... diagram omitted for brevity in markdown ...]

## 2. Component Roles

### Loki --- Log Devourer

-   Stores logs efficiently
-   Query-friendly
-   Ideal for high-volume logging

### Promtail --- Log Courier

-   Collects logs from journald, nginx, apps
-   Ships them to Loki

### Prometheus --- Metric Farmer

-   Scrapes metrics from exporters and services
-   Stores time-series metrics

### Alertmanager --- Notification Engine

-   Handles alerts from Prometheus
-   Sends notifications to channels

### Node Exporter / cAdvisor --- Metric Scouts

-   Host-level and container-level metrics

### Grafana --- Dashboard UI

-   Visualizes logs, metrics, alerts

------------------------------------------------------------------------

## 3. Deployment Structure

    /etc/monitoring/
       ├── loki/
       ├── promtail/
       ├── prometheus/
       └── grafana/

------------------------------------------------------------------------

## 4. Configuration Templates

### promtail-config.yaml

``` yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0
...
```

### loki-config.yaml

``` yaml
auth_enabled: false
server:
  http_listen_port: 3100
...
```

### prometheus.yml

``` yaml
global:
  scrape_interval: 5s
...
```

------------------------------------------------------------------------

## 5. Alert Rules (Prometheus)

``` yaml
groups:
  - name: system_alerts
    rules:
      - alert: HighCPU
        expr: avg(rate(node_cpu_seconds_total{mode!="idle"}[2m])) > 0.85
...
```

------------------------------------------------------------------------

## 6. Recommended Grafana Dashboards

-   Node Exporter Full
-   Host Metrics Overview
-   Fail2ban Dashboard
-   NGINX Access Metrics
-   Loki Log Explorer

------------------------------------------------------------------------

## 7. System Behavior Summary

-   Fail2ban → blocks attackers
-   NGINX → rate-limits & filters
-   SELinux → isolates services
-   Loki/Promtail → logs everything
-   Prometheus → metrics everywhere
-   Grafana → visual control panel
-   Alertmanager → wakes you up when needed

This forms the **Monitoring God Squad**.
