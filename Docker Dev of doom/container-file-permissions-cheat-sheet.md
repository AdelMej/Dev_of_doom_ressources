# Container File Permissions Cheat Sheet (Docker / Podman)

This cheat sheet explains **all permission layers affecting files inside containers**.
Think in **layers**: outer layers override inner ones.

---

## 1. Mount Permissions (STRONGEST)

Applied when mounting volumes or bind mounts.

### Syntax
```bash
-v host_path:container_path:ro
-v host_path:container_path:rw   # default
```

### Meaning
- `ro` → read-only, **nothing can write**, not even root
- `rw` → read-write

**Invariant:**  
Mount permissions override everything inside the container.

---

## 2. Linux File Permissions (chmod / chown)

Classic Unix permissions still apply **inside containers**.

### Common Modes
| Mode | Meaning |
|----|----|
| 644 | owner rw, others r |
| 755 | owner rwx, others rx |
| 600 | owner rw only |

### Example
```dockerfile
RUN chmod 644 /etc/nginx/conf.d/default.conf
```

⚠️ If the mount is `:ro`, chmod does **nothing**.

---

## 3. User Permissions (UID / GID)

Containers run as users.

### Options
```dockerfile
USER appuser
```

```bash
--user 1000:1000
```

If the user does not own the file → no write access.

---

## 4. Linux Capabilities (Root Is Limited)

Containers run with **reduced root privileges**.

### Common Capabilities
| Capability | Purpose |
|----|----|
| NET_BIND_SERVICE | bind ports <1024 |
| CHOWN | change ownership |
| DAC_OVERRIDE | bypass file perms |
| SYS_ADMIN | extremely dangerous |

### Usage
```bash
--cap-drop ALL
--cap-add NET_BIND_SERVICE
```

---

## 5. Read-only Root Filesystem

Locks the entire filesystem.

```bash
--read-only
```

Only explicitly mounted paths remain writable.

Typical pattern:
```bash
--read-only -v /tmp -v /var/run
```

---

## 6. SELinux / AppArmor (Host-Level Security)

On Fedora / RHEL / Arch with MAC enabled.

### SELinux Mount Flags
```bash
:Z   # private label
:z   # shared label
```

Without correct labels → access denied even with correct perms.

---

## Permission Precedence Order

1. Mount mode (`ro` / `rw`)
2. Read-only root FS
3. SELinux / AppArmor
4. Linux capabilities
5. User (UID/GID)
6. File permissions (chmod)

---

## Best-Practice Permission Patterns

### Config Files
```text
Read-only mount + non-root user
```

### Databases
```text
Named volume + rw
```

### Logs
```text
stdout / stderr → journald
```

### Secrets
```text
:ro + 600 perms + non-root
```

### Production Containers
```text
--read-only
--cap-drop ALL
--user non-root
```

---

## Mental Model (Invariant Thinking)

- **Mount permissions are absolute**
- **Root is not god**
- **If a container can write everywhere, it is misconfigured**
- **Integrity beats convenience**

---

## One-line Summary

> Containers are secure **only if you remove permissions on purpose**.

