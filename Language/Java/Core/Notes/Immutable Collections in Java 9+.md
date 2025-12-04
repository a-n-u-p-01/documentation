Starting from **Java 9**, the **Collections Framework** introduced **factory methods** to create **immutable (read-only) collections** easily. These collections **cannot be modified** after creation—no additions, removals, or updates are allowed.

---

## **1. What is an Immutable Collection?**

**Definition:**  
An **immutable collection** is a collection whose **contents cannot be changed** once it is created.

- Benefits:
    
    - Thread-safe without explicit synchronization.
        
    - Prevents accidental modification.
        
    - Can improve performance for read-only data.
        

**Types:** Immutable **List, Set, Map**.

---

## **2. Factory Methods in Java 9+**

Java 9 introduced **static factory methods** in `List`, `Set`, and `Map` interfaces:

|Collection|Method Syntax|Description|
|---|---|---|
|List|`List.of(E... elements)`|Creates an immutable list with the given elements|
|Set|`Set.of(E... elements)`|Creates an immutable set (no duplicates allowed)|
|Map|`Map.of(K,V...)`|Creates an immutable map (up to 10 entries with `of`)|
|Map|`Map.ofEntries(Map.Entry<K,V>...)`|Creates immutable map with more than 10 entries|

> Note: These methods **throw `NullPointerException`** if any element or key/value is null.

---

## **3. Creating Immutable Collections**

### **3.1 Immutable List**

```java
List<String> list = List.of("A", "B", "C");
System.out.println(list); // [A, B, C]

// list.add("D"); // UnsupportedOperationException
// list.remove(0); // UnsupportedOperationException
```

### **3.2 Immutable Set**

```java
Set<String> set = Set.of("X", "Y", "Z");
System.out.println(set); // [X, Y, Z]

// set.add("W"); // UnsupportedOperationException
```

> **Important:** `Set.of()` **does not allow duplicates**; adding duplicate elements throws `IllegalArgumentException`.

### **3.3 Immutable Map**

```java
Map<String, Integer> map = Map.of("A", 1, "B", 2, "C", 3);
System.out.println(map); // {A=1, B=2, C=3}

// map.put("D", 4); // UnsupportedOperationException
```

**For more than 10 entries:**

```java
Map<String, Integer> bigMap = Map.ofEntries(
    Map.entry("A", 1),
    Map.entry("B", 2),
    Map.entry("C", 3),
    Map.entry("D", 4)
);
```

---

## **4. Characteristics of Java 9 Immutable Collections**

|Feature|Description|
|---|---|
|Unmodifiable|Cannot add, remove, or modify elements|
|Null handling|`NullPointerException` if element/key/value is null|
|Thread-safe|Safe for concurrent read operations|
|Fixed size|Size cannot change after creation|
|No duplicates (Set & Map)|Duplicates are rejected in `Set.of()` and `Map.of()`|

---

## **5. Comparison with Collections.unmodifiableXXX()**

- `Collections.unmodifiableList(list)` wraps an **existing mutable collection** into an unmodifiable view.
    
- Java 9 `List.of()` creates a **true immutable collection** directly.
    

|Feature|Collections.unmodifiableList|List.of (Java 9+)|
|---|---|---|
|Original collection mutable?|Yes|No|
|Immutable at creation?|No|Yes|
|Null elements allowed?|Depends on original list|No|
|Thread-safe for reading|No (original list can change)|Yes|

---

## **6. Best Practices**

1. Use **immutable collections** for **read-only data** to avoid accidental modification.
    
2. Use **List.of(), Set.of(), Map.of()** instead of `Collections.unmodifiableList` for **cleaner and safer code**.
    
3. Use **Map.ofEntries()** for maps with more than 10 entries.
    
4. Prefer immutable collections for **thread-safe reads** without explicit synchronization.
    

---

## **7. Example: Full Usage**

```java
List<String> fruits = List.of("Apple", "Banana", "Orange");
Set<Integer> numbers = Set.of(10, 20, 30);
Map<String, String> countryCodes = Map.of(
    "US", "United States",
    "IN", "India",
    "JP", "Japan"
);

System.out.println(fruits);       // [Apple, Banana, Orange]
System.out.println(numbers);      // [10, 20, 30]
System.out.println(countryCodes); // {US=United States, IN=India, JP=Japan}

// fruits.add("Mango"); // UnsupportedOperationException
```

---

## **8. Summary**

- Java 9+ provides **factory methods** (`List.of()`, `Set.of()`, `Map.of()`) to create **immutable collections**.
    
- Immutable collections are **thread-safe**, **read-only**, and **cannot contain null elements**.
    
- Preferred over `Collections.unmodifiableXXX()` for cleaner, safer, and truly immutable collections.
    

---