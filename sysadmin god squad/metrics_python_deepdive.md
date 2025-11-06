# 🧠 Deep Dive: Exposing Prometheus Metrics in Python (Flask)
**Purpose:** Hands-on, production-aware guide showing how to instrument a Flask app for Prometheus, with **bare-metal** and **Docker** examples.

---

## Overview
This guide covers:
- Using `prometheus_client` in a Flask app
- Counters, Gauges, Histograms, Summaries
- Best practices (metrics names, labels, cardinality)
- Running on bare metal (development & systemd/gunicorn)
- Containerized setup with Docker and docker-compose
- Prometheus scrape config and Grafana tips

---

## 1) Install dependencies (local / bare-metal)
```bash
# create venv (optional but recommended)
python3 -m venv venv
source venv/bin/activate

# install Flask and prometheus client
pip install flask prometheus_client
```

---

## 2) Minimal Flask app with metrics (development / single process)

`app.py`
```python
from flask import Flask, Response, request
from prometheus_client import Counter, Gauge, Histogram, generate_latest, CONTENT_TYPE_LATEST
import time
import random
import psutil  # optional: pip install psutil for system metrics inside app

app = Flask(__name__)

# Metrics
REQUEST_COUNT = Counter(
    "myapp_http_requests_total", "Total HTTP requests",
    ["method", "endpoint", "status"]
)
IN_PROGRESS = Gauge("myapp_inprogress_requests", "In-progress requests")
REQUEST_LATENCY = Histogram("myapp_request_latency_seconds", "Request latency seconds", ["endpoint"])

# Example route
@app.route("/")
def index():
    start = time.time()
    IN_PROGRESS.inc()
    try:
        # simulate work
        time.sleep(random.uniform(0.01, 0.2))
        status = "200"
        return "Hello, world!"
    finally:
        duration = time.time() - start
        REQUEST_COUNT.labels(method="GET", endpoint="/", status=status).inc()
        REQUEST_LATENCY.labels(endpoint="/").observe(duration)
        IN_PROGRESS.dec()

# metrics endpoint
@app.route("/metrics")
def metrics():
    # Optional: include process or custom metrics here
    return Response(generate_latest(), mimetype=CONTENT_TYPE_LATEST)

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

**Test locally**
```bash
python app.py
curl http://localhost:5000/metrics
```

---

## 3) Important notes (single-process)
- `prometheus_client` stores metrics in-process. For **multi-worker** servers (gunicorn), you must use the **multi-process** mode or an external push gateway.
- Keep label cardinality low (don't use user IDs or timestamps as labels).
- Use stable, snake_case metric names and `HELP`/`TYPE` will be added automatically by `prometheus_client`.

---

## 4) Running under Gunicorn (production) — multi-process mode
When running multiple workers (e.g., `gunicorn -w 4`), use `prometheus_client`'s **multiprocess** mode.

**Steps**
1. Set the environment variable:
   ```bash
   export PROMETHEUS_MULTIPROC_DIR=/tmp/prom-multiproc
   mkdir -p /tmp/prom-multiproc
   ```
2. Use `prometheus_client`'s `CollectorRegistry` with `MultiProcessCollector` in a small metrics app, or use the helper `make_wsgi_app`. Example below.

`wsgi_metrics.py`
```python
from prometheus_client import CollectorRegistry, generate_latest, CONTENT_TYPE_LATEST
from prometheus_client.multiprocess import MultiProcessCollector
from flask import Response
import os

registry = CollectorRegistry()
MultiProcessCollector(registry)

def metrics():
    return Response(generate_latest(registry), mimetype=CONTENT_TYPE_LATEST)
```

`app.py` (modified)
```python
# inside your Flask app, keep using the default global metrics
# but don't expose generate_latest() directly from the global registry
# instead import metrics() from wsgi_metrics and register route:
from wsgi_metrics import metrics as metrics_endpoint

