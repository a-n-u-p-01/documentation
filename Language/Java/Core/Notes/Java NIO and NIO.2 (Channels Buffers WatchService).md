Java NIO (New I/O) is a modern I/O system designed for **fast, scalable, non-blocking, buffer-oriented data handling**.  
NIO.2 (Java 7) improves file handling with **Path, Files, WatchService**.

This note breaks everything down clearly.

---

# **1. What is Java NIO?**

Java NIO replaces the old Stream-based I/O with:

- **Buffers** → data stored temporarily
    
- **Channels** → connection to file/network
    
- **Selectors** → handle many channels with one thread
    

NIO is **non-blocking**, works with **buffers**, and is faster for large files and network servers.

---

# **2. Buffers**

A **Buffer** is a block of memory used to store data during I/O operations.

Common buffers:

- ByteBuffer
    
- CharBuffer
    
- IntBuffer
    
- DoubleBuffer
    

### Why Buffer?

Streams send data one-by-one.  
Buffers store data in chunks → much faster.

### Basic example

```java
ByteBuffer buffer = ByteBuffer.allocate(1024);
buffer.put((byte) 10);
buffer.flip();
byte b = buffer.get();
```

### Important buffer operations:

- **put()** → write into buffer
    
- **get()** → read
    
- **flip()** → prepare buffer for reading
    
- **clear()** → prepare for writing again
    
- **rewind()** → read again from start
    

### Buffer internal fields:

- **capacity** → total size
    
- **position** → next read/write index
    
- **limit** → max readable/writable index
    

These fields control how data moves.

---

# **3. Channels**

A **Channel** transfers data between a buffer and an I/O source like:

- File
    
- Socket
    
- Datagram (UDP)
    
- Pipe
    

Channels are **bidirectional** (can read and write), unlike streams.

### Common channel classes:

- FileChannel
    
- SocketChannel
    
- ServerSocketChannel
    
- DatagramChannel
    

### Simple FileChannel read example

```java
FileChannel channel = new FileInputStream("data.txt").getChannel();
ByteBuffer buffer = ByteBuffer.allocate(100);
channel.read(buffer);
buffer.flip();
channel.close();
```

### FileChannel write example

```java
FileChannel channel = new FileOutputStream("output.txt").getChannel();
ByteBuffer buffer = ByteBuffer.wrap("Hello".getBytes());
channel.write(buffer);
channel.close();
```

---

# **4. Selectors (Non-Blocking I/O)**

Selectors allow **one thread** to manage **multiple channels**.

This is how modern servers handle:

- thousands of connections
    
- without blocking
    
- without creating thousands of threads
    

### What a Selector does:

- Checks if a channel is ready to read
    
- Checks if ready to write
    
- Checks if a connection is ready
    

This avoids busy-waiting.

### When to use a Selector?

When building scalable servers like:

- Chat applications
    
- Web servers
    
- Multiplayer game servers
    

---

# **5. NIO.2 – Path & Files API (Java 7)**

NIO.2 improved file handling with modern APIs.

---

## **Path**

Represents a file or folder path.

```java
Path path = Paths.get("folder/data.txt");
```

Better than `File` because:

- More methods
    
- Works on Windows/Linux/Mac
    
- Supports symbolic links
    
- Supports path operations
    

---

## **Files**

Utility class with powerful file operations.

### Common operations:

```java
Files.exists(path);
Files.copy(src, dest);
Files.move(src, dest);
Files.delete(path);
List<String> lines = Files.readAllLines(path);
Files.write(path, lines);
```

### Why Files API is better?

- Shorter code
    
- Better exception messages
    
- Works with symbolic links
    
- Supports file permissions
    

---

# **6. WatchService (Directory Monitoring)**

WatchService allows your program to "listen" for file system changes.

### Events it can detect:

- File created
    
- File modified
    
- File deleted
    

### Example

```java
WatchService ws = FileSystems.getDefault().newWatchService();
Path path = Paths.get("logs");

path.register(ws,
    StandardWatchEventKinds.ENTRY_CREATE,
    StandardWatchEventKinds.ENTRY_MODIFY,
    StandardWatchEventKinds.ENTRY_DELETE);

WatchKey key = ws.take();
```

Used for:

- Auto-refresh features
    
- Log monitoring
    
- Real-time backup systems
    
- File sync tools
    

---

# **7. Channels vs Streams (Very Important)**

|Streams|Channels|
|---|---|
|One-way (read OR write)|Two-way (read + write)|
|Slow (byte-by-byte)|Fast (buffer-based)|
|Blocking|Non-blocking supported|
|Old I/O|Modern I/O|
|Works on simple tasks|Best for large files & network|

---

# **8. Complete Example – File Copy Using NIO**

```java
Path source = Paths.get("input.txt");
Path target = Paths.get("output.txt");

Files.copy(source, target);
```

Zero effort required. Internally very fast.

---

# **9. Complete Example – Reading a File**

```java
Path path = Paths.get("notes.txt");
List<String> lines = Files.readAllLines(path);

for (String line : lines) {
    System.out.println(line);
}
```

---

# **10. Complete Example – WatchService Loop**

```java
WatchService ws = FileSystems.getDefault().newWatchService();
Path path = Paths.get("folder");

path.register(ws, 
    StandardWatchEventKinds.ENTRY_CREATE,
    StandardWatchEventKinds.ENTRY_MODIFY,
    StandardWatchEventKinds.ENTRY_DELETE);

while (true) {
    WatchKey key = ws.take();

    for (WatchEvent<?> event : key.pollEvents()) {
        System.out.println(event.kind() + ": " + event.context());
    }

    key.reset();
}
```

---

# **11. Interview Questions + Answers**

### **1. What is the main difference between IO and NIO?**

IO is stream-based and blocking.  
NIO is buffer-based and supports non-blocking operations.

---

### **2. What is a Buffer?**

A memory block that stores data for reading/writing. Faster than streams.

---

### **3. Why is flip() needed in Buffers?**

After writing into the buffer, flip() switches it into read mode.

---

### **4. What is a Channel?**

A bidirectional connection to a file or network used to read/write data via buffers.

---

### **5. What is a Selector and why is it used?**

Selector lets one thread handle many channels → used in scalable servers.

---

### **6. What is Path in NIO.2?**

A modern replacement for `File`, supports more operations and works across platforms.

---

### **7. What is WatchService?**

A service that monitors directories for changes (create/modify/delete).

---

### **8. Why NIO is faster than traditional IO?**

Uses buffers and channels + supports non-blocking operations.

---

# **12. Summary**

- **NIO** uses Buffers + Channels for fast, scalable I/O
    
- **Selectors** help build multi-connection servers
    
- **NIO.2** introduces Path, Files, WatchService
    
- Ideal for large files, real-time monitoring, network operations
    
---