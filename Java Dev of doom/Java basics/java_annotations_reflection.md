# 🏷️ Java Annotations & Reflection Cheat Sheet

Quick reference for Java annotations and reflection (Java 17+).

---

## 🏷️ 1. Annotations Basics

**Built-in annotations**
```java
@Override
public String toString() { return "Example"; }

@Deprecated
public void oldMethod() { }

@FunctionalInterface
interface MyFunc { void apply(); }
```

**Meta-annotations**
```java
@Retention(RetentionPolicy.RUNTIME) // available at runtime
@Target(ElementType.METHOD)         // can be used on methods
@interface MyAnnotation { }
```

---

## ✨ 2. Common Use-Cases

**@Override** ensures a method overrides a superclass method  
**@Deprecated** marks a method/class as deprecated  
**@SuppressWarnings** disables compiler warnings
```java
@SuppressWarnings("unchecked")
List list = new ArrayList();
```

---

## 🛠️ 3. Custom Annotations

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@interface MyAnnotation {
    String value();
    int count() default 1;
}
```

**Usage**
```java
class MyClass {
    @MyAnnotation(value = "hello", count = 3)
    public void annotatedMethod() { }
}
```

---

## 🔍 4. Reflection Basics

**Get class & methods**
```java
Class<?> clazz = MyClass.class;
Method[] methods = clazz.getDeclaredMethods();
Field[] fields = clazz.getDeclaredFields();
Constructor<?>[] constructors = clazz.getDeclaredConstructors();
```

**Invoke method**
```java
MyClass obj = new MyClass();
Method m = clazz.getDeclaredMethod("annotatedMethod");
m.setAccessible(true); // access private method if needed
m.invoke(obj); 
```

**Access private field**
```java
Field f = clazz.getDeclaredField("value");
f.setAccessible(true);
f.set(obj, 42);
Object val = f.get(obj);
```

**Read annotation at runtime**
```java
Method m = clazz.getDeclaredMethod("annotatedMethod");
MyAnnotation ann = m.getAnnotation(MyAnnotation.class);
System.out.println(ann.value()); // "hello"
```

---

## 💡 5. Best Practices

- Avoid excessive reflection (performance impact).  
- Use annotations for metadata/configuration.  
- Keep custom annotations small and meaningful.  
- Prefer standard APIs before using reflection.  
- Always set `setAccessible(true)` carefully for private fields/methods.  

---

*Made for quick reference — Java 17+ annotations and reflection essentials with examples.*
