### **1. File Handling (File, Path, Files API)**

**Purpose:**  
Java I/O allows reading from and writing to files, directories, and other data streams. The `java.io` and `java.nio.file` packages are commonly used.

---

#### **1.1 java.io.File Class**

- Represents **files and directories** in the filesystem.
    
- Can be used to **create, delete, or query** file properties.
    

**Common Methods:**

```java
File file = new File("example.txt");

// Check existence
file.exists();

// Create a new file
file.createNewFile();

// Delete a file
file.delete();

// Get file info
file.getName();
file.getAbsolutePath();
file.length();
file.isFile();
file.isDirectory();
```

**Notes:**

- `File` does not read/write data; it only represents file paths.
    
- Works on both **absolute** and **relative paths**.
    

---

#### **1.2 java.nio.file.Path and Paths**

- Introduced in Java 7 (NIO.2) for better file handling.
    
- `Path` represents **file/directory paths**.
    
- `Paths.get("file.txt")` creates a Path object.
    
- Supports operations like resolving, relativizing, and normalization.
    

**Example:**

```java
Path path = Paths.get("example.txt");

// Convert to absolute path
Path absolutePath = path.toAbsolutePath();

// Check existence
Files.exists(path);

// Delete file
Files.deleteIfExists(path);
```

---

#### **1.3 java.nio.file.Files Utility Class**

- Provides **high-level methods** for file operations.
    
- Supports reading/writing, copying, moving, deleting, and creating directories/files.
    

**Common Methods:**

```java
// Read all lines
List<String> lines = Files.readAllLines(path);

// Write lines to file
Files.write(path, lines, StandardOpenOption.CREATE, StandardOpenOption.APPEND);

// Copy a file
Files.copy(sourcePath, targetPath, StandardCopyOption.REPLACE_EXISTING);

// Move a file
Files.move(sourcePath, targetPath, StandardCopyOption.REPLACE_EXISTING);
```

**Advantages over java.io.File:**

- Better error handling with exceptions.
    
- Supports **atomic operations**.
    
- Works efficiently with large files.
    

---

#### **1.4 Directory Handling**

- `Files.createDirectory(path)` → Creates a single directory.
    
- `Files.createDirectories(path)` → Creates nested directories if not exist.
    
- `Files.list(path)` → Streams all entries in a directory.
    

---

#### **1.5 Key Points**

- **java.io.File**: Legacy, represents path info only.
    
- **java.nio.file.Path & Files**: Modern API, more flexible and efficient.
    
- Always handle exceptions (`IOException`) when dealing with file operations.
    
- Prefer **NIO.2 API** for new applications.
    

---
