# Structs & Unions

Structs group related data into one type — C's way of modeling records and the foundation of every data structure (nodes, trees, graphs). Unions overlay different views on the same memory. Bit fields pack flags tightly. Together they're the "make your own type" toolkit.

**The Intuition:** A struct is a blueprint: "every Student has a name, age, and GPA." Each student object gets its own copy of those fields, laid out consecutively in memory. A union is the opposite: all members share the *same* memory — it's "one block of storage, interpreted differently." You store one thing; you read it back as another type.

## Struct basics

```c
#include <stdio.h>
#include <string.h>

// Define the type
struct Student {
    char name[50];
    int age;
    double gpa;
};

// Use it
int main(void) {
    struct Student ada = {"Ada", 36, 3.9};   // aggregate init, in order
    struct Student bob = {.name = "Bob", .gpa = 3.5};  // designated (C99) — age = 0

    printf("%s is %d, GPA %.1f\n", ada.name, ada.age, ada.gpa);
    ada.gpa = 4.0;               // modify a field

    // Copy whole structs (shallow)
    struct Student copy = ada;   // all fields copied

    return 0;
}
```

## Typedef — name your structs

```c
typedef struct Student Student;          // now "Student" works without "struct"

// Or all at once:
typedef struct {
    double x, y;
} Point;                                 // anonymous struct + typedef

Point p = {3.0, 4.0};
```

## Struct layout & sizeof

```c
struct Example {
    char c;        // offset 0
    int i;         // offset 4 — NOT 1! Padding after char
    double d;      // offset 8
};
// sizeof(struct Example) == 16 (typically), NOT 13
// The compiler pads to keep each member aligned to its size

// Packing — when you need exact layout (file formats, network protocols):
#pragma pack(push, 1)
struct Packet {
    char type;     // offset 0
    int len;       // offset 1 (no padding)
};
#pragma pack(pop)
// sizeof == 5 — trade: unaligned access is slower, sometimes illegal on some CPUs
```

**Alignment matters:** CPUs read aligned words faster; some (ARM) *fault* on unaligned access. Order struct fields big-to-small to minimize padding: `double, int, char` instead of `char, int, double`.

## Structs and pointers — the `->` operator

```c
struct Point { double x, y; };

void scale(struct Point *p, double f) {
    p->x *= f;         // (*p).x *= f — same thing
    p->y *= f;
}

// Why pointers? Passing a struct BY VALUE copies the whole thing:
void print_by_value(struct Point p) { ... }    // copies 16 bytes
void print_by_ref(const struct Point *p) { ... } // copies 8 bytes (pointer)
// For large structs, by-reference wins. const documents read-only.
```

## Structs containing pointers — the linked structure pattern

```c
typedef struct Node {
    int value;
    struct Node *next;      // self-referential — MUST say "struct Node"
} Node;

// With typedef'd self-reference:
typedef struct Node Node;
struct Node { int value; Node *next; };
```

## Arrays of structs

```c
Student class[3] = {
    {"Ada", 36, 3.9},
    {"Bob", 25, 3.5},
    {"Cy", 30, 3.7}
};

for (int i = 0; i < 3; i++)
    printf("%s\n", class[i].name);

// Sorting with qsort — comparator:
int cmp_gpa(const void *a, const void *b) {
    double ga = ((const Student *)a)->gpa;
    double gb = ((const Student *)b)->gpa;
    return (ga > gb) - (ga < gb);
}
qsort(class, 3, sizeof(Student), cmp_gpa);
```

## Unions

```c
union Number {
    int i;          // all three share the same 8 bytes
    double d;
    char c;
};

union Number n;
n.i = 42;           // now the bytes hold an int
n.d = 3.14;         // REINTERPRETS the same bytes as a double (42 is gone)

// Use: one field at a time. Common for "tagged" values:
struct Variant {
    int type;            // 0 = int, 1 = double
    union {
        int i;
        double d;
    } value;
};

// Reinterpreting bytes (type punning) — technically UB in strict C,
// but common in systems code. Prefer memcpy for portable type punning.
```

## Bit fields — packed flags

