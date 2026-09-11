# Arrays & Strings

Arrays are contiguous blocks of memory. Strings are arrays of `char` ending in `'\0'`. Everything about C arrays flows from one fact: **an array name decays to a pointer to its first element**, and there is no bounds checking — reading past the end silently reads adjacent memory.

**The Intuition:** `int arr[5]` reserves 20 bytes (5 × 4) at consecutive addresses. `arr[2]` means `*(arr + 2)` — start at the base address, step 2 ints forward. "Decay" means passing `arr` to a function hands over just the address of element 0 — the function has no idea how big the array is. That's why you pass the size.

## Array basics

```c
int arr[5];                 // 5 ints, UNINITIALIZED (garbage)
int arr[5] = {1, 2, 3};     // rest zero-filled: {1,2,3,0,0}
int arr[] = {1, 2, 3, 4};   // size inferred = 4
int zeros[100] = {0};       // all zeros — common idiom
int arr[5] = {[0]=7, [3]=9};// designated initializers (C99)

arr[0] = 10;                // index — NO bounds check
int x = arr[4];             // last element
// arr[5] — UNDEFINED BEHAVIOR: reads past the array. No error at runtime.
```

## The decay rule

```c
int arr[5] = {1, 2, 3, 4, 5};

// In expressions, arr "decays" to int* pointing at arr[0]:
int *p = arr;        // same as int *p = &arr[0];
*p                   // 1
*(p + 2)             // 3  — pointer arithmetic: +2 ints, not +2 bytes
p[2]                 // 3  — indexing IS pointer arithmetic
arr[2]               // 3  — identical

// &arr is DIFFERENT — it's int(*)[5], a pointer to the whole array
// sizeof(arr)   → 20 (bytes of the array)
// sizeof(p)     → 8  (bytes of a pointer) — the decay in action
```

**Key consequence:** inside a function, `void f(int arr[])` → parameter is `int *arr`; `sizeof(arr)` is 8, not the array size. Always pass `n`.

## Multi-dimensional arrays

```c
int matrix[3][4];               // 3 rows × 4 cols, contiguous row-major
matrix[1][2] = 7;               // row 1, col 2

// Initialize
int m[2][3] = {{1,2,3},{4,5,6}};

// Passing to functions — all dimensions but the first must be specified:
void print_matrix(int m[][3], int rows) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < 3; j++)
            printf("%d ", m[i][j]);
        printf("\n");
    }
}
// Memory layout: row 0 then row 1 — m[1][0] is right after m[0][2]
```

## C strings — char arrays with '\0'

```c
char s1[] = "Hello";         // 6 bytes: H e l l o \0
char s2[6] = "Hello";        // exactly fits
char s3[10] = "Hello";       // 6 used, 4 zero-filled

// Strings are just arrays — same decay, same no-bounds-checking
char *p = "Hello";           // POINTS to a string LITERAL — read-only!
// p[0] = 'h';               // undefined behavior — literal may live in read-only memory

// String length — walks to '\0'
strlen(s1);                  // 5 — the \0 is NOT counted
sizeof(s1);                  // 6 — the \0 IS counted
```

## String library (`<string.h>`)

```c
#include <string.h>

char buf[50];
strcpy(buf, "Hello");        // copy — NO bounds check! overflow if buf too small
strncpy(buf, "Hello", 49);   // copy at most 49 — may not null-terminate!
buf[49] = '\0';              // safety net after strncpy

strcat(buf, " World");       // append — again, no bounds check
strncat(buf, " World", sizeof(buf) - strlen(buf) - 1);

// Compare — NEVER use == (compares addresses!)
strcmp(a, b) == 0            // equal
strcmp(a, b) < 0             // a < b (lexicographic)
strcmp(a, b) > 0             // a > b

strchr(s, 'o')               // pointer to first 'o', or NULL
strstr(s, "World")           // pointer to first "World", or NULL
strtok(s, ",")               // tokenizer — modifies the string!
```

## Common array algorithms

