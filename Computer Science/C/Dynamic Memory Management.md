# Dynamic Memory Management

C programs that need memory sized at *runtime* — linked lists, dynamic arrays, strings that grow — use the heap: `malloc`, `calloc`, `realloc`, `free`. There is no garbage collector. You allocate, you use, you free. Forget to free → leak; free twice → corruption. This is the deal with C.

**The Intuition:** The stack holds locals and auto-clears when functions return. The heap is a large pool you draw from and must return. `malloc` hands you a block; `free` returns it. The program owns everything it allocates — the allocator trusts you to return blocks exactly once, and it doesn't forgive mistakes.

## The four functions

```c
#include <stdlib.h>

// malloc — allocate n bytes, UNINITIALIZED
int *arr = malloc(10 * sizeof(int));      // 40 bytes (typical)
if (arr == NULL) { /* handle out of memory */ }
arr[0] = 5;                                // use it

// calloc — allocate AND zero-fill, takes count & size
int *zeroed = calloc(10, sizeof(int));     // all zeros

// realloc — resize an existing block (may MOVE it)
arr = realloc(arr, 20 * sizeof(int));      // grow to 20 ints
// WARNING: realloc may return a NEW pointer — old one is freed.
// The standard idiom:
int *tmp = realloc(arr, new_size);
if (tmp != NULL) arr = tmp;                // only update on success

// free — return the block
free(arr);
arr = NULL;                                 // good practice: kill dangling pointer
```

## malloc & the cast

```c
int *p = malloc(10 * sizeof(int));        // C: no cast needed (void* converts)
// In C++, you MUST cast:  int *p = (int*)malloc(...);
// Use sizeof(*p) so the type is stated once:
int *p = malloc(10 * sizeof(*p));         // safe against type changes
```

## Stack vs Heap

| | Stack | Heap |
|--|-------|------|
| Allocation | Automatic (function locals) | Manual (malloc/free) |
| Size limit | Small (typically 1–8 MB) | Large (system memory) |
| Lifetime | Until function returns | Until free() |
| Speed | Fast (SP increment) | Slower (allocator bookkeeping) |
| Failure | Stack overflow (crash) | malloc returns NULL |
| Typical use | Small, short-lived data | Large, dynamic, returned data |

## Ownership rules

1. Every `malloc`/`calloc`/`realloc` must eventually be matched by exactly one `free`
2. Free **once**, after the last use — never before, never after, never twice
3. After `free`, don't touch the memory (dangling pointer)
4. Who allocates decides who frees — document it (often the caller frees what a function returns)

```c
// Pattern: allocate in one function, free in the caller
char *read_line(FILE *f) {
    char *buf = malloc(1024);
    if (buf == NULL) return NULL;
    // ... read into buf ...
    return buf;              // caller owns buf
}

int main(void) {
    char *line = read_line(f);
    // use line...
    free(line);              // caller frees
}
```

## The failure modes

```c
// MEMORY LEAK — allocated, never freed
void leaky(void) {
    int *p = malloc(100 * sizeof(int));
    // ... no free(p) ...
}   // p is lost — the 400 bytes are unreachable forever

// USE-AFTER-FREE
int *p = malloc(sizeof(int));
free(p);
*p = 5;              // UB — p is dangling

// DOUBLE-FREE
int *p = malloc(sizeof(int));
free(p);
free(p);             // UB — allocator bookkeeping corrupted

// LOST UPDATE via realloc
int *p = malloc(10 * sizeof(int));
p = malloc(20 * sizeof(int));   // LEAK — original 10-block lost
// and if the second malloc fails, p becomes NULL and the old data is GONE
```

## Detecting errors — valgrind

```bash
gcc -g -Wall -o prog prog.c      # -g keeps debug symbols
valgrind --leak-check=full ./prog
# Reports: leaks (definitely/indirectly lost), invalid reads/writes,
#          use-after-free, double-free — with stack traces.
```

## Real structures — dynamic array and linked list

