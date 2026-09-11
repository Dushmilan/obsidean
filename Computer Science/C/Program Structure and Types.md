# C Program Structure & Types

C is a compiled, statically typed language close to the hardware. Every C program is a set of functions; the entry point is `main`. Types have fixed sizes, variables must be declared before use, and there's no garbage collector — you see exactly what the machine does.

**The Intuition:** C is a translation of assembly into a readable form. A `char` is one byte; an `int` is (usually) four. When you write `int x = 5;`, the compiler reserves 4 bytes and stores the bit pattern for 5. When you write `printf("%d", x)`, the format specifier tells the function *how to interpret those bits*.

## Program skeleton

```c
#include <stdio.h>      // preprocessor: paste in stdio.h (printf, scanf)
#include <stdlib.h>     // malloc, free, atoi, rand

// Function prototype — declaration before use
int add(int a, int b);

// Entry point — every program needs one
int main(void) {        // (void) = no command-line args
    printf("Hello\n");
    return 0;           // 0 = success to the OS; non-zero = error
}
// main() with args:
// int main(int argc, char *argv[])  — argc = count, argv = strings
```

## Fundamental types

| Type | Typical size | Range (approx) | printf specifier |
|------|-------------|----------------|------------------|
| `char` | 1 byte | -128..127 (signed) | `%c` / `%d` |
| `short` | 2 bytes | ±32k | `%hd` |
| `int` | 4 bytes | ±2.1 billion | `%d` |
| `long` | 4/8 bytes (platform) | varies | `%ld` |
| `long long` | 8 bytes | ±9.2 quintillion | `%lld` |
| `float` | 4 bytes | ~7 sig digits | `%f` |
| `double` | 8 bytes | ~15 sig digits | `%lf` / `%f` |
| `_Bool` | 1 byte | 0/1 | `%d` |

**Note:** sizes are *minimums* in the standard; actual sizes depend on the platform. `sizeof(int)` tells you at compile time. Use `<stdint.h>` for fixed-width types (`int32_t`, `uint64_t`).

## Unsigned & modifiers

```c
unsigned int u = 4000000000;    // no negatives, doubles the top end
signed char c = -5;
long double d;                  // extended precision
const int MAX = 100;            // read-only — compiler-enforced

// sizeof — number of BYTES
sizeof(int)          // 4 (typical)
sizeof('a')          // 1 in C (int in C++ — a famous difference!)
```

## Declaration vs assignment

```c
int x;          // declaration — memory reserved, value is GARBAGE
int y = 0;      // declaration + initialization — always init!
x = 42;         // assignment

// Multiple declarations
int a, b, c;
int i = 0, j = 1;
```

**Uninitialized locals contain indeterminate values** — reading them is undefined behavior. Initialize everything.

## Integer literals

```c
42          // decimal
0x2A        // hexadecimal
052         // octal (leading 0) — the classic source of bugs!
42L         // long
42ULL       // unsigned long long
```

## Casting

```c
int a = 7, b = 2;
double d = (double)a / b;    // 3.5 — cast makes division float
double e = a / b;            // 3.0 — both ints → integer division FIRST

char c = 'A';
int ascii = (int)c;          // 65 — though implicit widening happens anyway
```

---

**Setup:** Print the sizes of all fundamental types on your system.

**Solution:**
```c
#include <stdio.h>
int main(void) {
    printf("char: %zu\n", sizeof(char));
    printf("int:  %zu\n", sizeof(int));
    printf("long: %zu\n", sizeof(long));
    printf("long long: %zu\n", sizeof(long long));
    printf("float: %zu\n", sizeof(float));
    printf("double: %zu\n", sizeof(double));
    printf("pointer: %zu\n", sizeof(void *));
    return 0;
}
```

**Key insight:** `%zu` is the specifier for `sizeof`'s return type (`size_t`). This program is the first thing to run on a new platform — it tells you the memory model you're working with.

---

**Setup:** Convert a temperature in Celsius to Fahrenheit using `scanf`.

**Solution:**
```c
#include <stdio.h>
int main(void) {
    double c, f;
    printf("Celsius: ");
    scanf("%lf", &c);          // %lf for double; &c = address of c
    f = (c * 9.0 / 5.0) + 32.0;
    printf("%.2f C = %.2f F\n", c, f);
    return 0;
}
```

**Key insight:** `scanf` needs the *address* (`&`) to write into `c` — C passes by value, so you hand over where to store the result. `9.0/5.0` (not `9/5`) avoids integer division truncation.

---

**Setup:** Parse command-line arguments.

**Solution:**
```c
int main(int argc, char *argv[]) {
    printf("Program: %s\n", argv[0]);
    if (argc > 1) {
        int n = atoi(argv[1]);          // string → int
        printf("First arg as int: %d\n", n);
    }
    return 0;
}
// Run: ./prog 42
// → Program: ./prog
// → First arg as int: 42
```

**Key insight:** `argc` is the argument count (always ≥ 1 — the program name), `argv` is the array of strings. `atoi` parses ints from strings (no error checking — prefer `strtol` for robustness).

---

**Setup:** Demonstrate integer overflow wrapping.

**Solution:**
```c
int x = 2147483647;        // INT_MAX
printf("%d\n", x + 1);     // -2147483648 — wraps around!
// Signed overflow is UNDEFINED BEHAVIOR in the standard —
// compilers may do anything. Use unsigned for defined wraparound.
```

**Key insight:** Signed overflow is undefined behavior in C — a compiler may assume it never happens and optimize accordingly, producing surprising results. Unsigned overflow wraps (defined modulo 2ⁿ). Overflow bugs are the basis of real security vulnerabilities (the classic `n+1` buffer-size check).

---

## Practice (try before peeking)

1. Why is `int main()` vs `int main(void)` different in C?
2. `char c = 255;` with signed char — what value? 
3. What does `printf("%d", 7/2)` print?

<details><summary>Answers</summary>

1. `main(void)` = takes no arguments (prototype known); `main()` = unspecified args in C (pre-ANSI style) — use `(void)`.
2. `-1` — 255 (0xFF) in a signed char is -1 (two's complement). Values above 127 wrap negative.
3. `3` — integer division truncates toward zero. Use `7.0/2` or cast for 3.5.

</details>

---

**Common traps:**
- Leading-zero literals are **octal**: `010` is 8, not 10
- `char` signedness is implementation-defined — use `<stdint.h>` (`int8_t` etc.) for portable code
- Uninitialized variables → garbage values → mysterious bugs
- `scanf("%d", &x)` without `&` compiles (with a warning) and crashes at runtime (segfault)
- `%d` with a `double` or `%f` with an `int` = undefined behavior — specifier must match type exactly

---
