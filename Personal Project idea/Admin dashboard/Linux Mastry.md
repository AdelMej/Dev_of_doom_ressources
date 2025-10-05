# 💥 Linux Mastery Checklist (Hands-On + Progress-Oriented)

## 🧩 Level 0 — Quick Foundations
- [ ] Master shell navigation: `ls`, `cd`, `cp`, `mv`, `rm`, `cat`, `less`, `grep`, `find`
- [ ] Edit files confidently (`vim`, `nano`)
- [ ] Understand permissions: `chmod`, `chown`, `stat`
- [ ] Test user switching and permission boundaries

---

## 👤 Level 1 — System & Users
- [ ] Manage users and groups: `useradd`, `usermod`, `groupadd`, `passwd`
- [ ] Control privileges via `sudo` and `/etc/sudoers`
- [ ] Process management: `ps`, `top`, `htop`, `kill`, `nice`
- [ ] Logging: `journalctl -u <service>`, `journalctl --since "1 hour ago"`

---

## 💾 Level 2 — Storage & Filesystems
- [ ] Disk usage and devices: `df -h`, `du -sh`, `lsblk`, `fdisk -l`
- [ ] Mount and unmount manually: `mount`, `umount`, `/etc/fstab`
- [ ] Try loopback mounts and temporary filesystems
- [ ] Explore ext4 and Btrfs (snapshots if possible)

---

## 🌐 Level 3 — Networking Fundamentals
- [ ] Interface & route management: `ip a`, `ip route`, `ip link`
- [ ] Test connections: `ping`, `curl -v`, `traceroute`, `dig`
- [ ] Analyze open ports: `ss -tulpen`, `netstat -tulpen`
- [ ] Firewall rules: `ufw` or `nftables` — open/close specific ports

---

## 🔐 Level 4 — Secure Access & WireGuard
- [ ] Configure SSH key authentication and disable password login
- [ ] Harden SSH config (`sshd_config`)
- [ ] Set up WireGuard (server + client)
- [ ] Test private subnet reachability via WG tunnel

---

## ⚙️ Level 5 — systemd Deep Dive
- [ ] Create and enable a custom service (`.service`)
- [ ] Use `systemctl` for start/stop/status
- [ ] Experiment with `.timer` and templated units (`@.service`)
- [ ] Inspect properties: `systemctl show <unit>`
- [ ] Configure journald persistence and rate limits

---

## 🧠 Level 6 — D-Bus & systemd Bus
- [ ] Explore with `busctl tree org.freedesktop.systemd1`
- [ ] Introspect the `Manager` interface and its methods
- [ ] Use `busctl call` and `dbus-monitor --system`
- [ ] Write a Python script (read-only) to call `ListUnits()`
- [ ] Experiment with `StartUnit`/`StopUnit` and Polkit restrictions

---

## 🛡️ Level 7 — SELinux (or AppArmor)
- [ ] Check mode with `sestatus` and switch between Enforcing/Permissive
- [ ] Investigate denials with `ausearch`, `sealert`
- [ ] Use `restorecon` to fix mislabeled files
- [ ] Write a minimal SELinux policy or adjust booleans for your app

---

## 🔒 Level 8 — Security Hardening & Privilege Control
- [ ] Apply systemd sandboxing: `ProtectSystem`, `PrivateTmp`, `NoNewPrivileges`
- [ ] Configure Polkit rules for dashboard user
- [ ] Limit allowed services and actions
- [ ] Explore PAM authentication and session limits
- [ ] Audit all sensitive operations with timestamps

---

## 📈 Level 9 — Monitoring, Backups & Recovery
- [ ] Use `psutil` or `/proc` to get CPU/mem/temperature data
- [ ] Configure `logrotate` for dashboard logs
- [ ] Automate snapshots or backups, test restoring them
- [ ] Simulate service crashes and recovery procedures

---

## 🧰 Level 10 — Integration & Ops Workflow
- [ ] Connect everything: WG → Auth → D-Bus → REST → Dashboard
- [ ] GitOps-style config updates with rollback
- [ ] Maintain clean documentation and cheat sheets
- [ ] Test full restart/recovery cycle of your system
- [ ] Regularly review and update your security model

---

## 🗒️ Notes
_(Keep experiment logs, gotchas, and command results here for future cheat-sheet entries.)_
