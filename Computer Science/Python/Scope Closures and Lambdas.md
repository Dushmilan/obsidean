# Scope, Closures & Lambdas

Names in Python live in scopes, and the rules for finding them follow **LEGB** — Local, Enclosing, Global, Builtin. Closures capture enclosing scope even after the outer function returns. Lambdas are anonymous one-liners that shine when passed as arguments.

**The Intuition:** Every name lookup walks outward: local function variables first, then enclosing function scopes, then module globals, then builtins. Assignment is what *creates* a local — which is why "I assigned a global inside a function" silently makes a new local instead. Closures are functions that "remember" their birth scope. Lambdas are functions without names — perfect for "use once, pass to sorted/map."

## LEGB — the lookup order

```python
x = "global"                    # module scope

def outer():
    x = "enclosing"             # enclosing function scope
    def inner():
        x = "local"             # local scope
        print(x)                # "local"
    inner()

print(x)                        # "global" — inner's changes don't leak
```

## Assignment makes locals — the classic surprise

```python
x = 10

def f():
    x = x + 1      # UnboundLocalError!

# Why: the assignment x = ... makes x LOCAL to f.
# The RHS `x + 1` then looks up a local that doesn't exist yet.
```

```python
x = 10

def f():
    global x       # declare: use the module-level x
    x = x + 1      # now works — mutates the global

def counter():
    count = 0
    def inc():
        nonlocal count   # declare: use enclosing scope's count
        count += 1
        return count
    return inc
```

## Closures — functions that remember

```python
def make_multiplier(n):
    def multiplier(x):
        return x * n        # captures n from enclosing scope
    return multiplier

double = make_multiplier(2)
triple = make_multiplier(3)
double(10)      # 20
triple(10)      # 30
# n survives after make_multiplier returned — that's the closure
```

**Closure captures are by reference** — the classic loop bug:
```python
funcs = [lambda: i for i in range(3)]
[f() for f in funcs]        # [2, 2, 2] — all see the FINAL i!

# Fix: bind with a default argument (evaluated eagerly)
funcs = [lambda i=i: i for i in range(3)]
[f() for f in funcs]        # [0, 1, 2]
```

## Lambdas

```python
square = lambda x: x**2       # named lambda — usually better as def
square(5)                     # 25

# Lambdas shine as arguments:
sorted(pairs, key=lambda p: p[1])                    # sort by second element
max(names, key=lambda n: len(n))                     # longest name
list(map(lambda x: x * 2, [1, 2, 3]))                # [2,4,6]
list(filter(lambda x: x > 2, [1, 2, 3, 4]))          # [3,4]
```

Lambda limitations:
- Single expression only — no statements, no assignment, no if/elif chain (use conditional expr `a if c else b`)
- No docstring, no annotations — if it needs those, it needs a `def`

## Common functional tools

```python
from functools import reduce
reduce(lambda a, b: a + b, [1, 2, 3, 4])     # 10 — fold

# Prefer comprehensions over map/filter — more readable:
[x * 2 for x in [1, 2, 3]]       # vs list(map(...))
[x for x in [1,2,3,4] if x > 2]  # vs list(filter(...))
```

---

**Setup:** Write a `counter()` that returns a function; each call returns 1, 2, 3, ...

**Solution:**
```python
def counter():
    n = 0
    def inc():
        nonlocal n
        n += 1
        return n
    return inc

c = counter()
c()   # 1
c()   # 2
```

**Key insight:** `nonlocal` is required because `n += 1` assigns to `n` — without it, `n` becomes local and we get UnboundLocalError. The closure keeps `n` alive between calls: a private mutable state, like a tiny object.

---

**Setup:** Sort a list of dicts by the `"score"` key, descending.

**Solution:**
```python
data = [{"name": "Ada", "score": 92}, {"name": "Bob", "score": 85}]
sorted(data, key=lambda d: d["score"], reverse=True)
```

**Key insight:** The key function transforms each element into the comparison value. Lambda keeps it inline; a named `def` is better if the logic grows.

---

**Setup:** Memoize a function using a closure (hand-rolled caching).

**Solution:**
```python
def memoize(f):
    cache = {}
    def wrapper(x):
        if x not in cache:
            cache[x] = f(x)
        return cache[x]
    return wrapper

@memoize
def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)

fib(35)   # fast — cache avoids exponential recomputation
```

**Key insight:** This closure is the *manual* version of what `functools.lru_cache` does — and it's the same idea as DP memoization in your DSA notes. The decorator syntax `@memoize` rewires `fib` to the wrapper.

---

## Practice (try before peeking)

1. What does `[lambda x=x: x for x in range(3)]` produce when each is called?
2. `x = 1; def f(): print(x)` then `f()` — what prints, and why is there no error?
3. Use `sorted` with a lambda to sort words by their *last* character.

<details><summary>Answers</summary>

1. `[0, 1, 2]` — the default `x=x` binds the current value eagerly, breaking the by-reference closure trap.
2. `1` — `f` reads `x` but never assigns it, so LEGB finds the global. (Assignment is what creates a local.)
3. `sorted(words, key=lambda w: w[-1])`.

</details>

---

**Common traps:**
- The closure loop bug (see above) — always `lambda x=x:` to capture current value
- `global`/`nonlocal` must precede the first *use* of the name in the function
- Lambda bodies are expressions — `lambda x: if x > 0` is a SyntaxError; use `lambda x: x if x > 0 else 0`
- Shadowing builtins: `list = [...]` or `max = 5` breaks the builtin lookup for the rest of the module
- `map`/`filter` return **lazy iterators** in Python 3 — wrap with `list()` to materialize

---
