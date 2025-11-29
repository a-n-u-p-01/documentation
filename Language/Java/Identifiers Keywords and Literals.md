#### ✅ **1. Identifiers**

Identifiers are **names** used to identify variables, methods, classes, packages, interfaces, and other user-defined items in Java.

##### ✔ Rules for Identifiers

1. Must start with:
    
    - a letter (A–Z or a–z)
        
    - a currency character (`$`)
        
    - an underscore (`_`)
        
2. Subsequent characters can be:
    
    - letters, digits, `$`, `_`
        
3. Cannot start with a digit.
    
4. Cannot be a Java **keyword**.
    
5. Case-sensitive (`age`, `Age`, `AGE` are different).
    
6. No spaces allowed.
    
7. Unlimited length.
    

### ✔ Valid vs Invalid Examples

|Valid|Invalid|Reason|
|---|---|---|
|`myVar`|`2value`|cannot start with a digit|
|`_count`|`my var`|contains a space|
|`$price`|`class`|keyword|
|`totalMarks1`|`@sum`|invalid character `@`|

---

## ✅ **2. Keywords**

Keywords are **reserved words** that have predefined meaning in Java. They cannot be used as identifiers.

Java has **67 keywords** (including literals like `true`, `false`, `null`).

### ✔ Commonly Used Keywords (Grouped)

#### **Access Modifiers**

```
public, private, protected
```

#### **Class & Object Related**

```
class, interface, enum, extends, implements, abstract, final, static
```

#### **Control Flow**

```
if, else, switch, case, default
for, while, do, break, continue
return
```

#### **Exception Handling**

```
try, catch, finally, throw, throws
```

#### **Memory & Threading**

```
new, synchronized, volatile, transient
```

#### **Miscellaneous**

```
package, import, super, this, instanceof, assert
```

---

## 🔍 **Keywords You Cannot Use as Identifiers**

Examples:

```java
int class = 10; // ❌ ERROR
String for = "Hi"; // ❌ ERROR
```

---

## ✅ **3. Literals**

Literals are **fixed constant values** used in the code.

### ✔ Types of Literals in Java

#### **1. Integer Literals**

```java
int a = 10;
int b = 0b1010;    // binary
int c = 012;       // octal
int d = 0xA;       // hexadecimal
```

#### **2. Floating-Point Literals**

```java
float x = 10.5f;
double y = 20.99;
```

#### **3. Character Literals**

```java
char c1 = 'A';
char c2 = '\n';      // escape sequence
char c3 = '\u0041';  // Unicode
```

#### **4. String Literals**

```java
String s = "Hello";
```

#### **5. Boolean Literals**

```java
boolean flag = true;
```

#### **6. Null Literal**

```java
String name = null;
```

---

## 🔍 **Summary Table**

|Concept|Meaning|Notes|
|---|---|---|
|**Identifier**|User-defined name|cannot use keywords|
|**Keyword**|Reserved Java word|predefined purpose|
|**Literal**|Constant value in code|numbers, strings, booleans, etc.|

---

## ✔ Quick Interview Questions

1. **Can an identifier start with `$` or `_`?**  
    Yes.
    
2. **Are keywords usable as variable names?**  
    No.
    
3. **Is `true` a keyword?**  
    It is a **literal**, not a keyword, but still _reserved_.
    
4. **Is `null` a keyword?**  
    No — it’s a **literal**.
    
5. **Difference between `'A'` and `"A"`?**  
    `'A'` → char literal  
    `"A"` → String literal
    

---

If you want, I can also generate:  
✅ practice MCQs  
✅ coding exercises  
✅ flashcards for revision  
Just tell me!