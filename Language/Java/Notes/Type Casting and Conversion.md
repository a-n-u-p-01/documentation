Type casting and conversion in Java allow you to **convert a value of one data type into another**. Java has **strict type rules**, so understanding casting is crucial to avoid data loss or runtime errors.

---

### **1. Two Types of Type Conversion**

##### **A. Implicit Type Conversion (Widening)**

- Also called **type promotion**.
    
- Happens **automatically** when a smaller type is assigned to a larger type.
    
- **Safe:** no data loss.
    
- Follows the **byte → short → int → long → float → double** hierarchy.
    

**Example:**

```java
int i = 100;
long l = i;   // int → long (widening)
float f = l;  // long → float
double d = f; // float → double
```

**Key points:**

- No explicit cast required.
    
- Compiler automatically promotes smaller types.
    
- Happens also in **arithmetic operations** (numeric promotion).
    

---

### **B. Explicit Type Conversion (Narrowing)**

- Also called **type casting**.
    
- Required when converting a **larger type to a smaller type**.
    
- **Risk:** possible **data loss or overflow**.
    
- Use the syntax: `(type) value`.
    

**Example:**

```java
double d = 123.45;
int i = (int) d;  // explicit cast, fractional part lost → 123

long l = 1000L;
short s = (short) l; // explicit cast, may lose data if value > Short.MAX_VALUE
```

**Key points:**

- Always explicit, compiler **does not allow implicit narrowing**.
    
- Can truncate decimal part or overflow integer limits.
    

---

## **2. Casting Between Primitive Types**

|From → To|Implicit Allowed?|Notes|
|---|---|---|
|byte → short, int, long, float, double|✅ Yes|Widening promotion|
|short → int, long, float, double|✅ Yes|Widening promotion|
|int → long, float, double|✅ Yes|Widening promotion|
|long → float, double|✅ Yes|Widening promotion|
|float → double|✅ Yes|Widening promotion|
|double → float, long, int, short, byte|❌ No|Requires explicit cast (data loss)|
|int → short, byte|❌ No|Explicit cast required|

**Example of narrowing:**

```java
int i = 300;
byte b = (byte) i; // 300 mod 256 = 44 (overflow)
```

---

## **3. Casting Between Reference Types (Objects)**

### **A. Upcasting**

- Converting a **subclass reference to a superclass type**.
    
- **Implicit**, always safe.
    

```java
class Animal {}
class Dog extends Animal {}

Dog dog = new Dog();
Animal animal = dog; // upcasting, implicit
```

### **B. Downcasting**

- Converting a **superclass reference to a subclass type**.
    
- **Explicit cast required**, may throw `ClassCastException`.
    

```java
Animal animal = new Dog();
Dog dog = (Dog) animal; // downcasting, explicit
```

**Safe Downcasting Tip:** Use `instanceof` to check before casting:

```java
if (animal instanceof Dog) {
    Dog dog = (Dog) animal;
}
```

---

## **4. Casting Between Wrapper Classes (Autoboxing + Conversion)**

- Java **autoboxes/unboxes** between primitives and wrapper types.
    
- Widening works automatically; narrowing requires explicit cast.
    

```java
Integer i = 100;        // autoboxing int → Integer
int j = i;               // unboxing Integer → int

double d = i;            // unboxing + widening
int x = (int) (double) i; // unboxing + explicit narrowing
```

---

## **5. Type Casting in Expressions**

- **Numeric promotions** occur during arithmetic operations:
    
    - `byte + byte → int`
        
    - `short + short → int`
        
    - Mixed types: promoted to **largest type**
        

```java
byte b1 = 10, b2 = 20;
byte b3 = (byte) (b1 + b2); // addition promoted to int
```

- **Operator precedence** affects result of casts:
    

```java
int x = (int) 3.5 + 2; // 3 + 2 = 5
int y = (int) (3.5 + 2); // 5.5 → 5
```

---

## **6. Special Cases**

### **A. Char and Numeric Conversion**

- `char` is 16-bit unsigned → can be cast to `int`, `long`, `float`, `double`.
    
- Negative values cannot be directly assigned to `char`.
    

```java
char c = 'A';   // 65 in ASCII
int i = c;      // 65, widening
```

### **B. Boolean**

- Cannot be cast to/from numeric types.
    

---

## **7. Common Pitfalls**

- **Overflow in narrowing:** `(byte) 130 → -126`
    
- **Loss of decimal:** `(int) 3.99 → 3`
    
- **Integer division:** `int/int → int` → cast to get float
    

```java
int a = 5, b = 2;
double d = (double) a / b; // 2.5
```

- **Reference type casting:** may cause `ClassCastException` if wrong type.
    

---

## **8. Interview Tips**

- Always remember **implicit widening is safe**, explicit narrowing **may lose data**.
    
- Use **`instanceof` for downcasting** to avoid exceptions.
    
- Be careful with **arithmetic promotion** and **autoboxing/unboxing**.
    
- Never try to cast **boolean to numeric**.
    