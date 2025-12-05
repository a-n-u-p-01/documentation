`Externalizable` is an advanced mechanism for serialization in Java that gives you **full control** over how objects are written and read.

It is part of the package:

```java
java.io.Externalizable
```

---

# **1. What Is Externalizable?**

`Externalizable` is an alternative to `Serializable`, but **not automatic**.

If a class implements `Serializable`, Java automatically serializes all non-transient, non-static fields.

If a class implements **Externalizable**, Java:

❌ does NOT automatically serialize anything  
✔ requires YOU to manually specify what to save  
✔ requires YOU to manually specify how to read it

---

# **2. Required Methods**

A class implementing `Externalizable` **must override**:

```java
void writeExternal(ObjectOutput out) throws IOException
void readExternal(ObjectInput in) throws IOException, ClassNotFoundException
```

These methods control:

- What to write
    
- How to write
    
- What to read
    
- How to read
    

---

# **3. Example: Basic Externalizable Implementation**

```java
import java.io.*;

public class User implements Externalizable {
    private String name;
    private transient String password;

    public User() {
        // Mandatory public no-arg constructor
    }

    public User(String name, String password) {
        this.name = name;
        this.password = password;
    }

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeObject(name);  
        out.writeObject(encrypt(password));  // custom behavior
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        name = (String) in.readObject();
        password = decrypt((String) in.readObject());
    }
}
```

### Key Observations:

- You decide **exactly** what gets serialized.
    
- `password` is explicitly encrypted before writing.
    
- Constructor must be **public no-arg** (very important).
    

---

# **4. Differences Between Serializable and Externalizable**

|Serializable|Externalizable|
|---|---|
|Automatic serialization|Manual serialization|
|Easy to use|More complex|
|JVM handles object graph|Developer handles fields|
|Good for most use cases|Good for performance-critical use cases|
|transient keyword used to skip fields|You manually skip fields|
|Constructor not called during deserialization|No-arg constructor **must exist**|

---

# **5. Advantages of Externalizable**

### ✔ Full Control

You control what fields are serialized and how.

### ✔ Better Performance

You serialize **only what is required**, reducing:

- file size
    
- network bandwidth
    
- memory usage
    

### ✔ Custom Encryption / Compression

Useful for storing passwords, secure fields, or optimized binary formats.

---

# **6. Disadvantages of Externalizable**

### ✖ Must Write All Logic Manually

Risk of mistakes increases.

### ✖ Requires Public No-Arg Constructor

If missing → deserialization fails.

### ✖ Breaks Easily

If you forget to read/write fields in the correct order → errors occur.

### ✖ Not suitable for large and frequently changing classes.

---

# **7. When Should You Use Externalizable?**

Use Externalizable when:

✔ You need full control over the serialized format  
✔ You want to optimize storage or speed  
✔ You want custom encryption or transformation  
✔ You want only specific fields saved

Do NOT use when:

❌ Class structure changes frequently  
❌ You prefer simplicity  
❌ You don’t need custom behavior

---

# **8. Internal Working (Deep but Clear)**

When you serialize an object using Externalizable:

1. JVM creates the object **by calling the no-arg constructor**
    
2. JVM calls your **writeExternal()** to record data
    
3. During deserialization:
    
    - JVM again creates the object via no-arg constructor
        
    - Then calls **readExternal()**
        
    - You must reconstruct the object manually
        

This is very different from Serializable, where:

- No constructor is called
    
- JVM restores fields automatically
    

---

# **9. Simple Practical Example**

### Writing:

```java
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("user.ser"));
oos.writeObject(new User("Anupam", "12345"));
oos.close();
```

### Reading:

```java
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("user.ser"));
User u = (User) ois.readObject();
ois.close();
```

---

# **10. Frequently Asked Interview Questions**

### **1. What is Externalizable?**

An interface that provides full control over serialization by implementing `writeExternal()` and `readExternal()` methods.

---

### **2. How is it different from Serializable?**

`Serializable` is automatic; `Externalizable` is manual.

---

### **3. Why does Externalizable require a no-arg constructor?**

JVM uses it to create an empty object before calling `readExternal()`.

---

### **4. If you forget to write a field in writeExternal(), what happens?**

That field will not be restored during deserialization → data loss.

---

### **5. Can Externalizable be used for performance optimization?**

Yes, that's one of its main purposes.

---

# ✔ Summary (Clean & Short)

- Implements **Externalizable** when you need complete control
    
- Requires two methods: `writeExternal()` and `readExternal()`
    
- Must have a **public no-arg constructor**
    
- Good for performance + security
    
- Risky if not handled carefully
    

---