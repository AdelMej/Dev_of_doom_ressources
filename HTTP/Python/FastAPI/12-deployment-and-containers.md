# FastAPI Deployment & Containers Cheat Sheet

## Purpose

This document defines **clean, repeatable deployment practices** for
FastAPI.

Goals: - predictable builds - environment parity - horizontal
scalability - zero framework coupling to infrastructure

------------------------------------------------------------------------

## Core Principles

-   app code must be **infra-agnostic**
-   configuration via environment variables
-   containers are immutable
-   scaling happens outside the app

------------------------------------------------------------------------

## Deployment Flow

    Code
     → Tests
     → Build Image
     → Push Image
     → Deploy (Pods / VMs)
     → Observe

FastAPI is only responsible for **request handling**.

------------------------------------------------------------------------

## Docker Basics

### Minimal Dockerfile

``` dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Rules: - slim base images - no dev tools in prod - single responsibility
container

------------------------------------------------------------------------

## Environment Configuration

Never bake config into images.

Use: - `.env` locally - environment variables in prod

Example:

``` env
DATABASE_URL=postgresql://...
ENV=production
```

Load via settings layer.

------------------------------------------------------------------------

## Development vs Production

  Concern   Dev        Prod
  --------- ---------- ------------
  Reload    yes        no
  Workers   1          many
  Logs      verbose    structured
  TLS       optional   mandatory

------------------------------------------------------------------------

## Uvicorn / Gunicorn in Prod

Recommended:

``` bash
gunicorn   -k uvicorn.workers.UvicornWorker   -w 4   main:app
```

Scaling rule: - scale **workers**, not threads - one worker ≈ one CPU
core

------------------------------------------------------------------------

## Reverse Proxy

Always place FastAPI behind: - Nginx - Traefik - Envoy

Responsibilities: - TLS termination - compression - rate limiting -
security headers

------------------------------------------------------------------------

## Containers & Databases

Rules: - DBs are external services - containers are stateless -
migrations run separately

Never: 🚫 run DB inside same container\
🚫 auto-migrate on app startup

------------------------------------------------------------------------

## Kubernetes Mental Model

    Pod
     ├── FastAPI container
     └── Config (env, secrets)

FastAPI must be: - stateless - restart-safe - horizontally scalable

------------------------------------------------------------------------

## Health Checks

Expose:

``` python
@app.get("/health")
def health():
    return {"status": "ok"}
```

Used by: - load balancers - orchestrators

No business logic inside health checks.

------------------------------------------------------------------------

## Zero-Downtime Deployments

Requirements: - no in-memory state - graceful shutdown - idempotent
startup

Uvicorn supports graceful shutdown by default.

------------------------------------------------------------------------

## Observability in Prod

Must have: - logs → stdout - metrics → external system - traces →
optional

Containers do not store logs.

------------------------------------------------------------------------

## Common Deployment Mistakes

🚫 Putting uvicorn logic in app code\
🚫 Hardcoded config\
🚫 Running as root\
🚫 One container = entire system\
🚫 Stateful app design

------------------------------------------------------------------------

## Mental Model

    FastAPI = HTTP adapter
    Docker = packaging
    Proxy = security + routing
    Orchestrator = scaling

------------------------------------------------------------------------

## TL;DR

-   Containers are immutable
-   Config via env vars
-   TLS at proxy
-   Scale workers, not threads
-   App stays dumb & stateless

> If your app knows where it runs, it's already wrong.
