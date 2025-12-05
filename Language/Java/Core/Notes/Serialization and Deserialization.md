Serialization and Deserialization are mechanisms in Java used to **convert objects into a format that can be stored or transferred**, and to **recreate them later**.

They are essential for:

- Saving object state to files
    
- Sending objects over a network
    
- Caching objects
    
- Deep cloning
    
- Distributed systems, RMI, messaging, etc.
    

---

# **1. What is Serialization?**

**Serialization** is the process of converting a **Java object → sequence of bytes**.

These bytes can then be:

- Stored (file, DB, memory)
    
- Transmitted (network, sockets)
    
- Cached
    
- Reconstructed later
    

Java uses:

- `ObjectOutputStream` → to serialize
    
- `Serializable` interface → to mark an object as serializable
    

---

# **2. What is Deserialization?**

**Deserialization** is the reverse process:  
Converting **bytes → original Java object**.

Java uses:

- `ObjectInputStream` → to read bytes and rebuild the object
    

---

# **3. How to Make a Class Serializable?**

A class must implement **`java.io.Serializable`**:

```java
class Student implements Serializable {
    private int id;
    private String name;
}
```

### Key points:

- `Serializable` is a **marker interface** → no methods
    
- Serialization is automatic after implementing it
    

---

# **4. Basic Serialization Example**

```java
Student s = new Student(1, "Anupam");

FileOutputStream fos = new FileOutputStream("student.ser");
ObjectOutputStream oos = new ObjectOutputStream(fos);

oos.writeObject(s);

oos.close();
```

This converts the object into bytes and writes them into _student.ser_.

---

# **5. Basic Deserialization Example**

```java
FileInputStream fis = new FileInputStream("student.ser");
ObjectInputStream ois = new ObjectInputStream(fis);

Student s = (Student) ois.readObject();

ois.close();
```

The object is fully reconstructed with all values.

---

# **6. serialVersionUID (VERY IMPORTANT)**

Every serializable class should declare:

```java
private static final long serialVersionUID = 1L;
```

### Why needed?

- It **prevents InvalidClassException** during deserialization
    
- Helps version control of serialized objects
    

If you **do not** provide it:

- JVM generates one dynamically
    
- Any change in class structure will break deserialization
    

---

# **7. What All Gets Serialized?**

By default:  
✔ All **non-static**, **non-transient** fields  
✔ All object references, recursively (if they are serializable)

Not serialized:  
❌ static fields  
❌ transient fields  
❌ methods  
❌ constructors  
❌ classes that do NOT implement Serializable

---

# **8. The transient Keyword**

`transient` prevents a field from being serialized.

Example:

```java
class User implements Serializable {
    String name;
    transient String password;
}
```

- `name` will be serialized
    
- `password` will NOT be serialized
    

Why use transient?

- For sensitive data (passwords)
    
- Derived or calculated data
    
- Fields that shouldn’t be saved
    

---

# **9. Custom Serialization (Advanced)**

If you want full control, implement:

```java
private void writeObject(ObjectOutputStream oos) throws Exception
```

and

```java
private void readObject(ObjectInputStream ois) throws Exception
```

Example:

```java
private void writeObject(ObjectOutputStream oos) throws Exception {
    oos.defaultWriteObject();
    oos.writeObject(encrypt(password));
}
```

Used for:

- Encrypting data
    
- Partial serialization
    
- Custom validation
    

---

# **10. Serialization of Object Graphs**

If an object contains other objects:

```java
class A { B b; }
class B { C c; }
class C {}
```

Serialization automatically handles the entire graph IF all classes implement Serializable.

If **any one** is not Serializable → `NotSerializableException`.

---

# **11. Externalizable Interface (Advanced Alternative)**

`Externalizable` gives complete control over what gets serialized.

You must implement:

- `writeExternal()`
    
- `readExternal()`
    

Advantages:  
✔ Full control  
✔ More performance

Disadvantages:  
❌ More complex  
❌ Must handle all fields manually

---

# **12. Common Problems in Serialization**

### **NotSerializableException**

Occurs when serializing an object containing a non-serializable field.

### **InvalidClassException**

Occurs when:

- serialVersionUID mismatches
    
- Class structure changed
    

### **Performance overhead**

Serialization is slower than JSON/XML in some cases.

---

# **13. Serialization Use Cases**

✔ Saving application state  
✔ Caching objects  
✔ Storing session data  
✔ Sending objects across network  
✔ Activating passive distributed objects (RMI)  
✔ Deep cloning objects (via byte array)

---

# **14. Serialization Best Practices**

- Always define `serialVersionUID`
    
- Use `transient` for sensitive fields
    
- Avoid serializing large object graphs
    
- Prefer JSON for interoperability
    
- Use Externalizable when you need custom behavior
    

---

# **15. ADVANCED Internal Working (Clear Explanation)**

### During Serialization:

1. JVM checks if class implements Serializable
    
2. serialVersionUID is written
    
3. Object fields are written
    
4. References are serialized recursively
    
5. Output is streamed as bytes
    

### During Deserialization:

1. serialVersionUID is checked
    
2. JVM creates object **without calling constructor**
    
3. Fields are restored
    
4. Object returned
    

---

# **16. Common Interview Questions**

### **1. What is serialization?**

Converting an object into bytes for storage or transfer.

---

### **2. What is transient?**

A keyword that prevents a field from being serialized.

---

### **3. Why serialVersionUID is used?**

To ensure compatibility between serialized and deserialized versions of a class.

---

### **4. Difference between Serializable and Externalizable?**

|Serializable|Externalizable|
|---|---|
|Automatic|Manual|
|Less control|Full control|
|Simpler|More complex|

---

### **5. Can constructors run during deserialization?**

No. JVM creates object **without calling the constructor**.

---

### **6. Are static fields serialized?**

No. They belong to the class, not the object.

---

# **Summary**

- Serialization = object → bytes
    
- Deserialization = bytes → object
    
- Requires Serializable
    
- Use transient for hidden fields
    
- serialVersionUID is crucial
    
- ObjectOutputStream / ObjectInputStream are used
    
- Custom serialization allows special handling
    

---
