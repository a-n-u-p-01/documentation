In Java, a **String** represents a **sequence of characters**. It is one of the most commonly used classes in Java.

**Key Points:**

1. Strings are **objects** in Java (not primitive types).
    
2. Strings are **immutable**:
    
    - Once created, the content of a `String` cannot be changed.
        
    - Any modification creates a **new String object**.
        
3. Strings are stored in a special memory area called the **String Constant Pool (SCP)**.
    

```java
String s1 = "Hello"; // Stored in String Pool
String s2 = "Hello"; // s1 and s2 refer to same object in pool
String s3 = new String("Hello"); // Creates new object in heap
```

---

# **2. String Creation**

### **2.1 Using String Literal**

```java
String s1 = "Java";
```

- Stored in **String Pool**
    
- Reuses existing objects if same value exists
    

### **2.2 Using `new` Keyword**

```java
String s2 = new String("Java");
```

- Stored in **heap memory**
    
- Does not reuse pool objects unless explicitly interned
    

---

# **3. Immutability of Strings**

- Strings cannot be changed after creation
    
- Modifying a string returns a **new String object**:
    

```java
String s = "Hello";
String s2 = s.concat(" World"); // s remains "Hello", s2 = "Hello World"
```

- Benefits of immutability:
    
    1. Thread-safe (no synchronization required)
        
    2. Can be shared safely in the String Pool
        
    3. Efficient for **hash-based collections** (like `HashMap`)
        

---

# **4. String Pool (SCP)**

- Special memory area inside JVM heap for **string literals**
    
- Reduces memory usage by **reusing string objects**
    
- Methods:
    
    - `intern()` → returns reference from String Pool if exists, else adds it
        

```java
String s1 = new String("Java").intern(); // points to pool object
```

---

# **5. Commonly Used String Methods**

## **5.1 Character and Length Methods**

|Method|Description|Example|
|---|---|---|
|`length()`|Returns length of string|`"Hello".length()` → 5|
|`charAt(int index)`|Returns character at index|`"Java".charAt(2)` → 'v'|
|`codePointAt(int index)`|Returns Unicode code of char|`"A".codePointAt(0)` → 65|

---

## **5.2 Comparison Methods**

|Method|Description|Example|
|---|---|---|
|`equals(Object obj)`|Checks value equality|`"Java".equals("Java")` → true|
|`equalsIgnoreCase(String str)`|Case-insensitive equality|`"java".equalsIgnoreCase("JAVA")` → true|
|`compareTo(String str)`|Lexicographical comparison|`"abc".compareTo("abd")` → -1|
|`compareToIgnoreCase(String str)`|Ignore case in comparison|`"abc".compareToIgnoreCase("ABC")` → 0|

---

## **5.3 Searching and Indexing**

|Method|Description|Example|
|---|---|---|
|`contains(CharSequence s)`|Checks if substring exists|`"Hello".contains("ll")` → true|
|`startsWith(String prefix)`|Checks starting substring|`"Java".startsWith("Ja")` → true|
|`endsWith(String suffix)`|Checks ending substring|`"Java".endsWith("va")` → true|
|`indexOf(char c)`|Returns first index of char|`"Hello".indexOf('l')` → 2|
|`lastIndexOf(char c)`|Returns last index of char|`"Hello".lastIndexOf('l')` → 3|

---

## **5.4 Substring and Splitting**

```java
String str = "Hello World";
String sub = str.substring(0, 5); // "Hello" (end index exclusive)
String[] parts = str.split(" ");   // {"Hello", "World"}
```

---

## **5.5 Case Conversion and Trimming**

```java
String s = "  Java  ";
s.toLowerCase(); // "  java  "
s.toUpperCase(); // "  JAVA  "
s.trim();        // "Java" (removes leading/trailing spaces)
```

---

## **5.6 Replacement**

```java
String s = "Hello World";
s.replace('o', 'a');        // "Hella Warld"
s.replaceAll("World", "Java"); // "Hello Java" (supports regex)
```

---

## **5.7 Conversion Between String and Other Types**

```java
int x = 100;
String s = String.valueOf(x); // "100"
int y = Integer.parseInt(s);  // 100
```

---

# **6. String Immutability vs StringBuilder/StringBuffer**

|Feature|String|StringBuilder|StringBuffer|
|---|---|---|---|
|Mutability|Immutable|Mutable|Mutable|
|Thread-safe|Yes|No|Yes|
|Performance|Slow for modifications|Fast|Slightly slower than StringBuilder|

**Example:**

```java
String s = "Hello";
s.concat(" World"); // New String created
```

```java
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World"); // Modifies original object
```

---

# **7. String Pool vs Heap**

- **String Pool**: stores literals; shared references
    
- **Heap**: stores objects created via `new` keyword
    

```java
String s1 = "Java";          // pool
String s2 = new String("Java"); // heap
System.out.println(s1 == s2); // false
System.out.println(s1.equals(s2)); // true
```

---

# **8. Important Notes**

1. ` == ` checks **reference equality**, `.equals()` checks **value equality**.
    
2. String concatenation with `+` creates new objects if immutable strings are used.
    
3. For frequent modifications, use **StringBuilder** or **StringBuffer**.
    
4. Strings are **final class**, so cannot be subclassed.
    

---

# **9. Common Interview Questions on String Class**

### **Basic**

1. Difference between ` == ` and `equals()` for Strings.
    
2. What is a String Pool?
    
3. Why Strings are immutable in Java?
    
4. Difference between String, StringBuilder, and StringBuffer.
    

### **Intermediate**

5. How to reverse a String in Java?
    

```java
String s = "Hello";
String rev = new StringBuilder(s).reverse().toString();
```

6. How to check if a String is a palindrome?
    

```java
String s = "madam";
String rev = new StringBuilder(s).reverse().toString();
System.out.println(s.equals(rev)); // true
```

7. How to convert String to int, double, or char array?
    
8. Count occurrences of a character in a String.
    

### **Advanced**

9. How to remove all white spaces from a String?
    

```java
String s = " H e l l o ";
s = s.replaceAll("\\s", ""); // "Hello"
```

10. How to check if two Strings are **anagrams**?
    
11. Difference between `substring()` behavior before and after Java 7 (memory optimization).
    
12. Explain **intern()** method and when to use it.
    
13. How to split a String based on multiple delimiters using regex?
    

---

# **10. Summary**

- Strings are **immutable, final, and widely used objects**.
    
- String Pool reduces memory usage and improves performance.
    
- Use **StringBuilder/StringBuffer** for mutable strings.
    
- Mastery of **String methods** is essential for **coding interviews** and real-world Java applications.
    
