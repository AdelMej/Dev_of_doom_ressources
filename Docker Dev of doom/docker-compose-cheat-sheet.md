# Docker Compose Cheat Sheet (docker-compose.yml)

This cheat sheet covers the **core invariants**, common patterns, and best practices for `docker-compose.yml`.
Works for **Docker Compose v2** and **Podman Compose** (with minor differences).

---

## 1. File Basics

```yaml
version: "3.9"

services:
  app:
    image: myapp:latest
```

### Invariants
- YAML is **indentation-sensitive**
- Use **spaces**, never tabs
- Two spaces per level is standard

---

## 2. Services (Core Unit)

```yaml
services:
  backend:
    image: my-backend
    container_name: backend
```

### Key Rules
- One service = one container
- Services talk to each other via **service name DNS**
- No `localhost` between containers

---

## 3. Images vs Build

### Use a prebuilt image
```yaml
image: nginx:latest
```

### Build from Dockerfile
```yaml
build:
  context: .
  dockerfile: Dockerfile
```

### Invariant
- `image` pulls
- `build` builds
- You can use **both** (build then tag)

---

## 4. Ports (Host ↔ Container)

```yaml
ports:
  - "9000:80"
```

Format:
```
HOST:CONTAINER
```

### Invariants
- Only needed to expose to the host
- Containers do NOT need ports to talk to each other

---

## 5. Volumes (Persistence)

### Named volume
```yaml
volumes:
  - db-data:/var/lib/postgresql/data
```

### Bind mount
```yaml
volumes:
  - ./config:/etc/app:ro
```

### Invariants
- Volumes persist after container deletion
- `:ro` = read-only
- Bind mounts mirror host files

---

## 6. Networks (Default is Enough)

```yaml
networks:
  default:
    driver: bridge
```

### Invariants
- All services are on the same network by default
- DNS name = service name
- No IP hardcoding

---

## 7. Environment Variables

```yaml
environment:
  POSTGRES_USER: user
  POSTGRES_PASSWORD: pass
```

Or from file:
```yaml
env_file:
  - .env
```

### Invariant
- Never hardcode secrets in git

---

## 8. Depends On (Startup Order)

```yaml
depends_on:
  - db
```

### Important
- Controls startup order ONLY
- Does NOT wait for readiness

---

## 9. Command & Entrypoint

```yaml
command: ["npm", "run", "dev"]
```

Overrides Dockerfile CMD

---

## 10. Restart Policies

```yaml
restart: unless-stopped
```

Options:
- no
- always
- on-failure
- unless-stopped

---

## 11. Logging

```yaml
logging:
  driver: json-file
```

### Invariant
- Containers log to stdout/stderr
- Host handles log storage

---

## 12. Example: Full Stack

```yaml
services:
  frontend:
    image: nginx:latest
    ports:
      - "9000:80"

  backend:
    build: ./backend
    ports:
      - "5252:5252"

  db:
    image: postgres:16
    volumes:
      - db-data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: dev

volumes:
  db-data:
```

---

## 13. Debug Commands

```bash
docker compose up
docker compose down
docker compose logs -f
docker compose ps
docker compose exec backend sh
```

(Podman: `podman-compose`)

---

## 14. Hard Invariants (Never Break These)

- One concern per container
- Stateless containers
- State lives in volumes
- Service name > localhost
- Rebuild > patch
- Delete containers without fear

---

## 15. Dev vs Prod Mindset

| Dev | Prod |
|----|------|
| Bind mounts | Named volumes |
| Exposed ports | Reverse proxy |
| Debug logs | Structured logs |
| Resettable DB | Persistent DB |

---

**docker-compose is orchestration, not magic.**
If something breaks: inspect → isolate → rebuild.
