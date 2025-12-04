# **1. What are Nested and Inner Classes?**

**Nested classes** are **classes defined within another class** in Java.

Purpose:

- Group classes logically.
    
- Increase encapsulation.
    
- Improve readability and maintainability.
    

**Types of Nested Classes:**

1. **Static Nested Class** – declared with `static` keyword.
    
2. **Inner Class** – non-static nested class:
    
    - Member Inner Class
        
    - Local Inner Class
        
    - Anonymous Inner Class
        

---

# **2. Static Nested Class**

- Declared with the `static` keyword.
    
- Can access **static members** of the outer class directly.
    
- Cannot access **non-static members** of outer class directly.
    
- **Independent** of outer class instance.
    

**Syntax:**

```java
class Outer {
    static int data = 30;

    static class StaticNested {
        void display() {
            System.out.println("Data: " + data);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.StaticNested nested = new Outer.StaticNested();
        nested.display();
    }
}
```

**Key Points:**

- Requires **outer class name** to create an object.
    
- Can have **static members** inside.
    

---

# **3. Inner Classes (Non-static Nested Classes)**

Inner classes are associated with an **instance of the outer class**. They can access **both static and non-static members** of the outer class.

## **3.1 Member Inner Class**

- Declared inside a class but outside any method.
    
- Can access all members of outer class.
    

```java
class Outer {
    private int data = 10;

    class Inner {
        void msg() {
            System.out.println("Data: " + data);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.msg();
    }
}
```

**Key Points:**

- Needs **outer class instance** to create inner class object.
    
- Can access **private members** of outer class.
    

---

## **3.2 Local Inner Class**

- Defined **inside a method** of outer class.
    
- **Scope limited** to the method.
    
- Can access **final or effectively final local variables** of method.
    

```java
class Outer {
    void display() {
        final int num = 100;

        class LocalInner {
            void msg() {
                System.out.println("Number: " + num);
            }
        }

        LocalInner inner = new LocalInner();
        inner.msg();
    }
}

public class Main {
    public static void main(String[] args) {
        Outer o = new Outer();
        o.display();
    }
}
```

**Key Points:**

- Not accessible outside method.
    
- Used for short-lived, specific tasks.
    

---

## **3.3 Anonymous Inner Class**

- **No class name**.
    
- Defined **at the point of instantiation**.
    
- Useful for **implementing interfaces or extending classes** in a concise way.
    

```java
interface Hello {
    void greet();
}

public class Main {
    public static void main(String[] args) {
        Hello h = new Hello() {
            public void greet() {
                System.out.println("Hello from Anonymous Class");
            }
        };
        h.greet();
    }
}
```

**Key Points:**

- Can access **final or effectively final variables** from enclosing scope.
    
- Cannot have explicit constructors (because no class name).
    

---

# **4. Differences Between Nested and Inner Classes**

|Feature|Static Nested Class|Inner Class|
|---|---|---|
|Static Keyword|Yes|No|
|Access Outer Members|Only static members|All members|
|Requires Outer Instance|No|Yes|
|Can have static members|Yes|No (except constants)|
|Associated With|Class|Object|

---

# **5. Use Cases**

1. **Logical grouping of classes**: Inner classes are used when a class is used only by one outer class.
    
2. **Event handling in GUI**: Anonymous inner classes are widely used for event listeners.
    
3. **Encapsulation**: Inner classes help hide classes from outside access.
    
4. **Helper classes**: Nested classes can be helper utilities for outer class.
    

---

# **6. Best Practices**

- Use **static nested class** if inner class does not need access to instance members.
    
- Use **inner class** if it needs access to outer class instance variables.
    
- Use **anonymous inner class** for simple, one-time use.
    
- Avoid overusing inner classes to prevent **code complexity**.
    

---

# **7. Example: All Types Together**

```java
class Outer {
    private static int staticData = 50;
    private int instanceData = 100;

    static class StaticNested {
        void display() { System.out.println("Static data: " + staticData); }
    }

    class MemberInner {
        void display() { System.out.println("Instance data: " + instanceData); }
    }

    void method() {
        final int localVar = 10;

        class LocalInner {
            void display() { System.out.println("Local variable: " + localVar); }
        }

        LocalInner li = new LocalInner();
        li.display();
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.StaticNested sn = new Outer.StaticNested();
        sn.display();

        Outer outer = new Outer();
        Outer.MemberInner mi = outer.new MemberInner();
        mi.display();

        outer.method();
    }
}
```

---

# **8. Summary**

- Nested classes are classes **inside another class**.
    
- **Static nested class** = belongs to class, cannot access instance members.
    
- **Inner class** = belongs to object, can access all outer members.
    
- **Local inner class** = defined inside a method.
    
- **Anonymous inner class** = no name, used for one-time use.
    
- **Benefits**: encapsulation, code readability, logical grouping, event handling.
    

---

I can next prepare the **full detailed note for Static and Final Classes**, which comes after Nested and Inner Classes in your OOP section.

Do you want me to continue with that?