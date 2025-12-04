## **Big O, Big Θ, Big Ω Notation**

### **1. Why Do We Use Them?**

- To describe **how the runtime or space of an algorithm grows** with input size `n`.
    
- Provides a **mathematical way to compare efficiency** of algorithms, independent of machine or programming language.
    

---

### **2. Big O (O) – Upper Bound**

- Represents the **worst-case scenario** of an algorithm.
    
- Guarantees the algorithm **won’t be slower** than this bound.
    
- Example:
    
    - Linear search in an array of size `n` → O(n)
        
    - Accessing element in array → O(1)
        

**Formula:**

> T(n) = O(f(n)) ⇒ T(n) ≤ c × f(n) for large n, some constant c > 0

---

### **3. Big Ω (Ω) – Lower Bound**

- Represents the **best-case scenario**.
    
- Guarantees the algorithm **won’t be faster** than this bound.
    
- Example:
    
    - Best case of linear search (element found at first position) → Ω(1)
        

**Formula:**

> T(n) = Ω(f(n)) ⇒ T(n) ≥ c × f(n) for large n, some constant c > 0

---

### **4. Big Θ (Θ) – Tight Bound**

- Represents the **average or exact growth rate**.
    
- The algorithm takes **roughly f(n) time** in both best and worst cases.
    
- Example:
    
    - Merge Sort → Θ(n log n)
        

**Formula:**

> T(n) = Θ(f(n)) ⇒ c1 × f(n) ≤ T(n) ≤ c2 × f(n) for large n, constants c1, c2 > 0

---

### **5. Quick Summary Table**

|Notation|Meaning|Best / Worst Case Example|
|---|---|---|
|O(f(n))|Upper bound|Worst case of linear search → O(n)|
|Ω(f(n))|Lower bound|Best case of linear search → Ω(1)|
|Θ(f(n))|Tight bound|Merge Sort → Θ(n log n)|

---

### **6. Key Points**

- Big O is **most commonly used** in interviews & practice.
    
- Focus on **dominant terms**; ignore constants (O(2n) → O(n)).
    
- Helps in **algorithm selection** for large inputs.
    

---