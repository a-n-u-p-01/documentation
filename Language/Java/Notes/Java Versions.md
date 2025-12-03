# **Java Version Features and Differences**

Java has evolved rapidly, adding major **language features, libraries, JVM improvements**, and API enhancements in every version. Below is a detailed version-wise summary.

---

## **1. Java 1.0 (1996)**

- **First official release of Java.**
    
- Key Features:
    
    - Applets for web-based GUI.
        
    - AWT (Abstract Window Toolkit) for GUI.
        
    - Basic I/O (`java.io` package).
        
    - Core packages: `java.lang`, `java.util`.
        
    - Platform independence via JVM.
        
- Limitations:
    
    - No Collections Framework.
        
    - No Exception hierarchy improvements.
        
    - No JDBC.
        

---

## **2. Java 1.1 (1997)**

- Introduced **inner classes** (non-static nested classes).
    
- JavaBeans for component architecture.
    
- JDBC (Java Database Connectivity) for database interaction.
    
- Reflection API (basic capability).
    
- RMI (Remote Method Invocation) for distributed computing.
    
- Event model improvement in AWT.
    

---

## **3. Java 1.2 (1998) – “Java 2”**

- **Collections Framework** introduced.
    
- Swing GUI toolkit added (`javax.swing`).
    
- JIT compiler improvements for performance.
    
- Java Plug-in for browser applets.
    
- `java.util` package improved with `HashMap`, `HashSet`, `ArrayList`.
    
- Enumerations not yet part of language.

---

## **4. Java 1.3 (2000)**

- HotSpot JVM became standard.
    
- Java Sound API added.
    
- RMI over IIOP (interoperable CORBA support).
    
- JNDI (Java Naming and Directory Interface) updates.
    
- Performance improvements in garbage collection.
    

---

## **5. Java 1.4 (2002)**

- **Exception chaining** added (`Throwable.initCause()`).
    
- Logging API (`java.util.logging`).
    
- Image I/O API for image file handling.
    
- Regular expressions (`java.util.regex` package).
    
- Assertions for runtime checks (`assert` keyword).
    
- NIO (New I/O) introduced for file and buffer operations.
    

---

## **6. Java 5 / 1.5 (2004)**

- **Generics** for type safety.
    
- Enhanced for-loop (`for-each` loop).
    
- Autoboxing/unboxing for primitives and wrapper classes.
    
- Enum types (`enum` keyword).
    
- Varargs (`...`) for variable-length argument methods.
    
- Metadata annotations: `@Override`, `@Deprecated`, `@SuppressWarnings`.
    
- Static imports (`import static`).
    
- `java.util.concurrent` package (basic classes like `ConcurrentHashMap`, `Executors`).
    

---

## **7. Java 6 (2006)**

- Scripting API (`javax.script`) to run JavaScript code via Nashorn.
    
- JDBC 4.0 improvements: auto-loading drivers, new exception classes.
    
- JAX-WS for web services.
    
- Compiler API (`javax.tools`).
    
- Monitoring and management APIs (JMX improvements).
    
- Pluggable annotations processing.
    

---

## **8. Java 7 (2011)**

- Try-with-resources statement (`AutoCloseable`).
    
- Diamond operator (`<>`) for type inference.
    
- Strings in `switch` statements.
    
- Fork/Join framework for parallelism.
    
- NIO.2 (`java.nio.file`) for file I/O operations.
    
- Binary literals and underscores in numeric literals.
    
- Catch multiple exceptions in a single `catch` block.
    

---

## **9. Java 8 (2014)**

- Lambda expressions (`(args) -> expression`) for functional programming.
    
- Functional interfaces (`@FunctionalInterface`).
    
- Stream API (`java.util.stream`) for collection processing.
    
- Optional class for null-safety (`java.util.Optional`).
    
- Default and static methods in interfaces.
    
- New Date and Time API (`java.time`) replacing `java.util.Date`.
    
- Nashorn JavaScript engine integration.
    
- Parallel streams for multicore utilization.
    

---

## **10. Java 9 (2017)**

- Module system (Project Jigsaw): `module-info.java`.
    
