# 🧠 Systemd D-Bus Control Roadmap

## ⚙️ 1. Core Understanding
**Goal:** Learn what D-Bus is and how systemd exposes its API.

- [ ] Read about `org.freedesktop.systemd1` on the system bus.  
- [ ] Understand the main object path: `/org/freedesktop/systemd1`.  
- [ ] Identify the `Manager` interface and its key methods:  
  - `ListUnits()`  
  - `StartUnit()`  
  - `StopUnit()`  
  - `RestartUnit()`  
  - `EnableUnitFiles()`  
- [ ] Explore with `busctl` to understand the structure.

---

## 🧱 2. Manual Exploration (CLI)
**Goal:** Get hands-on with D-Bus before automating.

- [ ] Run `busctl tree org.freedesktop.systemd1` to explore the hierarchy.  
- [ ] Use `busctl introspect org.freedesktop.systemd1 /org/freedesktop/systemd1`.  
- [ ] List running services:  
  `busctl call org.freedesktop.systemd1 /org/freedesktop/systemd1 org.freedesktop.systemd1.Manager ListUnits`.  
- [ ] Query a specific service’s path and details.  
- [ ] Experiment with `gdbus` or `dbus-send` for alternative approaches.  
- [ ] Observe D-Bus activity with `dbus-monitor --system`.

---

## 🧩 3. Python Experiments (Read-Only)
**Goal:** Connect to systemd D-Bus and list units safely.

- [ ] Install `dbus-next` or `pydbus` (`pip install dbus-next`).  
- [ ] Connect to the **system bus**.  
- [ ] Get the `org.freedesktop.systemd1.Manager` object.  
- [ ] Call `ListUnits()` and print each service name + active state.  
- [ ] Filter only `.service` units.  
- [ ] Output formatted JSON to use later in your dashboard.

---

## 🔧 4. Service Control (Write Operations)
**Goal:** Safely manage specific services.

- [ ] Use `StartUnit('nginx.service', 'replace')` to start a service.  
- [ ] Use `StopUnit('nginx.service', 'replace')` to stop one.  
- [ ] Use `RestartUnit('nginx.service', 'replace')` to restart.  
- [ ] Experiment with `EnableUnitFiles()` and `DisableUnitFiles()`.  
- [ ] Confirm that write access requires privileges (root or Polkit).  
- [ ] Log all operations with timestamps for auditing.

---

## 🔐 5. Integrating Polkit for Safe Permissions
**Goal:** Allow limited, specific service management without root.

- [ ] Create `/etc/polkit-1/rules.d/90-dashboard.rules`.  
- [ ] Grant permission for dashboard user to manage only whitelisted services.  
- [ ] Deny all other operations.  
- [ ] Test with `pkttyagent` and your app’s user to confirm behavior.  
- [ ] Verify no password prompt for allowed services.  
- [ ] Check `/var/log/auth.log` or `journalctl` for audit traces.

---

## 🌐 6. Web API Integration
**Goal:** Expose a REST layer for your dashboard.

- [ ] Create `/api/services` endpoint → returns JSON from `ListUnits()`.  
- [ ] Create `/api/service/<name>/status` → returns `ActiveState`.  
- [ ] Create `/api/service/<name>/restart` → calls `RestartUnit()`.  
- [ ] Add authentication (session or token-based).  
- [ ] Implement logging for every service action.  
- [ ] Test access restrictions (only sudoers or PAM-auth users).

---

## 🧰 7. Advanced Features
**Goal:** Make the dashboard dynamic and reactive.

- [ ] Subscribe to D-Bus `PropertiesChanged` signals for live updates.  
- [ ] Implement a WebSocket channel for real-time service status.  
- [ ] Build a small front-end panel to show service states (Active, Failed, Inactive).  
- [ ] Add buttons for start/stop/restart tied to your REST API.  
- [ ] Log and visualize service uptime data.

---

## 🧠 8. Safety Checklist
**Goal:** Keep system control safe and contained.

- [ ] Dashboard never runs as root.  
- [ ] Only Polkit grants service access.  
- [ ] All inputs are validated and sanitized.  
- [ ] Audit logs track every management action.  
- [ ] HTTPS enforced; WireGuard-only external access.  
- [ ] Backups before deploying new dashboard versions.

---

## 🗒️ Progress Notes
_(Keep notes about D-Bus responses, Polkit quirks, and service behavior.)_
