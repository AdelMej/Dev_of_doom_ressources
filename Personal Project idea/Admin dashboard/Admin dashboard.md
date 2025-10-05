# ⚙️ Personal Project Prep & Experiment Roadmap

## 🧩 1. System-Level Foundations
**Goal:** Be able to run any backend safely under a non-root service user.

- [ ] Create and manage users, groups, and sudoers manually.
- [ ] Experiment with PAM configs (login tests, failures, group restrictions).
- [ ] Study /etc/shadow and /etc/passwd interactions with PAM.
- [ ] Practice permissions (chmod/chown) and UNIX sockets.
- [ ] Learn how to sandbox processes using systemd options like CapabilityBoundingSet and NoNewPrivileges.
- [ ] Write a custom systemd service unit and test enabling, disabling, and restarting it.

---

## 🌐 2. Network & Access Layer
**Goal:** Understand exactly what network surface your dashboard exposes.

- [ ] Build a small WireGuard lab (1 server + 1 client).
- [ ] Test access control: allow dashboard access only via WireGuard.
- [ ] Write nftables or ufw rules to restrict dashboard ports.
- [ ] Verify active sockets with ss or netstat.
- [ ] Experiment with HTTPS setup (self-signed + Let’s Encrypt).
- [ ] Bind services to specific interfaces/IPs and confirm behavior.

---

## 🔒 3. Auth Experiments (Core of the Project)
**Goal:** Build your own “local system-level auth” securely.

- [ ] Write a small Python script that authenticates via PAM.
- [ ] Test sudo group detection (check if user ∈ sudo).
- [ ] Write a minimal PAM helper using a UNIX socket.
- [ ] Restrict socket access via UNIX permissions.
- [ ] Add simple rate-limiting for failed logins.
- [ ] Explore Flask/FastAPI session management (cookies, expiry, flags).

---

## 🧱 4. Backend & API Foundations
**Goal:** Get fluent enough with backend patterns to migrate to JEE later.

- [ ] Create a small REST API with /auth/login and /system/info endpoints.
- [ ] Serialize clean JSON responses (avoid clutter).
- [ ] Test both JWT and cookie-based sessions.
- [ ] Add a /api/logs/recent endpoint for practice.
- [ ] Implement proper error handling and status codes.

---

## 💾 5. System Info Gathering (for Dashboard)
**Goal:** Produce the raw data your dashboard will visualize.

- [ ] Read CPU and RAM info from /proc.
- [ ] Parse /sys/class/thermal for temperature data.
- [ ] Query disk usage via os.statvfs() or psutil.
- [ ] Collect service states with systemctl is-active.
- [ ] Combine everything into a Python JSON output module.

---

## 🧠 6. Security Layer Testing
**Goal:** Verify your admin dashboard is fully locked down.

- [ ] Restrict access: test firewall + WireGuard rules.
- [ ] Run external nmap scans to confirm invisibility.
- [ ] Check HTTPS headers using curl -v.
- [ ] Verify cookie flags (Secure, HttpOnly, SameSite).
- [ ] Simulate failed auth attempts and confirm logging.

---

## 💥 7. Deployment Experiments
**Goal:** Understand your full service lifecycle from boot to crash recovery.

- [ ] Host a basic test site using Nginx.
- [ ] Reverse proxy your Python app (localhost → Nginx).
- [ ] Use systemd to manage your dashboard service.
- [ ] Test auto-restart and logging after crashes.
- [ ] Simulate a failure and verify your recovery process.

---

## 🧩 8. Future Integrations
**Goal:** Extend functionality once your base is rock-solid.

- [ ] Add a metrics database (SQLite or InfluxDB) for historical data.
- [ ] Send alerts via email or webhook.
- [ ] Add audit logging (logins, failed attempts).
- [ ] Integrate a front-end graph library (Chart.js or Recharts).

---

**Progress Notes:**  
_(Use this space to jot what you learned, what worked, and what broke 😄)_
