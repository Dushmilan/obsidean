# Polymorphism

Polymorphism — "many forms" — is the payoff of OOP: **one method call, many behaviors**, decided at runtime by the object's actual type. It's what lets you write code against abstractions (`Animal`) and have it work for every concrete type (`Dog`, `Cat`, whatever comes next).

**The Intuition:** Press "play" on a music player: a CD plays CD tracks, a streaming service streams, a radio tunes — *same button, different behavior*. The system doesn't need to know what kind of player it is; each player knows itself. Polymorphism is that "each object knows its own behavior" idea made rigorous.

## The mechanism — dynamic dispatch

```java
class Animal {
    public void speak() { System.out.println("..."); }
}
class Dog extends Animal {
    @Override public void speak() { System.out.println("Woof!"); }
}
class Cat extends Animal {
    @Override public void speak() { System.out.println("Meow!"); }
}

Animal a = new Dog();
a.speak();      // "Woof!"  — decided at RUNTIME by the object's type
```

**How it works:** Each object carries a reference to its class's method table. `a.speak()` looks up `speak` in *Dog's* table (not Animal's) because `a` *is* a Dog. The variable's declared type (`Animal`) only sets what's *callable*; the runtime type decides *which* implementation runs.

## Two kinds of polymorphism

**1. Compile-time (overloading)** — same method name, different signatures, resolved by argument types at compile time:
```java
class Calc {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }   // overload
}
calc.add(1, 2);        // int version — decided at compile time
calc.add(1.5, 2.5);    // double version
```

**2. Runtime (overriding)** — subclasses override; dispatch happens at runtime. This is "the" polymorphism of OOP.

## Why it matters — programming to the interface

```java
// WITHOUT polymorphism — every new type needs a new branch:
void makeSound(Object o) {
    if (o instanceof Dog) ((Dog) o).speak();
    else if (o instanceof Cat) ((Cat) o).speak();
    // ...add a branch for every new animal, forever!
}

// WITH polymorphism — one line, works for every future subclass:
void makeSound(Animal a) {
    a.speak();          // Dog says Woof, Cat says Meow, tomorrow's Snake hisses
}
```

**The payoff:** new subtypes plug in with **zero changes** to existing code — this is the Open/Closed principle in action.

## The classic real-world example — payment processing

```java
interface Payment {
    void pay(double amount);
}

class CreditCard implements Payment {
    @Override public void pay(double amount) { /* charge card */ }
}
class PayPal implements Payment {
    @Override public void pay(double amount) { /* PayPal API */ }
}
class Crypto implements Payment {
    @Override public void pay(double amount) { /* blockchain */ }
}

// The checkout code:
class Checkout {
    void complete(Payment method, double amount) {
        method.pay(amount);        // ONE call — any payment type
    }
}
// Add Bitcoin tomorrow: new class, zero changes to Checkout.
```

## Substitutability — the Liskov requirement

Polymorphism is only safe if every subclass honors its parent's contract. If `Dog.speak()` threw an exception, code relying on `Animal.speak()` would break. **Liskov Substitution Principle:** a subclass must be usable wherever its parent is, *without surprising the caller*. Polymorphism without Liskov is a time bomb.

## Method resolution in practice

```java
class A {
    public void f() { System.out.println("A.f"); }
    public void g() { f(); }               // calls THIS.f — dynamic!
}
class B extends A {
    @Override public void f() { System.out.println("B.f"); }
}

B b = new B();
b.g();      // "B.f" — g() is inherited, but its call to f() dispatches
            // to B's version. Inheritance + polymorphism compose.
```

**This is why overriding is dangerous:** the parent's methods call *your* overrides — make sure they honor the parent's expectations.

---

**Setup:** A `Shape` hierarchy computing area polymorphically.

**Solution:**
```java
abstract class Shape {
    public abstract double area();      // contract
}

class Circle extends Shape {
    private double r;
    public Circle(double r) { this.r = r; }
    @Override public double area() { return Math.PI * r * r; }
}

class Square extends Shape {
    private double side;
    public Square(double side) { this.side = side; }
    @Override public double area() { return side * side; }
}

double totalArea(List<Shape> shapes) {
    double total = 0;
    for (Shape s : shapes) total += s.area();   // each computes its own
    return total;
}
```

**Key insight:** `totalArea` knows *nothing* about circles or squares — it only needs the `Shape` contract. Each shape supplies its own math. Adding a `Triangle` = one new class, zero changes to `totalArea`.

---

**Setup:** Replace a chain of if/instanceof with polymorphism.

**Solution:**
```java
// BEFORE — fragile, must edit for every new type:
double tax(Object product) {
    if (product instanceof Book) return 0.0;
    if (product instanceof Electronics) return 0.2;
    if (product instanceof Food) return 0.08;
    // forgetting a type → silent 0 tax
}

// AFTER — the tax lives with each type:
interface Product {
    double tax();                       // each product knows its own rate
}
class Book implements Product {
    @Override public double tax() { return 0.0; }
}
class Electronics implements Product {
    @Override public double tax() { return 0.2; }
}
double tax(Product p) { return p.tax(); }     // no branches to maintain
```

**Key insight:** The if/instanceof chain is the *smell* polymorphism removes. Each behavior travels with its own type. New types can't be forgotten — they must implement the contract.

---

**Setup:** Why does `List<Dog>` not work where `List<Animal>` is expected?

**Solution:** Generics are **invariant** — `List<Dog>` is not a subtype of `List<Animal>`, even though `Dog` is a subtype of `Animal`. (If it were, you could add a `Cat` to what's really a dog-only list.) The escape hatch is the wildcard `? extends`:
```java
void showAll(List<? extends Animal> animals) {
    for (Animal a : animals) a.speak();    // reads fine
}
showAll(dogList);       // OK with wildcard
```

**Key insight:** Polymorphism applies to the *element* type (a Dog IS-A Animal); the *container* stays invariant for safety. Wildcards restore read/write flexibility without breaking it.

---

## Practice (try before peeking)

1. Which dispatch is at runtime — overloading or overriding?
2. What does the Open/Closed principle have to do with polymorphism?
3. `Animal a = new Cat(); ((Dog) a).speak();` — what happens?

<details><summary>Answers</summary>

1. Overriding (runtime polymorphism, dynamic dispatch). Overloading resolves at compile time.
2. Polymorphism IS the Open/Closed mechanism — extend behavior by adding subclasses (open) without modifying existing code (closed).
3. `ClassCastException` at runtime — `a` is a Cat, not a Dog. Always guard with `instanceof`.

</details>

---

**Common traps:**
- `instanceof` chains instead of polymorphism — the design smell; the polymorphism is the fix
- Overloading vs overriding confusion — different method names bind differently
- Static methods aren't polymorphic — they bind to the *reference* type at compile time
- Downcasting without `instanceof` — ClassCastException
- Writing code that depends on the *concrete* type when the abstract type suffices — `Dog d = new Dog(); d.speak()` instead of `Animal d = new Dog(); d.speak()`

---
