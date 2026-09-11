# Control Flow & Loops

Python's control flow is where its readability philosophy shows: `if`/`elif`/`else` with no parentheses, `for` loops over *anything iterable*, `while` loops with `else` clauses, and `break`/`continue`/`else` control — including Python's unique `for...else`.

**The Intuition:** `for` loops in Python iterate over *iterables* (lists, strings, dicts, files, ranges), not over indices. When you genuinely need an index, you use `enumerate`. `while` loops run until a condition turns falsy. The `else` on loops is Python's "no break happened" marker — it fires only if the loop ended *naturally*.

## if / elif / else

```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:      # Python's "else if" — no switch statement!
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"

# Ternary — one-line conditional expression
status = "pass" if score >= 50 else "fail"

# Chained comparisons — Pythonic, reads like math
if 0 <= score <= 100:      # means 0 <= score AND score <= 100
    print("valid")
if 80 <= score < 90:
    print("B range")
```

## for loops — over iterables

```python
for i in range(5):            # 0..4 — range is LAZY (no big list created)
    print(i)

for i in range(2, 10, 2):     # 2,4,6,8 — start, stop, step
    print(i)

for i in range(10, 0, -1):    # 10..1 — descending
    print(i)

# Iterate with index
for i, ch in enumerate("hello"):     # i, ch pairs
    print(i, ch)

# Iterate over two lists in parallel
for name, score in zip(names, scores):
    print(name, score)

# Iterate a dict
d = {"a": 1, "b": 2}
for k in d:              # keys
for v in d.values():     # values
for k, v in d.items():   # both — the common one

# Iterate in sorted order without modifying
for x in sorted(lst):
for x in sorted(set(lst), key=len):   # by length
```

## while loops

```python
n = 10
while n > 0:
    print(n)
    n -= 1          # no ++ / -- in Python — use += / -=

# while with input loop (read until sentinel)
while True:
    line = input("> ")
    if line == "quit":
        break
    if not line:
        continue      # skip empty, go back to top
    print(line.upper())
```

## break / continue / else

```python
# for...else — the else runs if NO break happened
for n in range(2, 10):
    for d in range(2, n):
        if n % d == 0:
            break
    else:                      # no divisor found → prime
        print(n, "is prime")
# 2 3 5 7 are prime

# Search loop: find first occurrence
for item in data:
    if matches(item):
        print("found:", item)
        break
else:
    print("not found")          # only if no break
```

**Key insight:** The `else`-on-loop replaces a "found" flag — a whole class of `found = False; ... if found:` patterns collapses into `break`/`else`.

## range vs list

```python
range(10)         # range(0, 10) — lazy, O(1) memory, iterable
list(range(10))   # [0..9] — materialized, O(n) memory
# for 10 million items: range wins big on memory
```

---

**Setup:** Print a multiplication table 1–12 for a given number, formatted.

**Solution:**
```python
n = 7
for i in range(1, 13):
    print(f"{n} × {i:2} = {n * i:3}")
# 7 ×  1 =   7
# ...
```

**Key insight:** The `:2` and `:3` width specifiers right-align — the table lines up. Formatting inside the f-string is the readable approach.

---

**Setup:** Find all primes up to N using a loop with `else`.

**Solution:**
```python
def primes_up_to(n):
    primes = []
    for num in range(2, n + 1):
        for d in range(2, int(num**0.5) + 1):
            if num % d == 0:
                break
        else:
            primes.append(num)
    return primes
```

**Key insight:** The inner `for...else` appends only when no divisor was found. Checking only up to √n is the classic optimization — if n has a factor, it has one ≤ √n.

---

**Setup:** Find the index of the first negative number in a list, or report none.

**Solution:**
```python
nums = [3, 5, -1, 8]
for i, x in enumerate(nums):
    if x < 0:
        print(f"First negative at index {i}")
        break
else:
    print("No negatives")
```

**Key insight:** `enumerate` gives index and value together; `break`/`else` handles the "not found" case without a flag variable.

---

**Setup:** Print a right triangle of `*` of height n, using nested loops.

**Solution:**
```python
n = 5
for i in range(1, n + 1):
    print("*" * i)
# *
# **
# ***
# ****
# *****
```

**Key insight:** String multiplication (`"*" * i`) replaces the inner loop entirely — a reminder that Python's operators often eliminate explicit loops.

---

## Practice (try before peeking)

1. Write a `while` loop that prints the Collatz sequence from n until reaching 1 (even → n//2, odd → 3n+1).
2. Using `for...else`, check if a list is sorted ascending.
3. What does `[i for i in range(10) if i % 3 == 0]` produce?

<details><summary>Answers</summary>

1. `while n != 1: print(n); n = n // 2 if n % 2 == 0 else 3 * n + 1`.
2. `for i in range(1, len(lst)): if lst[i] < lst[i-1]: break; else: print("sorted")`.
3. `[0, 3, 6, 9]` — but that's a list comprehension, coming in the comprehensions note.

</details>

---

**Common traps:**
- Modifying a list while iterating it: `for x in lst: lst.remove(x)` skips elements — iterate over a copy `lst[:]`
- `range` vs `list(range)`: `range` is not a list; you can't index with a float and it's lazy
- No `switch`/`case` in Python (pre-3.10) — `match` in 3.10+ exists but if/elif is the norm
- `while` with no exit condition = infinite loop; ensure the condition eventually falsifies
- Comparing in loops: `for i in range(len(lst))` when you could `for x in lst` — prefer direct iteration

---
