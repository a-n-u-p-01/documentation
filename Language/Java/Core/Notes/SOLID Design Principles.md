# SOLID Principles – Full Note

SOLID is a set of five object-oriented design principles that guide how classes, modules, and dependencies should be structured.

They are not language rules.  
They are not framework features.

They exist to solve one main engineering problem:

“How do we design software so that change is cheap, safe, and localized?”

Large systems fail not because of syntax, but because:

• one change breaks many parts  
• classes become untestable  
• teams block each other  
• features require rewrites instead of extensions

SOLID directly targets these failures.

---

## 1. Single Responsibility Principle (SRP)

Definition  
A class should have only one reason to change.

Real meaning  
A class should be responsible to only one business policy, one actor, or one category of change.

SRP is not about “number of methods”.  
SRP is about “number of independent reasons this class can be modified”.

Problem SRP solves  
When different kinds of logic are mixed, unrelated changes affect the same class, creating:

• fragile code  
• merge conflicts  
• ripple-effect bugs  
• large untestable units

Example of violation (mixed responsibilities)

```java
class EmployeeManager {

    public double calculateMonthlySalary(Employee e) {
        return e.getBaseSalary() + calculateBonus(e) - calculateTax(e);
    }

    private double calculateBonus(Employee e) {
        return e.getPerformanceScore() * 1000;
    }

    private double calculateTax(Employee e) {
        return e.getBaseSalary() * 0.18;
    }

    public void printReport(Employee e) {
        System.out.println("Name: " + e.getName());
        System.out.println("Salary: " + calculateMonthlySalary(e));
    }

    public String exportCsv(Employee e) {
        return e.getName() + "," + calculateMonthlySalary(e);
    }
}
```

This class changes when:

• salary policy changes  
• tax law changes  
• report format changes

Three unrelated forces. One class. High risk.

SRP-compliant design

```java
class SalaryService {

    public double calculateMonthly(Employee e) {
        return e.getBaseSalary() + calculateBonus(e);
    }

    public double calculateYearly(Employee e) {
        return calculateMonthly(e) * 12;
    }

    private double calculateBonus(Employee e) {
        return e.getPerformanceScore() * 1000;
    }
}
```

```java
class TaxService {

    public double calculateMonthly(Employee e) {
        return e.getBaseSalary() * 0.18;
    }

    public double calculateYearly(Employee e) {
        return calculateMonthly(e) * 12;
    }

    public boolean isTaxable(Employee e) {
        return e.getBaseSalary() > 25000;
    }
}
```

```java
class EmployeeReportService {

    public void print(Employee e, double salary, double tax) {
        System.out.println(buildHeader(e));
        System.out.println("Salary: " + salary);
        System.out.println("Tax: " + tax);
    }

    public String buildHeader(Employee e) {
        return "Employee Report for " + e.getName();
    }

    public String exportCsv(Employee e, double salary, double tax) {
        return e.getName() + "," + salary + "," + tax;
    }
}
```

Now:

SalaryService changes only when salary policy changes  
TaxService changes only when tax rules change  
EmployeeReportService changes only when reporting needs change

That is SRP.

Engineering benefit

• changes are localized  
• tests are small and focused  
• parallel development becomes safe  
• system becomes modular

---

## 2. Open / Closed Principle (OCP)

Definition  
Software entities should be open for extension, but closed for modification.

Real meaning  
You should be able to add new behavior without rewriting existing tested code.

Problem OCP solves

Without OCP, systems evolve like this:

• every new feature edits old classes  
• risk grows exponentially  
• regression bugs increase  
• code becomes fragile

Violation example

```java
class NotificationService {
    public void send(String type, String msg) {
        if (type.equals("EMAIL")) { }
        else if (type.equals("SMS")) { }
        else if (type.equals("PUSH")) { }
    }
}
```

Every new notification type requires modifying this class.

SRP is already weak here. OCP is broken.

OCP-compliant design

```java
interface Notification {
    void send(String message);
}
```

```java
class EmailNotification implements Notification {
    public void send(String message) { }
}

class SmsNotification implements Notification {
    public void send(String message) { }
}
```

```java
class NotificationService {
    public void notify(Notification notification, String msg) {
        notification.send(msg);
    }
}
```

