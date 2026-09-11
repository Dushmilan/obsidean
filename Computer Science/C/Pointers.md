# Pointers

Pointers are what make C powerful and dangerous. A pointer is a variable that holds a memory address. With `*` you read/write the memory it points to; with `&` you take an address; with pointer arithmetic you step through arrays. Every serious C program — and every data structure in your DSA notes — is built on them.

**The Intuition:** Memory is a giant array of bytes; every byte has an address. A pointer is just an integer containing an address, typed so the compiler knows how many bytes each step covers. `int *p` says "p holds the address of an int." `*p` says "go to that address and act on the int there." The type on the pointer tells the compiler the *size* of what's being pointed at — `p + 1` steps one `int` (4 bytes), not one byte.

## Syntax — the three pieces

```c
int x = 42;
int *p;          // declaration: p is a pointer to int
p = &x;          // &x = address of x → p now points to x
*p = 100;        // *p = dereference → write 100 into x

// Combined:
int *q = &x;     // declare AND point
```

## The golden rules

```c
// 1. A pointer must point at something valid before you dereference
int *p;              // UNINITIALIZED — garbage address
*p = 5;              // UB — writing to a random address (likely segfault)

// 2. NULL means "points to nothing" — check before dereferencing
int *p = NULL;
if (p != NULL) *p = 5;      // safe guard

// 3. Dereference on the left = WRITE, on the right = READ
*p = 10;              // write 10 to the pointed-to location
int y = *p;           // read the pointed-to value into y

// 4. & on a variable gives its address; * on a pointer gives its target
// They are inverses:  *(&x) == x  and  &(*p) == p
```

## Pointer arithmetic

```c
int arr[5] = {10, 20, 30, 40, 50};
int *p = arr;           // points to arr[0]

p + 1     // address of arr[1] — steps sizeof(int)=4 bytes
p + 3     // address of arr[3]
*(p + 2)  // 30 — value at arr[2]
p[2]      // 30 — indexing is syntactic sugar for *(p + 2)

p++;      // now points to arr[1]
p--;      // back to arr[0]

// Difference of pointers = distance in ELEMENTS
int *a = &arr[1];
int *b = &arr[4];
b - a     // 3 — elements apart

// char pointers step 1 byte at a time
char *cp = "hello";
cp + 1    // points at 'e' — 1 byte
```

## Pointers to pointers

```c
int x = 5;
int *p = &x;        // p → x
int **pp = &p;      // pp → p → x

**pp = 7;           // x is now 7
*pp                 // the pointer p (address of x)
pp                  // address of p

// Used for: arrays of strings, and modifying a pointer INSIDE a function
```

## Arrays of strings — the classic

```c
char *names[] = {"Ada", "Bob", "Cy"};
// names is an array of 3 char pointers
names[0]      // pointer to "Ada"
names[1][0]   // 'B' — first char of names[1]
strlen(names[2])   // 2 — "Cy"

// The usual way to write main:
int main(int argc, char *argv[])   // argv is char** — array of char*
// argv[i] is the i-th command-line argument string
```

## Function pointers — functions as data

```c
int add(int a, int b) { return a + b; }
int mul(int a, int b) { return a * b; }

int (*op)(int, int);      // op is a pointer to a function taking two ints
op = add;                 // point at add
op(3, 4);                 // 7 — call through the pointer
op = mul;
op(3, 4);                 // 12

// Function pointers enable callbacks & qsort's comparator:
int cmp_int(const void *a, const void *b) {
    int x = *(const int *)a, y = *(const int *)b;
    return (x > y) - (x < y);
}
qsort(arr, n, sizeof(int), cmp_int);   // pass the function itself
```

## The void pointer

```c
void *p;        // "pointer to nothing in particular" — generic address
// Must CAST to a typed pointer before dereferencing:
int x = 5;
void *vp = &x;
int *ip = (int *)vp;      // cast back
*ip;                      // 5
// malloc returns void* — that's why you often see casts:
int *arr = (int *)malloc(10 * sizeof(int));
```

---

**Setup:** Write a function that reverses a string in place using pointers.

**Solution:**
```c
void reverse(char *s) {
    char *end = s;
    while (*end) end++;      // walk to the '\0'
    end--;                   // back to last real char
    while (s < end) {
        char t = *s;
        *s++ = *end;
        *end-- = t;
    }
}
```

**Key insight:** Two pointers close in from the ends — the same two-pointer algorithm from DSA, expressed with pointer arithmetic. `*s++` dereferences *then* advances — the post-increment works on the pointer.

---

**Setup:** Implement a linked list node and a print function.

**Solution:**
```c
struct Node {
    int value;
    struct Node *next;
};

void print_list(const struct Node *head) {
    for (const struct Node *cur = head; cur != NULL; cur = cur->next)
        printf("%d → ", cur->value);
    printf("NULL\n");
}
```

**Key insight:** `->` is `(*p).field`. Walking with `cur = cur->next` is the traversal pattern from your DSA Linked Lists notes — pointers are the connective tissue of every linked structure.

---

**Setup:** Find the largest value in an array using a pointer walk.

**Solution:**
```c
int array_max(const int *arr, int n) {
    const int *end = arr + n;        // one past the last element
    int best = *arr;
    for (const int *p = arr + 1; p < end; p++)
        if (*p > best) best = *p;
    return best;
}
```

**Key insight:** The `end` pointer idiom — "one past the last element" — makes loops compare pointers, not counters. This is how `std::end` and range loops work in C++ and it's the idiomatic C pointer-walk.

---

**Setup:** What happens if you `free(p)` then use `*p`?

**Solution:** **Use-after-free** — undefined behavior. The memory is returned to the allocator; the pointer still holds the old address (a *dangling pointer*). Reading/writing it may work (corrupting reused memory), crash, or silently produce wrong data. Set `p = NULL` after `free(p)` to make the bug obvious.

**Key insight:** Dangling pointers (pointing to freed or out-of-scope memory) are the most insidious C bugs because they're intermittent. The double-free (`free` twice) is equally fatal — the allocator's bookkeeping is corrupted.

---

## Practice (try before peeking)

1. `int *p;` then `*p = 42;` — safe? What should you do first?
2. If `arr` is `int[5]`, what's `arr + 1` in *bytes* past `arr`?
3. `int x; int *p = &x; int **pp = &p;` — write 99 into x through pp.

<details><summary>Answers</summary>

1. Unsafe — `p` is uninitialized; `*p = 42` writes to a garbage address (UB). Initialize: `p = &some_int;` or `p = malloc(...)` or `p = NULL` and check.
2. 4 bytes (one `sizeof(int)`) — pointer arithmetic scales by the pointed-to type.
3. `**pp = 99;`.

</details>

---

**Common traps:**
- Uninitialized pointers (garbage address) — always initialize
- Dereferencing NULL — segfault; check first
- Type mismatch: `int *` assigned a `char *` — compiler warning, wrong stepping
- `p = x` vs `*p = x` — assigning the address vs the value
- Dangling pointers after `free` or after the function returned — set to NULL
- Pointer arithmetic on `void*` — not allowed in standard C (no size to step by)

---
