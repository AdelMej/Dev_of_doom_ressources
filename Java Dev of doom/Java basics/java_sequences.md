# 🌊 Java Sequences Cheat Sheet

A compact but thorough guide to Java sequence types and common operations (Java 17+).  
Includes arrays, lists, sets, maps, iteration patterns, Streams, utilities and Big‑O.

---

## 📋 1. Arrays

**Declaration / initialization**
```java
int[] nums = {1, 2, 3};
String[] names = new String[3]; // all null
var more = new int[] {4,5,6};
```

**Access & update**
```java
int x = nums[0];
nums[1] = 42;
```

**Iterate**
```java
for (int i = 0; i < nums.length; i++) System.out.println(nums[i]);
for (int v : nums) System.out.println(v);
```

**Utilities**
```java
Arrays.sort(nums);
int idx = Arrays.binarySearch(nums, 42); // requires sorted array
String joined = Arrays.toString(nums);
```

---

## 📚 2. Lists (ArrayList, LinkedList)

**ArrayList (resizable array)**
```java
List<String> list = new ArrayList<>();
list.add("a");
list.add("b");
list.get(0);      // "a"
list.remove(1);   // remove by index
list.addAll(List.of("c","d"));
```

**LinkedList (doubly-linked)**
```java
Deque<String> dq = new LinkedList<>();
dq.addFirst("head");
dq.addLast("tail");
dq.removeFirst();
```

**Common operations**
```java
list.contains("a");
list.indexOf("c");
list.size();
list.clear();
```

**When to use**
- `ArrayList`: random access, compact memory, good default.
- `LinkedList`: frequent inserts/removals at ends or iterator-based removals.

---

## 🧮 3. Sets (HashSet, LinkedHashSet, TreeSet)

**HashSet (unique, unordered)**
```java
Set<String> s = new HashSet<>();
s.add("x");
s.add("y");
s.contains("x");
```

**LinkedHashSet (insertion-order)**
```java
Set<String> ordered = new LinkedHashSet<>();
```

**TreeSet (sorted)**
```java
NavigableSet<Integer> sorted = new TreeSet<>();
sorted.add(5);
sorted.add(1);
sorted.first(); // 1
```

**Use cases**
- `HashSet` for dedup and O(1) membership.
- `LinkedHashSet` when you want stable iteration order.
- `TreeSet` when you need sorted set operations.

---

## 🗂️ 4. Maps (HashMap, LinkedHashMap, TreeMap)

**HashMap (key → value)**
```java
Map<String, Integer> map = new HashMap<>();
map.put("alice", 42);
map.get("alice"); // 42
map.remove("bob");
map.putIfAbsent("carol", 7);
```

**LinkedHashMap (insertion-order or access-order)**
```java
Map<String,Integer> lmap = new LinkedHashMap<>();
// Can be used for simple LRU cache when access-order=true in constructor
```

**TreeMap (sorted keys)**
```java
NavigableMap<String,Integer> tmap = new TreeMap<>();
tmap.firstKey();
tmap.floorKey("m");
```

**Common patterns**
```java
map.computeIfAbsent("k", k -> new ArrayList<>()).add("value");
map.merge("k", 1, Integer::sum); // increment counter
```

---

## 🔁 5. Iteration (for-each, iterator, streams)

**For-each**
```java
for (String v : list) System.out.println(v);
```

**Iterator (safe removal)**
```java
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String v = it.next();
    if (v.equals("skip")) it.remove();
}
```

**Map iteration**
```java
for (var e : map.entrySet()) {
    System.out.println(e.getKey() + " -> " + e.getValue());
}
```

**Streams (lazy, powerful)**
```java
List<String> res = list.stream()
    .filter(s -> s.length() > 2)
    .map(String::toUpperCase)
    .sorted()
    .collect(Collectors.toList());
```

---

## ⚙️ 6. Utilities (Collections, Arrays)

```java
Collections.sort(list);               // in-place sort
Collections.reverse(list);
Collections.shuffle(list);
Collections.unmodifiableList(list);   // read-only view
int[] a = {3,1,2};
Arrays.sort(a);
Arrays.fill(a, 0);
```

**Copy / immutable**
```java
List<String> copy = new ArrayList<>(list);
List<String> imm = List.copyOf(list); // unmodifiable copy
```

---

## 🌊 7. Streams API (examples)

**Filter / map / collect**
```java
var evens = IntStream.rangeClosed(1,10)
              .filter(n -> n % 2 == 0)
              .boxed()
              .collect(Collectors.toList());
```

**Group by**
```java
Map<Integer, List<String>> byLength = list.stream()
    .collect(Collectors.groupingBy(String::length));
```

**Reduce**
```java
int sum = list.stream()
    .mapToInt(String::length)
    .sum();
```

**Parallel stream (use with care)**
```java
list.parallelStream().forEach(...);
```

---

## 🧠 8. Performance (Big‑O) Quick Table

| Operation | ArrayList | LinkedList | HashSet/HashMap | TreeSet/TreeMap |
|----------:|:---------:|:----------:|:---------------:|:---------------:|
| get by index / lookup | O(1) | O(n) | O(1) avg | O(log n) |
| add (end) | O(1) amortized | O(1) | O(1) avg | O(log n) |
| add (middle) | O(n) | O(1) if at node | - | O(log n) |
| remove | O(n) | O(1) if at node | O(1) avg | O(log n) |
| iteration | O(n) | O(n) | O(n) | O(n) |

---

## 💡 9. Best Practices & Pitfalls

- Prefer `List`/`Map`/`Set` interfaces in signatures (`List<String> list = new ArrayList<>();`).  
- Avoid excessive autoboxing in hot loops (use primitive streams `IntStream`).  
- Use `computeIfAbsent` for multi-value maps instead of manual checks.  
- Use immutable copies for public API returns (`List.copyOf(...)`).  
- Be careful with `null` keys/values: `HashMap` allows one null key, `TreeMap` doesn't unless comparator handles it.  
- For concurrency use `ConcurrentHashMap`, `CopyOnWriteArrayList` or synchronized wrappers.

---

## 🔧 10. Quick Examples

**Counting words**
```java
String text = "a b a c";
Map<String, Integer> counts = Arrays.stream(text.split("\s+"))
    .collect(Collectors.toMap(w -> w, w -> 1, Integer::sum));
```

**Top N by value**
```java
List<Map.Entry<String,Integer>> top = map.entrySet().stream()
    .sorted(Map.Entry.<String,Integer>comparingByValue().reversed())
    .limit(5)
    .collect(Collectors.toList());
```

**Simple LRU with LinkedHashMap**
```java
var cache = new LinkedHashMap<K,V>(16, 0.75f, true) {
    protected boolean removeEldestEntry(Map.Entry<K,V> e) {
        return size() > 100;
    }
};
```

---

## 🔚 Summary

- Use `ArrayList` by default for ordered collections.  
- Use `HashMap` / `HashSet` for fast lookup and dedup.  
- Use `Tree*` when you need ordering.  
- Streams are great for transformations but be mindful of performance and boxing.  
- Know the Big‑O and pick data structures based on access patterns.

---

*Made for quick reference — Java 17+ idioms and practical examples.*  