```c
struct Flags {
    unsigned int read : 1;     // 1 bit
    unsigned int write : 1;    // 1 bit
    unsigned int exec : 1;     // 1 bit
    unsigned int mode : 5;     // 5 bits
};
// Total: 8 bits = 1 byte (vs 3-4 bytes for three ints)

struct Flags f = {1, 1, 0, 3};
if (f.read && f.write) { ... }
// Portability caveat: bit-field layout/order is implementation-defined —
// fine for internal flags, dangerous for file formats.
```

---

**Setup:** Define a `Rectangle` with a function computing its area, avoiding struct copying.

**Solution:**
```c
typedef struct { double width, height; } Rectangle;

double area(const Rectangle *r) {
    return r->width * r->height;
}

Rectangle box = {10.0, 5.0};
printf("%.1f\n", area(&box));    // 50.0
```

**Key insight:** Passing `const Rectangle *` avoids copying 16 bytes and documents read-only access. The `->` reads fields through the pointer. This is the standard C idiom for "operate on a struct."

---

**Setup:** Build a linked list node that owns a dynamically allocated string.

**Solution:**
```c
typedef struct Node {
    char *name;            // heap-allocated string
    struct Node *next;
} Node;

Node *make_node(const char *name) {
    Node *n = malloc(sizeof(*n));
    if (n == NULL) return NULL;
    n->name = malloc(strlen(name) + 1);   // deep copy — node owns its string
    if (n->name == NULL) { free(n); return NULL; }
    strcpy(n->name, name);
    n->next = NULL;
    return n;
}

void free_node(Node *n) {
    free(n->name);     // free the string first
    free(n);           // then the node
}
```

**Key insight:** Ownership discipline: the node *owns* its `name` string, so the free function must free the string before the node. Shallow copy would leave two nodes pointing at one string — double free.

---

**Setup:** Use a union to store an int-or-double value with a type tag.

**Solution:**
```c
typedef struct {
    int is_double;
    union { int i; double d; } v;
} Value;

void print_value(Value val) {
    if (val.is_double)
        printf("%f\n", val.v.d);
    else
        printf("%d\n", val.v.i);
}

Value a = {0, {.i = 42}};
Value b = {1, {.d = 3.14}};
```

**Key insight:** The tag (`is_double`) tells the reader how to interpret the union — the *discriminated union*, C's answer to Python's dynamic types. This is the same idea as C++ `std::variant` and Rust's enums (but unchecked — the tag is your responsibility).

---

**Setup:** Write a function that returns a `Point` (a struct by value) — is that OK?

**Solution:** Yes — small structs are returned by value fine (the compiler uses registers or a hidden pointer).
```c
Point midpoint(Point a, Point b) {
    Point m = {(a.x + b.x) / 2, (a.y + b.y) / 2};
    return m;
}
Point m = midpoint(p1, p2);
```

**Key insight:** Small structs (≤ 16 bytes) by value are idiomatic and fast. Large structs should go by pointer. The rule: "copy small, reference large."

---

## Practice (try before peeking)

1. `sizeof(struct {char c; int i;})` — what's the likely value and why?
2. Can a union member be a struct? Can a struct contain a union?
3. Why does a struct Node need `struct Node *next` written with the keyword `struct`?

<details><summary>Answers</summary>

1. 8 (typically) — the int needs 4-byte alignment, so the char is padded with 3 bytes.
2. Yes and yes — unions and structs nest freely; a "variant" struct with a union field is the classic pattern.
3. Inside the struct's own definition, the type `Node` isn't complete yet — you must write `struct Node *next` (or pre-declare `typedef struct Node Node;` first).

</details>

---

**Common traps:**
- Shallow copies sharing heap-allocated fields → double free; deep copy when ownership demands it
- Struct padding surprises: `sizeof` ≠ sum of member sizes; file/network layouts need packing
- Returning a pointer to a *local* struct — it's stack memory, dead after return
- Union misuse: reading a member that wasn't the last one written (UB unless it's char[] or via memcpy)
- `->` vs `.`: `p->field` for pointers, `p.field` for values — mixing them is a compile error that beginners misdiagnose

---