```c
// Sum
int sum = 0;
for (int i = 0; i < n; i++) sum += arr[i];

// Reverse in place — two pointers
for (int i = 0, j = n - 1; i < j; i++, j--) {
    int t = arr[i]; arr[i] = arr[j]; arr[j] = t;
}

// Find max
int max = arr[0];
for (int i = 1; i < n; i++) if (arr[i] > max) max = arr[i];

// Copy
for (int i = 0; i < n; i++) dest[i] = src[i];
// or: memcpy(dest, src, n * sizeof(int));
```

---

**Setup:** Implement `string_length` and `string_copy` manually (the classic).

**Solution:**
```c
size_t string_length(const char *s) {
    size_t len = 0;
    while (s[len] != '\0') len++;
    return len;
}

char *string_copy(char *dest, const char *src) {
    char *d = dest;
    while ((*d++ = *src++) != '\0')   // copy char, check the copied value
        ;
    return dest;
}
```

**Key insight:** The copy loop's trick — `(*d++ = *src++) != '\0'` — copies a char, checks it's not the terminator, and continues. The assignment-expression's value IS the char just copied, and it stops exactly after copying the `'\0'`. Reading this idiom is essential for reading real C.

---

**Setup:** Check if a string is a palindrome.

**Solution:**
```c
int is_palindrome(const char *s) {
    size_t len = strlen(s);
    for (size_t i = 0, j = len - 1; i < j; i++, j--) {
        if (s[i] != s[j]) return 0;
    }
    return 1;
}
```

**Key insight:** The two-pointer walk from both ends — the same pattern as the DSA two-pointers note. `const char *` documents read-only access.

---

**Setup:** Count words in a string (whitespace-separated).

**Solution:**
```c
int count_words(const char *s) {
    int words = 0;
    int in_word = 0;
    while (*s) {
        if (*s == ' ' || *s == '\t' || *s == '\n') {
            in_word = 0;
        } else if (!in_word) {
            in_word = 1;
            words++;
        }
        s++;
    }
    return words;
}
```

**Key insight:** The `in_word` flag tracks state transitions (state machine!). It's the same logic as a DFA from your Theory of Computation notes — a mini lexical analyzer.

---

**Setup:** Find the most frequent element in an array (assuming values in a known range).

**Solution:**
```c
int most_frequent(int arr[], int n, int max_value) {
    int counts[1000] = {0};     // count array (bounded range)
    for (int i = 0; i < n; i++)
        counts[arr[i]]++;
    int best = 0;
    for (int i = 1; i <= max_value; i++)
        if (counts[i] > counts[best]) best = i;
    return best;
}
```

**Key insight:** Counting arrays are C's poor-man's hash table — O(n) with an O(max_value) count buffer. The same idea powers counting sort in your DSA Sorting notes.

---

## Practice (try before peeking)

1. `char s[4] = "Hello";` — what's wrong?
2. `strlen("")` and `sizeof("")` — what are they?
3. Why is `if (s1 == s2)` wrong for comparing two strings?

<details><summary>Answers</summary>

1. `"Hello"` needs 6 bytes (5 chars + '\0'); `s[4]` holds only 4 — the string literal overflows the array (UB). Compilers usually warn.
2. `strlen("")` = 0 (no chars before '\0'); `sizeof("")` = 1 (the '\0' itself).
3. `==` compares the two *pointers* (addresses), not the contents. Use `strcmp(s1, s2) == 0`.

</details>

---

**Common traps:**
- Buffer overflow: `strcpy`/`strcat`/`sprintf` don't check capacity — the #1 source of C security bugs. Prefer `strncpy`, `snprintf`, and explicit bounds
- Off-by-one with the `'\0'` — allocate `len + 1`, always
- String literals are read-only — `char *p = "x"; p[0]='y'` is UB
- `strcmp` returns 0 on *equality* — `if (strcmp(a,b))` means "if DIFFERENT"
- Multi-dim arrays: only the first dimension may be omitted in parameters — get this wrong and the indexing math is garbage

---
