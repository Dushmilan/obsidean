# Inheritance & Polymorphism

Inheritance models "is-a" relationships and enables **polymorphism** — the same method call behaving differently by object type. Java uses single inheritance for classes (`extends`) plus interfaces (`implements`). This is where OOP's pillar concepts get their Java mechanics.

**The Intuition:** A `Dog` is an `Animal` — so `Dog extends Animal` inherits everything Animal has and can override what differs. The magic: a variable of type `Animal` can hold a `Dog`, and calling `speak()` dispatches to whatever the object *really* is at runtime (dynamic dispatch). You write code against the general type; the object decides the specifics.

## Basic inheritance

```java
public class Animal {
    protected String name;          // protected: visible to subclasses

    public Animal(String name) {
        this.name = name;
    }

    public void speak() {
        System.out.println("...");
    }
}

public class Dog extends Animal {
    public Dog(String name) {
        super(name);                // must call the parent constructor FIRST
    }

    @Override
    public void speak() {           // override — replace the parent behavior
        System.out.println("Woof!");
    }

    public void fetch() {           // new behavior — Dog-only
        System.out.println("fetching...");
    }
}
```

## Constructor chain — super() is mandatory

```java
// If the parent has NO no-arg constructor, the child MUST call super(...):
public class Dog extends Animal {
    public Dog(String name) {
        super(name);        // first statement, required
    }
}
// The full chain runs: Animal(...) then Dog(...) — parent first.

// this(...) vs super(...) — both must be first statements, so:
// you can call one or the other, never both.
```

## Overriding vs overloading

| | Override | Overload |
|--|----------|----------|
| Relationship | Parent-child | Same class |
| Signature | **Same** signature | Different parameters |
| Purpose | Replace behavior | Multiple forms |
| Keyword | `@Override` (checks!) | — |
| Dispatch | Runtime (polymorphic) | Compile time |

```java
@Override                          // compiler verifies you really override
public void speak() { ... }

public void speak(int times) {     // overload — different signature
    for (int i = 0; i < times; i++) speak();
}
```

## Polymorphism — the payoff

```java
Animal a = new Dog("Rex");
Animal b = new Cat("Tom");

a.speak();     // "Woof!"  — runtime picks Dog's version
b.speak();     // "Meow!"  — runtime picks Cat's version

// Writing code against Animal:
void makeThemTalk(List<Animal> animals) {
    for (Animal animal : animals) {
        animal.speak();        // each speaks its own way — ONE line, N behaviors
    }
}

// Adding a new subclass requires ZERO changes to makeThemTalk
// — that's the Open/Closed principle (see SOLID note)
```

## The instanceof / casting rules

```java
Animal a = new Dog("Rex");

// Upcast — implicit, always safe
Dog d = new Dog("Rex");
Animal a = d;                 // Dog → Animal: implicit

// Downcast — must cast, may fail
if (a instanceof Dog dog) {   // pattern matching instanceof (Java 16+)
    dog.fetch();              // a is a Dog — safe to use
}
// Without the check: ClassCastException at runtime:
// ((Dog) a).fetch();          // works if a IS a Dog

// NEVER:
// Animal a = new Animal("x");
// Dog d = (Dog) a;            // ClassCastException — a is not a Dog
```

## Method resolution rules

1. `this.method()` → the object's runtime class wins (dynamic dispatch)
2. `super.method()` → explicitly the parent's version
3. `static` methods are NOT polymorphic — resolved at compile time by reference type:
```java
Animal a = new Dog("Rex");
a.speak();        // Dog's version (instance → polymorphic)
a.identify();     // Animal's static version — static binds to reference type!
```

## final — stopping inheritance

```java
public final class String { ... }     // String cannot be extended
public final void method() { }        // cannot be overridden
// final class = immutable hierarchy; final method = fixed behavior
```

## Composition over inheritance

```java
// Sometimes a Dog shouldn't extend Animal at all:
class Dog {
    private Animal animal;    // HAS-A instead of IS-A
    private String breed;
}
// Composition is more flexible: swap parts at runtime, no fragile hierarchies.
// The rule of thumb: prefer composition; use inheritance for genuine is-a + reuse.
```

---

**Setup:** Design a payment system where different methods charge differently.

**Solution:**
```java
public class Payment {
    protected double feePercent;

    public Payment(double feePercent) { this.feePercent = feePercent; }

    public double fee(double amount) {
        return amount * feePercent / 100;
    }
}

public class CreditCardPayment extends Payment {
    public CreditCardPayment() { super(2.9); }     // 2.9% fee
}

public class CryptoPayment extends Payment {
    public CryptoPayment() { super(0.5); }         // 0.5% fee
}

// Polymorphic use:
double totalFee(Payment[] methods, double amount) {
    double sum = 0;
    for (Payment p : methods) sum += p.fee(amount);
    return sum;
}
```

**Key insight:** The parent defines the contract (`fee`); subclasses customize the parameters via their constructors. The loop works on any Payment — new types plug in without changing `totalFee`.

---

**Setup:** Why does `@Override` matter?

**Solution:** Without it, a typo creates a *new method* instead of an override:
```java
public class Dog extends Animal {
    public void speek() { ... }      // typo — new method, speak() still Animal's!
}
```
`@Override` makes the compiler *refuse* to compile unless a parent method genuinely matches — the typo becomes a compile error instead of a silent behavior bug.

**Key insight:** `@Override` is a compiler-checked guarantee. The absence of overriding is one of the sneakiest bugs in OOP — it silently falls back to the parent implementation.

---

**Setup:** A square can be a rectangle — should `Square extends Rectangle`?

**Solution:** Classic Liskov trap (see SOLID note). If `Rectangle` has independent `setWidth`/`setHeight`, a `Square` overriding them to keep sides equal *violates the parent's contract* — code using a Rectangle assumes width and height are independent. The fix: neither extends the other, or make them both implement a `Shape` interface.

**Key insight:** Inheritance must preserve the parent's behavioral contract — "is-a" in the *semantic* sense, not just "has the same shape." When a subclass can't honor the parent's promises, the hierarchy is wrong.

---

## Practice (try before peeking)

1. Can a class extend two classes? What about implement two interfaces?
2. `Animal a = new Dog();` — which `speak()` runs and why?
3. What must the first statement of a constructor be when the parent lacks a no-arg constructor?

<details><summary>Answers</summary>

1. No to classes (single inheritance — the diamond problem), yes to interfaces (multiple contracts are fine).
2. Dog's — dynamic dispatch resolves the *runtime* type of the object, not the declared type of the variable.
3. `super(...)` with the matching arguments — the parent constructor must run first.

</details>

---

**Common traps:**
- Missing `super(...)` call when the parent has no no-arg constructor — compile error
- `@Override` forgotten → silent "override" that's actually a new method
- Downcasting without `instanceof` → ClassCastException
- Static methods don't override — they hide (binds at compile time)
- Deep inheritance hierarchies — every level couples to all above; prefer composition

---
