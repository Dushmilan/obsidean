# Types & Operators

Java is **statically typed**: every variable's type is known at compile time, and the compiler rejects type mismatches. The type system splits into primitives (by value) and references (to objects). Operators are the vocabulary for computation, with several Java-specific behaviors worth knowing cold.

**The Intuition:** The compiler is a strict gatekeeper — it checks types *before* the program runs. This catches entire classes of bugs (the "wrong type" errors) at compile time, at the cost of verbosity. The 8 primitives are the machine-level types; everything else is an object reference.

## The 8 primitives

| Type | Size | Range | Default |
|------|------|-------|---------|
| `byte` | 1 B | -128..127 | 0 |
| `short` | 2 B | ±32k | 0 |
| `int` | 4 B | ±2.1B | 0 |
| `long` | 8 B | ±9.2 quintillion | 0L |
| `float` | 4 B | ~7 sig digits | 0.0f |
| `double` | 8 B | ~15 sig digits | 0.0 |
| `char` | 2 B | Unicode 0..65535 | '\u0000' |
| `boolean` | 1 B (JVMs vary) | true/false | false |

**Locals have no default** — using an uninitialized local is a *compile error* (unlike fields, which get defaults).

## Literals & constants

```java
int n = 42;
long big = 4_000_000_000L;      // L suffix — otherwise int overflow
double d = 3.14;
float f = 3.14f;                 // f suffix — otherwise double
char c = 'A';
boolean b = true;

int hex = 0xFF;                  // 255
int bin = 0b1010;                // 10
int big = 1_000_000;             // underscores for readability (Java 7+)

final int MAX = 100;             // final = constant (convention: UPPER_SNAKE)
```

## Numeric promotion & casting

```java
// Implicit widening — small → big, always safe
int i = 5;
long l = i;          // int → long: OK
double d = i;        // int → double: OK

// Narrowing — big → small, REQUIRES a cast, may lose data
long l = 100L;
int i = (int) l;            // explicit cast
double d = 3.99;
int t = (int) d;            // 3 — truncates toward zero

// Mixed arithmetic promotes to the WIDEST type:
int a = 5, b = 2;
double q = a / b;           // 2.0 — division happened in INT first!
double r = (double) a / b;  // 2.5 — cast makes it double division

// byte/short arithmetic promotes to int:
byte x = 1, y = 2;
byte z = (byte) (x + y);    // x + y is int — cast needed to store back
```

## Operators

```java
// Arithmetic
+ - * / %          // % = remainder: 7 % 3 = 1, -7 % 3 = -1 (sign of dividend!)

// Increment/decrement — pre vs post
int i = 5;
int a = i++;       // a = 5, i = 6  (post: old value)
int b = ++i;       // b = 7, i = 7  (pre: new value)

// Comparison
== != < > <= >=    // == on objects = REFERENCE comparison!

// Logical — short-circuit
&& || !            // && || short-circuit: right side only evaluated if needed

// Ternary
int max = (a > b) ? a : b;

// Bitwise — int/long only
& | ^ ~ << >> >>>  // >>> is unsigned right shift

// String concatenation
String s = "a" + 1 + true;    // "a1true" — everything converts
int tricky = 1 + 2 + "x";     // "3x" — left to right: (1+2)+"x"
String trap = "x" + 1 + 2;    // "x12" — NOT "x3"! Once a String, all concatenation
```

## The wrapper classes & autoboxing

```java
// Every primitive has a wrapper for when an object is needed:
Integer, Long, Double, Float, Boolean, Character, Byte, Short

int n = 42;
Integer boxed = n;         // autoboxing: int → Integer
int back = boxed;          // unboxing: Integer → int

Integer a = 127, b = 127;
a == b                     // true — Integer caches -128..127!
Integer c = 200, d = 200;
c == d                     // FALSE — outside cache, different objects
c.equals(d)                // true — ALWAYS use equals for wrappers

// Null danger:
Integer maybe = null;
int x = maybe;             // NullPointerException on unboxing!
```

