# 🧠 Linux, Networking & Git Mastery Roadmap

## ⚙️ 1. Linux System Internals
**Goal:** Understand and control your system environment deeply.

- [ ] Explore `/proc` and `/sys` to see live kernel/system info.  
- [ ] Use `ps`, `top`, `htop`, and `free` to monitor processes and memory.  
- [ ] Check disk usage with `df`, `du`, and `lsblk`.  
- [ ] Play with `chmod`, `chown`, and `umask` — practice permission control.  
- [ ] Create new users/groups and adjust `/etc/sudoers` safely.  
- [ ] Study `journalctl` filtering (`-u`, `--since`, etc.) to read logs efficiently.  
- [ ] Write and test a custom `systemd` service unit (ExecStart, Restart, etc.).  
- [ ] Practice enabling/disabling and restarting services via `systemctl`.  
- [ ] Simulate a crash and confirm your service auto-recovers.

---

## 🌐 2. Networking & Connectivity
**Goal:** Learn to inspect, configure, and debug connections like a sysadmin.

- [ ] List interfaces and routes using `ip a`, `ip route`, and `ip link`.  
- [ ] Experiment with `ping`, `curl`, `wget`, `dig`, and `traceroute`.  
- [ ] Open and close ports with `ufw` or `nftables`.  
- [ ] Run `ss -tulpen` and `netstat` to view active sockets.  
- [ ] Inspect DNS resolution and host lookup using `/etc/hosts` and `/etc/resolv.conf`.  
- [ ] Practice binding services to specific interfaces (e.g., `127.0.0.1` vs `0.0.0.0`).  
- [ ] Set up a minimal WireGuard connection (server + client).  
- [ ] Test access control: block HTTP access unless on WireGuard.  
- [ ] Simulate bad routing and packet loss to understand network resilience.

---

## 🔐 3. Security & Permissions
**Goal:** Apply real-world restrictions and hardening.

- [ ] Lock down a directory with strict permissions and test access as different users.  
- [ ] Configure and test `sudo` privileges for a limited user.  
- [ ] Learn how `setuid` and `setgid` work (safely, with dummy binaries).  
- [ ] Inspect running services with `ps aux` and confirm who owns them.  
- [ ] Enable firewall logging and verify blocked attempts.  
- [ ] Explore PAM configs in `/etc/pam.d` and test authentication modules.  
- [ ] Create a restricted shell environment (limited PATH, etc.).

---

## 🧩 4. Git & GitHub CLI Mastery
**Goal:** Manage full repositories directly from the terminal.

- [ ] Install and configure `git` (user.name, user.email, signing keys).  
- [ ] Create a local repo: `git init`, `add`, `commit`, `log`.  
- [ ] Connect to GitHub via SSH or token.  
- [ ] Use branches: `git branch`, `checkout`, `merge`, and resolve conflicts.  
- [ ] Rebase workflow: `git rebase main` and fix small conflicts manually.  
- [ ] Test stashing changes (`git stash`, `pop`, `apply`).  
- [ ] Use GitHub CLI (`gh`) to create issues and pull requests from terminal.  
- [ ] Automate repo creation: `gh repo create <name> --private`.  
- [ ] Create a test release with `gh release create` and attach a changelog.

---

## 🧰 5. Practice & Integration
**Goal:** Combine your Linux, networking, and git skills in small projects.

- [ ] Host a simple Python app behind Nginx on Fedora.  
- [ ] Manage the app with a `systemd` service unit.  
- [ ] Secure it with a WireGuard-only access rule.  
- [ ] Track project code via GitHub, push from CLI only.  
- [ ] Document everything in Markdown as you go (commands, issues, fixes).  
- [ ] Review and clean your cheat sheets once per week to solidify knowledge.

---

**🗒️ Progress Notes:**  
_(Use this space for what worked, what failed, and what surprised you.)_
