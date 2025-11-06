# 🧠 Universal `/metrics` Exposure Guide  
### The Sixth Scroll of the God Squad

---

## ⚙️ Overview

In the Prometheus ecosystem, the `/metrics` endpoint is the **holy gateway** between your app and the monitoring universe.  
It’s an **HTTP endpoint** that exposes internal data in **plain text**, formatted in the **Prometheus exposition format**.

Prometheus regularly **scrapes** this endpoint to collect data and store it in its time-series database.

---

## 🧩 Format of Metrics

Example of a valid `/metrics` response:
```
# HELP http_requests_total The total number of HTTP requests.
# TYPE http_requests_total counter
http_requests_total{method="get",endpoint="/"} 1234
http_requests_total{method="post",endpoint="/api"} 456
```

### Key Points:
- `# HELP` → A short description of the metric.  
- `# TYPE` → The metric type (`counter`, `gauge`, `histogram`, `summary`).  
- Metric lines → Label-value pairs that describe different dimensions.

---

## 🐍 Python Example (Flask + Prometheus Client)

```python
from flask import Flask
from prometheus_client import Counter, generate_latest, CONTENT_TYPE_LATEST

app = Flask(__name__)

# Define a simple counter metric
http_requests_total = Counter("http_requests_total", "Total HTTP requests", ["method", "endpoint"])

@app.route("/")
def home():
    http_requests_total.labels(method="get", endpoint="/").inc()
    return "Hello from Flask!"

@app.route("/metrics")
def metrics():
    return generate_latest(), 200, {"Content-Type": CONTENT_TYPE_LATEST}

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

✅ Run this app, then visit:  
👉 `http://localhost:5000/metrics`  
You’ll see Prometheus-formatted metrics ready to be scraped.

---

## ☕ Java Example (Spring Boot + Micrometer)

Add this dependency in your `pom.xml`:
```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

Then, in `application.properties`:
```
management.endpoints.web.exposure.include=prometheus
management.endpoint.prometheus.enabled=true
```

Start your app, and you’ll get a working `/actuator/prometheus` endpoint automatically.  
Prometheus can scrape it directly — GG no RE ☕.

---

## 💻 C Example (with prometheus-c library)

You can use [prometheus-c](https://github.com/digitalocean/prometheus-client-c) or manually print metrics.

```c
#include <stdio.h>

int main(void) {
    printf("# HELP app_requests_total Total number of processed requests\n");
    printf("# TYPE app_requests_total counter\n");
    printf("app_requests_total{endpoint=\"/\",method=\"GET\"} 42\n");
    return 0;
}
```

Host this output through a small HTTP server, and Prometheus can scrape it exactly the same way.

---

## 🧠 Custom Metrics

You can expose **anything**:
- CPU temperature  
- Disk usage  
- Systemd service states  
- Game server statistics  
- API request timings  

Just make sure to:
1. Keep metric names **snake_case**.  
2. Add clear `HELP` and `TYPE`.  
3. Avoid using spaces or special chars in labels.

---

## 🛰️ Example Prometheus Job

Add this to `prometheus.yml`:
```yaml
scrape_configs:
  - job_name: "my_backend"
    static_configs:
      - targets: ["localhost:5000"]
```

That’s it. Prometheus will now periodically scrape your app’s `/metrics` endpoint — and it becomes part of the **God Squad network**.

---

## 🔮 TL;DR
| Stack | Role | Endpoint |
|-------|------|-----------|
| Prometheus | Scraper | `/metrics` |
| Grafana | Visualizer | N/A |
| Loki | Logs | `/loki/api/v1/push` |
| Promtail | Log forwarder | Local → Loki |
| Node Exporter | System metrics | `/metrics` |
| Your App | Custom metrics | `/metrics` |

Expose `/metrics`, configure Prometheus, visualize in Grafana — **and ascend**. 🧙‍♂️  
GG no RE.

---
