# Operators & Control Flow

C's operators and control flow come straight from the machine — the `if`/`for`/`while` shapes match assembly-level jumps, and the operator set includes C-specific tricks (increment, ternary, comma, bitwise) that appear constantly in real C code.

**The Intuition:** C control flow is structural sugar over jumps. A `for` loop is a counter + condition + jump. The operators are a compact vocabulary: `++` is "increment this variable" (one instruction), `?:` is "if-else as an expression," and the bitwise operators manipulate individual bits — the level where C lives.

## Arithmetic & assignment

```c
int a = 10, b = 3;
a + b, a - b, a * b, a / b, a % b   // 13, 7, 30, 3, 1
// Integer division truncates toward zero: -7/2 == -3 (NOT -4, unlike Python)

a += 5;      // a = a + 5   (also -=, *=, /=, %=, <<=, >>=, &=, |=, ^=)
a++;         // post-increment: use a, THEN add 1
++a;         // pre-increment:  add 1, THEN use a
```

```c
// ++ in expressions — subtle:
int x = 5;
int y = x++;    // y = 5, x = 6   (post: returns old value)
int z = ++x;    // z = 7, x = 7   (pre: returns new value)
// In isolation (x++; alone) they're identical — keep it that way
```

## Comparison & logic

```c
int x = 5;
x == 5    // 1 (true)
x != 5    // 0 (false)
x < 10    // 1
x > 5     // 0
x >= 5    // 1

// Logical AND/OR — short-circuit!
if (a != 0 && b / a > 2)   // b/a only evaluated if a != 0
if (ptr != NULL && ptr->x) // safe dereference pattern

// In C, "true" is any non-zero value; "false" is zero
// Common idiom: 
if (x)         // if x != 0
if (!ptr)      // if ptr == NULL
```

## Bitwise operators — the C specialty

```c
unsigned char a = 0b1100;   // 12
unsigned char b = 0b1010;   // 10

a & b    // 0b1000 (8)   — AND
a | b    // 0b1110 (14)  — OR
a ^ b    // 0b0110 (6)   — XOR
~a       // 0b0011 (3, in 4 bits) — NOT (inverts all bits)
a << 2   // 0b110000 (48) — shift left = ×4
a >> 2   // 0b0011 (3)   — shift right = ÷4

// Common uses:
// Set bit 3:      x |= (1 << 3)
// Clear bit 3:    x &= ~(1 << 3)
// Test bit 3:     if (x & (1 << 3))
// Toggle bit 3:   x ^= (1 << 3)
// Power of 2?     (x & (x - 1)) == 0
// Even/odd:       x & 1
// Swap:           a ^= b; b ^= a; a ^= b;   (XOR swap — mostly a curiosity)
```

## Ternary & comma

```c
int max = (a > b) ? a : b;        // expression if-else
int abs_val = (x < 0) ? -x : x;

// Comma operator — evaluate left, discard, return right
int y = (x = 5, x * 2);    // x = 5, then y = 10
// Rarely needed — mostly seen in for loops:
for (int i = 0, j = n - 1; i < j; i++, j--) { ... }
```

## Control flow

```c
// if / else if / else — same shape as all C-family languages
if (score >= 90) {
    grade = 'A';
} else if (score >= 80) {
    grade = 'B';
} else {
    grade = 'F';
}

// switch — dispatch on integer values (NOT strings)
switch (day) {
    case 0: printf("Sun"); break;
    case 1: printf("Mon"); break;
    case 6: printf("Sat"); break;
    default: printf("?");
}
// Forgetting break = FALLTHROUGH (runs next case) — intentional or bug?
// The compiler warns with -Wimplicit-fallthrough

// for / while / do-while
for (int i = 0; i < n; i++) { ... }
while (condition) { ... }        // test THEN body
do { ... } while (condition);    // body THEN test — runs at least once

// break / continue
// break  — exit loop immediately
// continue — skip to next iteration
```

## The classic for-loop shapes

```c
// Walk an array
for (int i = 0; i < n; i++) sum += arr[i];

// Walk from both ends (two pointers!)
for (int i = 0, j = n - 1; i < j; i++, j--) { ... }

// Sentinel loop
while ((c = getchar()) != EOF) { ... }

// Infinite loop with exit
for (;;) { if (done) break; }
```

---

**Setup:** Count set bits in an integer (Brian Kernighan's algorithm).

**Solution:**
```c
int count_set_bits(int x) {
    int count = 0;
    while (x) {
        x &= (x - 1);    // clears the lowest set bit
        count++;
    }
    return count;
}
```

**Key insight:** `x & (x-1)` clears the lowest set bit — each iteration removes one bit, so the loop runs once per set bit, not once per bit. This is the same algorithm from your DSA Bit Manipulation notes.

---

**Setup:** Print a number in binary (8 bits).

**Solution:**
```c
void print_binary(unsigned char x) {
    for (int i = 7; i >= 0; i--) {
        putchar((x & (1 << i)) ? '1' : '0');
    }
    putchar('\n');
}
```

**Key insight:** The mask `1 << i` selects bit i; the ternary turns it into a character. Building the shift inside the test is the standard bit-display idiom.

---

**Setup:** Find the largest of three numbers with a single ternary.

**Solution:**
```c
int max3(int a, int b, int c) {
    return (a > b) ? ((a > c) ? a : c) : ((b > c) ? b : c);
}
```

**Key insight:** Nested ternaries express multi-way comparisons as expressions. Readability suffers beyond two levels — prefer if/else in real code, but the pattern is worth recognizing.

---

**Setup:** Why does `if (x = 5)` compile without error?

**Solution:** `=` is assignment — it assigns 5 to x and the *expression's value* (5, truthy) is tested. The condition is always true, and x is silently clobbered. `==` is comparison. Compilers warn (`-Wall` catches it: "suggest parentheses around assignment used as truth value").

**Key insight:** If you genuinely mean assignment-in-condition (rare), write `if ((x = 5))` — the double parens signal intent and silence the warning. The `x == 5` vs `x = 5` confusion is the most common C typo.

---

## Practice (try before peeking)

1. `int x = 3; int y = x++ + ++x;` — what are x and y? (Tricky — and the answer is: it's *undefined behavior*! Don't modify x twice in one expression.)
2. Print even numbers 0..20 with a single for loop using `%`.
3. What does `~0` equal (as a 32-bit int)?

<details><summary>Answers</summary>

1. **Undefined behavior** — modifying the same variable twice between sequence points is UB. The compiler may produce anything. Never write this.
2. `for (int i = 0; i <= 20; i++) if (i % 2 == 0) printf("%d\n", i);`
3. `-1` — inverting all 32 bits of 0 gives all 1s, which is -1 in two's complement.

</details>

---

**Common traps:**
- `=` vs `==` in conditions (the #1 bug) — compile with `-Wall -Wextra` to catch it
- Signed overflow is UB; unsigned wraps — don't assume `INT_MAX + 1` is defined
- Shift counts ≥ bit width is UB; shifting signed negative values is UB
- Fallthrough in `switch` without `break` — usually a bug
- `&&`/`||` short-circuit is guaranteed — don't reorder side effects that need to run unconditionally

---
