#### **1. What is the Java Collection Framework?**

The **Java Collection Framework (JCF)** is a **standardized architecture** to store and manipulate **groups of objects**. It provides:

1. **Interfaces** – abstract contracts defining operations for collections.
    
2. **Implementations** – concrete classes providing functionality.
    
3. **Algorithms** – reusable methods for sorting, searching, etc.
    

**Benefits:**

- Reduces programming effort by providing ready-made data structures.
    
- Improves performance with optimized algorithms.
    
- Provides interoperability with a standard set of interfaces.
    
- Ensures **type safety** using generics.
    
- Thread-safe options for concurrent programming.
    

---

### **2. Core Interfaces**

### **2.1 Iterable**

- Root interface of all collections.
    
- Provides a method:
    

```java
Iterator<E> iterator();
```

- Allows **enhanced for-loop iteration**.
    

---

### **2.2 Collection**

- Extends `Iterable`.
    
- Base interface for `List`, `Set`, `Queue`.
    
- **Common Methods:**
    

```java
add(E e), remove(Object o), size(), isEmpty(), contains(Object o), clear(), toArray()
```

- `boolean removeIf(Predicate<? super E> filter)` – removes elements conditionally.
    

---

### **2.3 List**

- Ordered collection (elements maintain insertion order).
    
- Allows **duplicate elements** and **null**.
    
- Implementations:
    
    - `ArrayList` – dynamic array, fast random access.
        
    - `LinkedList` – doubly-linked list, fast insertion/deletion.
        
    - `Vector` – synchronized dynamic array (legacy).
        
    - `Stack` – LIFO, extends Vector.
        

**Important Methods:**

```java
get(int index), set(int index, E element), add(int index, E element), remove(int index), indexOf(Object o)
```

---

### **2.4 Set**

- Collection **without duplicates**.
    
- Does not guarantee order (unless specific implementation).
    

**Implementations:**

1. **HashSet** – unordered, backed by a HashMap, allows one null.
    
2. **LinkedHashSet** – preserves insertion order.
    
3. **TreeSet** – sorted order, implements `NavigableSet`, uses **Red-Black tree**.
    

**Set Example:**

```java
Set<String> set = new HashSet<>();
set.add("A");
set.add("B");
set.add("A"); // ignored
```

---

### **2.5 Queue**

- **FIFO (First In, First Out)** data structure.
    
- Methods: `offer()`, `poll()`, `peek()`, `element()`
    
- Implementations:
    
    - `LinkedList` – also implements `List`
        
    - `PriorityQueue` – elements ordered by priority
        

```java
Queue<Integer> q = new PriorityQueue<>();
q.offer(10);
q.offer(5);
System.out.println(q.poll()); // 5 (min priority first)
```

---

### **2.6 Deque**

- **Double-ended queue** (insertion/removal from both ends).
    
- Methods: `addFirst()`, `addLast()`, `removeFirst()`, `removeLast()`
    
- Implementations: `ArrayDeque`, `LinkedList`.
    

```java
Deque<String> deque = new ArrayDeque<>();
deque.addFirst("A");
deque.addLast("B");
System.out.println(deque.removeLast()); // B
```

---

### **2.7 Map**

- **Key-value pairs**, keys must be unique, values can be duplicated.
    
- **Does NOT extend Collection**.
    

**Implementations:**

1. `HashMap` – unordered, allows one null key, multiple null values.
    
2. `LinkedHashMap` – preserves insertion order.
    
3. `TreeMap` – sorted keys, implements `NavigableMap`.
    
4. `Hashtable` – legacy, synchronized, no null keys/values.
    

**Important Methods:**

```java
put(K key, V value), get(Object key), remove(Object key), containsKey(Object key), containsValue(Object value), keySet(), values(), entrySet()
```

---

## **3. Abstract Classes**

- `AbstractCollection<E>` – partially implements Collection interface.
    
- `AbstractList<E>` – partially implements List.
    
- `AbstractSet<E>` – partially implements Set.
    
- `AbstractMap<K,V>` – partially implements Map.
    

**Why Abstract Classes:**

- Reduce code duplication across implementations.
    
- Provide default implementations for some methods.
    

---

## **4. Key Classes Overview**

