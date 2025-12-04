## **1. Key Terminology**

Before diving into the maps, it’s important to understand the following terms:

1. **Hashing**:
    
    - Process of converting a key into an integer (hash code) using `hashCode()` method.
        
    - Determines the **bucket index** where the entry will be stored.
        
2. **Bucket**:
    
    - A location in the underlying array of a HashMap where entries are stored.
        
    - Multiple keys can map to the same bucket (collision).
        
3. **Collision**:
    
    - Occurs when **two different keys have the same hash code** or map to the same bucket.
        
    - Handled using **Linked List** or **Red-Black Tree** (treeification) in Java 8+.
        
4. **Load Factor**:
    
    - Threshold to decide when to **resize the HashMap**.
        
    - Default is 0.75 (i.e., resize when 75% full).
        
5. **Red-Black Tree**:
    
    - A **self-balancing binary search tree**.
        
    - Used in Java HashMap and TreeMap for efficient search and ordering.
        
6. **Rehashing**:
    
    - Process of **recalculating bucket index** and redistributing entries when HashMap resizes.
        
7. **LRU Cache**:
    
    - "Least Recently Used" caching strategy.
        
    - Oldest accessed entries are removed when capacity is exceeded.
        

---

## **2. HashMap**

**Definition:**  
`HashMap<K,V>` is a **key-value collection** allowing one null key and multiple null values. It is **unordered** and provides **fast lookup and insertion** using hashing.

### **Internal Structure**

- Backed by an **array of buckets**.
    
- Each bucket stores a **Node**:
    

```java
Node { int hash; K key; V value; Node next; }
```

- **Bucket Index Calculation:**
    

```
index = hash(key) % capacity
```

- **Collision Handling:**
    
    - Keys with same bucket index → stored in **linked list**.
        
    - If linked list size ≥ 8 → converted to **Red-Black Tree**.
        
- **Resizing:**
    
    - When `size > capacity * loadFactor`, capacity doubles, and entries are rehashed.
        

### **Key Features**

- Unordered collection
    
- 1 null key, multiple null values allowed
    
- Not synchronized
    
- Replaces value if key exists
    
- Fast O(1) average for get/put
    

### **Time Complexity**

|Operation|Average|Worst Case|
|---|---|---|
|get()|O(1)|O(log n)|
|put()|O(1)|O(log n)|
|remove()|O(1)|O(log n)|

### **Example**

```java
HashMap<String, Integer> map = new HashMap<>();
map.put("A", 1);
map.put("B", 2);
map.put("A", 10); // updates value
System.out.println(map.get("A")); // 10
```

---

## **3. LinkedHashMap**

**Definition:**  
`LinkedHashMap<K,V>` extends HashMap and maintains **insertion order** or **access order** using a **doubly-linked list** connecting entries.

### **Internal Structure**

- Combines **HashMap** (for fast lookup) + **Doubly-Linked List** (for order).
    
- **Ordering Modes:**
    
    1. **Insertion Order (default)** – preserves order of insertion.
        
    2. **Access Order (`accessOrder = true`)** – moves recently accessed entries to end.
        
- **Collision Handling:** Same as HashMap (linked list/tree in bucket).
    

### **Key Features**

- Maintains predictable iteration order
    
- Allows 1 null key, multiple null values
    
- Slightly slower than HashMap due to linked list overhead
    

### **Time Complexity**

- get(), put(), remove() – O(1) average, O(log n) worst
    
- Traversal – O(n)
    

### **Example**

```java
LinkedHashMap<String, Integer> map = new LinkedHashMap<>();
map.put("A", 1);
map.put("B", 2);
map.put("C", 3);
System.out.println(map); // {A=1, B=2, C=3}
```

### **LRU Cache Example**

```java
LinkedHashMap<Integer, String> lru = new LinkedHashMap<>(16, 0.75f, true) {
    protected boolean removeEldestEntry(Map.Entry eldest) {
        return size() > 5; // remove oldest entry when size > 5
    }
};
```

---

## **4. TreeMap**

**Definition:**  
`TreeMap<K,V>` is a **sorted map** backed by a **Red-Black Tree**, storing keys in **natural order** or using a **custom Comparator**.

### **Internal Structure**

- Backed by **Red-Black Tree**.
    
- Keys must implement `Comparable` or a `Comparator` must be provided.
    
- Null key not allowed; null values allowed.
    

### **Key Features**

- Keys are sorted automatically
    
- Unique keys only
    
- Slower than HashMap/LinkedHashMap (`O(log n)`)
    
- Supports **NavigableMap operations**:
    

|Method|Description|
|---|---|
|firstKey()|Returns smallest key|
|lastKey()|Returns largest key|
|higherKey(k)|Next higher key than k|
|lowerKey(k)|Next lower key than k|
|ceilingKey(k)|>= k|
|floorKey(k)|<= k|
|subMap(from, to)|Range query|

### **Time Complexity**

|Operation|Complexity|
|---|---|
|get()|O(log n)|
|put()|O(log n)|
|remove()|O(log n)|
|traversal|O(n)|

### **Example**

```java
TreeMap<Integer, String> map = new TreeMap<>();
map.put(30, "C");
map.put(10, "A");
map.put(20, "B");
System.out.println(map); // {10=A, 20=B, 30=C}
```

---

## **5. Comparison Table**

|Feature|HashMap|LinkedHashMap|TreeMap|
|---|---|---|---|
|Order|Unordered|Insertion/Access|Sorted|
|Null Key|1|1|Not allowed|
|Null Values|Allowed|Allowed|Allowed|
|Speed|Fastest|Slightly slower|Slowest|
|Underlying DS|Hash Table|Hash Table + Linked List|Red-Black Tree|
|Collision Handling|Linked List / Tree|Linked List / Tree|Tree inherently handles ordering|
|Iteration Order|Unpredictable|Predictable|Sorted|
|Use Case|Fast lookup|Ordered lookup / LRU|Sorted key-value pairs / range queries|

---

## **6. Summary**

1. **HashMap:** Best for **fast, unordered key-value storage**.
    
2. **LinkedHashMap:** Best for **predictable order**, insertion/access order, or **LRU cache**.
    
3. **TreeMap:** Best for **sorted key-value storage**, **range queries**, and **ordered operations**.
    
