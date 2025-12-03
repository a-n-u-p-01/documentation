Loops are used to **execute a block of code repeatedly** until a condition is satisfied. Java provides multiple loop constructs for different scenarios.

#### **while Loop**

Executes a block **while a condition is true**. Condition is checked **before execution** (pre-test loop).

```java
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}
```

**Key Points:**

- Condition evaluated before each iteration.
    
- May not execute if the condition is false initially.
    
- Can cause infinite loop if increment/update is missing.
    

---

## **do-while Loop**

Executes a block **at least once**, then checks the condition (post-test loop).

```java
int i = 0;
do {
    System.out.println(i);
    i++;
} while (i < 5);
```

**Key Points:**

- Executes at least once, even if the condition is false.
    
- Useful when the loop body must run before the condition check.
    

---

## **for Loop**

Includes **initialization, condition, and update** in a single line.

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
```

**Key Points:**

- Initialization runs once; update runs after each iteration.
    
- All components are optional: `for(; i<5;) { ... }`
    
- Can create infinite loops if condition is omitted or always true.
    

---

## **Enhanced for Loop (for-each)**

Used for **arrays and collections** to iterate **without using an index**.

```java
int[] arr = {1, 2, 3, 4};
for (int num : arr) {
    System.out.println(num);
}
```

**Key Points:**

- Cannot modify collection size during iteration.
    
- Supports arrays, List, Set; for Map use `entrySet()`.
    
- Read-only iteration is safe and concise.
    

---

## **break Statement**

Exits the **current loop immediately**. Can be used with labels for **nested loops**.

```java
for (int i = 0; i < 5; i++) {
    if (i == 3) break;
    System.out.println(i); // prints 0,1,2
}
```

---

## **continue Statement**

Skips the **current iteration** and moves to the next.

```java
for (int i = 0; i < 5; i++) {
    if (i == 2) continue;
    System.out.println(i); // prints 0,1,3,4
}
```

---

## **Labeled break/continue**

Useful in **nested loops** to control outer loop.

```java
outer:
for(int i=0;i<3;i++){
    for(int j=0;j<3;j++){
        if(i==1 && j==1) break outer;
        System.out.println(i + " " + j);
    }
}
```

---

## **Common Pitfalls**

- Forgetting `break` in switch or nested loops → **fall-through**.
    
- Modifying collection inside enhanced for-loop → **ConcurrentModificationException**.
    
- Off-by-one errors with arrays.
    
- NullPointerException when iterating over null collections.
    
- Using ` == ` instead of `.equals()` for object comparison.
    

---

## **Interview Points / Tricky Cases**

- **Difference between for, while, and do-while:**
    
    - `for` → known iteration count.
        
    - `while` → pre-test loop.
        
    - `do-while` → executes at least once.
        
- **Infinite loop examples:**
    

```java
while(true) { }
for(;;) { }
```

- **Tricky behavior without braces:**
    

```java
for(int i=0;i<5;i++)
    if(i==3) continue;
    System.out.println(i);
```

- Prints **0,1,2,3,4** because `continue` only applies to the if statement.
    
- **Removing elements in enhanced for-loop:** Use `Iterator.remove()`; direct modification throws exception.
    
- **Nested loops with labels:** Allow breaking/continuing outer loops cleanly.
    
