# The God Squad: Full Monitoring Stack Overview

This document explains how **Prometheus**, **Grafana**, **Loki**, **Promtail**, and **Node Exporter** fit together to form a complete observability and monitoring system — often called the *God Squad* of sysadmin tools.

---

## 🧠 Core Idea

This stack provides a unified system to collect, store, and visualize **metrics**, **logs**, and **system data** in real-time.

| Component | Type | Purpose |
|------------|------|----------|
| **Prometheus** | Metrics database | Scrapes and stores metrics from monitored targets. |
| **Grafana** | Visualization | Displays data from Prometheus and Loki with beautiful dashboards. |
| **Loki** | Log database | Collects and indexes logs efficiently. |
| **Promtail** | Log shipper | Sends logs from your system to Loki. |
| **Node Exporter** | Metrics exporter | Exposes system-level metrics for Prometheus (CPU, RAM, Disk, etc). |

---

## ⚙️ System Architecture

```
+--------------------+
|   Node Exporter    |  →  Exposes system metrics
+--------------------+
           ↓
+--------------------+
|    Prometheus      |  →  Scrapes metrics from Node Exporter
+--------------------+
           ↓
+--------------------+
|      Grafana       |  →  Visualizes metrics from Prometheus
+--------------------+

+--------------------+
|     Promtail       |  →  Collects logs (journald, files, etc)
+--------------------+
           ↓
+--------------------+
|        Loki        |  →  Stores and indexes logs
+--------------------+
           ↓
+--------------------+
|      Grafana       |  →  Visualizes logs from Loki
+--------------------+
```

---

## 🔄 Data Flow Summary

### Metrics Flow
1. **Node Exporter** gathers hardware/system data.  
2. **Prometheus** scrapes and stores those metrics.  
3. **Grafana** queries Prometheus to display graphs and stats.

### Logs Flow
1. **Promtail** reads logs (systemd, journald, app logs).  
2. **Loki** receives and indexes them.  
3. **Grafana** visualizes logs with flexible filtering and correlation.

---

## 🧩 Integration Example

When you open Grafana, you can:
- Add **Prometheus** as a data source → build dashboards with CPU, memory, etc.  
- Add **Loki** as a data source → explore logs in real time.  
- Correlate both in one view (e.g., see logs that match a spike in CPU).

---

## 🚀 Deployment Tips

- Use **Docker Compose** or **systemd services** for easy setup.
- Run all components in separate containers or services:
  - `prometheus.yml` for scrape configs.
  - `promtail-config.yml` for log sources.
- Secure access to Grafana with authentication.
- Use persistent storage for Prometheus and Loki.

---

## 🛠️ Example Docker Compose Setup (Simplified)

```yaml
version: '3'
services:
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  node_exporter:
    image: prom/node-exporter
    ports:
      - "9100:9100"

  loki:
    image: grafana/loki
    ports:
      - "3100:3100"
    volumes:
      - ./loki-config.yml:/etc/loki/local-config.yaml

  promtail:
    image: grafana/promtail
    volumes:
      - ./promtail-config.yml:/etc/promtail/config.yml
    depends_on:
      - loki

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    depends_on:
      - prometheus
      - loki
```

---

## 🧭 Summary

| Component | Port | Description |
|------------|------|-------------|
| Prometheus | 9090 | Metrics collection and queries |
| Node Exporter | 9100 | Exposes system metrics |
| Loki | 3100 | Log aggregation backend |
| Promtail | (none) | Ships logs to Loki |
| Grafana | 3000 | Visualization UI |

---

## ⚡ Why It’s Powerful

✅ Full observability (metrics + logs)  
✅ Lightweight and scalable  
✅ Works across Linux servers, containers, and cloud instances  
✅ Integrates with alerting and automation  
✅ 100% open source

---

**The God Squad in one sentence:**  
> “Prometheus measures, Loki listens, Grafana reveals.”
