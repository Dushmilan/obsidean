# Preprocessor & Header Files

Before compilation, the **preprocessor** transforms your source text: `#include` pastes files, `#define` substitutes macros, `#ifdef` conditionally compiles. Header files (`.h`) declare interfaces; `.c` files define implementations. Getting the header/includes discipline right is what makes multi-file C projects build.

**The Intuition:** The preprocessor is a text-rewriter that runs before the compiler sees your code. `#include <stdio.h>` literally inserts stdio.h's contents at that line. `#define MAX 100` makes the preprocessor replace every `MAX` with `100` before compilation. A `.h` file is a *contract* (what functions exist); the `.c` file is the *implementation* (how they work).

## The common directives

```c
#include <stdio.h>      // system headers — angle brackets, searched in system paths
#include "myfile.h"     // your headers — quotes, searched relative to source

#define MAX_SIZE 100          // object-like macro (constant)
#define SQUARE(x) ((x) * (x)) // function-like macro — parens EVERYWHERE

#ifdef DEBUG               // if DEBUG is defined...
    #define LOG(msg) printf("DBG: %s\n", msg)
#else
    #define LOG(msg)      // empty — compiles to nothing
#endif

#ifndef GUARD_H           // include guard — prevents double inclusion
#define GUARD_H
// ... header content ...
#endif

#undef TEMP               // undefine a macro
#pragma once              // alternative include guard (non-standard but universal)
```

## Why macros need so many parens

```c
#define SQUARE(x) ((x) * (x))
// Without parens:  #define SQUARE(x) x * x
SQUARE(2 + 3)    // 2 + 3 * 2 + 3 = 11, not 25!
// With parens: ((2+3) * (2+3)) = 25 ✓

// The argument-evaluation trap:
SQUARE(i++)    // ((i++) * (i++)) — i incremented TWICE! 
// Prefer inline functions over macros when possible:
static inline int square(int x) { return x * x; }
```

## Header organization — the discipline

```c
// point.h — the INTERFACE (what everyone else sees)
#ifndef POINT_H
#define POINT_H

typedef struct { double x, y; } Point;

Point make_point(double x, double y);
double distance(const Point *a, const Point *b);

#endif

// point.c — the IMPLEMENTATION (compiled separately)
#include "point.h"
#include <math.h>

Point make_point(double x, double y) {
    Point p = {x, y};
    return p;
}

double distance(const Point *a, const Point *b) {
    double dx = a->x - b->x, dy = a->y - b->y;
    return sqrt(dx * dx + dy * dy);
}

// main.c — the USER
#include "point.h"
#include <stdio.h>
int main(void) {
    Point a = make_point(0, 0), b = make_point(3, 4);
    printf("%.1f\n", distance(&a, &b));   // 5.0
    return 0;
}
```

**Build:**
```bash
# Compile each .c to an object file, then link:
gcc -Wall -Wextra -c point.c -o point.o
gcc -Wall -Wextra -c main.c -o main.o
gcc point.o main.o -o prog -lm
# or all at once:
gcc -Wall -Wextra point.c main.c -o prog -lm
# -lm links the math library (sqrt needs it)
```

## Include guards — why

```c
// If main.c includes both "a.h" and "b.h", and both include "common.h",
// common.h would be pasted TWICE — duplicate typedef/struct → compile error.
// The guard #ifndef/#define makes the second inclusion a no-op.
```

## `static` for file privacy

```c
// In point.c:
static int instances = 0;        // visible ONLY in this file
static void internal_helper(void) { ... }   // not exported

// Functions/globals without static have EXTERNAL linkage —
// visible across files (that's how main.c calls make_point).
// Keep everything else static: minimal surface, fewer conflicts.
```

## Conditional compilation — one source, many builds

```c
// config.h
#define FEATURE_FAST     1
#define PLATFORM_LINUX   0
#define PLATFORM_WINDOWS 1

// main.c
#ifdef FEATURE_FAST
    // fast path
#else
    // safe path
#endif

// Passed on the command line:
// gcc -DDEBUG main.c        — defines DEBUG
// gcc -DMAX_SIZE=500 main.c — defines MAX_SIZE as 500
// gcc -UDEBUG main.c        — undefines

// Common pattern: debug builds
#ifdef DEBUG
    #define CHECK(cond) do { if (!(cond)) { fprintf(stderr, "FAIL %s:%d: %s\n", __FILE__, __LINE__, #cond); abort(); } } while (0)
#else
    #define CHECK(cond)
#endif
```

