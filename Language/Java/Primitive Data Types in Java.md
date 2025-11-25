Java provides **8 primitive data types** which store **simple and raw values** directly in memory (not objects).  
They are the **building blocks** of all data manipulation in Java and are stored in **stack memory** (except when inside objects).

---

#### 🔥 **Why Primitive Types? (Concept Explanation)**

- They provide **fast performance** because operations happen directly on values.
    
- They require **less memory**, unlike objects.
    
- They avoid overhead of object creation.
    
- JVM uses primitives extensively for optimization.
    

---

#### 📌 **1. Integer Types**

#### **a) byte**

- **Size**: 1 byte (8 bits)
    
- **Range**: –128 to 127 (2⁷ to 2⁷–1)
    
- **Usage**: Useful in **large arrays**; saves memory.
    
- **Example**:
    
    ```java
    byte age = 25;
    ```
    

---

#### **b) short**

- **Size**: 2 bytes
    
- **Range**: –32,768 to 32,767
    
- **Usage**: Rare; used in memory-constrained systems.
    
- **Example**:
    
    ```java
    short marks = 30000;
    ```
    

---

#### **c) int**

- **Size**: 4 bytes
    
- **Range**: –2,147,483,648 to 2,147,483,647
    
- **Default type for integers**
    
- **Most commonly used numeric type**
    
- **Example**:
    
    ```java
    int salary = 50000;
    ```
    

---

#### **d) long**

- **Size**: 8 bytes
    
- **Range**: Huge… (±9 quintillion)
    
- Must use **L** suffix
    
- Used when **int is not enough**
    
- Example:
    
    ```java
    long population = 7800000000L;
    ```
    

---

### 📌 **2. Floating Point Types**

#### **a) float**

- **Size**: 4 bytes
    
- Stores 6–7 decimal digits
    
- Must use **f** suffix
    
- Rarely used (double preferred)
    
- Example:
    
    ```java
    float temperature = 36.6f;
    ```
    

---

#### **b) double**

- **Size**: 8 bytes
    
- Stores 15–16 decimal digits
    
- **Default** type for decimals
    
- Used in scientific calculations
    
- Example:
    
    ```java
    double price = 12345.6789;
    ```
    

---

### 📌 **3. Character Type**

#### **char**

- **Size**: 2 bytes (**Unicode**)
    
- Can store any character, including symbols & languages
    
- Example:
    
    ```java
    char letter = 'A';
    char symbol = '#';
    ```
    

---

### 📌 **4. Boolean Type**

#### **boolean**

- **Size**: JVM-dependent (logically 1 bit)
    
- Stores **true** or **false**
    
- Used in decision-making
    
- Example:
    
    ```java
    boolean isJavaEasy = true;
        ```
    

---

#### 🔍 **Default Values of Primitive Types**

|Type|Default Value|
|---|---|
|byte|0|
|short|0|
|int|0|
|long|0L|
|float|0.0f|
|double|0.0d|
|char|'\u0000'|
|boolean|false|

>IMP:  Default values apply **only for instance variables**, not local variables.

---

#### 🔥 **Key Technical Points (Useful for Interviews)**

###### Primitives are **not objects**
They do NOT inherit from the `Object` class.
###### Stored in **stack memory**
(Except when used inside objects—still separate from object headers.)
- Local primitive → Stack
    
- Primitive inside object → Heap (as part of object memory)
###### Faster than wrapper classes
`int` is faster than `Integer`.
###### Autoboxing & Unboxing
Java automatically converts:  
`int → Integer` (boxing)  
`Integer → int` (unboxing)

---

#### **Example with All Primitive Types**

```java
byte b = 10;
short s = 200;
int i = 50000;
long l = 2000000000L;

float f = 12.34f;
double d = 12.3456789;

char ch = 'J';
boolean flag = true;

System.out.println(i + d);
```

---

#### 📝 **Interview Questions (Most Important)**

##### **1. Why does Java have 8 primitive data types?**
To provide fast performance and memory efficiency.
##### **2. What is the default type of integer and decimal literals?**
- Integer → **int**
    
- Decimal → **double**
##### **3. Why is `char` 2 bytes in Java?**
Because Java uses **Unicode** (not ASCII) to support international languages.
##### **4. Can a boolean be converted to an int?**
No. They are **not compatible**.
##### **5. Why must we write `L` with long and `f` with float?**
To tell the compiler the literal type explicitly.
##### **6. What is the default value of local variables?**
**Local variables have no default values** — they must be initialized.
##### **7. What is the difference between float and double?**
- float = 4 bytes → lower precision
    
- double = 8 bytes → higher precision
##### **8. What is Unicode? Why does Java use it?**
Unicode is a universal character encoding standard that supports all languages.  
Java uses it for **global compatibility**.
