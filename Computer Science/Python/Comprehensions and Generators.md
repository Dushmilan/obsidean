# Comprehensions & Generators

Comprehensions build new containers in one expression: `[x for x in ...]` (list), `{x for x in ...}` (set), `{k: v for ...}` (dict), `(x for x in ...)` (generator). Generators are Python's lazy sequences — they produce values on demand, never holding the whole collection in memory.

**The Intuition:** A comprehension is a compact for-loop that *returns a collection* — filter with `if`, transform with the expression. A generator is a comprehension that *yields one at a time* — the computation is deferred until you iterate. For huge data, generators are the difference between O(n) memory and O(1).

## The four forms

```python
# List comprehension — eager, builds a list
squares = [x**2 for x in range(10)]

# Set comprehension — dedupes
unique_lens = {len(w) for w in words}

# Dict comprehension
squares_dict = {x: x**2 for x in range(5)}

# Generator expression — LAZY, yields on demand
gen = (x**2 for x in range(10))     # no list built yet!
```

## Anatomy

```python
[ expression  for  item  in  iterable  if  condition ]
#   └─what's stored─┘      └─loop─┘      └─filter─┘

# Both orderings of filter/transform read naturally:
evens = [x for x in range(20) if x % 2 == 0]
doubled_evens = [x * 2 for x in range(20) if x % 2 == 0]
```

## Nested comprehensions — the order is the loop order

```python
# Flat
matrix = [[1, 2, 3], [4, 5, 6]]
flattened = [x for row in matrix for x in row]     # [1,2,3,4,5,6]

# This is equivalent to:
result = []
for row in matrix:
    for x in row:
        result.append(x)

# Transpose
list(zip(*matrix))     # [(1,4),(2,5),(3,6)]
# Or: [[row[i] for row in matrix] for i in range(len(matrix[0]))]
```

## Generators — lazy evaluation

```python
# Generator function — uses yield instead of return
def countdown(n):
    while n > 0:
        yield n          # pause here, resume on next next()
        n -= 1

for x in countdown(3):   # 3, 2, 1
    print(x)

# Generator expression — lazy comprehension
total = sum(x**2 for x in range(10**6))    # O(1) memory!
# vs sum([x**2 for x in range(10**6)])     # O(n) memory — the list is built

# Generators are single-use iterators:
gen = (x for x in range(3))
list(gen)    # [0, 1, 2]
list(gen)    # [] — exhausted!
```

**Why generators matter:** processing a 1 GB log file line by line uses a constant amount of memory; loading it into a list uses a gigabyte. Generators also give *lazy pipelines* — transformations chain without intermediate storage.

## Advanced patterns

```python
# Nested list trap avoided:
matrix = [[0] * 3 for _ in range(3)]      # correct (fresh inner lists)

# Conditional expression inside comprehension (different from filter!)
# [A if cond else B for x in it]  — transforms every element
labels = ["even" if x % 2 == 0 else "odd" for x in range(5)]

# zip with comprehension
pairs = [(a, b) for a, b in zip(keys, values)]

# Building with side effects — avoid, but sometimes needed
# [print(x) for x in data]  — works, returns [None,...] — a smell
```

## itertools — the generator toolbox

```python
import itertools
itertools.chain(a, b)          # flatten multiple iterables lazily
itertools.islice(gen, 10)      # first 10 items without consuming rest
itertools.product([1,2], "ab") # (1,'a'),(1,'b'),(2,'a'),(2,'b')
itertools.combinations("abc", 2)  # ('a','b'),('a','c'),('b','c')
itertools.permutations("abc", 2)
itertools.groupby(sorted(data), key=...)   # consecutive groups
itertools.count(0, 2)          # infinite counter — take with islice
```

---

**Setup:** Given a list of words, find words with more than 3 letters, uppercased.

**Solution:**
```python
words = ["cat", "elephant", "dog", "bird"]
[w.upper() for w in words if len(w) > 3]    # ['ELEPHANT', 'BIRD']
```

**Key insight:** Filter (`if len(w) > 3`) runs before the transform (`w.upper()` applies only to survivors). Reading order matches mental model: "for each word, if long, uppercase it."

---

**Setup:** Build a dict mapping each word to its length, only for unique words.

**Solution:**
```python
{word: len(word) for word in words}
# If duplicates exist, later wins — that's the dedupe.
```

**Key insight:** A dict comprehension's key collision rule is "last write wins" — which conveniently makes it a dedupe with the last occurrence's value.

---

**Setup:** Sum the squares of even numbers from 1 to 10⁶ without building a big list.

**Solution:**
```python
total = sum(x * x for x in range(1, 10**6 + 1) if x % 2 == 0)
```

**Key insight:** The generator expression `(...)` passed directly to `sum` never materializes a list — memory stays O(1) even for a million items. This is the canonical "lazy pipeline" pattern.

---

**Setup:** Read only the first 5 non-empty, non-comment lines of a file.

**Solution:**
```python
import itertools
with open("config.txt") as f:
    lines = (line.strip() for line in f)                    # lazy strip
    useful = (ln for ln in lines if ln and not ln.startswith("#"))
    first5 = list(itertools.islice(useful, 5))
```

**Key insight:** Three chained generators form a lazy pipeline — the file is read only as needed (only ~5 lines ever enter memory, not the whole file). `islice` truncates without consuming the rest.

---

## Practice (try before peeking)

1. `[x for x in range(10) if x % 2]` — what does this produce? (Trick: truthiness of 0/1.)
2. Write a comprehension that produces all (x, y) pairs where x in range(3) and y in range(3) and x < y.
3. What's the difference between `(x for x in r)` and `[x for x in r]`?

<details><summary>Answers</summary>

1. `[1, 3, 5, 7, 9]` — `x % 2` is 0 (falsy) for evens, 1 (truthy) for odds.
2. `[(x, y) for x in range(3) for y in range(3) if x < y]` → `[(0,1),(0,2),(1,2)]`.
3. Generator expression (lazy, single-use, O(1) memory) vs list comprehension (eager, reusable, O(n) memory).

</details>

---

**Common traps:**
- Parentheses vs brackets: `(x for x in ...)` is a generator; `[x for x in ...]` is a list — one-use vs reusable
- Nested comprehension order: left-to-right is outer-to-inner; reversing it silently changes the result
- Generators are exhausted after one pass — don't try to iterate twice or reuse them as lists
- Comprehension scoping: in Python 3, the loop variable doesn't leak out (it did in Python 2)
- Side effects inside comprehensions (like `print`) are an anti-pattern — a comprehension should *compute*, not *act*

---
