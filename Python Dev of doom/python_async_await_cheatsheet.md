Python Async/Await Cheat Sheet
=============================

1. Basics: async and await
--------------------------
```python
import asyncio

async def say_hello():
    print("Hello")
    await asyncio.sleep(1)
    print("World")

asyncio.run(say_hello())
```

2. Running multiple coroutines concurrently
------------------------------------------
# asyncio.gather
```python
async def task(n):
    await asyncio.sleep(n)
    return f"Task {n} done"

async def main():
    results = await asyncio.gather(task(1), task(2), task(3))
    print(results)

asyncio.run(main())
```

# asyncio.create_task
```python
async def main():
    t1 = asyncio.create_task(task(1))
    t2 = asyncio.create_task(task(2))
    await t1
    await t2

asyncio.run(main())
```

3. asyncio.sleep
----------------
```python
await asyncio.sleep(2)  # Non-blocking sleep
```

4. Async context managers
------------------------
```python
class AsyncCM:
    async def __aenter__(self):
        print("Enter")
        return self
    async def __aexit__(self, exc_type, exc, tb):
        print("Exit")

async def main():
    async with AsyncCM():
        print("Inside context")

asyncio.run(main())
```

5. Async iteration
------------------
```python
class AsyncCounter:
    def __init__(self, n):
        self.n = n
    def __aiter__(self):
        self.i = 0
        return self
    async def __anext__(self):
        if self.i >= self.n:
            raise StopAsyncIteration
        await asyncio.sleep(0.5)
        self.i += 1
        return self.i

async def main():
    async for num in AsyncCounter(3):
        print(num)

asyncio.run(main())
```

6. Error handling in async
-------------------------
```python
async def may_fail(n):
    if n == 2:
        raise ValueError("Oops")
    return n

async def main():
    try:
        results = await asyncio.gather(may_fail(1), may_fail(2))
    except ValueError as e:
        print("Caught:", e)

asyncio.run(main())
```

7. Key Points
-------------
- `await` can only be used inside `async def`.
- Use `asyncio.run()` as entry point in scripts.
- `asyncio.create_task()` → fire-and-forget.
- `asyncio.gather()` → wait for multiple coroutines.
- `async for` → iterate asynchronously.
- `async with` → use async context managers.

