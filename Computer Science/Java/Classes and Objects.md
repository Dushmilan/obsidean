# Classes & Objects

Everything in Java is an object — created from a class blueprint. The class defines fields (state) and methods (behavior); `new` constructs instances. This note is the practical mechanics of classes; the *principles* behind them live in the OOP notes.

**The Intuition:** A class is a cookie cutter; objects are the cookies. `Student` defines what every student *has* (fields) and *can do* (methods); `new Student("Ada", 36)` creates one specific student. Fields are the state; methods are the operations; `this` refers to "the object currently executing."

## Class anatomy

```java
public class Student {
    // Fields — state (private by convention = encapsulation)
    private String name;
    private int age;
    private double gpa;

    // Static field — belongs to the CLASS, shared by all instances
    private static int studentCount = 0;

    // Constant
    public static final int MAX_AGE = 130;

    // Constructor — runs on `new`, initializes state
    public Student(String name, int age, double gpa) {
        this.name = name;      // this.field = parameter (disambiguation)
        this.age = age;
        this.gpa = gpa;
        studentCount++;
    }

    // Methods — behavior
    public String getName() { return name; }

    public void celebrate() {
        gpa += 0.1;
    }

    // Static method — call on the class, not an instance
    public static int getCount() { return studentCount; }
}
```

## Construction

```java
// new: allocate heap memory → run constructor → return reference
Student ada = new Student("Ada", 36, 3.9);
Student bob = new Student("Bob", 25, 3.5);

ada.getName()          // "Ada"
ada.celebrate();
Student.getCount()     // 2 — static access via class
```

## Constructor overloading & chaining

```java
public class Student {
    private String name;
    private int age;
    private double gpa;

    // Three constructors — overloaded by signature
    public Student(String name) {
        this(name, 0, 0.0);          // delegates to the full constructor
    }
    public Student(String name, int age) {
        this(name, age, 0.0);
    }
    public Student(String name, int age, double gpa) {
        this.name = name;
        this.age = age;
        this.gpa = gpa;
    }
}
// this(...) must be the FIRST statement
```

## Access modifiers

| Modifier | Same class | Same package | Subclass | Everyone |
|----------|:---:|:---:|:---:|:---:|
| `private` | ✓ | | | |
| (package-private) | ✓ | ✓ | | |
| `protected` | ✓ | ✓ | ✓ | |
| `public` | ✓ | ✓ | ✓ | ✓ |

## `static` — class-level vs instance-level

```java
// Instance members: one per object, accessed via reference
Student ada = new Student("Ada");
ada.getName();          // instance method
ada.age;                // instance field (if accessible)

// Static members: one per class, accessed via class name
Student.getCount();     // static method
Student.MAX_AGE;        // static constant
// Calling static via an instance compiles but is bad style:
// ada.MAX_AGE  ← works, but confuses — use the class

// Static methods can't access instance fields/methods:
static int x() {
    return getCount();   // OK — static
    // return name;      // ERROR — no instance to get name from
}
```

## `this` — the current object

```java
public class Point {
    private int x, y;

    public Point(int x, int y) {
        this.x = x;        // disambiguate field from parameter
        this.y = y;
    }

    public Point translate(int dx, int dy) {
        this.x += dx;      // explicit this — optional here
        this.y += dy;
        return this;       // return this → fluent chaining
    }
}
// p.translate(1,2).translate(3,4);   // fluent style
```

## Reference semantics — what `==` means

```java
Student a = new Student("Ada");
Student b = a;               // b aliases a — same object
Student c = new Student("Ada");

a == b      // true  — same reference
a == c      // false — different objects
a.equals(c) // false — default equals is ALSO reference-based!
// You must OVERRIDE equals to get content equality:
```

## The equals/hashCode contract

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Student s = (Student) o;
    return age == s.age && Double.compare(s.gpa, gpa) == 0
            && name.equals(s.name);
}

@Override
public int hashCode() {
    return Objects.hash(name, age, gpa);
}
// Contract: equal objects MUST have equal hashCodes.
// Violate it → HashMap/HashSet misbehave (objects "disappear").
```

## Records — the data-class shortcut (Java 16+)

```java
// A record is an immutable data carrier: fields, constructor,
// getters (name()), equals/hashCode/toString — ALL generated.
public record Point(int x, int y) { }

Point p = new Point(3, 4);
p.x()           // getter is x() not getX()
p.equals(new Point(3, 4))   // true — content equality built in
```

---

**Setup:** Implement a `BankAccount` with balance protection and a withdrawal that fails gracefully.

**Solution:**
```java
public class BankAccount {
    private double balance;          // private — nobody touches it directly

    public BankAccount(double initial) {
        if (initial < 0) throw new IllegalArgumentException("negative initial");
        balance = initial;
    }

    public void deposit(double amt) {
        if (amt < 0) throw new IllegalArgumentException("negative deposit");
        balance += amt;
    }

    public boolean withdraw(double amt) {
        if (amt < 0 || amt > balance) return false;   // fail, don't corrupt
        balance -= amt;
        return true;
    }

    public double getBalance() { return balance; }
}
```

**Key insight:** `private` state + public methods = the account enforces its own invariants (never negative, never corrupted). Callers can't bypass the rules — that's encapsulation in action (see OOP notes).

---

**Setup:** Count how many `Student` objects exist.

**Solution:**
```java
public class Student {
    private static int count = 0;     // shared across ALL instances

    public Student(String name) {
        count++;
    }

    public static int getCount() { return count; }
}
// Every constructor call increments the class-level counter.
```

**Key insight:** `static int count` is one variable for the whole class, not one per object. Static state is global state — use it sparingly (testability suffers).

---

**Setup:** Make `Student` sortable by GPA.

**Solution:**
```java
// Option A: Comparable — natural ordering
public class Student implements Comparable<Student> {
    public int compareTo(Student o) {
        return Double.compare(this.gpa, o.gpa);
    }
}
Arrays.sort(students);

// Option B: Comparator — custom, per-use ordering
Arrays.sort(students, (s1, s2) -> Double.compare(s1.gpa, s2.gpa));
Arrays.sort(students, Comparator.comparingDouble(Student::gpa).reversed());
```

**Key insight:** `Comparable` defines the natural order (one way); `Comparator` gives arbitrary orderings per call site. The lambda comparator is the modern idiom.

---

## Practice (try before peeking)

1. Why are fields usually `private`?
2. Can a static method call an instance method?
3. `new Student("Ada") == new Student("Ada")` — true or false?

<details><summary>Answers</summary>

1. Encapsulation — the class controls access to its state, so it can enforce invariants (valid values, consistency). Public fields let anyone corrupt the object.
2. No — a static method has no instance to call it on. It can only access static members (or receive an instance as a parameter).
3. False — two separate objects. `==` compares references. (If equals/hashCode are overridden, `.equals()` would be true.)

</details>

---

**Common traps:**
- Constructor vs method: constructors have no return type and the class name
- `this.name = name` vs `name = name` — the latter assigns to itself (no-op shadowing bug)
- Forgetting `new` — `Student s = Student("Ada")` is a compile error
- Static methods accessing instance fields — compile error
- Not overriding `equals`/`hashCode` together — broken collections

---