```c
// Dynamic array (growable) — the C version of ArrayList
typedef struct {
    int *data;
    int size;
    int capacity;
} IntVec;

IntVec vec_new(int cap) {
    IntVec v = {malloc(cap * sizeof(int)), 0, cap};
    if (v.data == NULL) v.capacity = 0;
    return v;
}

void vec_push(IntVec *v, int x) {
    if (v->size == v->capacity) {          // grow: double capacity
        int new_cap = v->capacity * 2;
        int *tmp = realloc(v->data, new_cap * sizeof(int));
        if (tmp == NULL) return;           // keep old data on failure
        v->data = tmp;
        v->capacity = new_cap;
    }
    v->data[v->size++] = x;
}

void vec_free(IntVec *v) {
    free(v->data);
    v->data = NULL;
    v->size = v->capacity = 0;
}
```

---

**Setup:** Allocate, fill, and sum a user-sized array.

**Solution:**
```c
int n;
printf("How many numbers? ");
scanf("%d", &n);
int *arr = malloc(n * sizeof(*arr));
if (arr == NULL) return 1;               // handle OOM

for (int i = 0; i < n; i++)
    arr[i] = i * i;

long sum = 0;
for (int i = 0; i < n; i++) sum += arr[i];
printf("Sum: %ld\n", sum);

free(arr);                               // matching free
```

**Key insight:** Every successful `malloc` is matched by a `free`. The NULL check after malloc is non-negotiable — on constrained systems (embedded, servers under pressure) it does happen.

---

**Setup:** Copy a string into freshly allocated memory (reimplement `strdup`).

**Solution:**
```c
char *my_strdup(const char *s) {
    char *copy = malloc(strlen(s) + 1);   // +1 for the '\0'
    if (copy == NULL) return NULL;
    strcpy(copy, s);                       // safe: copy has room
    return copy;                           // caller frees
}
```

**Key insight:** The `+1` for the null terminator is the canonical off-by-one that causes overflows. `strdup` exists in POSIX and does exactly this — worth knowing both the function and what it does under the hood.

---

**Setup:** Build a linked list of numbers read from input, then free it.

**Solution:**
```c
struct Node { int value; struct Node *next; };

// Prepend (each new node becomes head)
struct Node *head = NULL;
int x;
while (scanf("%d", &x) == 1) {
    struct Node *node = malloc(sizeof(*node));
    if (node == NULL) break;
    node->value = x;
    node->next = head;
    head = node;
}

// Free the whole list
struct Node *cur = head;
while (cur != NULL) {
    struct Node *next = cur->next;   // save before freeing!
    free(cur);
    cur = next;
}
```

**Key insight:** The free loop must save `next` *before* `free(cur)` — after freeing, reading `cur->next` is use-after-free. This ordering is the classic linked-list cleanup pattern.

---

**Setup:** What does `realloc` do if it can't grow in place?

**Solution:** It allocates a new larger block, **copies** the old contents, frees the old block, and returns the new pointer. That's why you never do `p = realloc(p, ...)` naively — if realloc fails, it returns NULL *and the original block is still allocated* (leaked, because you overwrote p). Always use a temporary.

**Key insight:** Realloc can move memory — any other pointers into that block become dangling. This is why growing a vector invalidates pointers into it (same as C++ `std::vector`).

---

## Practice (try before peeking)

1. `int *p = malloc(100);` — how many bytes? Is that 100 ints?
2. What's wrong with `p = malloc(10 * sizeof(int)); free(p); p = malloc(5 * sizeof(int));`?
3. When should you use `calloc` over `malloc`?

<details><summary>Answers</summary>

1. 100 *bytes* — that's only 25 ints (if int is 4 bytes). Always `malloc(n * sizeof(int))` or `malloc(n * sizeof(*p))`.
2. Nothing is wrong per se — re-malloc after free is fine. The bug would be using `p` between free and realloc.
3. `calloc(n, size)` when you need zero-initialized memory (safer for arrays you'll partially fill) — it also guards against `n * size` integer overflow.

</details>

---

**Common traps:**
- `sizeof` mistakes: `malloc(n)` instead of `malloc(n * sizeof(int))`
- Missing NULL checks after allocation (embedded/constrained systems)
- Leaks in early-return paths — every `return` before `free` is a leak (use goto-cleanup or restructure)
- `realloc` overwriting the only pointer on failure
- Freeing stack memory or string literals — `free("literal")` is UB

---