Now:

New type = new class  
No change to NotificationService

Engineering benefit

• old code stays stable  
• new features are isolated  
• testing scope is limited  
• system grows instead of mutates

OCP is mainly achieved through:

• abstraction  
• polymorphism  
• composition

---

## 3. Liskov Substitution Principle (LSP)

Definition  
Subtypes must be substitutable for their base types.

Real meaning  
Inheritance must preserve behavior, not just method signatures.

If code expects a parent, giving it a child should not surprise or break it.

Problem LSP solves

Wrong inheritance creates:

• runtime failures  
• hidden bugs  
• defensive checks everywhere  
• broken polymorphism

Violation example

```java
class Account {
    public void withdraw(double amount) {
        // withdraw allowed
    }
}

class FixedDepositAccount extends Account {
    public void withdraw(double amount) {
        throw new UnsupportedOperationException();
    }
}
```

Now:

```java
Account acc = new FixedDepositAccount();
acc.withdraw(100); // runtime crash
```

This inheritance lies.

Good design

```java
interface Withdrawable {
    void withdraw(double amount);
}
```

```java
abstract class Account {
    public abstract double balance();
}
```

```java
class SavingsAccount extends Account implements Withdrawable {
    public void withdraw(double amount) { }
    public double balance() { return 0; }
}
```

```java
class FixedDepositAccount extends Account {
    public double balance() { return 0; }
}
```

Now no class promises behavior it cannot fulfill.

Engineering benefit

• inheritance becomes safe  
• polymorphism works correctly  
• fewer runtime surprises  
• cleaner domain models

LSP is about contract correctness.

---

## 4. Interface Segregation Principle (ISP)

Definition  
Clients should not be forced to depend on methods they do not use.

Real meaning  
Large general interfaces should be split into small role-based interfaces.

Problem ISP solves

Fat interfaces cause:

• meaningless implementations  
• fragile dependencies  
• forced recompilations  
• fake or empty methods

Violation example

```java
interface Worker {
    void code();
    void test();
    void design();
    void manage();
}
```

A junior developer is forced to implement manage().

ISP-compliant design

```java
interface Coder {
    void code();
}

interface Tester {
    void test();
}

interface Designer {
    void design();
}

interface Manager {
    void manage();
}
```

Now each class implements only what it truly supports.

Engineering benefit

• no fake methods  
• loose coupling  
• flexible role composition  
• safer API evolution

---

## 5. Dependency Inversion Principle (DIP)

Definition  
High-level modules should not depend on low-level modules.  
Both should depend on abstractions.

Real meaning  
Business logic must not be tied to technical details.

Problem DIP solves

Without DIP:

• database choice spreads everywhere  
• testing becomes painful  
• replacing infrastructure is expensive  
• architecture becomes rigid

Violation example

```java
class OrderService {
    private MySqlOrderRepository repo = new MySqlOrderRepository();
}
```

OrderService is controlled by database choice.

DIP-compliant design

```java
interface OrderRepository {
    void save(Order order);
}
```

```java
class MySqlOrderRepository implements OrderRepository {
    public void save(Order order) { }
}
```

```java
class OrderService {
    private OrderRepository repo;

    public OrderService(OrderRepository repo) {
        this.repo = repo;
    }
}
```

Now business logic depends on abstraction.

Engineering benefit

• easy testing (mocking)  
• replaceable infrastructure  
• framework independence  
• clean architecture layers

DIP is the foundation of Spring Dependency Injection.

---

## How SOLID shapes real backend systems

SRP → clean service, domain, repository layers  
OCP → plugin-like feature growth  
LSP → safe polymorphic models  
ISP → stable APIs and microservices  
DIP → testable, decoupled architecture

Without SOLID, microservices and modular monoliths fail.

---

## Final mental model

SRP controls “who can change this class”  
OCP controls “how features grow”  
LSP controls “truth of inheritance”  
ISP controls “API boundaries”  
DIP controls “dependency direction”

SOLID is not about writing more classes.  
It is about controlling change.

---

If you want next, I can turn this into:

• Spring Boot architecture mapping  
• interview-grade answers  
• system design patterns from SOLID  
• or a single evolving project refactored using all five.

Just tell me which one you want.