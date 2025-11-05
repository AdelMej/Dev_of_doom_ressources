# Loki Cheat Sheet

## 🧩 Overview
Loki is a **log aggregation system** developed by Grafana Labs. It’s designed to store and query logs efficiently — inspired by Prometheus but optimized for logs instead of metrics.

Unlike Elasticsearch, Loki doesn’t index the full log content. It indexes **labels (metadata)** only, making it cheaper, faster, and easier to scale.

---

## ⚙️ Core Concepts

| Concept | Description |
|----------|--------------|
| **Label** | Key-value pairs used to organize and filter logs (like Prometheus labels). |
| **Stream** | A unique combination of labels that identifies a set of logs. |
| **Chunk** | A compressed, indexed block of log data from one stream. |
| **Tenant** | Used in multi-tenant mode for isolating data and queries. |

---

## 📦 Architecture Components

1. **Promtail** – log shipper that collects and sends logs to Loki.
2. **Loki Distributor** – receives logs and distributes them across ingesters.
3. **Ingester** – batches and stores log chunks in memory or backend storage (S3, GCS, etc.).
4. **Querier** – processes log queries.
5. **Query Frontend** – optional caching and parallelization layer.

---

## 🔧 Installation (Docker Example)

```bash
docker run -d --name=loki -p 3100:3100 grafana/loki:latest
```

Check Loki at: `http://localhost:3100/ready`

---

## 📄 Configuration Example (`loki-config.yaml`)

```yaml
server:
  http_listen_port: 3100

ingester:
  lifecycler:
    address: 127.0.0.1
    ring:
      kvstore:
        store: inmemory
      replication_factor: 1
  chunk_idle_period: 5m
  chunk_retain_period: 30s

schema_config:
  configs:
    - from: 2020-10-24
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h

storage_config:
  boltdb_shipper:
    active_index_directory: /tmp/loki/index
    cache_location: /tmp/loki/cache
    shared_store: filesystem
  filesystem:
    directory: /tmp/loki/chunks

limits_config:
  enforce_metric_name: false
  reject_old_samples: true
  reject_old_samples_max_age: 168h
```

Run Loki:
```bash
loki -config.file=loki-config.yaml
```

---

## 🕵️ Querying Logs (LogQL)

Loki uses **LogQL**, similar to PromQL but for logs.

### Basic Query
```logql
{job="systemd-journal"}
```
Returns all logs where the label `job` equals `systemd-journal`.

### Filtering
```logql
{app="nginx"} |= "error"
```
Returns all Nginx logs containing the word `error`.

### Regex Matching
```logql
{app="nginx"} |~ "5\\d{2}"
```
Matches logs containing HTTP 5xx codes.

### Count Errors per Label
```logql
count_over_time({app="nginx"} |= "error" [5m])
```
Counts the number of error logs in the last 5 minutes.

---

## 📊 Integration with Grafana

1. Open Grafana → **Connections → Data Sources**.
2. Add **Loki** as a data source.
3. Enter Loki URL: `http://localhost:3100`.
4. Query your logs directly in Grafana panels.

---

## 🚀 Tips & Tricks

- Use consistent **labels** (`job`, `instance`, `env`) for better filtering.
- Pair with **Promtail** or **Fluent Bit** to collect logs.
- Use **object storage** (S3, GCS, MinIO) for long-term retention.
- Combine Loki with **Grafana alerts** for real-time log-based notifications.

---

## 📚 Useful References
- [Official Loki Docs](https://grafana.com/docs/loki/latest/)
- [Loki GitHub Repo](https://github.com/grafana/loki)
- [LogQL Guide](https://grafana.com/docs/loki/latest/logql/)

---

**Loki = The Trickster of Logs.**  🌀  
Paired with Promtail and Grafana, you control the chaos of system logs with divine precision.