## `#` and `##` — stringizing & token pasting

```c
#define STR(x) #x          // stringize: STR(hello) → "hello"
#define CAT(a, b) a ## b   // paste: CAT(foo, bar) → foobar

// The do-while(0) macro idiom — makes a multi-statement macro safe:
#define LOG_ERR(msg) do { \
    fprintf(stderr, "%s\n", msg); \
    exit(1); \
} while (0)
// so that:  if (x) LOG_ERR("bad");  works as one statement
```

---

**Setup:** Write a reusable `safe_malloc` in a header.

**Solution:**
```c
// mem.h
#ifndef MEM_H
#define MEM_H
#include <stdlib.h>
#include <stdio.h>

static inline void *safe_malloc(size_t n) {
    void *p = malloc(n);
    if (p == NULL) {
        fprintf(stderr, "out of memory\n");
        exit(1);
    }
    return p;
}
#endif
```

**Key insight:** `static inline` in a header gives every including file its own copy — no duplicate-symbol link errors, and the compiler can inline it. The NULL check + exit makes OOM a loud failure instead of a silent crash later.

---

**Setup:** Debug-only logging controlled by a compile flag.

**Solution:**
```c
#ifdef DEBUG
    #define DEBUG_LOG(fmt, ...) fprintf(stderr, "DEBUG %s:%d: " fmt "\n", __FILE__, __LINE__, __VA_ARGS__)
#else
    #define DEBUG_LOG(fmt, ...) ((void)0)
#endif

// Usage:
DEBUG_LOG("value = %d", value);
// Build: gcc -DDEBUG main.c   → logging on
//        gcc main.c           → logging compiled out
```

**Key insight:** `__VA_ARGS__` passes the variable arguments through; `__FILE__`/`__LINE__` bake in the source location. In non-debug builds the macro compiles to nothing — zero runtime cost.

---

**Setup:** Fix a "duplicate definition" linker error.

**Solution:** The classic cause: a definition (not declaration) in a header included by two `.c` files.
```c
// WRONG in common.h:  int counter = 0;      — a DEFINITION, duplicated per .c
// RIGHT:             extern int counter;    — a DECLARATION
// And in exactly ONE .c file:  int counter = 0;
```
Or use `static`/`static inline` for header-scope helpers.

**Key insight:** Headers declare (`extern int x;`, prototypes); exactly one `.c` file defines. `const` globals are an exception (they have internal linkage by default in C).

---

**Setup:** Why does this fail to compile? `#define MAX(a, b) a > b ? a : b` used as `int m = MAX(x, y);`?

**Solution:** It compiles *but is wrong*: `MAX(1, 2) + 3` → `1 > 2 ? 1 : 2 + 3` → evaluates to `5` (operator precedence!). The correct macro is `#define MAX(a, b) ((a) > (b) ? (a) : (b))` — and even that double-evaluates arguments. Prefer a `static inline` function.

**Key insight:** Macros are text substitution, not functions. Every precedence trap comes from unparenthesized expansion. This is the #1 macro bug.

---

## Practice (try before peeking)

1. What does `#include "x.h"` vs `#include <x.h>` differ in?
2. Why do headers need include guards?
3. `#define DOUBLE(x) x + x` — what does `DOUBLE(2) * 3` give?

<details><summary>Answers</summary>

1. Search paths: `""` searches the including file's directory first; `<>` searches system include paths.
2. Without guards, including a header twice (directly + via another header) pastes its contents twice → duplicate typedef/struct/function errors.
3. `2 + 2 * 3` = 8, not 12 — parens missing. Should be `((x) + (x))`.

</details>

---

**Common traps:**
- Missing include guards → duplicate definitions
- Function-like macros without full parens → precedence bugs
- Definitions in headers → duplicate symbol link errors (declare in header, define in one .c)
- `#define` shadowing real identifiers (`#define free ...`) — catastrophic and confusing
- Macros vs inline functions: macros can't be debugged (no symbol), don't respect scope, double-evaluate — prefer inline functions when possible

---
