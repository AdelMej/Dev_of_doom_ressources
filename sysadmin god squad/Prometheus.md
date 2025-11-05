# 🧠 Prometheus Cheat Sheet
**Role:** The Watcher of Metrics  
**Purpose:** Collects, stores, and queries time-series metrics from systems and services.

---

## 🚀 Overview
Prometheus is a **monitoring system** and **time-series database** that scrapes metrics from targets via HTTP endpoints.  
It’s ideal for collecting numeric data over time (CPU, memory, temperature, etc.).

---

## 🏗️ Architecture
- **Prometheus Server:** Pulls metrics from configured targets.
- **Targets:** Applications or nodes exposing `/metrics`.
- **Node Exporter:** Exposes Linux system metrics.
- **Service Discovery:** Dynamically finds targets.
- **Alertmanager:** Handles alerts (email, Slack, etc.).
- **Grafana:** Visualizes Prometheus data.

---

## ⚙️ Core Concepts
| Concept | Description |
|----------|-------------|
| **Time Series** | Data identified by a metric name + set of labels |
| **Metric Name** | e.g. `node_cpu_seconds_total` |
| **Label** | Key-value pairs to distinguish data (e.g. `cpu="0"`, `mode="user"`) |
| **Sample** | A single data point (value + timestamp) |
| **Job / Instance** | Logical groupings of targets |

---

## 📦 Installation (Linux)
```bash
# Download Prometheus
wget https://github.com/prometheus/prometheus/releases/latest/download/prometheus-*-linux-amd64.tar.gz
tar xvf prometheus-*-linux-amd64.tar.gz
cd prometheus-*-linux-amd64

# Start Prometheus
./prometheus --config.file=prometheus.yml
```

Default web UI:  
👉 `http://localhost:9090`

---

## 🧩 Basic Configuration
**`prometheus.yml`**
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
```

> Each `target` must expose a `/metrics` endpoint.

---

## 📈 Example Query (PromQL)
| Query | Description |
|--------|-------------|
| `up` | Checks if targets are up (1 = up, 0 = down) |
| `node_cpu_seconds_total` | CPU usage metrics |
| `rate(node_network_receive_bytes_total[5m])` | Network receive rate (bytes/sec) |
| `avg by (instance) (rate(node_cpu_seconds_total[5m]))` | Average CPU usage by instance |
| `sum(rate(http_requests_total[1m]))` | Total HTTP requests per second |

---

## 🧠 PromQL Tips
- `[5m]` = last 5 minutes of data  
- `rate()` = per-second average increase  
- `sum()`, `avg()`, `max()` = aggregate functions  
- `by (label)` = group results by label  
- `irate()` = instant rate (for fast-changing metrics)

---

## 🔔 Alerts Example
Add to `prometheus.yml`:
```yaml
rule_files:
  - "alerts.yml"
```

**`alerts.yml`**
```yaml
groups:
  - name: system_alerts
    rules:
      - alert: HighCPULoad
        expr: avg(rate(node_cpu_seconds_total{mode="system"}[5m])) > 0.7
        for: 2m
        labels:
          severity: warning
        annotations:
          summary: "High CPU load detected"
```

---

## 🧩 Integration
- **Grafana** → Visualization  
- **Alertmanager** → Notifications  
- **Loki** → Logs correlation  
- **Promtail** → Log collector  
- **Node Exporter** → System metrics  

---

## 🔍 Common Ports
| Service | Port |
|----------|------|
| Prometheus UI | 9090 |
| Node Exporter | 9100 |
| Alertmanager | 9093 |

---

## 🧰 Useful Commands
```bash
# Test config
./promtool check config prometheus.yml

# View targets
curl http://localhost:9090/api/v1/targets

# View active alerts
curl http://localhost:9090/api/v1/alerts
```

---

## 🧾 Useful Metrics
| Metric | Description |
|---------|-------------|
| `up` | Target availability |
| `node_load1` | 1-min load average |
| `node_memory_MemAvailable_bytes` | Available memory |
| `node_disk_read_bytes_total` | Disk read activity |
| `node_network_receive_bytes_total` | Network usage |

---

## 🧩 File Structure Example
```
/etc/prometheus/
│
├── prometheus.yml
├── alerts.yml
└── data/
```

---

## 🧠 TL;DR
> Prometheus **collects metrics**, **stores them efficiently**, and **makes them queryable** through PromQL.  
> Pair it with **Grafana**, **Loki**, **Promtail**, and **Node Exporter** — and you’ve got full system observability.

---

**Next Up:** [Grafana Cheat Sheet →](./Grafana.md)
