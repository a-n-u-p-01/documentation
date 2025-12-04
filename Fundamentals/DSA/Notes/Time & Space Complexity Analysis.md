## **Time & Space Complexity Analysis**

### **1. What is Complexity Analysis?**

- **Time Complexity:** Measures how **fast** an algorithm runs as the input size grows.
    
- **Space Complexity:** Measures how much **memory** an algorithm uses as the input size grows.
    

> Goal: Optimize both time and space for efficiency.

---

### **2. Time Complexity**

- Depends on the number of **basic operations** executed.
    
- Measured as a function of **input size `n`**.
    

**Common Time Complexities:**

|Complexity|Example|
|---|---|
|O(1)|Accessing array element|
|O(log n)|Binary search|
|O(n)|Linear search|
|O(n log n)|Merge Sort, Quick Sort|
|O(n²)|Bubble Sort, Nested loops|
|O(2^n)|Recursive Fibonacci|

---

### **3. Space Complexity**

- Total memory used = Input + Auxiliary space (extra memory used by algorithm).
    
- Example:
    
    - Iterative Fibonacci → O(1) space
        
    - Recursive Fibonacci → O(n) space (due to recursion stack)
        

---

### **4. Big O, Big Θ, Big Ω (Overview)**

- **Big O (O):** Worst-case complexity
    
- **Big Ω (Ω):** Best-case complexity
    
- **Big Θ (Θ):** Average-case (tight bound)
    

---

### **5. Why It Matters**

- Helps choose the **right algorithm** for large inputs.
    
- Prevents **time limit exceeded** or **memory overflow** in coding problems.
    
- Guides **optimization** in real-world applications.
    

---

### **Quick Tips**

- Nested loops → usually O(n²)
    
- Consecutive loops → usually O(n + m)
    
- Recursive calls → check **recursion tree** to find complexity
    
- Don’t ignore **hidden constants**, but in Big O we simplify them
    

---