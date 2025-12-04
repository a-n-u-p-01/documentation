# ArrayList

**Definition:**  
`ArrayList` is a **resizable array** implementation of the `List` interface in Java. It allows dynamic memory allocation and can grow as needed.

**Internal Working:**

- Backed by an `Object[]` array.
    
- When capacity is exceeded, a new array is created with **1.5x the old capacity**, and elements are copied over.
    
- Provides **fast random access** using index.
    

**Characteristics:**

- Maintains insertion order.
    
- Allows duplicates and `null`.
    
- Not synchronized (not thread-safe).
    
- Supports fast retrieval (`O(1)` for `get()`), but slow insertion/deletion in the middle (`O(n)`).
    

**Time Complexity:**

- `get(index)`: O(1)
    
- `add(element)`: O(1) amortized
    
- `add(index, element)`: O(n)
    
- `remove(index)`: O(n)
    
- `contains(element)`: O(n)
    

**Use Cases:**

- Read-heavy scenarios
    
- Random access required
    
- List of items that grow dynamically
    

**Example:**

```java
ArrayList<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");
System.out.println(list.get(1)); // B
```

---

# LinkedList

**Definition:**  
`LinkedList` is a **doubly linked list** implementation of both `List` and `Deque` interfaces.

**Internal Working:**

- Each element is stored as a **node** containing `prev`, `data`, and `next` references.
    
- No fixed capacity; memory allocated dynamically.
    

**Characteristics:**

- Maintains insertion order.
    
- Allows duplicates and `null`.
    
- Good for insertions and deletions in the middle.
    
- Random access is slow (`O(n)`).
    

**Time Complexity:**

- `add/remove` at ends: O(1)
    
- `get(index)`: O(n)
    
- `contains(element)`: O(n)
    

**Use Cases:**

- Frequent insertions/deletions
    
- Implementing stacks, queues, and deques
    

**Example:**

```java
LinkedList<String> list = new LinkedList<>();
list.add("A");
list.add("B");
list.addFirst("Start");
System.out.println(list); // [Start, A, B]
```

---

# HashSet

**Definition:**  
`HashSet` is a **Set** implementation backed by a **HashMap**. It stores **unique elements** and does not maintain order.

**Internal Working:**

- Uses a `HashMap` internally where the set element is the key, and a dummy value is used.
    
- Hashing is used to determine the bucket index.
    
- Collision occurs when two keys hash to the same index. Handled using a **linked list** or **red-black tree** (if many collisions in a bucket).
    

**Characteristics:**

- Does not allow duplicates.
    
- Allows one `null`.
    
- Unordered.
    
- Fast operations (`O(1)` average).
    

**Time Complexity:**

- `add`: O(1) average
    
- `contains`: O(1) average
    
- `remove`: O(1) average
    
- Worst-case: O(log n) if bucket treeified
    

**Use Cases:**

- Removing duplicates from a collection
    
- Fast membership tests
    

**Example:**

```java
HashSet<String> set = new HashSet<>();
set.add("A");
set.add("B");
set.add("A"); // ignored
System.out.println(set); // [A, B]
```

---

# TreeSet

**Definition:**  
`TreeSet` is a **sorted set** backed by a **TreeMap**. It stores **unique elements** in a **natural or custom order**.

**Internal Working:**

- Internally implemented using a **Red-Black tree** (self-balancing binary search tree).
    
- Maintains keys in sorted order.
    
- Null is **not allowed** as keys because comparison is required.
    

**Characteristics:**

- Unique elements only.
    
- Sorted order.
    
- Slower than HashSet (`O(log n)` operations).
    
- Provides **NavigableSet** methods like `first()`, `last()`, `higher()`, `lower()`.
    

**Time Complexity:**

- `add`: O(log n)
    
- `contains`: O(log n)
    
- `remove`: O(log n)
    

**Use Cases:**

- Sorted collection of elements
    
- Range queries
    
- Leaderboards, dictionaries
    

**Example:**

```java
TreeSet<Integer> set = new TreeSet<>();
set.add(30);
set.add(10);
set.add(20);
System.out.println(set); // [10, 20, 30]
```

---

# Comparison Table

|Feature|ArrayList|LinkedList|HashSet|TreeSet|
|---|---|---|---|---|
|Order|Maintains insertion|Maintains insertion|Unordered|Sorted|
|Duplicates|Allowed|Allowed|Not allowed|Not allowed|
|Null|Allowed|Allowed|1 allowed|Not allowed|
|Random Access|Fast O(1)|Slow O(n)|N/A|N/A|
|Insert/Delete|Slow in middle O(n)|Fast at ends O(1)|Fast O(1) avg|O(log n)|
|Implementation|Dynamic array|Doubly linked list|HashMap|TreeMap (Red-Black Tree)|
|Use Case|Read-heavy, random access|Frequent insert/delete|Unique elements, fast lookup|Sorted unique elements, range queries|

---

If you want, I can next provide **a fully detailed note on `HashMap, LinkedHashMap, TreeMap` with collision explanation, bucket diagram, and time complexity**, which is often asked together with these four collections.

Do you want me to do that next?