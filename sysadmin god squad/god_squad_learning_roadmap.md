# 🧠 God Squad Learning Roadmap

**Goal:** Master Prometheus, Grafana, Loki, Promtail, and Node Exporter — a.k.a. “The God Squad” — from bare-metal setup to Dockerized production-grade observability stack.  
**Style:** No time constraint, full depth, step-by-step practical mastery.

---

## 🩻 Phase 1 — Bare Metal Foundations

### 🎯 Objective
Understand how every tool works *independently* before containerizing. Learn configs, architecture, and basic troubleshooting.

### 🔹 Step 1: Node Exporter
- **Purpose:** Collect system metrics (CPU, RAM, disk, network).
- **Learn:**
  - Installation from binary and systemd setup.
  - Default metrics endpoint: `http://localhost:9100/metrics`.
  - Metric types: `counter`, `gauge`, `histogram`, `summary`.
- **Practice:**
  - Query metrics manually in a browser.
  - Enable it at startup (`systemctl enable node_exporter`).
  - Monitor local metrics in real time.

### 🔹 Step 2: Prometheus
- **Purpose:** Collect metrics from Node Exporter (and later, everything else).
- **Learn:**
  - Basic config file: `prometheus.yml`.
  - `scrape_configs`, `targets`, and `job_name`.
  - UI access at `http://localhost:9090`.
  - Query language **PromQL**.
- **Practice:**
  - Add Node Exporter as a scrape target.
  - Create alerts and test notification rules.
  - Understand data retention and storage.

### 🔹 Step 3: Grafana
- **Purpose:** Visualize everything from Prometheus (and Loki later).
- **Learn:**
  - Add Prometheus as a data source.
  - Build custom dashboards.
  - Add visualizations: graphs, gauges, single stat, heatmaps.
  - Permissions and folder organization.
- **Practice:**
  - Build a “System Overview” dashboard.
  - Create alerts from panels.
  - Export and import dashboards.

### 🔹 Step 4: Loki
- **Purpose:** Collect and store logs.
- **Learn:**
  - Architecture: Loki = label-based log storage.
  - Config basics (`loki-config.yml`).
  - Difference between Prometheus metrics and Loki logs.
- **Practice:**
  - Start Loki service locally.
  - Query logs via its API.
  - Learn **LogQL** syntax (similar to PromQL).

### 🔹 Step 5: Promtail
- **Purpose:** Send logs to Loki.
- **Learn:**
  - Configuring sources (`/var/log/*`).
  - Labels, pipelines, and targets.
  - Systemd service setup.
- **Practice:**
  - Send journald logs to Loki.
  - Verify logs appear in Loki’s API or Grafana.

### 🔹 Step 6: Integration Test
- **Goal:** Full stack on bare metal.
- **Steps:**
  - Node Exporter → Prometheus → Grafana (metrics).
  - Promtail → Loki → Grafana (logs).
- **Result:** Live graphs + logs viewable in Grafana.

---

## 🐳 Phase 2 — Dockerize the Stack

### 🎯 Objective
Containerize everything with consistent networking and data persistence.

### 🔹 Step 1: Docker Basics
- Learn how to use Docker networks, volumes, and compose files.
- Prepare directories for persistent storage (`/data/prometheus`, `/data/loki`).

### 🔹 Step 2: Create `docker-compose.yml`
- Include:
  - **Prometheus**
  - **Loki**
  - **Promtail**
  - **Node Exporter**
  - **Grafana**
- Use `depends_on` for startup order.
- Map config files from your host into containers.

### 🔹 Step 3: Persist and Scale
- Mount data volumes for Prometheus and Loki.
- Expose only Grafana externally.
- Learn to rebuild only one container with `docker compose up -d service_name`.

### 🔹 Step 4: Custom Dashboards & Alerts
- Add alert rules in Prometheus.
- Create Loki log panels in Grafana.
- Export dashboards as JSON for backup.

### 🔹 Step 5: Maintenance
- Learn how to upgrade versions cleanly.
- Automate with systemd or cron for updates.
- Monitor resource usage of the stack.

---

## 🧩 Phase 3 — Mastery Projects

### 🎯 Objective
Build real-world observability setups and your own Grafana-like frontend.

### 🛠 Project 1 — Personal Server Monitoring
- Monitor your mini PC server.
- Add temperature sensors via Node Exporter textfile collector.
- Setup Promtail to follow `/var/log/secure` and `/var/log/journal`.

### 🧰 Project 2 — Home Infrastructure Dashboard
- Add Docker containers as Prometheus targets.
- Create Grafana dashboards for each host.
- Add Loki filters for security logs.

### 🔮 Project 3 — Custom Grafana Replacement
- Use your **Python (FastAPI)** or **Java (Spring Boot)** backend.
- Query Prometheus and Loki APIs directly.
- Build a simple web dashboard that:
  - Displays metrics (graphs or tables).
  - Streams logs (filtered by labels).
  - Handles alert notifications.

---

## 🧘‍♂️ Final Mastery

When you can:
- Spin up the full stack blindfolded.
- Query any metric or log instantly.
- Design dashboards for others.
- Write alert rules that detect issues before users do.

You’re not a sysadmin anymore, bro.  
You’re the **God Squad Operator** 👑
