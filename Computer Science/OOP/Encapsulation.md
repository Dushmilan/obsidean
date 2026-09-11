# Encapsulation

Encapsulation is the practice of keeping an object's **state private** and exposing behavior through **methods**. It's the foundation the other OOP pillars build on — without it, inheritance and polymorphism operate on unsafe data.

**The Intuition:** A vending machine. You press buttons (the public API) — you never reach inside for the mechanism (private state). This isn't secrecy; it's *control*. The machine guarantees: every drink is properly dispensed, change is correct, and its internal design can change (new coin mechanism) without you noticing. Encapsulation gives the object the same guarantees about its data.

## The two halves

```java
class BankAccount {
    // PRIVATE state — nobody outside can touch this
    private double balance;

    // PUBLIC behavior — the only way in or out
    public void deposit(double amount) { ... }
    public boolean withdraw(double amount) { ... }
    public double getBalance() { return balance; }   // read-only view
}
```

## What encapsulation buys you

**1. Invariants — the object never enters a bad state**
```java
class Account {
    private double balance;

    public void deposit(double amt) {
        if (amt <= 0) throw new IllegalArgumentException("amount must be > 0");
        balance += amt;               // balance is always >= 0, by construction
    }
}
// With public balance, any code could set balance = -500. No more.
```

**2. Freedom to change internals**
```java
// Internals can change WITHOUT touching callers:
private double balance;                    // v1: double
private BigDecimal balance;                // v2: precise — callers unaffected
// The public API (deposit/withdraw/getBalance) never changes.
```

**3. Validation & cross-cutting logic in one place**
```java
private void log(String msg) { ... }
public void withdraw(double amt) {
    if (amt > balance) throw new InsufficientFunds();
    balance -= amt;
    log("withdrew " + amt);       // auditing happens HERE, always
}
```

## Getters & setters — with judgment

```java
// Naive "encapsulation" — getters/setters that just expose everything:
class Person {
    private int age;
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }   // no validation!
}
// This is encapsulation in form only — no more protection than public fields.

// Meaningful encapsulation — the setter adds VALUE:
public void setAge(int age) {
    if (age < 0 || age > 150) throw new IllegalArgumentException("bad age");
    this.age = age;                // rule enforced
}

// Or better — don't expose state you don't need to:
class BankAccount {
    // no setBalance() at all — balance changes ONLY via deposit/withdraw
}
```

**The rule:** expose the *behavior*, not the *data*. A getter is justified when callers genuinely need the value; a setter is justified when there's a rule to enforce. `getBalance()` is read-only state; `setBalance()` invites corruption.

## Getters that compute — encapsulation in action

```java
class Circle {
    private double radius;              // THE state

    public double getRadius() { return radius; }
    public double getArea() { return Math.PI * radius * radius; }    // computed
    public double getCircumference() { return 2 * Math.PI * radius; } // computed
}
// Callers ask for area() — they never know or care HOW it's computed.
// Later: cache the area, store diameter instead — callers unaffected.
```

## The public API — a contract

```java
class Queue {
    private int[] data;      // implementation detail
    private int head, tail;

    // The contract callers rely on:
    public void enqueue(int x) { ... }
    public int dequeue() { ... }
    public boolean isEmpty() { ... }
}
// Callers know the CONTRACT (enqueue/dequeue/isEmpty semantics).
// The implementation (array vs linked list) can change freely.
// This is why OOP enables refactoring: the contract is the stable part.
```

---

**Setup:** Design a `BankAccount` where the balance can be read but never set directly.

**Solution:**
```java
public class BankAccount {
    private double balance;

    public BankAccount(double initial) {
        if (initial < 0) throw new IllegalArgumentException();
        balance = initial;
    }

    public void deposit(double amt) {
        if (amt <= 0) throw new IllegalArgumentException();
        balance += amt;
    }

    public boolean withdraw(double amt) {
        if (amt <= 0 || amt > balance) return false;    // fail, don't corrupt
        balance -= amt;
        return true;
    }

    public double getBalance() { return balance; }      // read-only — NO setter
}
```

**Key insight:** There is no `setBalance` — the balance changes only through validated operations. The object *controls its own lifecycle*. This is encapsulation as it's meant to be used.

---

**Setup:** Why does making `list` private matter if it's an `ArrayList`?

**Solution:**
```java
public class ShoppingCart {
    private List<String> items = new ArrayList<>();

    public void add(String item) { items.add(item); }

    public List<String> getItems() {
        // DANGER: returning the internal list lets callers mutate it:
        //   cart.getItems().add("FREE STUFF");  ← bypasses all validation!
        return items;
    }
}
// The fix — return a copy (a read-only view):
public List<String> getItems() {
    return Collections.unmodifiableList(items);    // reads OK, writes throw
}
// or: return new ArrayList<>(items);               // a copy — safe
```

**Key insight:** A getter that hands out the internal collection *breaks* encapsulation — the caller can mutate private state through the returned reference. "Defensive copying" or unmodifiable views close the hole.

---

**Setup:** A `Thermostat` that only allows valid temperatures.

**Solution:**
```java
public class Thermostat {
    private double temperature;
    private static final double MIN = 10, MAX = 30;

    public Thermostat(double temp) { setTemperature(temp); }

    public void setTemperature(double temp) {
        if (temp < MIN || temp > MAX) throw new IllegalArgumentException();
        temperature = temp;               // validated before store
    }

    public double getTemperature() { return temperature; }
}
```

**Key insight:** Validation lives in the setter (and constructor reuses it) — one rule, one place. Every path that changes temperature goes through the check. The invariant `MIN <= temperature <= MAX` holds forever.

---

## Practice (try before peeking)

1. If all fields are private with naive getters/setters, is it encapsulated?
2. Should a `Student` class have `setName`? Why or why not?
3. What's the harm in `getItems()` returning the internal list?

<details><summary>Answers</summary>

1. In *form* only — if a setter just assigns without rules, it provides no protection. Encapsulation is about *control*, not syntax.
2. Usually no — a name, once set at construction, rarely needs changing, and renaming might need validation/logic. Prefer immutable fields set in the constructor.
3. Callers can mutate private state through the reference — bypassing validation, breaking invariants, and corrupting the object.

</details>

---

**Common traps:**
- Getters/setters for every field ("JavaBean syndrome") — form without protection
- Returning internal collections/references — the mutation hole
- Public fields "for convenience" — invariants become unenforceable
- Setting state without validation — the "setter that's just a field with extra steps"
- Over-encapsulating: hiding *everything* behind pointless methods — expose what callers legitimately need

---
