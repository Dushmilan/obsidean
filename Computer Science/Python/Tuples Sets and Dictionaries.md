# Tuples, Sets & Dictionaries

Beyond lists, Python's other containers each encode a different *contract*: tuples are immutable records, sets are unique-and-fast-membership, dicts are key→value maps. Choosing the right one is choosing the right data structure — the Python spelling of your DSA decisions.

**The Intuition:** Lists are for ordered collections you'll grow. Tuples are for fixed records that shouldn't change — like coordinates or a (name, score) pair. Sets are for "is this in here?" with no ordering and no duplicates. Dicts are lookups by key — the most-used data structure in real Python.

## Tuples — immutable records

```python
point = (3, 4)
point[0]              # 3
x, y = point          # unpacking
len(point)            # 2

# Single-element tuple — COMMA, not parens:
single = (5,)         # tuple
not_tuple = (5)       # just the int 5!

# Named tuples — readable records
from collections import namedtuple
Student = namedtuple("Student", ["name", "score"])
s = Student("Ada", 92)
s.name, s.score       # 'Ada', 92
s[0]                  # 'Ada' — still works like a tuple

# When to use a tuple instead of a list:
# - the data shouldn't change (documents intent)
# - you need a HASHABLE object (dict key, set member)
hash((3, 4))          # works
# hash([3, 4])        # TypeError — lists unhashable
```

## Sets — uniqueness + O(1) membership

```python
s = {3, 1, 4, 1, 5}       # {1, 3, 4, 5} — duplicates collapsed, unordered
s.add(2)
s.discard(99)             # remove if present, NO error if absent
# s.remove(99)            # raises KeyError if absent — use discard for optional
s.pop()                   # removes arbitrary element

# Set algebra — very useful
a = {1, 2, 3}
b = {2, 3, 4}
a | b      # {1,2,3,4} union
a & b      # {2,3} intersection
a - b      # {1} difference
a ^ b      # {1,4} symmetric difference
a <= b     # subset check
a < b      # proper subset
```

```python
# Dedupe
list(set([1, 2, 2, 3]))     # [1,2,3] — but ORDER NOT GUARANTEED

# Fast membership — THE reason to use sets
# x in list: O(n)   |   x in set: O(1)

# frozenset — immutable, hashable
fs = frozenset([1, 2, 3])   # can be a dict key
```

## Dictionaries — the workhorse

```python
d = {}                    # empty
d = {"name": "Ada", "score": 92}
d["name"]                 # 'Ada'
d["score"] = 95           # update
d["age"] = 36             # add
"name" in d               # True — key check (O(1))
d.get("missing")          # None — no KeyError
d.get("missing", 0)       # 0 — default value
d.setdefault("count", 0)  # insert if absent, return value
d.keys()                  # view of keys
d.values()                # view of values
d.items()                 # view of (k, v) pairs
d.pop("age")              # remove and return
del d["name"]             # remove

# Dict views are LIVE:
keys = d.keys()
d["new"] = 1
# keys now includes 'new'

# Building dicts
dict([("a", 1), ("b", 2)])       # from pairs
dict(zip(names, scores))         # from two lists
{word: len(word) for word in words}   # comprehension
```

## Dict patterns that come up constantly

```python
# Counter (count frequencies) — subclass of dict
from collections import Counter
c = Counter("abracadabra")
c["a"]            # 5
c.most_common(2)  # [('a', 5), ('b', 2)]

# defaultdict — auto-initialize missing keys
from collections import defaultdict
groups = defaultdict(list)
for name, city in data:
    groups[city].append(name)      # no KeyError on first append

# Sorting a dict by value
sorted(d.items(), key=lambda kv: kv[1])          # ascending
dict(sorted(d.items(), key=lambda kv: kv[1], reverse=True))

# Merging dicts (3.9+)
merged = {**a, **b}      # or: a | b
```

---

**Setup:** Count word frequencies and report the top 3 words.

**Solution:**
```python
from collections import Counter
text = "the cat and the dog and the bird"
counts = Counter(text.split())
counts.most_common(3)      # [('the', 3), ('and', 2), ('cat', 1)]
```

**Key insight:** `Counter` IS a dict with counting sugar — `most_common` sorts by count in one call. The naive `d.get(w, 0) + 1` loop is the manual version.

---

**Setup:** Group students by grade (a dict of lists).

**Solution:**
```python
from collections import defaultdict
students = [("Ada", "A"), ("Bob", "B"), ("Cy", "A")]
by_grade = defaultdict(list)
for name, grade in students:
    by_grade[grade].append(name)
# by_grade == {'A': ['Ada', 'Cy'], 'B': ['Bob']}
```

**Key insight:** `defaultdict(list)` eliminates the "is key present?" dance — accessing a missing key auto-inserts an empty list. The same pattern with `defaultdict(int)` gives auto-zero counters.

---

**Setup:** Find the intersection of two lists (elements in both) efficiently.

**Solution:**
```python
def intersection(a, b):
    return list(set(a) & set(b))
```

**Key insight:** Converting to sets makes the intersection O(n+m) via hashing, versus O(n·m) for nested loops. Order is lost — if order matters, filter one list by a set of the other.

---

**Setup:** Given two lists `names` and `scores`, build a dict sorted by score descending.

**Solution:**
```python
pairs = dict(zip(names, scores))
ranked = dict(sorted(pairs.items(), key=lambda kv: kv[1], reverse=True))
```

**Key insight:** `zip` pairs the lists; `sorted` with a key on the value; `dict()` rebuilds preserving insertion order (dicts are insertion-ordered in Python 3.7+).

---

## Practice (try before peeking)

1. What's the output of `{1, 2, 3} - {2, 3, 4}`? Of `{1,2} | {2,3}`?
2. `d = {"a": 1}; d.get("b", 5)` and `d["b"]` — what happens in each?
3. Why can't a `list` be a dict key? What about a `tuple` containing a list?

<details><summary>Answers</summary>

1. `{1}` and `{1, 2, 3}`.
2. `d.get("b", 5)` → `5` (no error, default returned, dict unchanged). `d["b"]` → raises `KeyError` — indexing does NOT use defaults.
3. Dict keys must be hashable. Lists are mutable → unhashable → TypeError. A tuple containing a list is also unhashable (hash depends on contents, which could change).

</details>

---

**Common traps:**
- `{1, 2, 3}` is a set but `{}` is an empty **dict** — empty set requires `set()`
- Sets/dicts are unordered (well: dicts are insertion-ordered, sets are not) — never rely on set iteration order
- Mutating a dict/set while iterating it raises RuntimeError — iterate over `list(d.items())` or rebuild
- `d[k]` vs `d.get(k)`: indexing raises, get returns None — know which failure mode you want
- Repeating a tuple `(1, 2) * 3` gives `(1,2,1,2,1,2)`; repeating a dict `*` is an error — container semantics differ

---
