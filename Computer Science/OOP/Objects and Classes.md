# Objects & Classes

Object-Oriented Programming organizes code around **objects** — bundles of state (fields) and behavior (methods). Before the four pillars, you need the basic mechanics: what a class is, what an object is, and the three fundamental relationships between classes (is-a, has-a, uses-a).

**The Intuition:** The world is full of objects: a bank account (balance + deposit/withdraw), a car (speed + accelerate/brake), a customer (name + purchase). OOP says: model each thing as one unit that *owns its data* and *knows how to behave*. The class is the blueprint; the object is the real thing built from it.

## Class vs Object

| | Class | Object |
|--|-------|--------|
| What | Blueprint / template | Instance created from the blueprint |
| When | Defined once, at design time | Created at runtime with `new` |
| State | Declares *what* fields exist | Holds *actual* values |
| Analogy | Cookie cutter | The cookies |
| In Java | `class Student { ... }` | `new Student("Ada")` |

```java
class Student {                 // blueprint
    String name;                // every student has a name...
    int age;                    // ...and an age
    void introduce() {
        System.out.println("I'm " + name);
    }
}

Student ada = new Student();    // object 1
Student bob = new Student();    // object 2 — each has its OWN name/age
```

## State & behavior — the two halves

```java
class Counter {
    // STATE — the data that varies per object
    private int count = 0;

    // BEHAVIOR — the operations on that state
    void increment() { count++; }
    int getCount() { return count; }
}
// The rule: data and the operations on it live TOGETHER.
// Code that uses a Counter never touches `count` directly.
```

**Why bundle them?** If data and operations were separate, every user of `count` would need to know its format and keep it consistent. Bundling means one owner, one place to change, one place to enforce rules.

## The three relationships between classes

**1. IS-A (inheritance)** — a Dog *is an* Animal
```java
class Animal { ... }
class Dog extends Animal { ... }      // Dog gets everything Animal has
```

**2. HAS-A (composition)** — a Car *has an* Engine
```java
class Engine { ... }
class Car {
    private Engine engine;            // Car contains an Engine
}
```

**3. USES-A (dependency)** — a Driver *uses* a Car
```java
class Driver {
    void drive(Car car) { car.accelerate(); }    // temporary use, not ownership
}
```

**The decision rule:** IS-A → inheritance; HAS-A → composition; USES-A → parameters/dependencies. Most real code overuses inheritance and underuses composition.

## Constructors & initialization

```java
class Student {
    private String name;
    private int age;

    // Constructors ensure the object starts in a VALID state
    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    // Without a constructor, fields start at defaults
    // (null, 0, false) — often invalid for real usage
}
```

**Object lifecycle:** `new` allocates memory → constructor runs (initial state guaranteed) → methods called → object becomes unreachable → garbage collector reclaims it.

## Encapsulation preview — why state is private

```java
class BankAccount {
    private double balance;          // THE ONLY way to change it is below

    void deposit(double amount) {
        if (amount > 0) balance += amount;
    }
    boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
}
// Invariant: balance is never negative — guaranteed BY THE CLASS ITSELF.
```

---

**Setup:** Model a `Temperature` object with `celsius` and a conversion method.

**Solution:**
```java
class Temperature {
    private double celsius;              // state — one unit of truth

    Temperature(double celsius) { this.celsius = celsius; }

    double toFahrenheit() { return celsius * 9.0 / 5.0 + 32; }   // behavior
    double getCelsius() { return celsius; }
}
```

**Key insight:** The object stores *one* representation (celsius) and *derives* the other on demand — single source of truth. If we stored both, they could drift out of sync. This is why "state + behavior together" prevents inconsistency.

---

**Setup:** A `Library` contains many `Book`s — which relationship?

**Solution:** HAS-A (composition):
```java
class Library {
    private List<Book> books = new ArrayList<>();

    void addBook(Book b) { books.add(b); }
    int count() { return books.size(); }
}
```
`Library` owns its books — when the library is discarded, its books go with it (ownership).

**Key insight:** The collection field IS the HAS-A relationship. Composition lets the whole manage its parts. Note `Book` is created independently and passed in — but once added, the Library owns it.

---

**Setup:** Why is `new Student("Ada")` different from assigning a name directly?

**Solution:** The constructor *guarantees* valid state. Compare:
```java
// With a constructor:
Student s = new Student("Ada", 36);   // can't forget the name — won't compile

// Without (public fields):
Student s = new Student();
s.name = "Ada";      // easy to forget → null name bugs
s.age = 36;          // easy to skip → 0 age (or set a negative!)
```

**Key insight:** Constructors turn "the object is ready" from a convention into a *requirement*. Combined with private fields, no code path can create an invalid object.

---

## Practice (try before peeking)

1. Which relationship: `Teacher` and `Course`?
2. What happens if a class has no constructor?
3. Why keep `balance` private?

<details><summary>Answers</summary>

1. A teacher *teaches* courses and a course *has* a teacher — both HAS-A (fields), and possibly USES-A at runtime. Not IS-A — neither is a subtype of the other.
2. Java provides a default no-arg constructor — fields get defaults (null/0/false). That's often an invalid initial state for real objects.
3. Encapsulation — the class alone controls its data, so it can enforce invariants (balance never negative, amounts validated) and change internals without breaking callers.

</details>

---

**Common traps:**
- Modeling IS-A when it's really HAS-A — "Student extends Course" (a student *takes* a course, isn't one)
- Public fields — anyone can set invalid state; make fields private
- Forgetting constructors → objects in invalid default state
- God objects — one class that owns everything; split by responsibility
- Objects with no behavior (data bags) — fine for records, but if every class is a data bag, you're not really doing OOP

---
