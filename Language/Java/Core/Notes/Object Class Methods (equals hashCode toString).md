# **Object Class Methods in Java**

The **`Object` class** is the **root of all classes in Java**.  
Every class in Java **implicitly extends `Object`** if no other superclass is defined.

Some of the most important methods of the `Object` class are:

1. `equals(Object obj)`
    
2. `hashCode()`
    
3. `toString()`
    

These methods are **commonly overridden** in custom classes for meaningful behavior.

---

## **1. equals(Object obj)**

The `equals` method is used to **compare two objects for logical equality**.

**Default behavior (in Object class):**

- Compares **references**, not actual content.
    
- Returns `true` if both references point to the **same object**.
    

```java
class Person {
    String name;
    
    Person(String name) { this.name = name; }
}

public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("Alice");
        Person p2 = new Person("Alice");
        
        System.out.println(p1.equals(p2)); // false (default)
    }
}
```

**Overriding equals:**

```java
@Override
public boolean equals(Object obj) {
    if(this == obj) return true; // same reference
    if(obj == null || getClass() != obj.getClass()) return false;
    Person other = (Person) obj;
    return name.equals(other.name); // compare content
}
```

**Notes:**

- Always override `hashCode()` when overriding `equals()`.
    
- Follows **contract**:
    
    - Reflexive: x.equals(x) → true
        
    - Symmetric: x.equals(y) → y.equals(x)
        
    - Transitive: x.equals(y) && y.equals(z) → x.equals(z)
        
    - Consistent
        
    - x.equals(null) → false
        

---

## **2. hashCode()**

The `hashCode()` method returns an **integer hash code** representing the object.

**Purpose:**

- Used in **hash-based collections** like `HashMap`, `HashSet`, `HashTable`.
    
- If two objects are **equal** according to `equals()`, their `hashCode()` must be **same**.
    

```java
@Override
public int hashCode() {
    return name.hashCode();
}
```

**Notes:**

- Equal objects → same hash code.
    
- Unequal objects → may have same hash code (collision).
    

**Example:**

```java
Person p1 = new Person("Alice");
Person p2 = new Person("Alice");

System.out.println(p1.equals(p2));    // true
System.out.println(p1.hashCode() == p2.hashCode()); // true
```

**Terminology:**

- **Collision**: When **two unequal objects** produce the **same hash code**.  
    Handled in Java collections via **buckets / linked lists / trees**.
    

---

## **3. toString()**

The `toString()` method returns a **string representation** of the object.

**Default behavior:**

- Returns: `ClassName@hashCodeInHex`
    

```java
Person p = new Person("Alice");
System.out.println(p); // Person@15db9742
```

**Overriding toString():**

```java
@Override
public String toString() {
    return "Person{name='" + name + "'}";
}

System.out.println(p); // Person{name='Alice'}
```

**Benefits:**

- Makes debugging easier.
    
- Provides human-readable information about objects.
    

---

## **4. Summary Table**

|Method|Default Behavior|Overriding Purpose|
|---|---|---|
|equals()|Compares references|Compare object content/logical equality|
|hashCode()|Returns memory-based integer|Ensure equal objects have same hash|
|toString()|Returns ClassName@hashcode|Provide meaningful string representation|

---

## **5. Best Practices**

1. Override **equals() and hashCode() together**.
    
2. Ensure **hashCode consistency** with equals.
    
3. Override **toString()** for logging and debugging.
    
4. Use **Objects.equals() and Objects.hash()** for safer implementations.
    

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof Person)) return false;
    Person person = (Person) o;
    return Objects.equals(name, person.name);
}

@Override
public int hashCode() {
    return Objects.hash(name);
}
```

---

Next topic after this in your list would be **Exception Handling** (`Try Catch Finally Blocks`).

Do you want me to prepare that next?