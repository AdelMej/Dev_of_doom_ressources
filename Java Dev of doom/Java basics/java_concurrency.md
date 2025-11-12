# 🧵 Java Concurrency & Multithreading Cheat Sheet

Quick reference for Java concurrency and multithreading (Java 17+).

---

## 🧵 1. Threads Basics

**Using Thread class**
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running");
    }
}

MyThread t = new MyThread();
t.start(); // runs in new thread
// t.run(); // ❌ runs in current thread
```

**Using Runnable**
```java
Runnable r = () -> System.out.println("Runnable running");
Thread t2 = new Thread(r);
t2.start();
```

**Thread naming and priorities**
```java
Thread t = new Thread(r, "Worker-1");
t.setPriority(Thread.MAX_PRIORITY);
t.start();
System.out.println(t.getName() + " priority: " + t.getPriority());
```

---

## ⚡ 2. ExecutorService

**Fixed Thread Pool**
```java
ExecutorService exec = Executors.newFixedThreadPool(3);
Future<Integer> future = exec.submit(() -> 1 + 2);
System.out.println(future.get());
exec.shutdown();
```

**Submitting tasks**
```java
exec.execute(() -> System.out.println("Task running")); // void
exec.submit(() -> 42); // returns Future
```

**Shutdown**
```java
exec.shutdown();        // graceful
exec.shutdownNow();     // immediate
```

---

## 🔒 3. Synchronization

**Synchronized method / block**
```java
class Counter {
    private int count = 0;
    public synchronized void increment() { count++; }
    public int getCount() { return count; }
}
```

**ReentrantLock**
```java
ReentrantLock lock = new ReentrantLock();
lock.lock();
try {
    // critical section
} finally {
    lock.unlock();
}
```

**Atomic Variables**
```java
AtomicInteger ai = new AtomicInteger(0);
ai.incrementAndGet();
ai.compareAndSet(1, 2);
```

---

## 🔄 4. High-level Concurrency

**Future & CompletableFuture**
```java
CompletableFuture<Integer> cf = CompletableFuture.supplyAsync(() -> 10 + 5);
cf.thenAccept(System.out::println);
```

**CountDownLatch / Semaphore**
```java
CountDownLatch latch = new CountDownLatch(3);
latch.countDown();
latch.await(); // waits until count reaches 0

Semaphore sem = new Semaphore(2);
sem.acquire();
try { /* critical section */ } finally { sem.release(); }
```

**Parallel streams**
```java
List<Integer> nums = List.of(1,2,3,4,5);
int sum = nums.parallelStream().mapToInt(i -> i * 2).sum();
```

---

## 💡 5. Best Practices

- Prefer `ExecutorService` over raw `Thread`.  
- Minimize synchronized blocks to reduce contention.  
- Avoid busy-wait loops; use proper signaling.  
- Use `volatile` carefully for visibility.  
- Always handle exceptions in threads to prevent silent failures.  
- Use `CompletableFuture` for async composition.  

---

*Made for quick reference — Java 17+ concurrency essentials with examples.*
