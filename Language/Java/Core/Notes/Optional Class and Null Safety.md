## 1. Why Optional Was Introduced (ROOT CAUSE)

Before Java 8, Java had a **fundamental design problem**:

```java
return null;
```

### Problems with `null`

- Causes `NullPointerException`
    
- No information about **why value is missing**
    
- Caller must _remember_ to check null
    
- Null checks clutter code
    
- Bugs appear at runtime, not compile time
    

> `null` is a **billion-dollar mistake** (Tony Hoare)

---

## 2. What Optional Is (Conceptually)

`Optional<T>` is a **container object** that may or may not contain a non-null value.

Conceptually:

```
Optional = Explicit representation of absence
```

Instead of:

```java
User user = findUser(); // maybe null
```

Use:

```java
Optional<User> user = findUser();
```

Now absence is:

- Explicit
    
- Visible
    
- Forced to be handled
    

---

## 3. What Optional Is NOT

This is very important.

Optional is NOT:

- A replacement for all nulls
    
- A field type (usually)
    
- A parameter type (usually)
    
- A collection
    
- A performance optimization
    

Optional IS:

- A **return type**
    
- A **signal**
    
- A **null-safety tool**
    

---

## 4. How Optional Improves Null Safety

### Without Optional

```java
User user = service.findUser(id);
if (user != null) {
    if (user.getAddress() != null) {
        System.out.println(user.getAddress().getCity());
    }
}
```

### With Optional

```java
service.findUser(id)
       .map(User::getAddress)
       .map(Address::getCity)
       .ifPresent(System.out::println);
```

No null checks.  
No NPE.  
Intent is clear.

---

## 5. How Optional Works Internally

Internally, Optional is very simple.

```java
class Optional<T> {
    private final T value;
}
```

- If `value == null` → empty
    
- If `value != null` → present
    

Optional **never exposes the raw value directly** without checks.

---

## 6. Creating Optional Objects

### empty()

```java
Optional<String> o = Optional.empty();
```

### of() — NEVER accepts null

```java
Optional<String> o = Optional.of("Java"); // OK
Optional<String> o = Optional.of(null);   // NPE
```

### ofNullable()

```java
Optional<String> o = Optional.ofNullable(value);
```

Use `ofNullable()` when value **may be null**.

---

## 7. Checking Presence (Low-Level)

### isPresent() / isEmpty()

```java
if (optional.isPresent()) {
    System.out.println(optional.get());
}
```

Avoid this pattern.  
It brings back null-style coding.

---

## 8. get() — Why It Is Dangerous

```java
optional.get();
```

If value is absent:

```
NoSuchElementException
```

This defeats Optional’s purpose.

Rule:

> If you call `get()`, you are probably using Optional wrong.

---

## 9. ifPresent() / ifPresentOrElse()

### ifPresent()

```java
optional.ifPresent(System.out::println);
```

### ifPresentOrElse()

```java
optional.ifPresentOrElse(
    System.out::println,
    () -> System.out.println("No value")
);
```

Preferred for side effects.

---

## 10. map() — Core of Optional Power

Transforms value **only if present**.

```java
Optional<String> city =
    userOptional.map(User::getCity);
```

If user is empty → result is empty.

No NPE possible.

---

## 11. flatMap() — Avoid Nested Optional

Problem:

```java
Optional<Optional<String>>
```

Solution:

```java
optional.flatMap(User::getAddressOptional);
```

Same idea as Stream `flatMap()`.

---

## 12. filter() — Conditional Presence

```java
optional.filter(x -> x.length() > 3);
```

If condition fails → empty Optional.

---

## 13. orElse(), orElseGet(), orElseThrow()

### orElse()

```java
String value = optional.orElse("default");
```

**Eager execution** — default is always created.

---

### orElseGet()

```java
String value = optional.orElseGet(() -> "default");
```

**Lazy execution** — default created only if needed.

---

### orElseThrow()

```java
optional.orElseThrow(
    () -> new IllegalArgumentException("Value missing")
);
```

Preferred over `get()`.

---

## 14. Optional vs Null (Key Differences)

|null|Optional|
|---|---|
|Implicit absence|Explicit absence|
|Causes NPE|Forces handling|
|Runtime failure|Safer design|
|No API|Rich API|

---

## 15. Optional with Streams

Streams return Optional in many terminal ops:

```java
Optional<Integer> max =
    list.stream().max(Integer::compareTo);
```

Safe handling:

```java
max.ifPresent(System.out::println);
```

---

## 16. Optional in Method Design (BEST PRACTICES)

### Good Use

```java
Optional<User> findUserById(Long id);
```

### Bad Use

```java
class User {
    Optional<Address> address; // avoid
}
```

Why?

- Serialization issues
    
- Complexity
    
- Optional was not designed for fields
    

---

## 17. Optional as Parameter (Usually Avoid)

```java
void process(Optional<String> value); // avoid
```

Better:

```java
void process(String value);
```

Optional is for **return values**, not inputs.

---

## 18. Performance Considerations

- Optional is a **small object**
    
- Slight overhead compared to null
    
- Negligible in business logic
    
- Avoid in hot loops
    

---

## 19. Common Mistakes

- Using `Optional.get()`
    
- Using Optional for fields
    
- Returning `Optional.of(null)`
    
- Using Optional everywhere blindly
    
- Treating Optional like a collection
    

---

## 20. Optional and Functional Programming

Optional supports:

- map
    
- flatMap
    
- filter
    

This allows:

- Null-safe functional chains
    
- Cleaner business logic
    
- Fewer defensive checks
    

---

## 21. Final Mental Model

```
Optional is NOT about avoiding null
Optional is about making absence explicit
```

If you understand:

- Why null is dangerous
    
- Why Optional is explicit
    
- Why map/flatMap exist
    
- Why get() is discouraged
    

Then you **truly understand Optional and null safety**.

---
