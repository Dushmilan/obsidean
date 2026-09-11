# Functions & Parameters

Functions in C are the units of composition. The signature — return type, name, parameters — is a contract. Parameters are passed **by value**: the function gets copies. To modify caller data, pass **pointers**. This distinction is the single most important idea in C programming.

**The Intuition:** When you call `f(x)`, C makes a *copy* of `x` and hands it to `f`. Changing the copy inside `f` leaves the caller's `x` untouched. If you want `f` to *change* `x`, you don't pass `x` — you pass its **address** (`&x`), and `f` modifies the memory at that address. This is "pass by value" and "pass by reference (via pointers)."

## Function anatomy

```c
// Prototype (declaration) — lets callers use the function before its definition
int add(int a, int b);

// Definition
int add(int a, int b) {      // returns int, takes two ints
    return a + b;
}

// Void — no return value
void greet(char name[]) {
    printf("Hello, %s!\n", name);
}

// Call
int sum = add(3, 4);         // 7
```

**Always prototype or define before use** — calling an undeclared function is an error in modern C (a warning in old compilers that silently assumed `int`).

## Pass by value — copies

```c
void increment(int n) {
    n++;                     // modifies the COPY
}

int x = 5;
increment(x);
// x is still 5!  The copy was incremented, x untouched.
```

## Pass by pointer — the way to modify

```c
void increment(int *n) {     // takes an ADDRESS
    (*n)++;                  // dereference, then increment
}

int x = 5;
increment(&x);               // pass the address of x
// x is now 6
```

```c
// Swap — the canonical example
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int x = 1, y = 2;
swap(&x, &y);    // x=2, y=1
```

## Why pointers as parameters?

1. **Modify caller data** (swap, counters, output params)
2. **Avoid copying large structs** — passing a struct by value copies the whole thing
3. **Represent "optional" parameters** — a NULL pointer means "not provided"
4. **Output parameters** — functions that "return" multiple values:

```c
// Returns status, and writes results through pointers
int divide(int a, int b, int *quotient, int *remainder) {
    if (b == 0) return 0;            // failure
    *quotient = a / b;
    *remainder = a % b;
    return 1;                        // success
}

int q, r;
if (divide(17, 5, &q, &r)) {
    printf("17/5 = %d rem %d\n", q, r);
}
```

## The `const` qualifier — reading this signature

```c
void process(const int *p);      // p points to data we will NOT modify
                                 // (the int is const; the pointer is not)

void print_struct(const struct Point *pt);  // read-only view — common for big data
```

`const int *p` = pointer to const int (data read-only). `int *const p` = const pointer (can't repoint). Reading `*p` is fine; `*p = x` is a compile error.

## Arrays as parameters — they decay

```c
void sum_array(int arr[], int n) {
    // arr[] is sugar for int *arr — the array DECAYS to a pointer
    // sizeof(arr) here = sizeof(int*), NOT the array size!
}
```

The array is not copied — you get a pointer to the first element, which is why modifications through `arr[i]` affect the caller's array, and why you must pass `n` separately.

## Recursion

```c
int factorial(int n) {
    if (n <= 1) return 1;              // base case — the exit
    return n * factorial(n - 1);       // recursive case
}

int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);    // exponential — see DSA DP notes
}
```

Each recursive call has its *own* copies of parameters and locals on the stack — pass-by-value is what makes recursion safe.

## Static functions & variables

```c
static int helper(void) { ... }        // visible only in THIS file
static int counter = 0;                // file-scope, private to this file

int next_id(void) {
    static int id = 0;                 // persists across calls!
    return id++;
}
// next_id(): 0, 1, 2, ... — static local survives between calls
```

---

**Setup:** Write a function that returns both the min and max of an array.

**Solution:**
```c
void find_min_max(const int arr[], int n, int *min, int *max) {
    *min = *max = arr[0];
    for (int i = 1; i < n; i++) {
        if (arr[i] < *min) *min = arr[i];
        if (arr[i] > *max) *max = arr[i];
    }
}

int lo, hi;
find_min_max(data, 100, &lo, &hi);
```

**Key insight:** Output parameters (`*min`, `*max`) let one function return two values. `const int arr[]` promises the array isn't modified. The caller sees the results through the pointers.

---

**Setup:** Compute the length of a string (reimplement `strlen`).

**Solution:**
```c
size_t my_strlen(const char *s) {
    size_t len = 0;
    while (*s != '\0') {     // walk until null terminator
        s++;                 // advance the pointer
        len++;
    }
    return len;
}
```

**Key insight:** Pointer arithmetic walks the string — `s++` moves to the next char. The `const` guarantees we only read. This is the *shape* of every C string function.

---

**Setup:** Binary search (recursive) on a sorted array.

**Solution:**
```c
int binary_search(int arr[], int lo, int hi, int target) {
    if (lo > hi) return -1;                 // not found
    int mid = lo + (hi - lo) / 2;           // avoids overflow of (lo+hi)
    if (arr[mid] == target) return mid;
    if (arr[mid] < target)
        return binary_search(arr, mid + 1, hi, target);
    return binary_search(arr, lo, mid - 1, target);
}
```

**Key insight:** Recursion mirrors the divide-and-conquer structure from your DSA notes. `lo + (hi - lo)/2` avoids the integer overflow of `(lo+hi)/2` — a real bug in production code. Each call's parameters are independent copies on the stack.

---

## Practice (try before peeking)

1. Why does `swap(a, b)` (not `swap(&a, &b)`) fail to swap?
2. Write `double average(int arr[], int n)` and call it correctly.
3. `void f(int a) { a = 99; }` — after `int x = 5; f(x);`, what's x?

<details><summary>Answers</summary>

1. `swap(a, b)` passes *copies* — the function swaps its local copies; the caller's variables never change. You must pass addresses.
2. `double average(int arr[], int n) { long sum = 0; for (int i=0;i<n;i++) sum += arr[i]; return (double)sum / n; }` — note the cast before division.
3. Still 5 — `a` is a copy. To change x, take `int *a`.

</details>

---

**Common traps:**
- Forgetting `&` when passing to a pointer parameter — compiler warning, runtime segfault
- Forgetting `*` when dereferencing — `n` instead of `*n` compares addresses not values
- Array decay: `sizeof(arr)` inside a function is pointer size (8), not array size — always pass `n`
- Missing prototype → implicit declaration errors (or silent `int` assumption in old code)
- `return` with no value in a non-void function is UB — the caller reads garbage

---
