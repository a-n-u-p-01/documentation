File handling is a common backend requirement used in:

- Profile picture uploads
    
- Document storage
    
- Invoice downloads
    
- Report generation
    
- Media services
    

Interviewers often ask this to evaluate your **real-world backend skills**, because it involves:

- HTTP understanding
    
- Streaming
    
- Memory management
    
- Security
    

---

# 1. How File Transfer Works Over HTTP

Files are transferred using specific content types.

### Upload uses:

```
multipart/form-data
```

### Download uses:

```
application/octet-stream
```

(or specific types like PDF, image, etc.)

---

# 2. File Upload in Spring Boot

Spring provides built-in support via **Multipart handling**.

You typically receive files using:

```
MultipartFile
```

---

## Basic Upload Example

```java
@PostMapping("/upload")
public String uploadFile(
        @RequestParam("file") MultipartFile file) {

    System.out.println(file.getOriginalFilename());
    return "Uploaded successfully";
}
```

---

## What MultipartFile Provides

Important methods:

|Method|Purpose|
|---|---|
|getOriginalFilename()|File name|
|getSize()|File size|
|getContentType()|MIME type|
|getBytes()|File data|
|transferTo()|Save file|

---

## Saving File to Disk (Common Approach)

```java
@PostMapping("/upload")
public String upload(
        @RequestParam MultipartFile file) throws IOException {

    Path path = Paths.get("uploads/" + file.getOriginalFilename());

    Files.write(path, file.getBytes());

    return "Uploaded";
}
```

Better approach:

```java
file.transferTo(path);
```

More efficient for large files.

---

# 3. File Size Configuration (VERY Important)

Always restrict upload size.

Otherwise server memory can crash.

### application.properties

```
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
```

Production must-have.

---

# 4. Handling Multiple Files

```java
@PostMapping("/uploadMultiple")
public String upload(
        @RequestParam MultipartFile[] files) {

    for(MultipartFile file : files){
        // store each
    }

    return "Uploaded";
}
```

Used in:

- Document bundles
    
- Media apps
    

---

# 5. File Download in Spring Boot

Downloads require proper headers.

Goal:

> Tell the browser this is a file — not JSON.

---

## Basic Download Example

```java
@GetMapping("/download")
public ResponseEntity<Resource> downloadFile()
        throws IOException {

    Path path = Paths.get("uploads/report.pdf");

    Resource resource = new UrlResource(path.toUri());

    return ResponseEntity.ok()
            .header("Content-Disposition",
                    "attachment; filename=report.pdf")
            .body(resource);
}
```

---

## Critical Header

```
Content-Disposition: attachment
```

Forces browser download instead of displaying.

Without it → file may open inline.

---

# 6. Content Type Matters

Example:

```
application/pdf
image/png
text/csv
```

Set it explicitly for better client behavior.

```java
.contentType(MediaType.APPLICATION_PDF)
```

If unknown:

```
application/octet-stream
```

Safe default.

---

# 7. Streaming Large Files (Advanced & Important)

Never load huge files fully into memory.

Bad:

```
byte[] file
```

Good:

Use streaming resources.

Example:

```
InputStreamResource
```

Benefits:

- Lower memory usage
    
- Better performance
    
- Supports large downloads
    

Used in production systems.

---

# 8. Upload Storage Strategies

## Local Disk

Easy but not scalable.

Good for:

- Small apps
    
- Internal tools
    

---

## Cloud Storage (Industry Standard)

Examples:

- AWS S3
    
- Google Cloud Storage
    
- Azure Blob
    

Benefits:

- Highly scalable
    
- Durable
    
- CDN-ready
    

Most production systems avoid local storage.

---

# 9. Security Best Practices (VERY HIGH VALUE)

## Never Trust File Names

Attackers can upload:

```
../../systemfile
```

Always sanitize.

---

## Validate File Type

Check MIME type.

Reject executables unless required.

---

## Limit File Size

Prevents denial-of-service attacks.

---

## Scan for Malware

Critical for enterprise apps.

---

## Store Outside Application Directory

Avoid exposing internal paths.

---

# 10. Common Developer Mistakes

### Loading Large Files into Memory

Causes OutOfMemory errors.

---

### No File Size Limits

Server crash risk.

---

### Not Validating Content Type

Security vulnerability.

---

### Using Original Filename Directly

Can overwrite files.

Generate unique names instead.

Example:

```
UUID + filename
```

---

# 11. Upload vs Download — Quick Table

|Feature|Upload|Download|
|---|---|---|
|Content Type|multipart/form-data|application/octet-stream|
|Key Class|MultipartFile|Resource|
|Risk|Malware|Data exposure|
|Memory Concern|Yes|Yes|

---

# 12. High-Probability Interview Questions

### How does Spring handle file uploads?

Using MultipartFile with multipart/form-data.

---

### Why limit file size?

To prevent memory exhaustion.

---

### How do you force download?

Content-Disposition header.

---

### Best way to serve large files?

Streaming (Resource / InputStream).

---

### Should files be stored locally in production?

Usually no — prefer cloud storage.

---

# Quick Memory Summary

```
Upload → MultipartFile
Download → Resource
Large files → Stream
Always → Validate & Limit
```

### Golden Rule:

> Never trust user-uploaded files.

---

# Final Takeaway

File handling tests whether you think like a **production backend engineer**, not just a CRUD developer.

Mastering this shows:

- Security awareness
    
- Performance understanding
    
- Infrastructure knowledge
    

### Professional Guideline:

> Validate files, limit sizes, stream large data, and prefer cloud storage for scalability.