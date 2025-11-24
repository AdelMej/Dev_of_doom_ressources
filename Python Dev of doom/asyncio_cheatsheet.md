Python asyncio Cheat Sheet
==========================

1. Run a coroutine
-----------------
```python
import asyncio

async def main():
    print("Hello asyncio")

asyncio.run(main())
```

2. Sleep (non-blocking)
-----------------------
```python
await asyncio.sleep(1)  # pause without blocking other coroutines
```

3. Run multiple coroutines concurrently
--------------------------------------
# Using asyncio.gather
```python
async def task(n):
    await asyncio.sleep(n)
    return f"Task {n} done"

async def main():
    results = await asyncio.gather(task(1), task(2), task(3))
    print(results)

asyncio.run(main())
```

# Using asyncio.create_task
```python
async def main():
    t1 = asyncio.create_task(task(1))
    t2 = asyncio.create_task(task(2))
    await t1
    await t2

asyncio.run(main())
```

4. Cancel a task
----------------
```python
async def task():
    await asyncio.sleep(5)

t = asyncio.create_task(task())
t.cancel()
```

5. Timeouts
-----------
```python
await asyncio.wait_for(task(), timeout=2)
```

6. Wait for tasks with different conditions
------------------------------------------
```python
done, pending = await asyncio.wait([task1(), task2()], return_when=asyncio.FIRST_COMPLETED)
```

7. Queues
---------
```python
queue = asyncio.Queue()
await queue.put(item)
item = await queue.get()
queue.task_done()
```

8. Locks
--------
```python
lock = asyncio.Lock()
async with lock:
    # critical section
    pass
```

9. Semaphores
-------------
```python
sem = asyncio.Semaphore(3)
async with sem:
    # max 3 concurrent
    pass
```

10. Streams (networking)
-----------------------
```python
reader, writer = await asyncio.open_connection('127.0.0.1', 8888)
data = await reader.read(100)
writer.write(b'Hello')
await writer.drain()
writer.close()
await writer.wait_closed()
```

11. Key Points
--------------
- `asyncio.run()` is the main entry point.
- `asyncio.create_task()` schedules coroutines concurrently.
- `asyncio.gather()` waits for multiple coroutines.
- `asyncio.wait()` can be used with different completion strategies.
- Use `asyncio.Queue`, `Lock`, `Semaphore` for concurrency control.

