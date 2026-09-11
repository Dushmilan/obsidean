# Variables, Types & Operators

Python's type system is **dynamic** — variables have no declared type; the *value* carries the type. This makes Python flexible but shifts type errors from compile time to runtime. Understanding exactly what each type is, and what the operators do to them, is the foundation of everything else.

**The Intuition:** In C/Java, a variable is a fixed-size box you label ("this box holds ints"). In Python, a variable is a *name tag* you stick onto a value. Reassigning the name moves the tag to a different value — potentially of a completely different type. The tag has no type; the object it points to does.

## Core Types

```python
# Numeric
int_ = 42                    # arbitrary precision — never overflows!
float_ = 3.14                # double precision (IEEE 754)
complex_ = 1 + 2j            # complex numbers built in

# Text
str_ = "hello"               # immutable sequence of Unicode chars

# Boolean — actually a subclass of int!
True == 1                    # True
False == 0                   # True

# NoneType — the "no value" value
nothing = None               # not 0, not "", not False — its own type

# Sequence & container types
list_ = [1, 2, 3]            # mutable, ordered
tuple_ = (1, 2, 3)           # immutable, ordered
dict_ = {"a": 1}             # mutable, key→value
set_ = {1, 2, 3}             # mutable, unordered, unique
```

**Checking types:**
```python
type(42)          # <class 'int'>
isinstance(42, int)   # True — the RIGHT way to check
isinstance(3.14, (int, float))  # True — can check multiple
# type(x) == int vs isinstance(x, int): isinstance respects inheritance
```

## Operators — and their overloaded behavior

| Operator | int | float | str | list | set |
|----------|-----|-------|-----|------|-----|
| `+` | add | add | **concat** | **concat** | union |
| `*` | multiply | multiply | **repeat** | **repeat** | intersection |
| `-` | subtract | subtract | ✗ | ✗ | difference |
| `/` | true div (→float) | true div | ✗ | ✗ | ✗ |
| `//` | floor div | floor div | ✗ | ✗ | ✗ |
| `%` | modulo | modulo | ✗ | ✗ | ✗ |
| `**` | power | power | ✗ | ✗ | ✗ |

```python
7 / 2    # 3.5  — true division ALWAYS returns float
7 // 2   # 3    — floor division
-7 // 2  # -4   — floors toward negative infinity!
7 % 2    # 1
-7 % 2   # 1    — modulo sign follows the DIVISOR, not the dividend
2 ** 10  # 1024
"ab" * 3 # "ababab"
[0] * 3  # [0, 0, 0]
```

## Assignment & Identity

```python
a = [1, 2, 3]
b = a            # b is ANOTHER NAME for the same list
b.append(4)      # a is now [1,2,3,4] too!
c = a[:]         # c is a COPY (slicing creates new list)

# ==  compares VALUES
# is  compares IDENTITY (same object)
a == b           # True
a is b           # True (same object)
a == c           # True (equal values)
a is c           # False (different objects)

# Small ints are interned — a quirk:
x = 5; y = 5
x is y           # True (small ints cached)
x = 300; y = 300
x is y           # False on CPython (not cached) — never rely on this!
```

## The Mutable / Immutable split — critical

| Immutable | Mutable |
|-----------|---------|
| int, float, complex, str, bool, tuple, frozenset | list, dict, set, bytearray, custom classes |

Immutability means operations *return new objects*:
```python
s = "abc"
t = s.upper()    # t = "ABC", s is unchanged — strings never modify in place
```

## Truthiness — every value is truthy or falsy

```python
# Falsy: None, False, 0, 0.0, 0j, "", [], (), {}, set()
# Everything else is truthy
if []: ...            # never runs
if "hello": ...       # runs
if 0.0: ...           # never runs
```
This is why `if x:` works where other languages need `if x is not None and len(x) > 0`.

---

**Setup:** Predict the output, then run: `print(5 * "a" + "b")`, `print("a" in "cat")`, `print(3 in [1,2,3])`, `print("b" in {"a": 1})`.

**Solution:** `"aaaaab"`, `True`, `True`, `False`. The last one is the trap: `in` on a dict checks **keys**, not values. `"a" in {"a": 1}` is `True`; `"b" in {"a": 1}` is `False`.

**Key insight:** Operator behavior depends on operand types (`*` repeats strings, checks membership). The dict `in` semantics are a classic interview gotcha.

---

**Setup:** `a = [1,2,3]; b = a; b += [4]`. What are `a` and `b`? Then `c = (1,2); d = c; d += (3,)`. What are `c` and `d`?

**Solution:** `a == b == [1,2,3,4]` — `+=` on a list mutates in place, and `b` is the same object as `a`. For tuples: `c == (1,2)`, `d == (1,2,3)` — `+=` on a tuple *rebinds* to a new tuple because tuples are immutable.

**Key insight:** `+=` is "mutate in place if possible, else rebind." This single fact explains most aliasing bugs in Python.

---

**Setup:** Why does `-7 // 2` equal `-4` and not `-3`?

**Solution:** Floor division rounds toward **negative infinity** (the floor), not toward zero. `-3.5` floored is `-4`. The modulo is consistent: `-7 = 2*(-4) + 1`, so `-7 % 2 == 1`.

**Key insight:** `a == (a // b) * b + (a % b)` always holds. This invariant — and the floor behavior — matters in algorithms that iterate backwards or index circularly with negatives.

---

**Setup:** Implement a swap without a temporary variable.

**Solution:** `a, b = b, a` — tuple unpacking: the right side builds a tuple `(b, a)`, then unpacks into `a, b`. No temp needed.

**Key insight:** Unpacking works for any iterable: `x, y = (1, 2)`, `first, *rest = [1,2,3,4]` (rest = `[2,3,4]`), `key, value = {"k": "v"}.popitem()`.

---

## Practice (try before peeking)

1. `7 / 2`, `7 // 2`, `int(7 / 2)`, `round(7 / 2)` — which are 3, 3.5, 4?
2. `x = [1, 2]; y = x; y = y + [3]` — is `x` affected? Why not, when `y += [3]` would affect it?
3. What's the truthiness of `"0"`, `[0]`, `float('nan')`?

<details><summary>Answers</summary>

1. `7/2 → 3.5`, `7//2 → 3`, `int(3.5) → 3` (truncates toward zero), `round(3.5) → 4` (banker's rounding: to even).
2. No — `y = y + [3]` creates a new list and rebinds `y`; `x` still points at `[1,2]`. The `+=` form mutates the shared list.
3. All truthy — non-empty strings, non-empty lists, and any float (even NaN) are truthy.

</details>

---

**Common traps:**
- `type(x) == int` fails for bools (`isinstance(True, int)` is True!) — use `isinstance` carefully
- Comparing `None` with `==`: `None == 0` is False, but `None == None` is True; use `is None` for identity
- Integer division in Python 2 (`7/2 == 3`) — in Python 3 it's always `3.5`; if you see `7/2 == 3` you're reading old code
- Floats: `0.1 + 0.2 != 0.3` (IEEE 754) — never compare floats with `==`; use `abs(a - b) < 1e-9`

---
