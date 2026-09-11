# Inheritance

Inheritance models **"is-a"** relationships and enables code reuse through **extension**: a subclass inherits the parent's fields and methods, can *override* them, and can *add* its own. When used correctly it's the basis of polymorphism; misused, it produces brittle hierarchies that are the #1 OOP design smell.

**The Intuition:** A `SavingsAccount` *is a* `BankAccount` — it has everything an account has (balance, deposit, withdraw) plus interest behavior. Inheritance captures that: the subclass *starts with* the parent's full capability and specializes it. The two promises inheritance makes:
1. **Substitutability** — a `SavingsAccount` can be used anywhere a `BankAccount` is expected
2. **Reuse** — common code written once in the parent, inherited everywhere

## The mechanics

```java
class BankAccount {
    protected double balance;              // protected: visible to subclasses

    public BankAccount(double initial) { balance = initial; }
    public void deposit(double amt) { balance += amt; }
    public boolean withdraw(double amt) {
        if (amt > balance) return false;
        balance -= amt;
        return true;
    }
}

class SavingsAccount extends BankAccount {
    private double interestRate;

    public SavingsAccount(double initial, double rate) {
        super(initial);                    // parent constructor FIRST
        interestRate = rate;
    }

    public void applyInterest() {
        balance += balance * interestRate; // uses inherited `balance`
    }

    @Override
    public boolean withdraw(double amt) {  // override — specialized behavior
        if (balance - amt < 500) return false;   // minimum balance rule
        return super.withdraw(amt);        // reuse the parent's logic
    }
}
```

## The constructor chain

```java
// new SavingsAccount(1000, 0.04):
//   1. BankAccount(1000) constructor runs FIRST (super())
//   2. SavingsAccount constructor body runs
// Parent state is always fully built before child state.
```

## What you can do in a subclass

| Operation | Allowed | Note |
|-----------|---------|------|
| Inherit fields/methods | ✓ | use them directly |
| Override a method | ✓ | same signature; `@Override` to verify |
| Call `super.method()` | ✓ | reuse parent logic inside the override |
| Add new fields/methods | ✓ | subclass-only behavior |
| Access `private` parent fields | ✗ | only via public/protected members |
| Override a `final` method | ✗ | compile error |
| Extend a `final` class | ✗ | compile error |

## Override vs overload vs hide — the three

```java
class Animal {
    public void speak() { System.out.println("..."); }
    public void eat(String food) { ... }
    public static void identify() { System.out.println("Animal"); }
}

class Dog extends Animal {
    @Override
    public void speak() { ... }              // OVERRIDE — replaces, runtime dispatch

    public void eat(String food, int bites) { ... }   // OVERLOAD — different signature

    public static void identify() { ... }    // HIDE — static, binds at compile time
}
```

## The substitutability promise — Liskov

```java
// ANY code written against BankAccount must work with a SavingsAccount:
void showBalance(BankAccount account) {
    System.out.println(account.getBalance());
}
showBalance(new BankAccount(1000));
showBalance(new SavingsAccount(1000, 0.04));   // works — is-a

// This is the Liskov Substitution Principle in action.
// If a subclass breaks the parent's contract (e.g., withdraw() returns
// true but doesn't actually withdraw), substitutability collapses.
```

## The classic misuse — Square extends Rectangle

```java
class Rectangle {
    private int w, h;
    public void setWidth(int w) { this.w = w; }
    public void setHeight(int h) { this.h = h; }
    public int area() { return w * h; }
}

class Square extends Rectangle {
    @Override public void setWidth(int w) { super.setWidth(w); super.setHeight(w); }
    @Override public void setHeight(int h) { super.setWidth(h); super.setHeight(h); }
}

// LISKOV VIOLATION:
Rectangle r = new Square();
r.setWidth(5);
r.setHeight(10);           // square forces both to 10
// r.area() == 100, but a 5×10 rectangle would be 50
// Code that relies on "width and height are independent" breaks.
```

**The fix:** neither extends the other; both implement a `Shape` interface (composition/interface over inheritance).

## When inheritance is right

| Use inheritance when... | Don't when... |
|-------------------------|---------------|
| Genuine "is-a" + behavioral reuse | "has-a" (use composition) |
| Subclasses truly specialize the parent | You just want to share a few methods |
| The parent's contract holds for all subclasses | The subclass would violate parent invariants |
| The hierarchy is shallow and stable | The hierarchy is deep and speculative |

## Composition as the alternative

```java
// Instead of: class Dog extends Animal (inheritance)
// Consider:  class Dog { private Animal animal; }   (composition)
// Composition wins when:
//   - you need to swap parts at runtime
//   - the "is-a" isn't semantically true
//   - you want to reuse a behavior without adopting the whole type
// "Prefer composition over inheritance" — the most quoted OOP guideline.
```

---

**Setup:** Design a `Vehicle` → `Car`/`Truck` hierarchy with a polymorphic `move`.

**Solution:**
```java
abstract class Vehicle {
    protected int speed;

    public abstract void move();          // contract: every vehicle moves

    public void speedUp() { speed += 10; }
}

class Car extends Vehicle {
    @Override public void move() { System.out.println("Driving on roads"); }
}

class Truck extends Vehicle {
    @Override public void move() { System.out.println("Hauling on highways"); }
}

// Uses:
List<Vehicle> fleet = List.of(new Car(), new Truck());
for (Vehicle v : fleet) v.move();         // each moves its own way
```

**Key insight:** The abstract method *forces* each subclass to implement `move` (no silent fallback to nothing). The loop treats all vehicles uniformly — add a `Boat` tomorrow, the loop doesn't change (Open/Closed).

---

**Setup:** Reuse parent logic inside an override.

**Solution:**
```java
class DiscountAccount extends BankAccount {
    @Override
    public boolean withdraw(double amt) {
        double fee = amt * 0.01;                   // new rule
        return super.withdraw(amt + fee);          // parent handles the rest
    }
}
```

**Key insight:** `super.method()` is how an override *extends* behavior instead of replacing it wholesale — the parent handles the shared part, the subclass adds its twist. This keeps the base logic in one place.

---

**Setup:** When is deep inheritance a problem?

**Solution:** `SavingsAccount extends Account extends BankProduct extends FinancialThing` — each level couples to all above it. A change in `FinancialThing` ripples down; a subclass at the bottom inherits behaviors it doesn't need; testing requires constructing the whole chain. The fix: shallow hierarchies + composition + interfaces.

**Key insight:** Inheritance *is* coupling — the subclass can't be understood without the parent. Prefer the shortest hierarchy that expresses genuine "is-a," and lean on composition for code sharing.

---

## Practice (try before peeking)

1. `class Penguin extends Bird` where `Bird.fly()` throws "can't fly" — good or bad design?
2. Can a subclass access a parent's `private` field?
3. What must be the first statement in a subclass constructor when the parent has no no-arg constructor?

<details><summary>Answers</summary>

1. Bad — the subclass can't fulfill the parent's contract (birds fly). Violates Liskov. Better: `Bird` without `fly`, and a `FlyingBird` subtype.
2. No — `private` is class-only. Subclasses use `protected` or public accessors.
3. `super(...)` — the parent constructor must run first (with its required arguments).

</details>

---

**Common traps:**
- Deep hierarchies — coupling balloons, changes ripple
- Forgetting `super(...)` when the parent needs it — compile error
- Overriding without `@Override` — typos silently create new methods
- Breaking the parent's contract — Liskov violations that only surface at runtime
- Using inheritance for code sharing when the relationship isn't truly "is-a" — prefer composition

---
