## **Operators in Java**

Operators are symbols that perform operations on variables and values. Java operators can be classified based on functionality:

### **Arithmetic Operators**

- `+`, `-`, `*`, `/`, `%`
    
- Operate on numeric types (`int`, `long`, `float`, `double`, `short`, `byte`).
    
- **Integer division truncates decimals**; floating-point division preserves them.
    
- `%` gives remainder; works with negatives but follows IEEE rules:
    

```java
System.out.println(-5 % 3); // -2
```

- Floating-point `%` also works:
    

```java
System.out.println(5.5 % 2.0); // 1.5
```

### **Unary Operators**

- `+` and `-` for sign, `++`/`--` for increment/decrement (pre and post).
    
- `!` negates boolean.
    
- `~` inverts bits (bitwise NOT).
    
- Pre vs Post increment/decrement:
    

```java
int i = 5;
int j = i++ + ++i; // careful: evaluates to 5 + 7 = 12
```

### **Assignment Operators**

- Simple: =
    
- Compound: `+=, -=, *=, /=, %=, &=, |=, ^=, <<=, >>=, >>>=`
    
- Compound assignments **implicitly cast**:
    

```java
byte b = 5;
b += 1; // works
b = b + 1; // needs explicit cast
```

### **Relational and Logical Operators**

- Relational: `<, <=, >, >=, ==, !=`
    
- Logical: `&&, ||, !` (short-circuit)
    
- Non-short-circuit: `&` and `|` always evaluate both sides; useful for side effects.
    

### **Bitwise and Shift Operators**

- Bitwise: `& | ^ ~`
    
- Shifts: `<<` (left), `>>` (signed right), `>>>` (unsigned right)
    
- Common in low-level programming, hashing, and cryptography.
    

### **Ternary Operator**

- `condition ? expr1 : expr2`
    
- Compact, can return a value. Avoid nesting too much.
    

```java
int max = (a > b) ? a : b;
```

### **instanceof Operator**

- Checks type of object; supports pattern matching in Java 14+:
    

```java
if (obj instanceof String s) {
    System.out.println(s.length());
}
```

### **Operator Precedence**

- Operators have defined precedence.
    
- Parentheses improve readability and prevent mistakes.
    

---

## **Control Structures in Java**

Control structures define the flow of program execution. Java supports selection, iteration, branching, and exception handling structures.

### **Selection Statements**

**if / else if / else**

- Basic conditional execution.
    
- Short-circuiting avoids unnecessary evaluation:
    

```java
if (obj != null && obj.isValid()) { ... }
```

- Avoid deeply nested ifs; prefer early returns or strategy pattern.
    

**switch (Statements and Expressions)**

- Pre-Java 14: supports `int, byte, short, char, enum, String`
    
- Java 14+: switch expressions allow arrows and `yield`:
    

```java
int result = switch (value) {
    case 1 -> 100;
    case 2 -> 200;
    default -> { log("fallback"); yield -1; }
};
```

- No implicit fall-through unless `break` is omitted intentionally.
    
- Clean alternative to multiple if-else.
    

---

### **Iteration Statements**

**for loop**

- Used when index control is required:
    

```java
for (int i = 0; i < n; i++) { ... }
```

**Enhanced for loop**

- Iterates over arrays or collections:
    

```java
for (String s : list) { ... }
```

- Cannot modify the underlying collection while iterating.
    
- No index access.
    

**while loop**

- Executes while a condition is true; good for indefinite loops:
    

```java
while (condition) { ... }
```

**do-while loop**

- Executes at least once:
    

```java
do { ... } while (condition);
```

**break, continue, labels**

- `break` exits a loop or switch.
    
- `continue` skips current iteration.
    
- Labels allow breaking/continuing outer loops:
    

```java
outer:
for (...) {
    for (...) {
        if (condition) break outer;
    }
}
```

---

### **Exception Handling Control Flow**

**try-catch-finally**

- Directs flow when exceptions occur.
    