| Class         | Interface    | Key Features                | Notes                      |
| ------------- | ------------ | --------------------------- | -------------------------- |
| ArrayList     | List         | Dynamic array, fast get/set | Not synchronized           |
| LinkedList    | List, Deque  | Doubly linked list          | Good for insert/delete     |
| Vector        | List         | Synchronized dynamic array  | Legacy                     |
| Stack         | Vector       | LIFO                        | Legacy                     |
| HashSet       | Set          | Hashing, no duplicates      | Fast                       |
| LinkedHashSet | Set          | Preserves insertion order   | Ordered HashSet            |
| TreeSet       | NavigableSet | Sorted, Red-Black tree      | No null                    |
| PriorityQueue | Queue        | Min-heap ordering           | Null not allowed           |
| ArrayDeque    | Deque        | Fast double-ended queue     | Alternative to Stack/Queue |
| HashMap       | Map          | Key-value mapping, fast     | Null key allowed           |
| LinkedHashMap | Map          | Insertion-order mapping     | Ordered HashMap            |
| TreeMap       | NavigableMap | Sorted keys, Red-Black tree | No null key                |
| Hashtable     | Map          | Legacy synchronized map     | No null key/value          |

---

## **5. Iterators**

1. **Iterator** – traverses Collection (fail-fast).
    
2. **ListIterator** – bidirectional iteration, specific to List.
    
3. **Enhanced for-loop** – syntactic sugar for iterator.
    

**Example:**

```java
List<String> list = new ArrayList<>();
list.add("A"); list.add("B");
Iterator<String> it = list.iterator();
while(it.hasNext()){
    System.out.println(it.next());
}
```

**Fail-Fast vs Fail-Safe**

|Type|Example|Behavior|
|---|---|---|
|Fail-Fast|ArrayList, HashMap|Throws `ConcurrentModificationException` if modified during iteration|
|Fail-Safe|CopyOnWriteArrayList, ConcurrentHashMap|Can be modified during iteration|

---

## **6. Collections Utility Class**

- Provides **algorithms and utilities**:
    

```java
Collections.sort(list);
Collections.reverse(list);
Collections.shuffle(list);
Collections.max(list);
Collections.min(list);
Collections.frequency(list, element);
Collections.fill(list, value);
Collections.copy(dest, src);
```

---

## **7. Generics in Collections**

- Ensures **compile-time type safety**.
    

```java
List<String> list = new ArrayList<>();
list.add("Java");
// list.add(10); // Compile-time error
```

- Wildcards:
    
    - `?` – unknown type
        
    - `? extends T` – upper bound
        
    - `? super T` – lower bound
        

---

## **8. Synchronized vs Non-Synchronized Collections**

|Feature|Synchronized|Non-Synchronized|
|---|---|---|
|Example|Vector, Hashtable|ArrayList, HashMap|
|Thread-Safe|Yes|No|
|Performance|Slower|Faster|
|Alternative|`Collections.synchronizedList(list)`|Use concurrent collections|

---

## **9. Key Differences Between Interfaces**

|Feature|List|Set|Map|
|---|---|---|---|
|Duplicates|Allowed|Not allowed|Keys not allowed|
|Order|Maintained|Depends|Depends|
|Access by Index|Yes|No|No|
|Null Values|Allowed|One (HashSet)|Multiple for values, one for key (HashMap)|
|Implementations|ArrayList, LinkedList|HashSet, TreeSet|HashMap, TreeMap|

---

## **10. Best Practices**

1. Use **interface types** for references:
    

```java
List<String> list = new ArrayList<>();
```

2. Choose implementation wisely:
    
    - `ArrayList` → random access
        
    - `LinkedList` → frequent insertion/deletion
        
    - `HashSet` → unique elements
        
    - `TreeMap` → sorted key-value pairs
        
3. Use **Iterator** for safe traversal.
    
4. Prefer **Generics** to ensure type safety.
    
5. Use **Collections utility class** for common operations.
    

---

## **11. Common Interview Questions**

**Basic**

- What is Collection Framework? Difference between Collection and Collections.
    
- Difference between List, Set, Map.
    
- ArrayList vs LinkedList.
    
- HashMap vs TreeMap vs LinkedHashMap.
    

**Intermediate**

- Iterator vs ListIterator.
    
- Fail-fast vs fail-safe.
    
- Synchronized vs non-synchronized collections.
    
- Comparable vs Comparator.
    

**Advanced**

- How HashMap handles collisions?
    
- How TreeMap maintains order?
    
- ConcurrentHashMap vs Hashtable.
    
- Deep copy vs shallow copy of collections.
    

---

## **12. Summary**

- **JCF** = Interfaces + Implementations + Algorithms.
    
- **Interfaces**: Collection, List, Set, Queue, Deque, Map.
    
- **Implementations**: ArrayList, LinkedList, HashSet, TreeSet, HashMap, TreeMap.
    
- **Algorithms**: sort, reverse, shuffle, min, max, fill.
    
- **Generics** = type safety; **synchronized collections** for thread safety.
    
- **Iterators** provide safe traversal; know fail-fast vs fail-safe.
    
