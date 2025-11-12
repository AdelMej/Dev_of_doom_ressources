# 🧠 Java Memory Management & Garbage Collection Cheat Sheet

## 📚 Java Memory Model Overview

Java divides memory into several key areas managed by the JVM:

| Area | Description |
|------|--------------|
| **Stack** | Stores primitive types and references to objects. Each thread has its own stack. |
| **Heap** | Stores all objects and arrays, shared among threads. |
| **Metaspace** | Stores class metadata (replaced PermGen since Java 8). |
| **PC Register** | Holds the current instruction address for each thread. |
| **Native Method Stack** | Used for native (JNI) code execution. |

---

## 🔄 Object Lifecycle

1. **Creation** → Object is allocated on the heap via `new`.
2. **Usage** → Application references and uses the object.
3. **Unreachable** → No references to object remain.
4. **Garbage Collection (GC)** → JVM reclaims memory.

---

## 🧩 Garbage Collection (GC) Basics

- **GC Roots**: Live objects directly accessible by the JVM (like thread stacks, static fields, etc.).  
- Objects not reachable from GC roots are *eligible for collection*.

### GC Phases:
1. **Mark** → Identify reachable objects.
2. **Sweep** → Delete unreachable objects.
3. **Compact** → Defragment the heap (optional, depends on GC).

---

## ⚙️ Common Garbage Collectors

| Collector | Description | When to Use |
|------------|--------------|-------------|
| **Serial GC** | Simple, single-threaded | Small apps / single-threaded environments |
| **Parallel GC** | Multi-threaded throughput collector | High-performance apps with multiple CPUs |
| **G1 GC** | Default for most JVMs; balances pause times | General-purpose, scalable systems |
| **ZGC** | Ultra-low pause GC | Low-latency applications (e.g., trading systems) |
| **Shenandoah GC** | Concurrent GC with short pauses | Large heap, low-latency environments |

---

## 🚀 JVM Options for Memory & GC

```bash
# Set initial and max heap size
-Xms512m
-Xmx2g

# Choose garbage collector
-XX:+UseG1GC
# or
-XX:+UseZGC

# Enable GC logs
-Xlog:gc*:file=gc.log:time,uptime,level,tags

# Show heap details
-XX:+PrintGCDetails -XX:+PrintGCDateStamps
```

---

## 🧰 Memory Optimization Tips

- ♻️ **Reuse objects** (especially heavy ones) via object pooling if necessary.  
- ⚖️ **Avoid unnecessary object creation** — prefer primitives where possible.  
- 🧹 **Close resources** (files, streams, sockets) to prevent leaks.  
- 🧍 **Use weak references** for caches (`WeakHashMap`).  
- 🧾 **Use `try-with-resources`** for automatic cleanup.  

---

## 🧠 Common Memory Issues

| Issue | Cause | Fix |
|--------|-------|-----|
| **Memory Leak** | Objects remain referenced unintentionally | Use profilers (`VisualVM`, `JProfiler`) |
| **OutOfMemoryError** | Heap too small / endless object creation | Increase heap or fix object lifecycle |
| **GC Overhead Limit Exceeded** | GC spending too much time cleaning | Tune GC or reduce object churn |

---

## 🧪 Tools for Monitoring Memory

- 🧰 **JConsole** – Built-in JVM monitor.  
- 📊 **VisualVM** – Visual profiling & heap dump inspection.  
- 🧠 **JMC (Java Mission Control)** – Advanced performance analysis.  
- 🧾 **Eclipse MAT** – Memory Analyzer Tool for heap dumps.

---

## 💡 Example: Checking Memory at Runtime

```java
public class MemoryExample {
    public static void main(String[] args) {
        Runtime runtime = Runtime.getRuntime();
        System.out.println("Total memory: " + runtime.totalMemory());
        System.out.println("Free memory: " + runtime.freeMemory());
        System.out.println("Max memory: " + runtime.maxMemory());
    }
}
```
