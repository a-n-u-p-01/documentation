Enums (short for **enumerations**) are a special data type that represents a **fixed set of constants**. They are type-safe and provide meaningful names instead of numeric or string values.

---

#### **Basic Enum Declaration**

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

**Usage:**

```java
Day today = Day.MONDAY;

if(today == Day.MONDAY) {
    System.out.println("Start of the week");
}
```

- Enum constants are **public, static, and final** by default.
    
- Enum type is **final**, cannot be extended.
    

---

#### **Enum Methods**

Every enum implicitly extends `java.lang.Enum`. Some useful methods:

- `values()` → returns all enum constants in an array.
    
- `valueOf(String name)` → returns the enum constant with the specified name.
    
- `ordinal()` → returns the position of the constant (0-based).
    
- `name()` → returns the name of the enum constant.
    

**Example:**

```java
for(Day d : Day.values()) {
    System.out.println(d + " at position " + d.ordinal());
}

Day d2 = Day.valueOf("FRIDAY");
System.out.println(d2); // FRIDAY
```

---

#### **Enums with Fields and Methods**

Enums can have **fields, constructors, and methods**.

```java
enum Season {
    WINTER(0), SPRING(15), SUMMER(30), FALL(10);

    private int temperature;

    Season(int temperature) {
        this.temperature = temperature;
    }

    public int getTemperature() {
        return temperature;
    }
}

System.out.println(Season.SUMMER.getTemperature()); // 30
```

- Enum constructors are **private by default**.
    
- Fields and methods can be added like regular classes.
    

---

#### **Switch Statement with Enums**

Enums integrate seamlessly with `switch`:

```java
Day today = Day.WEDNESDAY;

switch(today) {
    case MONDAY, TUESDAY, WEDNESDAY -> System.out.println("Weekdays");
    case THURSDAY, FRIDAY -> System.out.println("Almost Weekend");
    case SATURDAY, SUNDAY -> System.out.println("Weekend");
}
```

- Switch with enums improves **readability and type safety**.
    

---

#### **Advanced Enum Features**

- **Abstract methods in enums:** Each constant can implement abstract methods differently.
    

```java
enum Operation {
    ADD {
        public int apply(int a, int b) { return a + b; }
    },
    SUBTRACT {
        public int apply(int a, int b) { return a - b; }
    };

    public abstract int apply(int a, int b);
}
```

- **Singleton pattern with enum:** Best way to create singletons.
    

```java
enum Singleton {
    INSTANCE;
    public void show() {
        System.out.println("Singleton instance");
    }
}
```

---

#### **Best Practices**

- Use enums instead of **integer constants** or `String` constants.
    
- Use `switch` or `if` statements for enum logic instead of hardcoding values.
    
- Avoid mutable fields in enums unless necessary.
    
- For large enums with behavior differences, consider **abstract methods** inside enum.
    

---

#### **Common Pitfalls**

- Enums cannot extend another class (already extend `java.lang.Enum`).
    
- Enum constructors **cannot be public or protected**.
    
- Be cautious when using ordinal() for persistence or comparison—use **fields instead**.
    

---

#### **Interview Questions**

- Difference between `enum` and `class`?
    
    - Enum is type-safe, final, with predefined constants.
        
    - Cannot extend another class; all enum constants are static and final.
        
- Can enum implement interfaces?
    
    - Yes, enums can implement interfaces.
        
- Difference between `==` and `.equals()` for enums?
    
    - `==` is safe for enums; no need for `.equals()` because enums are singletons.
        
- Can enum have abstract methods?
    
    - Yes, each constant can override it.
        
- Why use enums over `int` constants?
    
    - Type safety, readability, maintainability, and ability to add behavior.
        
- Tricky question: What does `Day.valueOf("FUNDAY")` do?
    
    - Throws `IllegalArgumentException` at runtime.
        
- How is enum singleton better than normal singleton?
    
    - Handles **serialization** and **reflection** issues automatically.