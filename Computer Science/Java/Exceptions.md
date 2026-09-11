# Exceptions

Java's error model is exceptions — objects that describe what failed and where. The language makes a critical design distinction: **checked** exceptions (you must handle them) and **unchecked** exceptions (you may, if useful). This shapes how every Java API reports errors.

**The Intuition:** When something goes wrong, Java throws an exception object up the call stack until someone catches it. The `try`/`catch`/`finally` structure intercepts it. The distinction: checked exceptions are *recoverable, expected* problems the compiler forces you to confront (file missing, network down); unchecked are *programming errors* (null, bad index) that should surface and be fixed.

## The hierarchy

```
Throwable
├── Error                      — JVM failures: OutOfMemoryError, StackOverflowError
│                                DON'T catch these — you can't recover
└── Exception
    ├── RuntimeException       — UNCHECKED: programming bugs
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   ├── IllegalArgumentException
    │   ├── IllegalStateException
    │   └── ArithmeticException
    └── (checked exceptions)   — compiler FORCES handling
        ├── IOException
        ├── SQLException
        ├── InterruptedException
        └── FileNotFoundException (extends IOException)
```

## try / catch / finally

```java
try {
    FileReader fr = new FileReader("data.txt");   // checked IOException
    // ...
} catch (FileNotFoundException e) {
    System.out.println("File not there: " + e.getMessage());
} catch (IOException e) {                          // parent — catches other IO errors
    System.out.println("IO problem: " + e);
} finally {
    System.out.println("Always runs — cleanup here");
}

// Multi-catch (Java 7+):
} catch (IOException | SQLException e) {
    // both handled the same way
}

// try-with-resources (Java 7+) — auto-close, the modern way:
try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
    String line = br.readLine();
    // br is closed automatically — even on exceptions
} catch (IOException e) {
    // ...
}
```

## Checked vs unchecked

```java
// CHECKED — the compiler FORCES you to handle or declare:
void readFile(String path) throws IOException {    // declare it...
    FileReader fr = new FileReader(path);          // ...or catch it
}
// If you neither catch nor declare → COMPILE ERROR

// UNCHECKED — no obligation (programming errors):
int x = arr[10];       // may throw AIOOBE — no declaration required
String s = null;
s.length();            // may throw NPE — no declaration required
```

**Design rule:** Use checked exceptions for *recoverable, anticipated* conditions the caller can do something about (I/O, parsing external input). Use unchecked (`RuntimeException`) for programming errors and things the caller can't meaningfully handle.

## Throwing & wrapping

```java
public void withdraw(double amount) {
    if (amount < 0) {
        throw new IllegalArgumentException("amount must be >= 0");
    }
    if (amount > balance) {
        throw new InsufficientFundsException("balance: " + balance);
    }
    balance -= amount;
}

// Custom exception:
class InsufficientFundsException extends RuntimeException {
    public InsufficientFundsException(String msg) { super(msg); }
}

// Wrap & rethrow — preserve the ROOT CAUSE:
public void loadConfig(String path) throws ConfigException {
    try {
        Files.readAllLines(Path.of(path));
    } catch (IOException e) {
        throw new ConfigException("failed to load: " + path, e);   // chain the cause!
    }
}
// The chained cause shows in the stack trace:
// "Caused by: java.io.FileNotFoundException: ..."
```

## try-with-resources — why it matters

```java
// OLD way — easy to leak:
BufferedReader br = null;
try {
    br = new BufferedReader(new FileReader("f.txt"));
    // ...
} finally {
    if (br != null) br.close();       // manual, easy to forget, may itself throw
}

// NEW way — guaranteed:
try (BufferedReader br = new BufferedReader(new FileReader("f.txt"))) {
    // ...
}                                    // br.close() called automatically
// Multiple resources:
try (var in = new FileInputStream("a");
     var out = new FileOutputStream("b")) { ... }
```

## The `finally` subtleties

```java
try { return 1; } finally { System.out.println("runs"); }   // prints runs, returns 1
// finally runs even across return/break/continue

// TRAP: a return in finally OVERRIDES the try's return:
int f() {
    try { return 1; }
    finally { return 2; }        // f() == 2! finally's return wins
}
// Never return from finally — it silently discards the try's result
```

---

**Setup:** Read a file, fall back to defaults if it's missing, fail loudly otherwise.

**Solution:**
```java
public List<String> loadLines(String path) {
    try {
        return Files.readAllLines(Path.of(path));
    } catch (FileNotFoundException e) {          // specific: handle it
        System.err.println("Missing config, using defaults");
        return List.of("default=1");
    } catch (IOException e) {                    // broader: report it
        throw new RuntimeException("Failed to read " + path, e);
    }
}
```

**Key insight:** Catch the *specific* expected failure (file missing — recover with defaults), and wrap unexpected failures with context, chaining the cause. Blanket `catch (Exception e)` hides bugs.

---

**Setup:** Parse user input safely, retrying on bad input.

**Solution:**
```java
Scanner sc = new Scanner(System.in);
int n;
while (true) {
    System.out.print("Enter an integer: ");
    try {
        n = Integer.parseInt(sc.nextLine());
        break;
    } catch (NumberFormatException e) {
        System.out.println("Not an integer — try again.");
    }
}
```

**Key insight:** `Integer.parseInt` throws unchecked `NumberFormatException` — we catch it where user input enters the system. Retry loops with `try` around the risky operation are the standard input-validation pattern.

---

**Setup:** Convert a checked exception into an unchecked one at an API boundary.

**Solution:**
```java
public interface Repository {
    List<User> findAll();       // no checked exceptions in the interface
}

public class FileRepository implements Repository {
    @Override
    public List<User> findAll() {
        try {
            // ... read file, parse ...
            return users;
        } catch (IOException e) {
            throw new UncheckedIOException(e);   // standard unchecked wrapper
        }
    }
}
```

**Key insight:** Some APIs prefer unchecked exceptions so callers aren't forced to handle low-level details. `UncheckedIOException` is Java's built-in wrapper. The trade-off: callers lose the compile-time reminder — document it clearly.

---

## Practice (try before peeking)

1. Is `NullPointerException` checked or unchecked? `IOException`?
2. What does `try { return 5; } finally { }` return — does finally stop it?
3. What's wrong with `catch (Exception e) { }` with an empty body?

<details><summary>Answers</summary>

1. NPE is unchecked (a RuntimeException — a programming bug). IOException is checked — the compiler forces handling.
2. Returns 5 — an empty `finally` doesn't interfere. Only a `return` *inside* finally overrides.
3. It silently swallows every error — including ones you can't handle — making debugging nearly impossible. At minimum log; better, catch specific types.

</details>

---

**Common traps:**
- Catching `Exception` (or worse, `Throwable`/`Error`) broadly — hides bugs and even OOM
- Empty catch blocks — silent failures
- Forgetting `e.printStackTrace()` or logging — the error vanishes
- `return` in `finally` — overrides the try's result
- Not chaining the cause (`new MyException(e)` instead of `new MyException("msg", e)`) — loses the root cause

---