```java
try {
    riskyOperation();
} catch(IOException e) {
    handle(e);
} finally {
    cleanup();
}
```

**try-with-resources**

- Automatic resource management for `AutoCloseable` objects:
    

```java
try (BufferedReader br = new BufferedReader(new FileReader(file))) {
    ...
}
```

- Ensures deterministic closure, cleaner than manual `finally`.
    

---

## **Best Practices for Developers**

- Always use parentheses for clarity in complex expressions.
    
- Prefer enhanced for loops for read-only iteration.
    
- Use `switch` expressions for readability and returning values.
    
- Avoid mutating collections during iteration; use `Iterator` if needed.
    
- Short-circuit logical operators (`&&, ||`) can prevent NPEs.
    
- Be careful with pre/post increment operators inside expressions—they can be subtle in complex code.
    

---

**Difference between ` == ` and `.equals()` for objects**

- ` == ` checks **reference equality** (same memory).
    
- `.equals()` checks **logical equality** (content), usually overridden in classes like `String`.
    

**Numeric promotion in mixed-type arithmetic**

- Operands are promoted to the **largest type**: `byte/short → int → long → float → double`.
    
- Example: `int + double → double`.
    

**Why `b += 1` compiles but `b = b + 1` doesn’t**

- `b += 1` **implicitly casts** the result to `byte`.
    
- `b = b + 1` results in `int` → needs explicit cast: `b = (byte)(b + 1)`.
    

**Difference between `>>` and `>>>`**

- `>>` → signed right shift (preserves sign).
    
- `>>>` → unsigned right shift (fills with 0).
    

**Pre-increment vs post-increment in expressions**

- Pre (`++i`) → increments **before** use.
    
- Post (`i++`) → increments **after** use.
    

**`&` vs `&&`, `|` vs `||` differences and use cases**

- `&` and `|` → bitwise OR/AND (evaluates both operands).
    
- `&&` and `||` → logical AND/OR (short-circuits, safe for null checks).
    

**Tricky Operator Question**

- What does this print?
    

```java
int i = 1;
System.out.println(i++ + ++i); // ?
```

**Answer:** 1 + 3 → 4

---

## **Control Structures**

**Difference between old switch statement and Java 14+ switch expression**

- Old: `case` + `break`, no return value.
    
- New: arrow syntax (`case 1 ->`) and can **return values**, supports multi-label `case 1,2 ->`.
    

**When to avoid enhanced-for loop**

- When **modifying the collection** (remove/add elements) or need **index access**.
    

**How short-circuit evaluation can prevent NullPointerException**

```java
if(obj != null && obj.isValid()) { ... } // safe
```

- `obj.isValid()` is executed **only if obj != null**.
    

**Use cases for labeled break/continue**

- Breaking nested loops early:
    

```java
outer:
for(...) {
  for(...) {
    if(condition) break outer;
  }
}
```

**Difference between while and do-while**

- `while` → may not execute if condition is false.
    
- `do-while` → executes **at least once**.
    

**Why try-with-resources is preferred over regular try-finally**

- Automatic resource closure, cleaner, handles exceptions in `close()` safely.
    

---

### **Tricky / Good Additional Questions**

**Difference between ` == ` and `equals()` for `Integer` cache**

```java
Integer a = 127, b = 127;
System.out.println(a == b); // true
Integer x = 128, y = 128;
System.out.println(x == y); // false
```

- Explanation: JVM caches `-128` to `127`.
    

**What happens if you modify a list inside enhanced-for?**

- Throws `ConcurrentModificationException`.
    

**Difference between `break` and `return` inside loops**

- `break` → exits loop only
    
- `return` → exits **method entirely**
    

**Can a switch accept `null`?**

- Only `String` switches allow `null` in Java 7+, throws `NullPointerException`.
    

**Difference between `++i + i++` vs `i++ + ++i`**

- Tricky expressions to test **order of evaluation**; interview favorite.
    

---