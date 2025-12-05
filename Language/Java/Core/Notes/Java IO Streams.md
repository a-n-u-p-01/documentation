Java I/O (Input/Output) is a framework that allows Java programs to **read data** from different sources and **write data** to different destinations.  
Everything in Java I/O works around the concept of **streams**.

---

# **1. What is a Stream?**

A **Stream** is a sequence of data that flows from one point to another.

### Examples:

- Reading from a file → input stream
    
- Writing to a file → output stream
    
- Reading network data → input stream
    
- Writing logs → output stream
    

A stream is **one-directional** — it either reads or writes, not both.

---

# **2. Two Major Types of Streams**

Java divides streams based on the type of data they process:

---

## **A. Byte Streams (Binary data)**

- Handle **raw binary data (8-bit bytes)**
    
- Used for:
    
    - Images (JPG, PNG)
        
    - Audio / Video
        
    - PDF files
        
    - ZIP files
        
    - Any non-text data
        

### Parent classes:

- `InputStream` (reads bytes)
    
- `OutputStream` (writes bytes)
    

These streams read/write **one byte at a time** or **multiple bytes with a buffer**.

---

## **B. Character Streams (Text data)**

- Handle **Unicode characters (16-bit)**
    
- Used for reading/writing text files:
    
    - `.txt`, `.log`, `.csv`, `.json`, `.html`, `.xml`
        

### Parent classes:

- `Reader` (reads characters)
    
- `Writer` (writes characters)
    

Character streams automatically handle **character encoding** (UTF-8, UTF-16 etc.)

---

# **3. Why Two Types?**

Because computers store everything as **bytes**, but humans read **text**.

Byte Stream → for machines  
Character Stream → for humans

---

# **4. Low-Level vs High-Level Streams**

Java uses layering (decorator pattern).

### **Low-Level Streams**

Directly connected to the data source (file, network).

Examples:

- `FileInputStream`, `FileReader`
    

### **High-Level Streams**

Provide extra functionality like:

- buffering
    
- reading lines
    
- formatting
    
- converting bytes ↔ characters
    

Examples:

- `BufferedReader`
    
- `BufferedInputStream`
    
- `PrintWriter`
    

You wrap high-level streams around low-level ones.

---

# **5. Important Stream Classes (Explained Conceptually)**

### **A. FileInputStream / FileOutputStream**

- Used for reading/writing **binary files**.
    
- Does not understand text encoding.
    
- Reads raw bytes.
    

---

### **B. FileReader / FileWriter**

- Used for **text files**.
    
- Works with **characters**.
    
- Handles text encoding (default or specified).
    

---

### **C. BufferedReader / BufferedWriter**

Why buffering?

→ Reading a file character-by-character is slow.  
→ BufferedReader reads chunks into memory first.

Benefits:

- Very fast
    
- `readLine()` method available
    

---

### **D. BufferedInputStream / BufferedOutputStream**

Same as above but for **binary streams**.

---

### **E. InputStreamReader / OutputStreamWriter**

Bridge between **byte streams** and **character streams**.

Needed when:

- Source gives data as bytes (e.g., System.in)
    
- You want to treat it as characters (text)
    

Example:

```java
InputStreamReader isr = new InputStreamReader(System.in);
```

---

### **F. PrintWriter**

- Very convenient for writing formatted text.
    
- Supports `println()`, `printf()`.
    

---

### **G. ObjectInputStream / ObjectOutputStream**

Used for **serialization**:

- Converting Java objects → bytes (save to file)
    
- Reading bytes → Java objects (load from file)
    

---

# **6. try-with-resources (Essential Modern I/O)**

Streams must be closed to prevent memory and file handle leaks.

```java
try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
    String line = br.readLine();
}
```

Benefits:

- Automatically closes the stream
    
- Cleaner and safer
    

---

# **7. Character Encoding (Very Important)**

Text files use encodings like UTF-8, UTF-16.

`FileReader` uses default platform encoding → **not recommended**.

Better:

```java
new InputStreamReader(new FileInputStream("data.txt"), StandardCharsets.UTF_8);
```

This ensures consistent behavior across systems.

---

# **8. Common I/O Operations Explained Clearly**

---

## **A. Reading a Text File (Recommended)**

```java
try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}
```

**Why BufferedReader?**  
It reads efficiently and supports `readLine()`.

---

## **B. Writing a Text File**

```java
try (BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"))) {
    bw.write("Hello World");
    bw.newLine();
}
```

---

## **C. Copying a Binary File (Image, PDF)**

```java
try (FileInputStream fis = new FileInputStream("a.jpg");
     FileOutputStream fos = new FileOutputStream("b.jpg")) {

    byte[] buffer = new byte[1024];
    int bytesRead;

    while ((bytesRead = fis.read(buffer)) != -1) {
        fos.write(buffer, 0, bytesRead);
    }
}
```

This is how professional applications copy large files.

---

# **9. Common Exceptions in I/O**

|Exception|Meaning|
|---|---|
|**FileNotFoundException**|File not found or not accessible|
|**IOException**|General I/O failure (disk full, read error)|
|**EOFException**|Unexpected end-of-file|
|**UnsupportedEncodingException**|Wrong charset|

All I/O operations usually require `try-catch`.

---

# **10. Internal Working of Streams (Deep but Simple)**

Whenever you call `read()`:

1. Stream requests data from OS
    
2. OS reads disk/network/keyboard
    
3. Data moves into Java buffer
    
4. Stream passes bytes/chars to your program
    

Buffered streams improve Step 3 by reducing **disk I/O**, which is slow.

---

# **11. Stream Decorator Pattern (Advanced Insight)**

Java I/O uses **composition**, not inheritance.

Example:

```java
BufferedReader br =
    new BufferedReader(
        new InputStreamReader(
            new FileInputStream("data.txt")
        )
    );
```

Each layer adds features:

- FileInputStream → reads bytes
    
- InputStreamReader → converts bytes to characters
    
- BufferedReader → speeds up reading + `readLine()`
    

This architecture makes Java I/O extremely flexible.

---

# **12. Key Differences (High-Value Interview Content)**

---

## **Byte Stream vs Character Stream**

|Byte Stream|Character Stream|
|---|---|
|Reads/Writes bytes|Reads/Writes characters|
|For binary data|For text data|
|Doesn’t handle encoding|Handles encoding|

---

## **BufferedReader vs FileReader**

|FileReader|BufferedReader|
|---|---|
|Low-level|High-level|
|Reads char-by-char|Reads using buffer|
|Slower|Faster|
|No readLine()|Has readLine()|

---

## **FileInputStream vs FileReader**

|FileInputStream|FileReader|
|---|---|
|Binary|Text|
|Reads bytes|Reads characters|

---

# **13. Frequently Asked Interview Questions**

### **1. Why do we need two kinds of streams?**

Because binary data and text data behave differently and must be handled differently.

---

### **2. Why use BufferedReader instead of FileReader?**

BufferedReader is much faster and supports `readLine()`.

---

### **3. What is the purpose of InputStreamReader?**

To convert bytes into characters using a charset (UTF-8 etc.)

---

### **4. What is serialization?**

Converting an object into a sequence of bytes.

---

### **5. Why should we use try-with-resources?**

It automatically closes streams and prevents resource leaks.

---

# **14. Summary (Easy to Remember)**

- Streams = flow of data
    
- Byte streams = binary
    
- Character streams = text
    
- Buffered streams = fast
    
- Bridge streams = convert byte ↔ character
    
- Object streams = serialize objects
    
- Always close streams (use try-with-resources)
    

---