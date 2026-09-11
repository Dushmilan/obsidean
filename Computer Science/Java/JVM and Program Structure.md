# JVM & Program Structure

Java compiles to **bytecode** executed by the Java Virtual Machine (JVM) — a virtual computer that runs anywhere. Every Java program is a set of classes; the entry point is `main`. Understanding the JVM's three pillars — bytecode, the runtime data areas, and garbage collection — explains most Java behavior.

**The Intuition:** `javac` doesn't produce machine code; it produces `.class` files of bytecode — a compact instruction set for a *virtual* machine. The JVM interprets or JIT-compiles that bytecode to the host's machine code at runtime. That's the "write once, run anywhere" trick: your `.class` file is the same on Linux, Windows, and macOS; the JVM differs per platform.

## The pipeline

```java
// Hello.java  →  javac Hello.java  →  Hello.class  →  java Hello
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

```bash
javac Hello.java        # compiles to Hello.class (bytecode)
java Hello              # runs it on the JVM
javap -c Hello          # disassemble bytecode — see what the JVM actually runs
```

## Why every file needs a class

Java has no free functions — every method lives in a class. `main` is `static` so the JVM can call it *before any object exists*: `public static void main(String[] args)` is the contract.

## The JVM runtime areas

| Area | What lives there | Lifetime |
|------|------------------|----------|
| **Stack** | Local variables, method calls (one frame per call) | Per-thread, per-call |
| **Heap** | All objects (`new`), arrays | Shared, GC-managed |
| **Method area / Metaspace** | Class definitions, static fields, bytecode | Per-class, forever |
| **PC register** | Current instruction pointer | Per-thread |

```java
public class Memory {
    static int classVar = 10;          // Metaspace — one per class

    public static void main(String[] args) {
        int local = 5;                 // stack — dies when main returns
        int[] heap = new int[1000];    // heap — lives until GC collects it
    }
}
```

## Garbage collection — automatic memory management

Java has **no `free()`**. The GC finds unreachable objects and reclaims their memory:

```java
public class GcDemo {
    public static void main(String[] args) {
        for (int i = 0; i < 1_000_000; i++) {
            String s = new String("temp " + i);   // created...
            // ...and immediately unreachable after this iteration
        }   // GC reclaims them — you never call free
    }
}
```

**How it works (generational):** most objects die young. The heap is split into generations:
- **Young gen** (Eden + Survivors) — new objects; minor GCs are cheap and frequent
- **Old gen** — survivors promoted after many minor GCs; major GCs are expensive, rare

**Key implications:**
- No memory leaks *from forgetting free* — but **object leaks** exist: holding references you no longer need (e.g., a cache that never evicts) keeps objects alive forever
- GC pauses are the price — that's why Java has "stop-the-world" moments (G1, ZGC minimize them)
- `System.gc()` is a *suggestion*, not a command — don't call it

## Primitives vs reference types

```java
// Primitives — stored BY VALUE, on the stack (or in fields)
int age = 36;              // 4 bytes of actual number
double pi = 3.14;
char c = 'A';
boolean b = true;

// References — the variable holds an ADDRESS to a heap object
String name = "Ada";       // name is a pointer to a String object
int[] arr = new int[5];    // arr points to an array object on the heap

// The 8 primitives:
byte short int long float double char boolean
```

## The String pool & immutability

```java
String a = "hello";            // goes in the string POOL (Metaspace)
String b = "hello";            // SAME object as a
a == b                         // true — both point to the pooled literal

String c = new String("hello");// NEW heap object — not the pool
a == c                         // false! different objects

a.equals(c)                    // true — equals compares CONTENT
// ALWAYS use .equals() for strings, never ==

// String is immutable: every "modification" makes a new object
String s = "a";
s = s + "b";                   // new String "ab"; "a" is garbage
```

## Package & imports

```java
package com.myapp.core;        // must be first line — the namespace

import java.util.List;         // single class
import java.util.*;            // everything in java.util (not subpackages)
import static java.lang.Math.PI;   // static import — use PI directly

// Naming: package = reversed domain (com.company.product);
//         class = PascalCase; method/variable = camelCase; constant = UPPER_SNAKE
```

---

**Setup:** Compile and run a program that prints its arguments.

**Solution:**
```java
public class Args {
    public static void main(String[] args) {
        System.out.println("Program: " + Args.class.getName());
        for (int i = 0; i < args.length; i++) {
            System.out.println("arg[" + i + "] = " + args[i]);
        }
    }
}
// java Args one two three
// arg[0] = one, arg[1] = two, arg[2] = three
```

**Key insight:** `args` is an array of command-line strings — `args.length` (not a method call!) is the count. The `+` concatenation builds the message.

---

**Setup:** Demonstrate the difference between `==` and `.equals()` for strings.

**Solution:**
```java
String s1 = "cat";
String s2 = "cat";          // pooled — same object
String s3 = new String("cat");  // fresh object

s1 == s2        // true  — same reference (pooled literal)
s1 == s3        // false — different objects
s1.equals(s3)   // true  — same content

// The right rule: ALWAYS .equals() for content comparison
```

**Key insight:** `==` compares references; `.equals()` compares content (for String). The pool makes `s1 == s2` true *by accident of implementation* — code that relies on it breaks the moment a string is built at runtime. Never use `==` on strings.

---

**Setup:** Why is `main` declared `static`?

**Solution:** The JVM starts executing before any object exists — it can't call a method on a `new` instance. `static` means the method belongs to the *class*, callable as `Hello.main(...)` without an instance. The JVM effectively does: load class `Hello`, then invoke its static `main` method with the args array.

**Key insight:** `public` (JVM must access it), `static` (no instance yet), `void` (no return), `String[] args` (command-line). This exact signature is the only thing the launcher recognizes.

---

**Setup:** When would you see `OutOfMemoryError`?

**Solution:** When the heap is exhausted — either genuinely too much data, or an **object leak**: references held longer than needed. Common: static collections that grow forever, listeners never removed, caches without eviction.
```java
static Map<String, byte[]> cache = new HashMap<>();   // never cleared
// each put() keeps data alive FOREVER → OOM eventually
```

**Key insight:** The GC can't collect what's still reachable. "Memory leak" in Java = "unintentionally reachable." The fix is releasing references (clear the map, null the field, remove the listener).

---

## Practice (try before peeking)

1. `int x = 5;` — where does x live? `Integer y = 5;` — where does y's value live?
2. Why is `String` immutable in Java (what breaks if it weren't)?
3. `javac` produces what, and `java` runs what?

<details><summary>Answers</summary>

1. `x` (primitive int) lives on the stack by value. `y` is a reference to a heap object (autoboxed Integer).
2. The string pool caches interned strings by reference — if one reference mutated "hello", *every* reference to "hello" would change. Immutability also makes String safe to share across threads.
3. `javac` produces `.class` bytecode; `java` runs it on the JVM (JIT-compiling hot methods to native code).

</details>

---

**Common traps:**
- `==` on Strings (use `.equals()`) — the #1 Java bug
- `System.gc()` — doesn't force collection; don't rely on it
- Primitives vs wrappers: `int` can't be null, `Integer` can — NPEs from autoboxing nulls are common
- `new String("x")` — almost never needed; the literal `"x"` is pooled
- Forgetting `args.length` is a field, not `args.length()` — it's an array

---
