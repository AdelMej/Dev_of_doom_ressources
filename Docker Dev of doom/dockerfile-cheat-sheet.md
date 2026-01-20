# Dockerfile Cheat Sheet

## Basic Structure
```Dockerfile
FROM image:tag
WORKDIR /app
COPY . .
RUN command
CMD ["executable", "arg"]
```

---

## Base Image
```Dockerfile
FROM ubuntu:22.04
FROM node:20-alpine
FROM python:3.12-slim
```

- One `FROM` per stage
- First `FROM` defines the base
- Prefer slim/alpine when possible

---

## WORKDIR
```Dockerfile
WORKDIR /app
```

- Creates directory if missing
- Replaces `cd`
- Persists for following instructions

---

## COPY vs ADD
```Dockerfile
COPY src/ /app/src/
ADD archive.tar.gz /app/
```

- Prefer `COPY`
- `ADD` auto-extracts archives and supports URLs (often unwanted)

---

## RUN
```Dockerfile
RUN apt update && apt install -y curl
RUN npm install
```

- Each `RUN` = new layer
- Chain commands to reduce layers
- Clean caches when possible

---

## CMD vs ENTRYPOINT
```Dockerfile
CMD ["node", "app.js"]
ENTRYPOINT ["python"]
```

- `CMD` = default arguments
- `ENTRYPOINT` = fixed executable
- Can be combined

---

## EXPOSE
```Dockerfile
EXPOSE 8080
```

- Documentation only
- Does NOT publish ports

---

## ENV
```Dockerfile
ENV NODE_ENV=production
ENV PORT=8080
```

- Runtime environment variables
- Avoid secrets here

---

## ARG
```Dockerfile
ARG VERSION
ARG TARGET=prod
```

- Build-time only
- Not available at runtime

---

## USER
```Dockerfile
USER appuser
```

- Avoid running as root
- Works well with Podman rootless

---

## VOLUME
```Dockerfile
VOLUME /data
```

- Declares mount point
- Data persists outside container

---

## Multi-Stage Builds
```Dockerfile
FROM node:20 AS build
WORKDIR /app
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
```

- Smaller final images
- Build tools never shipped

---

## .dockerignore
```
node_modules
.git
.env
dist
```

- Reduces build context
- Faster builds
- Smaller images

---

## Best Practices
- One concern per container
- Minimize layers
- Pin image versions
- Avoid secrets in images
- Prefer immutable images

---

## Common Anti-Patterns
❌ Multiple services in one container  
❌ Running as root  
❌ Large base images without reason  
❌ Baking config/state into image  

---

## Works With
- Docker
- Podman
- Buildah
- Kubernetes (OCI)

Dockerfile = OCI build spec, not Docker-specific.
