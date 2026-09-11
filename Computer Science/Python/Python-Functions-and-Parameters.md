# Functions & Parameters

Functions are Python's unit of reusable logic. The parameter system is unusually flexible — positional, keyword, default, `*args`, `**kwargs` — and it follows one rule that explains most surprises: **arguments are passed by object reference** (call by sharing).

**The Intuition:** A function is a recipe: parameters are the ingredients it expects, the body is the steps, `return` hands back the result. Python lets you call the same function many ways — and lets you define functions that accept *any* calling style via `*args`/`**kwargs`.

## Defining & calling

```python
def greet(name, greeting="Hello", punctuation="!"):
    return f"{greeting}, {name}{punctuation}"

# Call styles — all valid:
greet("Ada")                        # positional
greet("Ada", "Hi")                  # positional
greet("Ada", punctuation="?")       # keyword (skip greeting!)
greet(name="Ada", greeting="Yo")    # all keywords
greet("Ada", greeting="Hey")        # mix: positional then keyword
```

Rules:
- Positional args first, then keyword args — `greet(greeting="Hi", "Ada")` is a SyntaxError
- Defaults are evaluated **once at definition time** — the mutable-default trap below

## *args and **kwargs — variable arguments

```python
def total(*args):                  # args = tuple of all positional args
    return sum(args)

total(1, 2, 3, 4)                  # 10
total()                            # 0

def show(**kwargs):                # kwargs = dict of all keyword args
    for k, v in kwargs.items():
        print(f"{k}={v}")

show(name="Ada", age=36)           # name=Ada, age=36

def flexible(a, b, *args, **kwargs):
    # a, b required; args = extra positional; kwargs = extra keyword
    pass

# Splat / unpacking when CALLING:
def add(a, b, c):
    return a + b + c

nums = [1, 2, 3]
add(*nums)                 # unpack list into positional — add(1,2,3)
d = {"a": 1, "b": 2, "c": 3}
add(**d)                   # unpack dict into keyword — add(a=1,b=2,c=3)
```

## Keyword-only and positional-only parameters

```python
def f(a, b, *, c):         # after *: keyword-ONLY
    pass
f(1, 2, c=3)               # OK
# f(1, 2, 3)               # TypeError — c must be keyword

def g(a, b, /):            # before /: positional-ONLY
    pass
# g(a=1, b=2)              # TypeError
```

## The mutable default trap — THE classic bug

```python
def append_item(item, lst=[]):     # BAD — default list created ONCE
    lst.append(item)
    return lst

append_item(1)      # [1]
append_item(2)      # [1, 2]  ← shared across calls!
append_item(3)      # [1, 2, 3]

# Correct:
def append_item(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

**Why:** The default `[]` is a single object created at *definition* time, reused by every call. The `None` sentinel forces a fresh list per call.

## Return semantics

```python
def f():
    pass                # returns None implicitly
def g():
    return              # returns None explicitly

# Returning multiple values = returning a tuple
def min_max(lst):
    return min(lst), max(lst)     # returns (min, max)
lo, hi = min_max(nums)            # unpacked

# Multiple return points are fine; keep them obvious
```

## Docstrings & annotations

```python
def factorial(n: int) -> int:
    """Return n! (product of 1..n)."""      # docstring — accessible via help(factorial)
    if n <= 1:
        return 1
    return n * factorial(n - 1)

# Annotations are DOCUMENTATION, not enforcement:
# factorial("x") runs fine until the recursion fails.
```

---

**Setup:** Write `safe_divide(a, b)` that returns the quotient or None for division by zero.

**Solution:**
```python
def safe_divide(a, b):
    return a / b if b != 0 else None
```

**Key insight:** The ternary returns early — `b == 0` never reaches the division. Returning `None` signals "no result" idiomatically; callers check `if result is None`.

---

**Setup:** Write a function that accepts any number of numbers and returns the product.

**Solution:**
```python
def product(*nums):
    result = 1
    for n in nums:
        result *= n
    return result

product(2, 3, 4)    # 24
product()           # 1 (empty product)
```

**Key insight:** `*args` collects variadic inputs into a tuple. This is how `print`, `max`, and `sum` accept any count.

---

**Setup:** Write a wrapper that prints the arguments and result of any function call (a mini-decorator).

**Solution:**
```python
def logged(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with {args} {kwargs}")
        result = func(*args, **kwargs)
        print(f"  -> {result}")
        return result
    return wrapper

@logged
def add(a, b):
    return a + b

add(2, 3)
# Calling add with (2, 3) {}
#   -> 5
```

**Key insight:** `*args, **kwargs` in the wrapper accepts *any* signature and forwards it — this is exactly why decorators use this pattern. A closure captures `func` from the enclosing scope.

---

**Setup:** Implement `avg` that takes either a list, or variadic numbers: `avg(1,2,3)` or `avg([1,2,3])` both work.

**Solution:**
```python
def avg(*nums):
    if len(nums) == 1 and isinstance(nums[0], (list, tuple)):
        nums = nums[0]
    return sum(nums) / len(nums)
```

**Key insight:** Normalizing inputs to a uniform shape at the top of the function ("flatten the API") is a common robustness pattern — one implementation, many call styles.

---

## Practice (try before peeking)

1. What does `f(x=1, 2)` do? And `f(1, x=2)` when `f(x, y)`?
2. Write `partial_sum(*nums, limit=100)` returning the sum but capped at `limit`.
3. `def m(lst=[]): lst.append(1); return lst` — call it 3 times. What's the result each time?

<details><summary>Answers</summary>

1. `f(x=1, 2)` is a SyntaxError — positional after keyword. `f(1, x=2)` is a TypeError — `x` given twice.
2. `return min(sum(nums), limit)`.
3. `[1]`, `[1, 1]`, `[1, 1, 1]` — the shared default list. Fix with `lst=None`.

</details>

---

**Common traps:**
- Mutable default arguments (see above) — the #1 function bug
- Positional-after-keyword → SyntaxError; duplicate argument → TypeError
- `return` inside a loop returns immediately — only the first value, not a list
- Forgetting `return` entirely → the function returns `None`, silently
- `*args` gives a **tuple** (immutable), `**kwargs` a **dict** — reassigning inside the function is fine but doesn't affect the caller's values

---
