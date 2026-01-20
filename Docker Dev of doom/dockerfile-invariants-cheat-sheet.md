# Dockerfile Invariants Cheat Sheet

These are **non-negotiable principles** for writing sane, maintainable Dockerfiles.

---

## 1. One Container = One Responsibility
- A container should do **one thing**
- API, worker, DB, job runner — not all at once
- Scale horizontally, not internally

---

## 2. Reproducible Builds
- Same Dockerfile → same result
- No interactive commands
- Always use `-y` or non-interactive flags
- No host dependencies

---

## 3. Stateless by Default
- Containers do **not** own data
- Persistent data must use volumes or bind mounts
- Deleting a container should be safe

---

## 4. Build Time ≠ Run Time
- `RUN` → build steps only
- `CMD` / `ENTRYPOINT` → runtime
- Never start services during build

---

## 5. One Foreground Process (PID 1)
- One main process per container
- No background daemons
- No `systemd` unless you truly know why

---

## 6. Logs Go to stdout/stderr
- No log files
- No log rotation inside containers
- Let the platform handle logs

---

## 7. Layer Order Matters (Caching)
- Least-changing steps first
- Most-changing steps last
- Avoid invalidating cache unnecessarily

---

## 8. Base Image Is a Design Choice
- ubuntu → dev & comfort
- debian-slim → balanced prod
- alpine → small but painful
- fedora → modern & SELinux-friendly

---

## 9. No Secrets in Dockerfiles
- Never hardcode secrets
- Use environment variables or secrets managers
- Assume Dockerfiles are public

---

## 10. ENTRYPOINT vs CMD
- CMD → default command
- ENTRYPOINT → container identity
- If unsure, use CMD

---

## 11. Containers Are Disposable
- You should be happy deleting containers
- Persistence belongs outside the container

---

## 12. Dockerfile Is Not Orchestration
- No networking logic
- No service dependency management
- Use compose, systemd, or Kubernetes

---

## Mental Model
> A Dockerfile answers:
> “From nothing, how do I build an environment that runs **one thing**, predictably, and exits cleanly?”

If it does more than that — it's wrong.