## `switch` — the dispatcher

```java
switch (day) {                       // works on int, char, String, enums
    case 1:
        System.out.println("Mon");
        break;                        // forget break → FALLTHROUGH
    case 2: case 3: case 4:           // multiple labels
        System.out.println("Weekday");
        break;
    default:
        System.out.println("?");
}
// Java 14+: switch EXPRESSIONS
String type = switch (day) {
    case 1 -> "Mon";
    case 2, 3, 4 -> "Weekday";
    default -> "?";
};
```

---

**Setup:** Swap two ints, then swap two Integer objects correctly.

**Solution:**
```java
// Primitives — pass by value, swap needs an array or a holder
void swap(int[] arr) {           // arrays are references — the swap sticks
    int t = arr[0]; arr[0] = arr[1]; arr[1] = t;
}

// Integers — NEVER compare with ==; equals only
Integer a = 1000, b = 1000;
a.equals(b)          // true
a == b               // false — no equals() on wrapper values
```

**Key insight:** Java passes *by value* — but for objects, the *reference* is the value, so the object's fields change while the caller's variable can't be reassigned. Wrapper comparison must use `.equals()`.

---

**Setup:** Compute `a/b` correctly for doubles when both are ints.

**Solution:**
```java
double ratio = (double) numerator / denominator;
// The cast makes the FIRST operand double → the division is double division
// Without it: int division (truncation) happens BEFORE the promotion
```

**Key insight:** Promotion happens *during* the operation, based on operand types. `(double)a / b` ≠ `(double)(a / b)` — the second truncates first. This is the classic numeric bug.

---

**Setup:** Detect integer overflow reliably.

**Solution:**
```java
int a = 2_000_000_000, b = 2_000_000_000;
int sum = a + b;             // -294967296 — silently wrapped!
if (a > Integer.MAX_VALUE - b) System.out.println("overflow");

// Math.addExact throws on overflow (Java 8+):
int safe = Math.addExact(a, b);     // throws ArithmeticException on overflow
// Also: Math.multiplyExact, Math.subtractExact, Math.toIntExact
```

**Key insight:** Java's `int` overflow is *defined* (wraps in two's complement) — no error by default. Use `Math.*Exact` where correctness matters (counters, prices).

---

**Setup:** Reverse a string using a loop (no StringBuilder).

**Solution:**
```java
String reverse(String s) {
    char[] chars = s.toCharArray();
    for (int i = 0, j = chars.length - 1; i < j; i++, j--) {
        char t = chars[i]; chars[i] = chars[j]; chars[j] = t;
    }
    return new String(chars);
}
```

**Key insight:** The two-pointer swap — the same algorithm from DSA. Strings are immutable so we convert to a char array, mutate, and build a new String.

---

## Practice (try before peeking)

1. `double x = 7 / 2;` — what is x? How do you get 3.5?
2. `Integer a = 128; Integer b = 128;` — is `a == b` true?
3. `String s = 1 + 2 + "3";` — what is s?

<details><summary>Answers</summary>

1. `3.0` — int division first. Cast: `double x = 7.0 / 2;` or `(double) 7 / 2`.
2. `false` — the Integer cache covers only -128..127; 128 is a new object each time.
3. `"33"` — `1 + 2` is int addition (3), then `3 + "3"` concatenates to "33". Order matters.

</details>

---

**Common traps:**
- `==` on wrappers (Integer/Long/etc.) — use `.equals()`
- Integer division truncation — cast before dividing
- `String` concatenation ordering with `+` mixing numbers and strings
- Unboxing null → NPE (`Integer i = null; int x = i;`)
- `switch` fallthrough without `break`
- `char` is 2 bytes (Unicode), not 1 — `sizeof` thinking from C doesn't apply

---
