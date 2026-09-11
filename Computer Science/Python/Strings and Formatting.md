# Strings & Formatting

Strings are the workhorse of real-world Python: parsing input, building output, processing text. Python strings are **immutable** sequences of Unicode characters, with a rich method library and four formatting systems (only one of which you should use today).

**The Intuition:** A string is a sequence you can index, slice, and iterate — but never change in place. Every method that "modifies" a string actually returns a *new* string. Formatting is about building strings from data — f-strings are the modern, readable way.

## Indexing, Slicing & Iteration

```python
s = "abcdef"
s[0]        # "a"  — first
s[-1]       # "f"  — last (negative indices from the end)
s[1:4]      # "bcd" — [start:stop) — stop EXCLUSIVE
s[:3]       # "abc" — from start
s[3:]       # "def" — to end
s[::2]      # "ace" — every 2nd
s[::-1]     # "fedcba" — REVERSE (classic trick)
s[1:5:2]    # "bd"
```

Slicing never raises IndexError — it clamps:
```python
s[2:100]    # "cdef" — no error, just stops
s[100:]     # "" — empty
```

## The Method Toolkit

```python
s = "  Hello, World!  "

s.strip()          # "Hello, World!"  — also lstrip/rstrip
s.lower()          # "  hello, world!  "
s.upper()
s.title()          # "  Hello, World!  "
s.replace("World", "Python")   # "  Hello, Python!  "
s.split(", ")      # ["  Hello", "World!  "]
" ".join(["a", "b", "c"])     # "a b c" — join is on the SEPARATOR
s.startswith("  He")           # True
s.endswith("!  ")              # True
s.find("World")    # 7  — index or -1 if not found
s.index("World")   # 7  — RAISES ValueError if not found
"abc123".isalpha()  # False — digits present
"abc".isalpha()     # True
"123".isdigit()     # True
```

## Character ↔ code

```python
ord("A")     # 65
chr(65)      # "A"
ord("é")     # 233 — Unicode, not ASCII
len("héllo") # 5 — counts characters
```

## Formatting — four ways, one winner

```python
name, score = "Ada", 92

# 1. Old %-formatting (C-style) — legacy
"%s scored %d" % (name, score)

# 2. str.format — fine, but verbose
"{} scored {}".format(name, score)
"{name} scored {score}".format(name=name, score=score)
"{:>10}".format("x")      # right-align, width 10

# 3. f-strings (Python 3.6+) — USE THESE ★
f"{name} scored {score}"
f"{score:03d}"          # "092" — zero-pad
f"{score:.2f}"          # "92.00" — 2 decimals
f"{3.14159:.3f}"        # "3.142"
f"{1000000:,}"          # "1,000,000" — thousands separator
f"{0.25:.1%}"           # "25.0%" — percentage
f"{'left':<10}|"        # left-align
f"{'right':>10}|"       # right-align
f"{2**10=}"             # "2**10=1024" — debug trick (3.8+)

# 4. Template strings — only when the format string is USER input (security)
from string import Template
Template("$name scored $score").substitute(name=name, score=score)
```

## Raw strings & escaping

```python
path = r"C:\Users\Ada\new"   # raw — \n is backslash-n, not newline
# Raw strings are why regexes are written r"pattern"
```

## Common string algorithms

```python
# Palindrome check
def is_palindrome(s):
    return s == s[::-1]

# Frequency count
from collections import Counter
Counter("hello")   # Counter({'h':1,'e':1,'l':2,'o':1})

# Anagram check
sorted("listen") == sorted("silent")   # True
```

---

**Setup:** Reverse the words in a sentence, preserving word order reversed: `"the quick brown fox"` → `"fox brown quick the"`.

**Solution:**
```python
sentence = "the quick brown fox"
" ".join(sentence.split()[::-1])
# or: " ".join(reversed(sentence.split()))
```

**Key insight:** Split on whitespace, reverse the list, join on space. The same pipeline — `split` → transform → `join` — solves most text-processing tasks.

---

**Setup:** Validate a phone number looks like `+1-234-567-8901`.

**Solution:**
```python
import re
def valid_phone(s):
    return bool(re.fullmatch(r"\+1-\d{3}-\d{3}-\d{4}", s))
```

**Key insight:** `re.fullmatch` requires the *entire* string to match (unlike `match` which anchors at start only). Raw strings `r"..."` keep `\d` as a regex escape instead of a string escape.

---

**Setup:** Count vowels in a string, case-insensitively.

**Solution:**
```python
text = "Hello World"
sum(1 for ch in text.lower() if ch in "aeiou")   # 3
# or: sum(text.lower().count(v) for v in "aeiou")
```

**Key insight:** Generator expression + `sum` — Python's idiomatic counting. The `if ch in "aeiou"` membership test is O(1) for short strings.

---

## Practice (try before peeking)

1. Write a one-liner that capitalizes the first letter of every word in a sentence.
2. What does `"abc".join(["1", "2"])` produce? (Trick: think about which string is the separator.)
3. Extract the filename and extension from `"archive.tar.gz"`.

<details><summary>Answers</summary>

1. `" ".join(w.capitalize() for w in s.split())` — or `s.title()` (but title() also capitalizes after apostrophes).
2. `"1abc2"` — the string *before* `.join` is the separator. `"sep".join(items)` inserts `"sep"` *between* items.
3. `name, ext = "archive.tar.gz".rsplit(".", 1)` → `name="archive.tar"`, `ext="gz"`. `rsplit` splits from the right; the `1` limits to one split. `split(".")` would give 3 pieces — wrong for this.

</details>

---

**Common traps:**
- `s.split()` (no arg) splits on *any* whitespace runs; `s.split(" ")` splits on single spaces — different results for double spaces
- Strings are immutable: `s[0] = "x"` raises TypeError
- `+` concatenation in a loop is O(n²) — use `"".join(...)` or a list
- Unicode: Python 3 strings are Unicode natively — no `u""` prefix needed, but beware `len()` counting characters while `encode()` changes byte length

---
