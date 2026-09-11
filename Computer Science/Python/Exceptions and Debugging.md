# Exceptions & Debugging

Python's error model is exceptions: when something goes wrong, an exception is raised, and you can catch it, inspect it, or let it crash. "Ask for forgiveness, not permission" (EAFP) is the Pythonic style — try the operation and handle the failure. Debugging is the systematic hunt for the bug — knowing the tooling and the read-the-traceback skill.

**The Intuition:** An exception is a message traveling up the call stack until someone catches it. `try/except` is the catcher. If nobody catches it, the program crashes with a traceback. The traceback isn't a punishment — it's the *map* to the bug: file, line, and the chain of calls that led there.

## try / except / else / finally

```python
try:
    x = int(input("Number: "))
    result = 100 / x
except ValueError as e:
    print(f"Not a number: {e}")          # input wasn't an int
except ZeroDivisionError:
    print("Can't divide by zero")
else:
    print(f"Result: {result}")           # runs only if NO exception
finally:
    print("Always runs")                 # cleanup — file close, etc.

# Catching multiple in one clause:
except (ValueError, TypeError):
    print("Bad input type")

# Catch-and-reraise (log then rethrow):
except ValueError as e:
    logging.error(f"bad input: {e}")
    raise                                # bare raise = re-raise same exception
```

## The builtin hierarchy (subset)

```
BaseException
├── SystemExit          # from sys.exit()
├── KeyboardInterrupt   # Ctrl-C — usually should NOT be caught
└── Exception           # ← catch THIS at most
    ├── ArithmeticError
    │   ├── ZeroDivisionError
    │   └── OverflowError
    ├── LookupError
    │   ├── IndexError      # lst[5]
    │   └── KeyError        # d["missing"]
    ├── ValueError          # int("abc"), unlist.index(99)
    ├── TypeError           # "a" + 1
    ├── AttributeError      # None.foo
    ├── FileNotFoundError   # open("nope.txt")
    └── ...
```

**Catching `Exception` broadly is usually a smell** — it hides bugs. Catch what you can handle specifically.

## Raising your own

```python
def withdraw(balance, amount):
    if amount < 0:
        raise ValueError("amount must be positive")
    if amount > balance:
        raise InsufficientFunds(f"need {amount}, have {balance}")
    return balance - amount

# Custom exception classes — subclass Exception
class InsufficientFunds(Exception):
    pass
```

## EAFP vs LBYL

```python
# Look Before You Leap (C-style, less Pythonic)
if "key" in d:
    value = d["key"]
else:
    value = 0

# Ask Forgiveness (Pythonic, EAFP)
try:
    value = d["key"]
except KeyError:
    value = 0
# Usually cleaner with .get():  value = d.get("key", 0)
```

## Debugging toolkit

```python
# 1. Read the traceback bottom-up — the LAST frame is where it broke

# 2. print() debugging — quick and effective
print("DEBUG:", len(data), type(data))   # check shapes and types

# 3. assert — self-checking code
assert len(args) == 2, f"expected 2 args, got {len(args)}"
# Run with python -O to disable asserts (they're stripped)

# 4. pdb — the interactive debugger
# import pdb; pdb.set_trace()          # or breakpoint() in 3.7+
breakpoint()          # drops into the debugger right here
# p commands: n (next), s (step into), c (continue), p expr (print),
#             l (list source), w (where/stack), q (quit)

# 5. logging — better than print for real programs
import logging
logging.basicConfig(level=logging.DEBUG)
logging.debug("data: %s", data)        # level-filtered output
logging.info("processed %d rows", n)
logging.error("failed: %s", e, exc_info=True)   # include traceback
```

## Common bugs and their exceptions

| Symptom | Exception | Likely cause |
|---------|-----------|--------------|
| `lst[99]` | IndexError | index out of range — off-by-one |
| `d["x"]` | KeyError | key absent — did you mean `.get`? |
| `"abc" + 1` | TypeError | type mismatch — cast first |
| `int("abc")` | ValueError | unparseable value |
| `None.name` | AttributeError | forgot to handle None — guard it |
| `open("x")` | FileNotFoundError | path wrong / file missing |
| `0/0` | ZeroDivisionError | unguarded division — check denominator |
| `[1,2][0] = 3` on tuple | TypeError | mutating an immutable |

---

**Setup:** Read an integer from the user, retrying until valid.

**Solution:**
```python
while True:
    try:
        n = int(input("Enter an integer: "))
        break
    except ValueError:
        print("That's not an integer — try again.")
```

**Key insight:** The retry loop with `try/except` is the standard input-validation pattern. `break` only happens on success; failures loop back. No flag variables needed.

---

**Setup:** Parse a file with one number per line, skipping bad lines and reporting the count of skips.

**Solution:**
```python
total = skipped = 0
with open("numbers.txt") as f:
    for line in f:
        try:
            total += int(line.strip())
        except ValueError:
            skipped += 1
print(f"sum={total}, skipped={skipped}")
```

**Key insight:** Fine-grained `try` around just the risky operation (the conversion) — not around the whole loop. This is EAFP applied to data cleaning: one bad line doesn't kill the batch.

---

**Setup:** Open a file, and if it's missing, create it with a default; if anything else goes wrong, re-raise.

**Solution:**
```python
try:
    with open("config.json") as f:
        data = f.read()
except FileNotFoundError:
    write_default("config.json")      # handled case
except OSError as e:                  # permission errors etc.
    raise RuntimeError("Could not read config") from e   # chain, keep cause
```

**Key insight:** Catch the specific `FileNotFoundError` to handle the expected case; catch broader `OSError` to wrap unexpected I/O failures into a meaningful message *while preserving the original* via `from e` (visible in the traceback as "The above exception was the direct cause...").

---

## Practice (try before peeking)

1. Write a `safe_list_get(lst, i, default)` returning the default instead of raising.
2. What's wrong with `try: ... except: pass` around a whole function?
3. `try: return 1 finally: print("x")` — does the print run?

<details><summary>Answers</summary>

1. `return lst[i] if -len(lst) <= i < len(lst) else default` — or `try/except IndexError`.
2. It swallows *everything* including KeyboardInterrupt, SystemExit, and genuine bugs — debugging becomes impossible. Catch specific exceptions.
3. Yes — `finally` always runs, even across `return`. The return value is computed, then finally executes, then the value is returned. (If `finally` has its own `return`, it overrides!)

</details>

---

**Common traps:**
- Bare `except:` catches `KeyboardInterrupt`/`SystemExit` too — use `except Exception:` at most
- Empty `except: pass` hides bugs silently — at minimum log
- Swallowing then continuing with corrupted state — validate or re-raise
- Using exceptions for *control flow* in hot loops (e.g., catch StopIteration to check emptiness) — slow and confusing
- Forgetting that `finally` runs even on `return`/`break`/`continue` — it's for cleanup, not result logic

---