- JShell: interactive REPL for Java.
    
- `private` methods in interfaces.
    
- Immutable collections (`List.of()`, `Set.of()`, `Map.of()`).
    
- Multi-release JAR files.
    
- Stream API improvements: `takeWhile()`, `dropWhile()`, `ofNullable()`.
    

---

## **11. Java 10 (2018)**

- Local-variable type inference (`var`) for declarations.
    
- Application class-data sharing.
    
- Parallel full GC for G1.
    
- `Optional.orElseThrow()` no-argument method.
    
- Improved Docker container awareness in JVM.
    

---

## **12. Java 11 (2018, LTS)**

- New String methods:
    
    - `lines()`, `isBlank()`, `strip()`, `repeat()`.
        
- `var` in lambda parameters.
    
- HTTP Client API standardized (`java.net.http`).
    
- Running single-file source-code programs (`java File.java`).
    
- Removal of Java EE and CORBA modules.
    
- Flight Recorder and low-overhead monitoring.
    

---

## **13. Java 12 (2019)**

- Switch expressions (preview) for simpler `switch`.
    
- `JVM Constants API`.
    
- Shenandoah low-pause-time garbage collector (experimental).
    
- String `transform()` method.
    
- Compact number formatting.
    

---

## **14. Java 13 (2019)**

- Text blocks preview (multi-line strings) using `"""`.
    
- Dynamic CDS archives.
    
- Switch expressions preview enhancements.
    

---

## **15. Java 14 (2020)**

- Records (preview) for immutable data classes.
    
- Pattern matching for `instanceof` (preview).
    
- Helpful NullPointerExceptions (`java.lang.NullPointerException` gives variable name).
    
- `switch` expressions continue preview.
    
- NVM improvements.
    

---

## **16. Java 15 (2020)**

- Sealed classes (preview) for controlled inheritance.
    
- Hidden classes for frameworks (dynamic JVM class generation).
    
- Text blocks finalized.
    
- Pattern matching for `instanceof` continues preview.
    
- ZGC low-latency garbage collector improvements.
    

---

## **17. Java 16 (2021)**

- Records finalized.
    
- Pattern matching for `instanceof` finalized.
    
- Vector API incubator for SIMD operations.
    
- Sealed classes continue preview.
    
- Stream.toList() method for immutable collections.
    

---

## **18. Java 17 (2021, LTS)**

- Sealed classes finalized.
    
- Pattern matching for `switch` (preview).
    
- Strong encapsulation of JDK internals.
    
- Foreign Function & Memory API (incubator).
    
- Deprecation/removal of older APIs like Applet.
    

---

## **19. Java 18 (2022)**

- Simple web server API (`SimpleFileServer`) for testing.
    
- UTF-8 charset by default.
    
- Code snippets in documentation (`@snippet`).
    
- Vector API incubator improvements.
    
- Pattern matching preview enhancements.
    

---

## **20. Java 19 (2022)**

- Virtual threads (Project Loom, preview) for lightweight concurrency.
    
- Structured concurrency (incubator).
    
- Record patterns (preview).
    
- Pattern matching enhancements.
    
- Foreign Function API improvements.
    

---

## **21. Java 20 (2023)**

- Pattern matching for `switch` (preview).
    
- Record patterns and sealed interface enhancements.
    
- Structured concurrency API improvements.
    
- Virtual threads continued preview.
    
- Vector API incubation continues.
    

---

## **22. Java 21 (2023, LTS)**

- Virtual threads finalized.
    
- Scoped values (incubator) for structured data sharing across threads.
    
- Record patterns and pattern matching for `switch` finalized.
    
- Structured concurrency finalized (incubator).
    
- Strong encapsulation and modern memory management improvements.
    
- Standardization of newer language APIs for modern development.
    

---

### **Key Takeaways**

- LTS (Long-Term Support) versions: **Java 8, 11, 17, 21**.
    
- Java evolves in **language features, libraries, concurrency, and JVM optimizations**.
    
- Modern Java heavily uses **streams, lambda expressions, modules, and virtual threads**.
    
- Knowing version differences is **essential for interviews, compatibility, and project planning**.
    

---