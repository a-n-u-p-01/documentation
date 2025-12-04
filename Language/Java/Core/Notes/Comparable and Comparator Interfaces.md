# **Comparable and Comparator Interfaces in Java**

Java provides **two main mechanisms** for **sorting objects**: `Comparable` and `Comparator`. These are used when working with **Collections**, **arrays**, or **sorted data structures** like `TreeSet` and `TreeMap`.

---

## **1. Comparable Interface**

**Definition:**  
`Comparable<T>` is an interface in `java.lang` used to define the **natural ordering** of objects. Classes implement this interface to allow their objects to be sorted.

**Package:** `java.lang`  
**Method to implement:**

```java
int compareTo(T obj)
```

### **1.1 Method: compareTo()**

|Return Value|Meaning|
|---|---|
|Negative|Current object `<` obj|
|Zero|Current object `==` obj|
|Positive|Current object `>` obj|

### **1.2 Key Points**

- Defines **natural ordering** (default sorting) for a class.
    
- Only **one sorting sequence** per class is possible.
    
- Often used with `Collections.sort()` or `Arrays.sort()`.
    
- Can sort **ascending or descending** by modifying the logic.
    

### **1.3 Example: Comparable**

```java
class Student implements Comparable<Student> {
    String name;
    int marks;

    Student(String name, int marks) {
        this.name = name;
        this.marks = marks;
    }

    @Override
    public int compareTo(Student s) {
        return this.marks - s.marks; // ascending order by marks
    }

    @Override
    public String toString() {
        return name + ":" + marks;
    }
}

List<Student> students = new ArrayList<>();
students.add(new Student("Alice", 85));
students.add(new Student("Bob", 75));
students.add(new Student("Charlie", 95));

Collections.sort(students); // uses compareTo
System.out.println(students); // [Bob:75, Alice:85, Charlie:95]
```

---

## **2. Comparator Interface**

**Definition:**  
`Comparator<T>` is an interface in `java.util` used to define **custom ordering**. Unlike `Comparable`, it can define **multiple sorting sequences** for the same class.

**Package:** `java.util`  
**Methods to implement:**

```java
int compare(T o1, T o2)
boolean equals(Object obj) // optional
```

### **2.1 Method: compare()**

|Return Value|Meaning|
|---|---|
|Negative|o1 `<` o2|
|Zero|o1 == o2|
|Positive|o1 `>` o2|

### **2.2 Key Points**

- Can define **multiple sorting strategies** for the same class.
    
- Can be used **without modifying the class**.
    
- Often used with `Collections.sort(list, comparator)` or `Arrays.sort(array, comparator)`.
    

### **2.3 Example: Comparator**

```java
class Student {
    String name;
    int marks;

    Student(String name, int marks) {
        this.name = name;
        this.marks = marks;
    }

    @Override
    public String toString() {
        return name + ":" + marks;
    }
}

// Comparator for sorting by name
Comparator<Student> nameComparator = new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        return s1.name.compareTo(s2.name);
    }
};

List<Student> students = new ArrayList<>();
students.add(new Student("Alice", 85));
students.add(new Student("Bob", 75));
students.add(new Student("Charlie", 95));

Collections.sort(students, nameComparator);
System.out.println(students); // [Alice:85, Bob:75, Charlie:95]
```

**Using Lambda Expression (Java 8+):**

```java
Collections.sort(students, (s1, s2) -> s2.marks - s1.marks); // descending by marks
```

---

## **3. Differences Between Comparable and Comparator**

|Feature|Comparable|Comparator|
|---|---|---|
|Package|java.lang|java.util|
|Method to implement|compareTo(T o)|compare(T o1, T o2)|
|Object modification|Class must implement Comparable|No need to modify the class|
|Number of sequences|Only 1 (natural ordering)|Multiple sequences possible|
|Sorting use|Collections.sort(list)|Collections.sort(list, comparator)|
|Example usage|Sorting by marks (default)|Sorting by name, marks, or other attributes|

---

## **4. Using with TreeSet and TreeMap**

- `TreeSet` and `TreeMap` **require sorting**.
    
- If class implements `Comparable`, natural ordering is used.
    
- If `Comparator` is provided, custom ordering is used.
    

**Example:**

```java
TreeSet<Student> set = new TreeSet<>(Comparator.comparing(s -> s.name));
set.add(new Student("Alice", 85));
set.add(new Student("Bob", 75));
set.add(new Student("Charlie", 95));

System.out.println(set); // sorted by name
```

---

## **5. Summary**

1. **Comparable:**
    
    - Defines **natural ordering**.
        
    - Implemented in the class.
        
    - One sorting order per class.
        
2. **Comparator:**
    
    - Defines **custom ordering**.
        
    - Can be **external to class**.
        
    - Multiple sorting orders possible.
        
3. **Use Case:**
    
    - Use Comparable for **default sorting**.
        
    - Use Comparator for **flexible/custom sorting** or when class cannot be modified.
        

---

I can next make a **detailed note on Collections Utility Class with examples** if you want, showing **sort, reverse, shuffle, frequency, etc.**, which ties in directly with Comparable and Comparator.

Do you want me to create that?