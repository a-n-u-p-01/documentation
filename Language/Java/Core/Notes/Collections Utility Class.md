# **Collections Utility Class in Java**

The `Collections` class in Java (`java.util.Collections`) is a **utility class** that provides **static methods** for performing **common operations on collections** such as sorting, searching, reversing, and more. It works with any **Collection** that implements the `List`, `Set`, or other collection interfaces.  

---

## **1. Key Features**

- All methods are **static**, so no need to create an object of `Collections`.  
- Works with **List, Set, Map** indirectly (via lists of keys/values).  
- Includes **algorithms** (sorting, searching, shuffling).  
- Provides **thread-safe wrappers** for collections.  
- Provides **singleton and unmodifiable collections**.  

---

## **2. Commonly Used Methods**

### **2.1 Sorting**

**Sorts elements in natural order (ascending) or using a custom Comparator.**

```java
List<Integer> numbers = Arrays.asList(5, 3, 8, 1);
Collections.sort(numbers); // ascending
System.out.println(numbers); // [1, 3, 5, 8]

Collections.sort(numbers, Collections.reverseOrder()); // descending
System.out.println(numbers); // [8, 5, 3, 1]
```

---

### **2.2 Searching**

**Binary search for elements in a sorted list.**  

```java
Collections.sort(numbers);
int index = Collections.binarySearch(numbers, 5);
System.out.println(index); // index of 5
```

> Note: List must be sorted before using `binarySearch()`.  

---

### **2.3 Reverse**

**Reverses the order of elements in a list.**

```java
Collections.reverse(numbers);
System.out.println(numbers); // [8, 5, 3, 1]
```

---

### **2.4 Shuffle**

**Randomly permutes elements in a list.**  

```java
Collections.shuffle(numbers);
System.out.println(numbers); // [3, 8, 1, 5] - random order
```

---

### **2.5 Min / Max**

**Finds minimum or maximum element according to natural ordering or a Comparator.**

```java
int min = Collections.min(numbers);
int max = Collections.max(numbers);
System.out.println("Min: " + min + ", Max: " + max);
```

---

### **2.6 Frequency**

**Counts occurrences of an element in a collection.**

```java
List<String> names = Arrays.asList("Alice", "Bob", "Alice", "Charlie");
int freq = Collections.frequency(names, "Alice");
System.out.println(freq); // 2
```

---

### **2.7 Fill**

**Replaces all elements with the specified value.**

```java
Collections.fill(names, "John");
System.out.println(names); // [John, John, John, John]
```

---

### **2.8 Copy**

**Copies elements from one list to another. Destination list must be **at least the same size**.**

```java
List<String> source = Arrays.asList("A", "B", "C");
List<String> dest = Arrays.asList(new String[source.size()]);
Collections.copy(dest, source);
System.out.println(dest); // [A, B, C]
```

---

### **2.9 Swap**

**Swaps elements at two specified positions.**

```java
Collections.swap(numbers, 0, 2);
System.out.println(numbers); // elements at index 0 and 2 swapped
```

---

### **2.10 Reverse Order / Custom Comparator**

**Sort or get comparator for reverse order.**

```java
Comparator<Integer> comp = Collections.reverseOrder();
Collections.sort(numbers, comp);
```

---

## **3. Thread-Safe Wrappers**

- `Collections.synchronizedList(list)`  
- `Collections.synchronizedSet(set)`  
- `Collections.synchronizedMap(map)`  

These methods **wrap a collection** to make it **thread-safe**.

```java
List<Integer> syncList = Collections.synchronizedList(new ArrayList<>());
```

---

## **4. Unmodifiable / Immutable Collections**

- `Collections.unmodifiableList(list)`  
- `Collections.unmodifiableSet(set)`  
- `Collections.unmodifiableMap(map)`  

Prevents modifications (add, remove, update) to the collection.

```java
List<String> list = new ArrayList<>(Arrays.asList("A", "B"));
List<String> unmodList = Collections.unmodifiableList(list);
```

> Throws `UnsupportedOperationException` if modified.

---

## **5. Singleton, Empty, and N Copies**

- `Collections.singleton("A")` → returns **immutable set** with a single element.  
- `Collections.emptyList()` → returns **empty list**.  
- `Collections.nCopies(5, "X")` → returns a list with **5 copies of "X"**.

---

## **6. Summary Table of Key Methods**

| Method                         | Description |
|--------------------------------|------------|
| sort(list)                      | Sorts list in natural order |
| sort(list, comparator)          | Sorts list using custom comparator |
| reverse(list)                   | Reverses the list |
| shuffle(list)                   | Randomly shuffles the list |
| min(collection)                  | Returns minimum element |
| max(collection)                  | Returns maximum element |
| frequency(collection, element)   | Counts occurrences |
| fill(list, element)             | Fills list with element |
| copy(dest, src)                 | Copies from src to dest |
| swap(list, i, j)                | Swaps elements at positions |
| synchronizedList(list)          | Thread-safe list |
| unmodifiableList(list)          | Immutable list |
| singleton(element)              | Immutable set with one element |
| emptyList()                      | Returns empty list |
| nCopies(n, element)             | Returns list with n copies of element |

---

## **7. Best Practices**

1. Use **Collections class methods** instead of writing custom algorithms for sorting, searching, or shuffling.  
2. For **thread-safe operations**, use `synchronizedList` or concurrent collections (`CopyOnWriteArrayList`).  
3. For **immutable collections**, use `unmodifiableList`, `singleton`, or `nCopies`.  
4. Combine with **Comparable** or **Comparator** for custom sorting.  

---
