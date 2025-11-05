# Node Exporter Cheat Sheet 🧠  
**Part of the GOD SQUAD — The Linux Observability Stack**

---

## 🧩 What Is Node Exporter?
Node Exporter is a Prometheus exporter for *hardware and OS metrics* exposed by *nix kernels.  
It provides metrics about CPU, memory, disk, network, and other system resources.

It’s the **data collector** for Prometheus on each machine.

---

## ⚙️ Installation

### On Linux
```bash
# Download and extract latest version
wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-linux-amd64.tar.gz
tar xvfz node_exporter-linux-amd64.tar.gz
cd node_exporter-*

# Move binary and create service
sudo mv node_exporter /usr/local/bin/
```

### Systemd Service
Create `/etc/systemd/system/node_exporter.service`:

```ini
[Unit]
Description=Prometheus Node Exporter
After=network.target

[Service]
User=nobody
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

Then:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now node_exporter
```

---

## 🔍 Verify It’s Running

```bash
curl http://localhost:9100/metrics
```

Expected output: a big list of metrics like:
```
# HELP node_cpu_seconds_total Seconds the CPUs spent in each mode.
node_cpu_seconds_total{cpu="0",mode="idle"} 123456.0
```

---

## 📡 Prometheus Configuration

Add this to your Prometheus `prometheus.yml`:

```yaml
scrape_configs:
  - job_name: "node_exporter"
    static_configs:
      - targets: ["localhost:9100"]
```

Restart Prometheus after editing.

---

## 🧮 Common Metrics

| Category | Metric | Description |
|-----------|---------|-------------|
| **CPU** | `node_cpu_seconds_total` | CPU time by mode (idle, user, system, etc.) |
| **Memory** | `node_memory_MemAvailable_bytes`, `node_memory_MemTotal_bytes` | RAM info |
| **Disk** | `node_disk_io_time_seconds_total`, `node_filesystem_size_bytes` | Disk usage & I/O |
| **Network** | `node_network_receive_bytes_total`, `node_network_transmit_bytes_total` | Network stats |
| **System** | `node_boot_time_seconds` | System boot time |

---

## 📊 Grafana Integration
In Grafana → **Add Data Source → Prometheus**  
Then use a prebuilt dashboard:

👉 [Node Exporter Full Dashboard](https://grafana.com/grafana/dashboards/1860-node-exporter-full/)

---

## 🧠 Tips

- Run Node Exporter on **every server** you want Prometheus to monitor.
- Always use port **9100** (default).
- Use systemd or Docker for easy management.
- Combine it with **Loki** and **Promtail** for full logs + metrics visibility.

---

## 🪄 Quick Docker Run
```bash
docker run -d -p 9100:9100 --name node_exporter prom/node-exporter
```

---

**🔥 TL;DR:**  
Node Exporter = system metrics  
Prometheus = metrics collector  
Grafana = data visualizer  
Loki + Promtail = logs  
→ Together = *the GOD SQUAD of system monitoring*
