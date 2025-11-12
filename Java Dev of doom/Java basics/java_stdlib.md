# 🏛️ Java Standard Library Cheat Sheet

A quick reference for commonly used Java standard library classes and utilities (Java 17+).

---

## 📦 1. java.lang

**Strings**
```java
String s = "Hello";
int len = s.length();
String upper = s.toUpperCase();
boolean eq = s.equals("Hello");
boolean contains = s.contains("H");
char c = s.charAt(0);
```

**Math**
```java
int max = Math.max(5, 10);
double sqrt = Math.sqrt(16);
double rnd = Math.random(); // 0.0 <= rnd < 1.0
```

**Wrappers & Conversion**
```java
int x = Integer.parseInt("42");
String str = Integer.toString(42);
boolean b = Boolean.parseBoolean("true");
```

**Object methods**
```java
Object o1 = new Object();
Object o2 = new Object();
o1.equals(o2);
o1.hashCode();
o1.toString();
```

---

## ⏱️ 2. java.time

**LocalDate / LocalDateTime**
```java
LocalDate today = LocalDate.now();
LocalDate birthday = LocalDate.of(1990, Month.JANUARY, 1);
LocalDate nextYear = today.plusYears(1);

LocalDateTime now = LocalDateTime.now();
LocalDateTime future = now.plusHours(5).plusMinutes(30);
```

**Duration & Period**
```java
Duration dur = Duration.between(now, future);
Period period = Period.between(birthday, today);
```

**Formatting**
```java
DateTimeFormatter fmt = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
String formatted = now.format(fmt);
LocalDateTime parsed = LocalDateTime.parse("2025-11-12 15:30", fmt);
```

---

## 🔢 3. java.util

**Collections**
```java
List<String> list = new ArrayList<>();
list.add("a"); list.add("b");

Set<Integer> set = new HashSet<>();
set.add(1); set.add(2);

Map<String, Integer> map = new HashMap<>();
map.put("x", 10); map.get("x");
```

**Iterating & Streams**
```java
list.forEach(System.out::println);
List<String> filtered = list.stream()
                            .filter(s -> s.length() > 0)
                            .map(String::toUpperCase)
                            .collect(Collectors.toList());
```

**Arrays**
```java
int[] arr = {3,1,2};
Arrays.sort(arr);
int idx = Arrays.binarySearch(arr, 2);
Arrays.fill(arr, 0);
```

**Random & Scanner**
```java
Random rnd = new Random();
int r = rnd.nextInt(100); // 0..99

Scanner sc = new Scanner(System.in);
String input = sc.nextLine();
```

**Optional**
```java
Optional<String> opt = Optional.ofNullable(null);
String val = opt.orElse("default");
opt.ifPresent(System.out::println);
```

---

## 🧵 4. java.util.concurrent

**ExecutorService**
```java
ExecutorService exec = Executors.newFixedThreadPool(2);
Future<Integer> future = exec.submit(() -> 1+1);
int result = future.get();
exec.shutdown();
```

**Atomic Variables**
```java
AtomicInteger ai = new AtomicInteger(0);
ai.incrementAndGet();
ai.compareAndSet(1,2);
```

---

## 📂 5. java.io & java.nio

**File & Streams**
```java
Path path = Path.of("file.txt");
Files.writeString(path, "Hello World");
String content = Files.readString(path);
```

**BufferedReader / Writer**
```java
try (BufferedReader br = Files.newBufferedReader(path)) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}
```

**InputStream / OutputStream**
```java
try (InputStream in = new FileInputStream("input.txt");
     OutputStream out = new FileOutputStream("out.txt")) {
    byte[] buffer = in.readAllBytes();
    out.write(buffer);
}
```

---

## ⚡ 6. java.net

**URL / HttpURLConnection**
```java
URL url = new URL("https://example.com");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("GET");
int status = conn.getResponseCode();
```

**Reading from URL**
```java
try (BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
    br.lines().forEach(System.out::println);
}
```

---

## 🛠️ 7. Utilities & Helpers

**Objects & UUID**
```java
Objects.requireNonNull(obj);
UUID id = UUID.randomUUID();
```

**Collections helpers**
```java
Collections.sort(list);
Collections.reverse(list);
Collections.shuffle(list);
Collections.unmodifiableList(list);
```

**Arrays helpers**
```java
Arrays.asList(1,2,3);
Arrays.copyOf(arr, 10);
Arrays.equals(arr1, arr2);
```

---

## 💡 8. Best Practices & Pitfalls

- Prefer `java.time` over `java.util.Date` / Calendar.  
- Streams & lambdas are powerful but avoid boxing/unboxing in hot loops.  
- Close resources (try-with-resources) to prevent leaks.  
- Use `Optional` to reduce null checks but don’t overuse.  
- For concurrency, prefer high-level abstractions (`ExecutorService`) over `Thread` directly.  

---

*Made for quick reference — Java 17+ idioms, standard library essentials, and practical examples.*
