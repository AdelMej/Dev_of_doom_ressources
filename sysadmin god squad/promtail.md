# 🐾 Promtail Cheat Sheet

Promtail is an agent that **collects logs** and **sends them to Loki**.  
Think of it as Loki’s “sidekick” — reading logs from your system and pushing them to your centralized log store.

---

## 🧩 Key Concepts

| Concept | Description |
|----------|--------------|
| **Target** | A log source (e.g., `/var/log/journal`, `/var/log/*.log`). |
| **Pipeline Stage** | A step in log processing (e.g., parsing, filtering, relabeling). |
| **Label** | Metadata added to logs to identify their origin (e.g., `job`, `hostname`, `app`). |
| **Scrape Config** | Defines what logs to read and how to label them. |

---

## ⚙️ Basic Configuration

File: `/etc/promtail/config.yml`

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://localhost:3100/loki/api/v1/push

scrape_configs:
  - job_name: system
    static_configs:
      - targets:
          - localhost
        labels:
          job: varlogs
          __path__: /var/log/*.log
```

---

## 🧠 Pipelines

Promtail allows **log transformation** through pipeline stages.

### Example Pipeline
```yaml
pipeline_stages:
  - regex:
      expression: 'level=(?P<level>\w+)'
  - labels:
      level:
```

This extracts `level` from a log line and makes it a label.

---

## 🧱 Journald Example

If your system uses `systemd`:

```yaml
scrape_configs:
  - job_name: journald
    journal:
      max_age: 12h
      labels:
        job: systemd-journal
```

---

## 🧰 Useful Commands

| Command | Description |
|----------|--------------|
| `promtail --version` | Show version |
| `systemctl status promtail` | Check Promtail service |
| `journalctl -u promtail -f` | Follow Promtail logs |

---

## 🚀 Tips

- Make sure **Promtail and Loki use the same labels** (for clean dashboards).  
- Store **positions** file on persistent storage to avoid duplicate logs after restart.  
- Combine with **Grafana Loki datasource** for instant log visualization.  

---

## 📚 References

- 🔗 [Official Docs](https://grafana.com/docs/loki/latest/clients/promtail/)
- 💬 [GitHub Repo](https://github.com/grafana/loki/tree/main/clients/cmd/promtail)
