# 🧠 Grafana Cheat Sheet

Grafana is an **open-source visualization and analytics platform** for monitoring metrics, logs, and traces.  
It connects to Prometheus, Loki, InfluxDB, Elasticsearch, and many others to visualize your system in real-time.

---

## 🚀 1. Installation

### 🐧 On Linux
```bash
sudo apt install -y grafana        # Debian/Ubuntu
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

### 🧩 On Arch
```bash
sudo pacman -S grafana
sudo systemctl enable grafana
sudo systemctl start grafana
```

Then go to:  
👉 **http://localhost:3000**  
Default login:  
```
user: admin
pass: admin
```

---

## ⚙️ 2. Configuration

Main config file:
```
/etc/grafana/grafana.ini
```

Useful sections:
- `[server]` → configure port, domain, root URL
- `[security]` → change admin password or disable login form
- `[dashboards.json]` → auto-load dashboards
- `[datasources]` → default data sources like Prometheus, Loki, etc.

Restart Grafana after editing:
```bash
sudo systemctl restart grafana-server
```

---

## 📊 3. Adding Data Sources

1. Go to **Settings → Data Sources**
2. Click **Add data source**
3. Choose one (example):
   - **Prometheus:** `http://localhost:9090`
   - **Loki:** `http://localhost:3100`

---

## 🧩 4. Dashboards

### Create a Dashboard
1. Click **+ (Create)** → **Dashboard**
2. Add a **Panel**
3. Select your data source (e.g., Prometheus)
4. Write a query like:
   ```
   node_cpu_seconds_total{mode="idle"}
   ```
5. Choose visualization (Graph, Gauge, Stat, Table…)

### Import Dashboards
You can import prebuilt dashboards from  
👉 [https://grafana.com/grafana/dashboards](https://grafana.com/grafana/dashboards)

Example:
- Node Exporter Full (ID: `1860`)
- Prometheus 2.0 Overview (ID: `3662`)

---

## 🧠 5. Alerts

1. Go to **Alerting → Alert Rules**
2. Create an alert tied to a metric (e.g., CPU > 80%)
3. Choose a **Contact Point**:
   - Email
   - Slack
   - Discord
   - Webhook

Grafana can **trigger alerts automatically** and send messages to your DevOps channels 💬

---

## 💾 6. Folder Structure

Default paths:
```
/var/lib/grafana/         → Grafana data (SQLite, dashboards, etc.)
/etc/grafana/             → Config files
/var/log/grafana/         → Logs
```

---

## 🧱 7. Connecting with Loki & Prometheus

### Example setup
- Prometheus → `http://localhost:9090`
- Loki → `http://localhost:3100`
- Grafana → `http://localhost:3000`

In Grafana:
- Add both **Prometheus** (metrics) and **Loki** (logs)
- Create panels combining both:
  - Metric graph (CPU usage)
  - Log panel showing container logs for the same time range

---

## 🧩 8. Handy Queries (Prometheus inside Grafana)

| Description | Query |
|--------------|--------|
| CPU Usage % | `100 - (avg by(instance)(irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)` |
| Memory Usage | `(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100` |
| Disk Usage | `(node_filesystem_size_bytes - node_filesystem_free_bytes) / node_filesystem_size_bytes * 100` |
| Network In | `rate(node_network_receive_bytes_total[1m])` |

---

## 🧰 9. Useful Tips

- **CTRL + S** → Save dashboard  
- **SHIFT + drag** → Select time range  
- **SHIFT + click refresh** → Force refresh  
- **Panel → Inspect → Query** → Debug PromQL  

---

## 🧩 10. Summary

| Component | Role |
|------------|------|
| Grafana | Visualizes metrics and logs |
| Prometheus | Collects metrics |
| Loki | Collects logs |
| Promtail | Sends logs to Loki |
| Node Exporter | Exposes system metrics |

---

Grafana is your **visual layer** in the God Squad —  
it turns raw metrics into dashboards that make sysadmins feel like gods. ⚡
