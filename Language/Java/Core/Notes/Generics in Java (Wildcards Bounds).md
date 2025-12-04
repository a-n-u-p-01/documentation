# **Generics in Java (Wildcards and Bounds)**

**Generics** in Java provide **type safety at compile-time** and **eliminate the need for type casting** when working with collections or other parameterized types.

Wildcards and bounds enhance generics by providing **flexibility in accepting different types** while maintaining type safety.

---

## **1. What are Generics?**

- A feature introduced in **Java 5**.
    
- Allows **parameterizing types** (classes, interfaces, methods).
    
- Ensures **compile-time type checking**.
    
- Works with **Collections Framework**, e.g., `List<T>`, `Map<K, V>`.
    

**Example:**

```java
List<String> list = new ArrayList<>();
list.add("Java");
String s = list.get(0); // No type cast needed
```

Without generics, you would need type casting:

```java
List list = new ArrayList();
list.add("Java");
String s = (String) list.get(0); // Type cast needed
```

---

## **2. Wildcards in Generics**

**Wildcard (`?`)** represents an **unknown type**.

- Useful when you want to write **flexible methods** that can work with **different generic types**.
    

### **2.1 Unbounded Wildcard `?`**

- Represents **any type**.
    
- Used when you **don’t care about the type**.
    

```java
List<?> list = new ArrayList<String>();
list = new ArrayList<Integer>();
```

**Rules:**

- Cannot add elements except `null`.
    
- Can read elements as `Object`.
    

**Example:**

```java
public void printList(List<?> list) {
    for (Object obj : list) {
        System.out.println(obj);
    }
}
```

---

### **2.2 Bounded Wildcards**

Allows **restricting the types** that can be used with generics.

#### **2.2.1 Upper Bounded Wildcard `<? extends T>`**

- Accepts **T or any subclass of T**.
    
- Commonly used in **read-only** scenarios.
    

```java
List<? extends Number> list;
list = new ArrayList<Integer>();
list = new ArrayList<Double>();
```

**Rules:**

- Can **read** elements as `Number`.
    
- Cannot **add** elements except `null`.
    

**Example:**

```java
public double sum(List<? extends Number> list) {
    double sum = 0;
    for (Number n : list) {
        sum += n.doubleValue();
    }
    return sum;
}
```

#### **2.2.2 Lower Bounded Wildcard `<? super T>`**

- Accepts **T or any superclass of T**.
    
- Commonly used in **write-only** scenarios.
    

```java
List<? super Integer> list;
list = new ArrayList<Number>();
list = new ArrayList<Object>();
```

**Rules:**

- Can **add elements of type T** or subclass.
    
- Can **read elements only as Object**.
    

**Example:**

```java
public void addNumbers(List<? super Integer> list) {
    list.add(10);
    list.add(20);
    // Cannot assume type when reading
    Object obj = list.get(0);
}
```

---

## **3. Generics in Methods**

- Generics can be **applied to methods** independent of class generics.
    

```java
public static <T> void printArray(T[] arr) {
    for (T element : arr) {
        System.out.println(element);
    }
}

Integer[] nums = {1, 2, 3};
printArray(nums);

String[] words = {"Java", "Generics"};
printArray(words);
```

---

## **4. PECS Rule (Producer Extends, Consumer Super)**

- **Guideline for using bounded wildcards**:
    
    - **Producer (`extends`)**: Use `? extends T` when the collection is producing values (you read from it).
        
    - **Consumer (`super`)**: Use `? super T` when the collection is consuming values (you write to it).
        

**Example:**

```java
List<? extends Number> producer = new ArrayList<Integer>();
Number n = producer.get(0); // read allowed
// producer.add(10); // not allowed

List<? super Integer> consumer = new ArrayList<Number>();
consumer.add(10); // write allowed
Object o = consumer.get(0); // read as Object
```

---

## **5. Advantages of Generics**

1. **Type Safety:** Compile-time checking prevents runtime `ClassCastException`.
    
2. **Reusability:** Can create a single class/method for multiple types.
    
3. **Eliminate Casting:** Reduces boilerplate code.
    
4. **Better API Documentation:** Clearer type contracts in method signatures.
    

---

## **6. Summary Table of Wildcards**

|Wildcard Type|Accepts|Add Elements?|Read Elements As|Use Case|
|---|---|---|---|---|
|`?`|Any type|No (except null)|Object|General, read-only methods|
|`? extends T`|T or subclasses of T|No (except null)|T|Producer / read-only|
|`? super T`|T or superclasses of T|Yes, T or subclass|Object|Consumer / write-only|

---

## **7. Examples Using Collections**

```java
List<Integer> intList = Arrays.asList(1, 2, 3);
List<Double> doubleList = Arrays.asList(1.5, 2.5);

public double sumList(List<? extends Number> list) {
    double sum = 0;
    for (Number n : list) {
        sum += n.doubleValue();
    }
    return sum;
}

System.out.println(sumList(intList));    // 6.0
System.out.println(sumList(doubleList)); // 4.0
```

---

## **8. Best Practices**

1. Use **generics** to ensure **type safety** in collections and methods.
    
2. Apply **upper-bounded (`extends`) wildcards** for **read-only collections**.
    
3. Apply **lower-bounded (`super`) wildcards** for **write-only collections**.
    
4. Use **PECS rule** for designing APIs with generics.
    
5. Avoid raw types (`List` instead of `List<T>`), as they **bypass type checking**.
    

---

If you want, I can next make a **detailed note on `ArrayList`, `LinkedList`, `HashSet`, `TreeSet`, `HashMap`, `LinkedHashMap`, and `TreeMap` with Generics examples**, combining everything for a **practical understanding**.

Do you want me to do that next?