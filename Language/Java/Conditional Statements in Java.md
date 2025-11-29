Conditional statements are used to **control the flow of execution** based on boolean conditions. Java provides several types of conditional constructs.

---

## **1. if Statement**

- Executes a block if the condition is `true`.
    

```java
if (condition) {
    // code to execute
}
```

**Example:**

```java
int age = 18;
if (age >= 18) {
    System.out.println("Adult");
}
```

---

## **2. if-else Statement**

- Executes one block if true, another if false.
    

```java
if (condition) {
    // true block
} else {
    // false block
}
```

**Example:**

```java
int age = 16;
System.out.println((age >= 18) ? "Adult" : "Minor");
```

---

## **3. if-else-if Ladder**

- For **multiple mutually exclusive conditions**.
    

```java
if (cond1) { }
else if (cond2) { }
else { }
```

**Example:**

```java
int marks = 85;
if (marks >= 90) System.out.println("A+");
else if (marks >= 75) System.out.println("A");
else System.out.println("Fail");
```

---

## **4. Nested if**

- `if` inside another `if` for finer control.
    

```java
if (age >= 18) {
    if (weight > 50) {
        System.out.println("Eligible to donate blood");
    }
}
```

---

## **5. switch Statement**

- Used for **discrete values**: `int`, `char`, `String` (Java 7+), enums.
    

```java
switch (expression) {
    case value1: // code; break;
    case value2: // code; break;
    default: // code;
}
```

**Notes:**

- `break` prevents **fall-through**.
    
- Java 14+ allows **switch expressions** returning values using `->`.
    

---

## **6. Ternary Operator (`? :`)**

- Short-hand **if-else** for assignments.
    

```java
variable = (condition) ? valueIfTrue : valueIfFalse;
```

**Example:**

```java
int age = 20;
String status = (age >= 18) ? "Adult" : "Minor";
```

---

## **7. Best Practices**

- Always use braces `{}` for clarity.
    
- Use `switch` for discrete values, `if-else` for ranges.
    
- Use ternary only for simple assignments.
    
- Avoid deeply nested if-else; consider methods.
    

---

## **8. Common Pitfalls**

- Forgetting `break` in switch → unintended fall-through.
    
- Using ` == `  with Strings instead of `.equals()`.
    
- Null in switch (Strings) → `NullPointerException`.
    

---

## **Interview Questions**

**Difference Between if-else and switch**

- `if-else` → evaluates **conditions/ranges**, `switch` → **discrete values only**.
    
- `switch` can use `int`, `char`, `String`, enums; not `long`, `float`, `double`, `boolean`.
    

**Ternary Operator vs if-else**

- Ternary → concise, returns value, suitable for single-line assignment.
    
- if-else → more flexible for multiple statements.
    

**Tricky Questions**

- Can switch have multiple labels?
    

```java
case 1, 2, 3 -> System.out.println("1-3");
```

- Can we switch on null string?
    
    - No, throws `NullPointerException`.
        
- What does this print?
    

```java
int x = 5;
System.out.println((x > 5) ? "A" : (x == 5) ? "B" : "C"); // B
```

- Difference between nested if and if-else-if ladder?
    
    - Ladder evaluates top to bottom, executes first true branch.
        
    - Nested if executes inner if **only if outer condition is true**.
        
- How does short-circuit in if-else prevent NPE?
    

```java
if(obj != null && obj.isValid()) { ... } // safe
```

---