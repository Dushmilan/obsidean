# Abstraction, Interfaces & Abstract Classes

Abstraction is "hide the *how*, expose the *what*." Interfaces and abstract classes are its two OOP tools. An interface is a **pure contract** (no implementation, no state); an abstract class is a **partial blueprint** (some implementation, some contract). Choosing between them is a daily Java design decision.

**The Intuition:** You drive a car without understanding the engine — the steering wheel and pedals are the *interface*. Abstraction in code is the same: expose the operations, hide the machinery. An interface says "anything that implements me can do these things." An abstract class says "here's a partial plan — I've filled in some parts, you supply the rest."

## The contract — what each provides

```java
// INTERFACE — pure contract. No state, no implementation (pre-Java 8).
interface Payment {
    boolean pay(double amount);               // signature only
    // Java 8+: default methods (optional implementation)
    default String describe() { return "payment"; }
    // Fields must be constants:
    double FEE_CAP = 1000;                    // implicitly public static final
}

// ABSTRACT CLASS — partial implementation + contract.
abstract class BankAccount {
    protected double balance;                 // CAN have state
    public BankAccount(double b) { balance = b; }   // CAN have constructors
    public void deposit(double amt) { balance += amt; }   // concrete method
    public abstract void applyInterest();     // abstract — subclasses must fill
}
```

## The key differences

| | Interface | Abstract class |
|--|-----------|----------------|
| State (fields) | ✗ (constants only) | ✓ (instance fields) |
| Constructors | ✗ | ✓ |
| Concrete methods | ✗ (default methods are a partial exception) | ✓ |
| Multiple inheritance | ✓ (implement many) | ✗ (extend one) |
| Semantics | "Can do" — a capability | "Is a" — a partial kind |
| When to use | Defines what a class CAN DO | Defines what a class IS + shares code |

## Multiple interfaces — the diamond question

```java
// Java allows ONE extends + MANY implements:
class DigitalCamera extends Device implements Camera, Networkable {
    // gets Device's fields/behavior
    // must implement Camera and Networkable methods
}

// Why safe: interfaces carry no state — two interfaces can't conflict on data.
// Default methods can clash, but you resolve explicitly:
interface A { default void x() { ... } }
interface B { default void x() { ... } }
class C implements A, B {
    @Override public void x() { A.super.x(); }    // pick one explicitly
}
```

## The classic use — decoupling (Dependency Inversion)

```java
// BAD — the high-level class depends on a concrete low-level detail:
class ReportService {
    private MySqlDatabase db = new MySqlDatabase();   // hard-wired!

    void save() { db.insert(report); }
}
// Testing? Change DB vendor? Both require editing ReportService.

// GOOD — depend on the INTERFACE:
interface Database {
    void insert(Report r);
}
class MySqlDatabase implements Database { ... }
class InMemoryDatabase implements Database { ... }   // for tests

class ReportService {
    private Database db;                       // injected abstraction
    ReportService(Database db) { this.db = db; }
    void save() { db.insert(report); }
}
// Swap implementations at the composition root. Test with InMemory.
```

## Abstract class as template — Template Method pattern

```java
abstract class DataParser {
    // The skeleton — fixed, calls the abstract steps:
    public final void parse() {
        open();
        while (nextLine()) {
            processLine();       // abstract — subclass supplies
        }
        close();
    }

    protected abstract void processLine();   // vary
    protected void open() { ... }            // common
    protected void close() { ... }           // common
}
// Subclasses supply ONLY the varying step.
```

## When to choose which

**Choose an interface when:**
- You define a *capability* ("anything that can do X")
- Multiple unrelated classes share a behavior
- You want polymorphism across an inheritance boundary
- You need to combine several contracts

**Choose an abstract class when:**
- Subclasses share *state* (fields)
- They share *implementation* (concrete methods)
- You need constructors to enforce setup
- It's genuinely "is-a" with partial realization

**Rule of thumb:** Start with an interface. Add an abstract class only when you find yourself duplicating implementation across implementors.

## The full pattern — interface + abstract class

```java
// Best of both — interface as contract, abstract class as convenience base:
interface Shape {
    double area();
}

abstract class AbstractShape implements Shape {
    protected String name;
    public String describe() { return name + ": " + area(); }   // shared logic
}

class Circle extends AbstractShape {
    private double r;
    public Circle(double r) { this.name = "circle"; this.r = r; }
    @Override public double area() { return Math.PI * r * r; }
}
// Callers program against Shape; AbstractShape removes boilerplate.
```

---

**Setup:** Design a notification system open to new channels.

**Solution:**
```java
interface Notifier {
    void send(String message);
}

class EmailNotifier implements Notifier {
    @Override public void send(String m) { /* SMTP */ }
}
class SmsNotifier implements Notifier {
    @Override public void send(String m) { /* SMS API */ }
}
class SlackNotifier implements Notifier {
    @Override public void send(String m) { /* webhook */ }
}

void broadcast(List<Notifier> channels, String msg) {
    for (Notifier n : channels) n.send(msg);
}
```

**Key insight:** `broadcast` depends only on `Notifier`. Adding a channel = new class, zero edits to existing code — the Open/Closed principle and the Strategy pattern in one.

---

**Setup:** An abstract class for accounts with shared state and a varying interest step.

**Solution:**
```java
abstract class Account {
    protected double balance;                    // shared state
    public Account(double b) { balance = b; }    // enforced setup
    public void deposit(double amt) { balance += amt; }   // shared behavior
    public abstract void applyInterest();        // the varying part
}

class Savings extends Account {
    public Savings(double b) { super(b); }
    @Override public void applyInterest() { balance *= 1.04; }
}
class Checking extends Account {
    public Checking(double b) { super(b); }
    @Override public void applyInterest() { balance *= 1.001; }
}
```

**Key insight:** The abstract class holds what's common (field, deposit, constructor) and *forces* each subclass to supply what varies (interest). This is the Template Method pattern — the fixed skeleton with pluggable steps.

---

**Setup:** When is a default method (Java 8+) justified in an interface?

**Solution:** When a capability has a sensible *common* behavior that most implementors won't override:
```java
interface Collection {
    int size();
    boolean isEmpty();                       // each implementor overrides (perf)
    default boolean isNotEmpty() { return !isEmpty(); }   // derived once, shared
}
```
Adding a method to an interface *breaks every implementor* — a `default` method adds it without breaking anyone.

**Key insight:** Default methods let interfaces grow without breaking existing implementors. Use them for *derived conveniences* (build on other interface methods), not for real logic — that's what abstract classes are for.

---

## Practice (try before peeking)

1. Can an interface have a field? A constructor?
2. `class A extends B implements C, D` — how many classes/interfaces, and why is this legal?
3. What's the single best reason to prefer an interface over an abstract class?

<details><summary>Answers</summary>

1. Fields: only `public static final` constants. Constructors: no — interfaces have no instance state to initialize.
2. One class (B) + two interfaces (C, D). Single *class* inheritance prevents the diamond-of-state problem; multiple *interfaces* are safe because they carry no state.
3. Decoupling — code depends on the capability contract, not on any implementation, so implementations swap freely (for production, tests, or new vendors).

</details>

---

**Common traps:**
- Putting real logic in default methods — they're for conveniences, not behavior
- Choosing an abstract class "just to share one method" — consider composition or a utility
- Interface with too many methods — split (Interface Segregation)
- Implementing an interface but not all methods — compile error (unless abstract)
- Confusing `implements` (interface) with `extends` (class) — the classic mix-up

---
