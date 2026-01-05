# FastAPI Production Server: Uvicorn & Gunicorn Cheat Sheet

## Purpose

This document explains **how to run FastAPI in production** correctly
using **Uvicorn** and **Gunicorn**.

Goals: - keep server config out of application code - understand worker
models - scale safely - avoid common production mistakes

Golden rule: \> **Your app should not know how it is served.**

------------------------------------------------------------------------

## Development vs Production

### Development

-   hot reload
-   single process
-   fast feedback

Use:

``` bash
fastapi dev app.main:app
```

------------------------------------------------------------------------

### Production

-   multiple workers
-   no reload
-   controlled concurrency

Use: - Uvicorn (simple) - Gunicorn + Uvicorn workers (recommended)

------------------------------------------------------------------------

## Uvicorn in Production (Basic)

### Command

``` bash
uvicorn app.main:app   --host 0.0.0.0   --port 8000   --workers 4
```

### Notes

-   `--workers` = number of processes
-   each worker has its own event loop
-   good enough for small deployments

------------------------------------------------------------------------

## Gunicorn + Uvicorn Workers (Recommended)

### Why Gunicorn?

Gunicorn provides: - process management - graceful restarts - signal
handling - better stability

Uvicorn provides: - ASGI event loop - async support

Best of both worlds.

------------------------------------------------------------------------

## Gunicorn Command

``` bash
gunicorn app.main:app   -k uvicorn.workers.UvicornWorker   -w 4   -b 0.0.0.0:8000
```

------------------------------------------------------------------------

## Worker Count Strategy

Rule of thumb:

``` text
workers = (CPU cores * 2) + 1
```

Examples: - 2 cores → 5 workers - 4 cores → 9 workers

Adjust based on: - memory usage - DB connections - traffic patterns

------------------------------------------------------------------------

## Threads vs Workers

### Workers

-   separate processes
-   true parallelism
-   higher memory usage

### Threads

-   shared memory
-   limited by GIL
-   useful for blocking I/O

FastAPI: - prefers workers over threads

------------------------------------------------------------------------

## Async vs Sync Workers

-   Sync endpoints → fine with multiple workers
-   Async endpoints → benefit from fewer workers + async concurrency

Do NOT oversubscribe workers blindly.

------------------------------------------------------------------------

## Configuration via Files

### `gunicorn.conf.py`

``` python
workers = 4
worker_class = "uvicorn.workers.UvicornWorker"
bind = "0.0.0.0:8000"
timeout = 30
loglevel = "info"
```

Run with:

``` bash
gunicorn -c gunicorn.conf.py app.main:app
```

------------------------------------------------------------------------

## Environment Variables

Never hardcode config.

Use: - `.env` - container env vars - secrets manager

Examples:

``` env
ENV=production
LOG_LEVEL=info
```

------------------------------------------------------------------------

## Graceful Shutdown

FastAPI supports lifespan events:

``` python
@app.on_event("shutdown")
def shutdown():
    close_db()
```

Gunicorn handles SIGTERM properly.

------------------------------------------------------------------------

## Reverse Proxy (Recommended)

Use: - Nginx - Traefik - Caddy

Responsibilities: - TLS termination - compression - request buffering -
rate limiting

FastAPI should not handle TLS directly.

------------------------------------------------------------------------

## Containers & Pods

Inside containers: - run **one Gunicorn instance** - let Kubernetes
scale pods - never scale workers × pods blindly

Rule: \> Scale **out** with pods first, then **up** with workers.

------------------------------------------------------------------------

## Health Checks

Expose:

``` http
GET /health
```

Used by: - load balancers - orchestrators - uptime monitors

Must be: - fast - dependency-light

------------------------------------------------------------------------

## Anti-Patterns

🚫 `uvicorn.run()` in code\
🚫 Reload in production\
🚫 One worker per request\
🚫 TLS inside FastAPI\
🚫 Hardcoded ports

------------------------------------------------------------------------

## Mental Model

    FastAPI = application
    Uvicorn = ASGI server
    Gunicorn = process manager
    Proxy = edge

Each layer has one job.

------------------------------------------------------------------------

## TL;DR

-   No server code in app
-   Dev: `fastapi dev`
-   Prod: Gunicorn + Uvicorn workers
-   Scale with workers and pods
-   Config via environment

> If you can swap servers without touching code, you did it right.
