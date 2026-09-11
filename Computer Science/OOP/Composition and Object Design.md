# Composition & Object Design

The four pillars get you started; **composition** and **design principles** keep you sane. "Prefer composition over inheritance" is the most quoted OOP guideline, and SOLID is the checklist that catches design rot early. This note is the bridge from "writes classes" to "designs object systems."

**The Intuition:** Inheritance is coupling — a subclass inherits everything, wanted or not. Composition is wiring — a `Car` *contains* an `Engine` and delegates to it. Wiring is easier to change than ancestry. The SOLID principles are diagnostics: when code feels rigid, one of the five is usually being violated. Learn to name the smell and the fix suggests itself.

## Composition — has-a over is-a

```java
// INHERITANCE: rigid, inherits everything
class Dog extends Animal { ... }

// COMPOSITION: flexible, contains what it needs
class Dog {
    private AnimalBehavior behavior;   // delegate behavior to a component
    private Bark bark;                 // pluggable parts
}
```

**When composition wins:**
1. **Swap parts at runtime** — a `Printer` with a swappable `PrintEngine`
2. **No inherited baggage** — composition takes only what you need
3. **Testing** — replace a component with a mock easily
4. **Multiple concerns** — a class can compose many collaborators but extend one parent

## The delegation pattern

```java
// Compose, then DELEGATE to the component:
class Printer {
    private PrintEngine engine = new LaserEngine();   // the part

    public void print(String doc) {
        engine.render(doc);          // delegate — Printer doesn't know HOW
    }
    public void setEngine(PrintEngine newEngine) {   // swap at runtime
        engine = newEngine;
    }
}
```
The `Printer` *is-a* device that prints (via delegation) but isn't forced to *be* an engine.

## Coupling & cohesion — the two quality dials

| | High | Low |
|--|------|-----|
| **Cohesion** (how related a class's parts are) | Good — one responsibility | Bad — class does many things |
| **Coupling** (how much classes depend on each other) | Bad — changes ripple | Good — changes are local |

**Goal:** *high cohesion, low coupling.* A `Report` class that generates, formats, saves, and emails has low cohesion (four jobs) and high coupling (four reasons to change). Split it.

## SOLID — the five diagnostics

**S — Single Responsibility:** one class, one reason to change.
```java
// BAD: Report generates + saves + emails
class Report { void generate(); void save(); void email(); }
// GOOD: split into Report, ReportSaver, EmailService
```

**O — Open/Closed:** open for extension, closed for modification.
```java
// BAD: adding a shape edits the if-chain
double area(Object s) { if (s instanceof Circle) ... else if ... }
// GOOD: new Shape subclass — no edits
interface Shape { double area(); }
```

**L — Liskov Substitution:** subclasses must honor the parent's contract (Square/Rectangle violation).

**I — Interface Segregation:** small, specific interfaces.
```java
// BAD: Robot forced to implement eat()/sleep()
interface Worker { void work(); void eat(); void sleep(); }
// GOOD: split — Workable, Feedable, Sleepable
```

**D — Dependency Inversion:** depend on abstractions, not concretions (constructor injection).

## Constructor injection — the D in practice

```java
// BAD — hard-wired dependency:
class OrderService {
    private EmailSender sender = new SmtpSender();   // can't test, can't swap

// GOOD — injected through the constructor:
class OrderService {
    private final EmailSender sender;                // depends on abstraction

    public OrderService(EmailSender sender) {        // caller supplies it
        this.sender = sender;
    }
}
// Tests: new OrderService(new MockSender());
// Prod:  new OrderService(new SmtpSender());
// Swap:  new OrderService(new QueuedSender());
```

## The Law of Demeter — talk to friends, not strangers

```java
// BAD — reaching through objects ("train wreck"):
order.getCustomer().getAddress().getCity().getName();

// GOOD — ask the object you already have:
order.getCustomerCity();
// Each method asks its own collaborator, one level deep.
// Reduces coupling: a change in Address doesn't ripple to every caller.
```

---

**Setup:** Refactor an if/instanceof chain into polymorphism + composition.

**Solution:**
```java
// BEFORE:
class Report {
    String format() {
        if (formatType == "PDF") return "pdf...";
        if (formatType == "HTML") return "<html>...";
        throw new IllegalArgumentException();
    }
}

// AFTER — composition with a strategy:
interface ReportFormatter {
    String format(Report r);
}
class PdfFormatter implements ReportFormatter { ... }
class HtmlFormatter implements ReportFormatter { ... }

class Report {
    private ReportFormatter formatter;            // injected component
    String format() { return formatter.format(this); }   // delegate
}
```

**Key insight:** The if-chain became a *strategy* — each format is a component. Adding a CSV format = new class + inject it; `Report` doesn't change (Open/Closed). This is the Strategy pattern, which is really "composition + polymorphism."

---

**Setup:** Apply Interface Segregation to a kitchen robot.

**Solution:**
```java
// BAD: one fat interface — every robot implements what it can't do:
interface Robot { void cook(); void clean(); void dance(); }
class CookingRobot implements Robot {
    public void cook() { ... }
    public void clean() { throw new UnsupportedOperationException(); }  // smell!
    public void dance() { throw new UnsupportedOperationException(); }
}

// GOOD: role interfaces — implement only what you do:
interface Cookable { void cook(); }
interface Cleanable { void clean(); }
class CookingRobot implements Cookable { public void cook() { ... } }
```

**Key insight:** `UnsupportedOperationException` in an implementation is the smoke of interface segregation violation. Split the interface so every implementor's contract matches its actual capability.

---

**Setup:** Choose composition over inheritance for a `FlyingCar`.

**Solution:**
```java
// Inheritance would force a choice — and a lie:
class FlyingCar extends Car { }        // ...but a car can't fly
class FlyingCar extends Plane { }      // ...but a plane can't drive

// Composition models the truth — it HAS both capabilities:
class FlyingCar {
    private Car car = new Car();           // drive behavior
    private Plane plane = new Plane();     // fly behavior

    public void drive() { car.drive(); }
    public void fly() { plane.fly(); }
}
```

**Key insight:** "What IS this thing?" has two answers — so inheritance can't express it. Composition expresses "it HAS both" cleanly. Whenever you're tempted to double-inherit, compose.

---

## Practice (try before peeking)

1. What's the smell that says "switch to composition"?
2. A class with 5 unrelated methods and 4 dependencies — what principle(s) does it violate?
3. `new OrderService(new MockSender())` — which SOLID principle is this?

<details><summary>Answers</summary>

1. Deep inheritance trees, subclasses that inherit behavior they don't use, `UnsupportedOperationException` in overrides, or "is-a" that isn't semantically true.
2. Single Responsibility (too many jobs) and likely Low Cohesion / High Coupling — it should be split.
3. Dependency Inversion — the dependency is injected through the abstraction (`EmailSender`), letting tests swap in a mock.

</details>

---

**Common traps:**
- Interface segregation taken to the extreme (a class per method) — aim for role-sized, not atom-sized
- Constructor injection everywhere including trivial cases — inject at the *boundaries* where swapping matters
- Law of Demeter as a rigid law — the guideline is "one level of indirection," not "never reach"
- Composition that merely wraps without delegating — use it to delegate, not to hide
- Treating SOLID as a scorecard — it's diagnostics; apply where change actually happens (YAGNI elsewhere)

---
