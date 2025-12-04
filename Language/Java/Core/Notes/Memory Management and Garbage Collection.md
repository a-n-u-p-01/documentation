Java manages memory **automatically** using the **JVM**, which helps prevent memory leaks and improves stability. Understanding memory management is crucial for writing **efficient and high-performance Java applications**.

---

## **Java Memory Model (Heap and Stack)**

Java memory is divided into several runtime areas managed by the JVM:

**Heap Memory**

- Stores **objects and arrays**.
    
- Shared across threads.
    
- Subject to Garbage Collection.
    
- Divided into:
    
    - **Young Generation**: new objects; collected frequently.
        
        - **Eden Space**: new objects allocated here.
            
        - **Survivor Spaces (S0/S1)**: objects that survive minor GC.
            
    - **Old (Tenured) Generation**: long-lived objects.
        
    - **Permanent Generation / Metaspace (Java 8+)**: class metadata, method info.
        

**Stack Memory**

- Stores **local variables and method call frames**.
    
- Each thread has its own stack.
    
- Automatically cleaned when method ends.
    

**Other areas**

- **Method Area / Metaspace**: class structures, constants, static variables.
    
- **PC Registers**: program counter per thread.
    
- **Native Memory**: for JVM internals, native libraries, buffers.
    

---

## **Garbage Collection (GC)**

Garbage Collector automatically **reclaims memory** occupied by objects no longer referenced.

**How GC Works**

- JVM identifies **unreachable objects** (no reference pointing to them).
    
- Reclaims memory and adds it back to the heap.
    

**Reachability Concept**

- **Strong Reference**: normal reference → object not eligible for GC.
    
- **Soft Reference**: object may be collected under memory pressure.
    
- **Weak Reference**: object collected more aggressively.
    
- **Phantom Reference**: allows post-mortem cleanup.
    

---

## **GC Algorithms / Collectors**

**Young Generation GC (Minor GC)**

- Runs frequently.
    
- Collects short-lived objects in **Eden space**.
    
- Surviving objects move to **Survivor spaces** or **Old Generation**.
    

**Old Generation GC (Major / Full GC)**

- Collects long-lived objects.
    
- Runs less frequently; **more expensive**.
    

**Garbage Collector Types in HotSpot JVM**

**Serial GC**

- Single-threaded. Good for small apps.
    

**Parallel GC**

- Multi-threaded GC, optimized for throughput.
    

**CMS (Concurrent Mark Sweep) GC**

- Minimizes pause time for old generation.
    

**G1 (Garbage First) GC**

- Divides heap into regions, collects regions with most garbage first.
    

**ZGC / Shenandoah (Java 11+)**

- Low-pause, large heap support.
    

---

## **Finalize Method**

- `finalize()` is called before GC destroys an object.
    
- Can be overridden but **not recommended**.
    
- Unreliable and can delay GC.
    
- Replaced by **try-with-resources, PhantomReference, Cleaner API**.
    

```java
@Override
protected void finalize() throws Throwable {
    System.out.println("Object is being collected");
}
```

---

## **Memory Leaks in Java**

A **memory leak in Java** occurs when objects are **no longer needed by the application** but **still remain referenced**, preventing the Garbage Collector (GC) from reclaiming their memory. Over time, this can **consume heap space**, degrade performance, and eventually lead to an **OutOfMemoryError**.

Even with GC, memory leaks can occur due to:

- Holding references in **static collections**.
    
- Caching objects without eviction.
    
- Listeners or callbacks not removed.
    
- Thread-local variables not cleared.
    

**Tips to avoid leaks:**

- Nullify unused references.
    
- Use **weak references** for caches.
    
- Use profiling tools: **VisualVM, JProfiler, YourKit**.
    

---

## **Best Practices for Memory Management**

- Minimize unnecessary object creation.
    
- Prefer **primitive types** over wrapper objects when possible.
    
- Use **StringBuilder** instead of concatenating strings in loops.
    
- Use **try-with-resources** for streams to release native memory.
    
- Avoid **large object graphs** with circular references; GC can handle cycles, but it's expensive.
    

---

## **GC Tuning**

- JVM flags allow tuning memory and GC behavior:
    

```bash
-Xms256m        # initial heap size
-Xmx1024m       # maximum heap size
-XX:+UseG1GC    # use G1 garbage collector
-XX:MaxGCPauseMillis=200
```

- Monitor GC with:
    
    - `-verbose:gc`
        
    - `-XX:+PrintGCDetails`
        
    - `-XX:+PrintGCDateStamps`
        

---

## **Interview Questions**

- Difference between Heap and Stack memory.
    
- Difference between Minor GC, Major GC, and Full GC.
    
- Explain Young Generation, Old Generation, and Survivor Space.
    
- What is a memory leak in Java? Can GC cause memory leaks?
    
- Difference between **strong, soft, weak, and phantom references**.
    
- Difference between **Serial, Parallel, CMS, G1, ZGC**.
    
- Why should we avoid using `finalize()`?
    
- How does **String constant pool** help memory efficiency?
    
- What is `OutOfMemoryError` vs `StackOverflowError`?
    
- How do you monitor memory usage in Java applications?
    

---