# 🎯 Java Generics Cheat Sheet

Quick reference for Java Generics (Java 17+).

---

## 📦 1. Basics of Generics

**Generic Class**
```java
class Box<T> {
    private T value;
    public Box(T value) { this.value = value; }
    public T get() { return value; }
    public void set(T value) { this.value = value; }
}

Box<String> strBox = new Box<>("Hello");
String val = strBox.get();
```

**Generic Method**
```java
public <T> void printArray(T[] arr) {
    for (T elem : arr) System.out.println(elem);
}

Integer[] nums = {1,2,3};
printArray(nums);
```

---

## 🔢 2. Bounded Types

**Upper bounds**
```java
class Calculator<T extends Number> {
    T value;
    Calculator(T value) { this.value = value; }
    double doubleValue() { return value.doubleValue(); }
}

Calculator<Integer> ci = new Calculator<>(10);
Calculator<Double> cd = new Calculator<>(10.5);
// Calculator<String> cs = new Calculator<>("no"); // ❌ Error
```

**Lower bounds in methods**
```java
public void addNumbers(List<? super Integer> list) {
    list.add(42); // safe
}
```

---

## 🛠️ 3. Wildcards

**? extends T (covariant) – read-only**
```java
List<? extends Number> nums = List.of(1, 2.0, 3L);
// nums.add(10); // ❌ Not allowed
Number n = nums.get(0);
```

**? super T (contravariant) – write-only**
```java
List<? super Integer> list = new ArrayList<Number>();
list.add(10); // ✅ Allowed
Object o = list.get(0); // reading is Object
```

---

## ✨ 4. Collections & Generics

```java
List<String> strings = new ArrayList<>();
strings.add("a");

Map<String, Integer> map = new HashMap<>();
map.put("x", 42);
int val = map.get("x");
```

**Avoid raw types**
```java
List raw = new ArrayList(); // ❌ Avoid
raw.add("a");
String s = (String) raw.get(0); // cast required
```

**Flexible read with ? extends**
```java
List<? extends Number> nums = List.of(1,2,3);
Number n = nums.get(0);
```

---

## 💡 5. Best Practices

- Prefer bounded types over casting.  
- Avoid raw types; always use `<T>` notation.  
- Use `<?>` wildcards only when necessary.  
- Keep generic method and class signatures readable.  
- Use generics to enforce compile-time type safety.  

---

*Made for quick reference — Java 17+ generics essentials with examples.*