app.add_url_rule('/metrics', 'metrics', metrics_endpoint)
```

3. Start Gunicorn:
```bash
PROMETHEUS_MULTIPROC_DIR=/tmp/prom-multiproc gunicorn -w 4 -b 0.0.0.0:8000 app:app
```

4. `/metrics` will aggregate metrics across workers.

**Cleanup note:** Remove files in `PROMETHEUS_MULTIPROC_DIR` after restarts to avoid stale metrics.

---

## 5) Dockerized Example (app + Prometheus + Grafana)
This example uses `docker-compose` with three services:
- `app` — your Flask app
- `prometheus` — Prometheus server
- `grafana` — Grafana UI

**Directory layout**
```
god_squad_local/
├── app/
│   ├── Dockerfile
│   └── app.py
├── prometheus/
│   └── prometheus.yml
└── docker-compose.yml
```

**app/Dockerfile**
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY app.py .
RUN pip install flask prometheus_client psutil
EXPOSE 5000
ENV PROMETHEUS_MULTIPROC_DIR=/tmp/prom-multiproc
RUN mkdir -p /tmp/prom-multiproc
CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]
```

**prometheus/prometheus.yml**
```yaml
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: "flask_app"
    static_configs:
      - targets: ["app:5000"]
```

**docker-compose.yml**
```yaml
version: "3.8"
services:
  app:
    build: ./app
    ports:
      - "5000:5000"
    volumes:
      - ./app:/app

  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml:ro
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    depends_on:
      - prometheus
```

**Run**
```bash
docker compose up -d --build
```

Open:
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000 (admin/admin)
- App metrics: http://localhost:5000/metrics

---

## 6) Prometheus scrape config examples
**Prometheus scraping a local app**
```yaml
scrape_configs:
  - job_name: 'local'
    static_configs:
      - targets: ['localhost:5000']
```

**Scrape in Docker Compose (service discovery via container name)**
```yaml
scrape_configs:
  - job_name: 'flask_app'
    static_configs:
      - targets: ['app:5000']
```

---

## 7) Grafana tips for visualizing app metrics
- Add Prometheus as data source: `http://prometheus:9090` (in Docker) or `http://localhost:9090` (bare metal).
- Quick panels:
  - Request rate: `sum(rate(myapp_http_requests_total[1m]))`
  - Avg latency: `histogram_quantile(0.95, sum(rate(myapp_request_latency_seconds_bucket[5m])) by (le))`
  - In-progress: `myapp_inprogress_requests`

---

## 8) Best Practices & Troubleshooting
- **Label cardinality:** Keep labels low-cardinality. Avoid per-user IDs as labels.
- **Use appropriate metric types:** counters for cumulative events, gauges for current values, histograms for latencies.
- **Multiprocess mode:** Use `PROMETHEUS_MULTIPROC_DIR` with Gunicorn or use a pushgateway for batch jobs.
- **Security:** Do not expose `/metrics` publicly; use firewall, auth proxy, or restrict Prometheus network access.
- **Testing:** `curl http://localhost:5000/metrics` to debug; `promtool check config prometheus.yml` to validate Prometheus config.

---

## 9) Extra: Pushgateway (for short-lived jobs)
For short-lived batch jobs (that exit quickly) use Prometheus Pushgateway.

Example (Python):
```python
from prometheus_client import CollectorRegistry, Gauge, push_to_gateway

registry = CollectorRegistry()
g = Gauge('batch_success', 'Did the batch succeed?', registry=registry)
g.set(1)
push_to_gateway('pushgateway:9091', job='batch_job', registry=registry)
```

---

## 10) TL;DR
- Instrument your app with `prometheus_client`.
- Expose `/metrics`.
- Configure Prometheus to scrape the endpoint.
- Visualize in Grafana.
- For production: use multiprocess mode with Gunicorn or containerize and use docker-compose/Kubernetes.

---

Happy ascending. Your app is now Prometheus-ready. 🧙‍♂️
