# FastAPI Logging & Observability Cheat Sheet

## Purpose

This document defines a **clean, production-grade logging strategy** for
FastAPI applications.

Goals: - structured, consistent logs - zero logging noise in business
logic - visibility across services - readiness for containers, pods, and
centralized logging

------------------------------------------------------------------------

## Logging Principles

1.  **Log once, log centrally**
2.  **Business logic does not log control flow**
3.  **Errors are logged at the boundary**
4.  **Logs are machine-readable**

Golden rule: \> If you need logs to understand business logic, the code
is already bad.

------------------------------------------------------------------------

## What Should Be Logged

### Always log

-   unhandled exceptions
-   request lifecycle (start/end)
-   authentication failures
-   database failures
-   external service failures

### Never log

-   passwords
-   tokens
-   secrets
-   personal data (PII)
-   internal stack traces to clients

------------------------------------------------------------------------

## Logging Layers

    Middleware        → request / response logs
    Exception Handler → error logs
    Infrastructure    → startup / shutdown
    Service           → NO logging

------------------------------------------------------------------------

## Logging Configuration Location

-   `app/logging.py`
-   configured once
-   imported in `main.py`

Never configure logging: - in routers - in services - in repositories

------------------------------------------------------------------------

## Basic Logging Configuration

``` python
import logging
import sys

def configure_logging():
    logging.basicConfig(
        level=logging.INFO,
        format="%(asctime)s %(levelname)s %(name)s %(message)s",
        handlers=[logging.StreamHandler(sys.stdout)],
    )
```

Called from `main.py` only.

------------------------------------------------------------------------

## Using Loggers Correctly

``` python
import logging

logger = logging.getLogger(__name__)
```

Rules: - one logger per module - never use root logger directly - never
print()

------------------------------------------------------------------------

## Request Logging (Middleware)

``` python
from fastapi import Request
import time
import logging

logger = logging.getLogger("http")

async def logging_middleware(request: Request, call_next):
    start = time.time()
    response = await call_next(request)
    duration = time.time() - start

    logger.info(
        "request completed",
        extra={
            "method": request.method,
            "path": request.url.path,
            "status": response.status_code,
            "duration_ms": int(duration * 1000),
        },
    )
    return response
```

------------------------------------------------------------------------

## Error Logging

Log errors **once**, globally:

``` python
def global_exception_handler(request, exc):
    logger.exception("Unhandled exception")
    return JSONResponse(...)
```

Never log the same error again downstream.

------------------------------------------------------------------------

## Correlation IDs (Request IDs)

Add a request ID for tracing:

``` python
import uuid

request_id = str(uuid.uuid4())
```

Attach it to: - logs - response headers

This is critical in distributed systems.

------------------------------------------------------------------------

## Structured Logging (Recommended)

Use JSON logs in production:

``` json
{
  "timestamp": "...",
  "level": "ERROR",
  "service": "auth",
  "request_id": "...",
  "message": "Database connection failed"
}
```

Benefits: - searchable - aggregatable - cloud-native

------------------------------------------------------------------------

## Uvicorn Logging

Uvicorn has its own loggers: - `uvicorn` - `uvicorn.error` -
`uvicorn.access`

Control them via logging config --- not code.

Never call:

``` python
uvicorn.run(..., log_config=...)
```

inside application logic.

------------------------------------------------------------------------

## Logging in Containers / Pods

Rules: - log to stdout - never log to files - let the platform collect
logs

Kubernetes, Docker, Nomad expect stdout logs.

------------------------------------------------------------------------

## Observability Beyond Logs

Logs are not enough.

Add: - metrics (Prometheus) - tracing (OpenTelemetry) - health checks

But keep logging simple and reliable first.

------------------------------------------------------------------------

## Anti-Patterns

🚫 Logging inside services\
🚫 Logging validation failures manually\
🚫 Logging secrets\
🚫 Using `print()`\
🚫 Multiple logging configs

------------------------------------------------------------------------

## Mental Model

    Logs answer: "What happened?"
    Metrics answer: "How often?"
    Traces answer: "Where?"

------------------------------------------------------------------------

## TL;DR

-   Configure logging once
-   Log at boundaries
-   Structured logs \> text logs
-   stdout only
-   Services stay silent

> Clean logs make production boring --- and boring is good.
