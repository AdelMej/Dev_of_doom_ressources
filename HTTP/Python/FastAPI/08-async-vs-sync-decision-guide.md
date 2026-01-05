# FastAPI Async vs Sync Decision Guide

## Purpose

This document explains **when to use async and when to stay
synchronous** in FastAPI.

Goals: - avoid fake async - prevent performance traps - keep code simple
and correct - make scaling decisions explicit

Golden rule: \> **Async is a tool, not a default.**

------------------------------------------------------------------------

## FastAPI Execution Model

FastAPI supports **both**: - synchronous (`def`) - asynchronous
(`async def`) endpoints

Under the hood: - sync code runs in a threadpool - async code runs on
the event loop

Both are valid. Mixing them blindly is not.

------------------------------------------------------------------------

## When to Use Sync (`def`)

Use **sync code** when: - using blocking libraries - using classic
SQLAlchemy (sync) - doing CPU-bound work - simplicity matters more than
concurrency

Examples: - password hashing - ORM queries (sync SQLAlchemy) - file
system access - CPU-heavy validation

### Sync Endpoint Example

``` python
@router.post("/users")
def create_user(payload: UserCreate):
    return service.create_user(payload)
```

✅ simple\
✅ predictable\
✅ safe

------------------------------------------------------------------------

## When to Use Async (`async def`)

Use **async code** when: - waiting on I/O - using async-native
libraries - handling many concurrent connections

Examples: - async DB drivers - HTTP calls (httpx, aiohttp) -
websockets - streaming responses

### Async Endpoint Example

``` python
@router.get("/external-data")
async def fetch_data():
    return await external_client.fetch()
```

------------------------------------------------------------------------

## Fake Async (Anti-Pattern)

🚫 Async wrapper around blocking code

``` python
async def bad():
    result = blocking_call()
    return result
```

This: - blocks the event loop - kills concurrency - is worse than sync

If the library is blocking → **stay sync**.

------------------------------------------------------------------------

## Async SQLAlchemy Rules

If you choose async DB: - EVERYTHING becomes async - repositories -
services - routers

Never mix: - async repo + sync service - sync repo + async service

Rule: \> **One async boundary per request.**

------------------------------------------------------------------------

## CPU-Bound Work

Async does NOT help CPU-bound tasks.

Examples: - encryption - compression - image processing

Solutions: - keep sync - offload to worker (Celery, RQ) - use
multiprocessing

------------------------------------------------------------------------

## Performance Reality Check

Async helps when: - waiting dominates execution time

Async does NOT help when: - CPU dominates - DB driver is blocking - code
is simple and fast

Most APIs are DB-bound → async helps **only with async DB drivers**.

------------------------------------------------------------------------

## Threadpool Reality

Sync endpoints: - run in a threadpool - scale reasonably well - simpler
to reason about

Modern servers handle **thousands** of sync requests just fine.

------------------------------------------------------------------------

## Decision Matrix

  Use Case               Choice
  ---------------------- --------
  Classic SQLAlchemy     Sync
  Async SQLAlchemy       Async
  External HTTP APIs     Async
  CPU-heavy logic        Sync
  Simple CRUD API        Sync
  High concurrency I/O   Async

------------------------------------------------------------------------

## Migration Strategy

Start: - sync everywhere

Migrate to async: - only when proven necessary - per feature, not
globally

Never rewrite everything for async prematurely.

------------------------------------------------------------------------

## Testing Considerations

-   sync tests are simpler
-   async tests require event loops
-   more tooling, more complexity

Prefer sync unless async is justified.

------------------------------------------------------------------------

## Mental Model

    Async = waiting efficiently
    Sync  = doing work efficiently

------------------------------------------------------------------------

## TL;DR

-   Async is not faster by default
-   Blocking libs → sync
-   Async DB → async everywhere
-   CPU work → sync
-   Simplicity beats theoretical performance

> Premature async is the new premature optimization.
