# Lists

Lists are Python's most used data structure — a **dynamic array** (like your DSA `ArrayList`). Indexing is O(1), appending is amortized O(1), and inserting at the front is O(n). Mastering list operations means knowing which operations are cheap and which are expensive.

**The Intuition:** A list is an ordered, mutable sequence that grows and shrinks. Under the hood (CPython), it's an array of pointers with spare capacity — which is why appends are cheap but inserts in the middle shift everything.

## Construction

```python
empty = []
literal = [1, 2, 3]
list("abc")            # ['a', 'b', 'c'] — from any iterable
range_list = list(range(5))      # [0,1,2,3,4]
repeated = [0] * 5     # [0,0,0,0,0] — but see the nested-list trap below
split = "a,b,c".split(",")       # ['a','b','c']
```

## Core operations — and their costs

| Operation | Code | Complexity |
|-----------|------|-----------|
| Index / assign | `lst[i]`, `lst[i] = v` | O(1) |
| Append | `lst.append(x)` | amortized O(1) |
| Pop from end | `lst.pop()` | O(1) |
| Pop from front | `lst.pop(0)` | **O(n)** — shifts everything |
| Insert | `lst.insert(i, x)` | O(n) |
| Remove by value | `lst.remove(x)` | O(n) — find + shift |
| Membership | `x in lst` | O(n) — linear scan |
| Slice copy | `lst[a:b]` | O(b−a) |
| Concatenate | `a + b` | O(len(a)+len(b)) — new list |
| Extend | `lst.extend(b)` | O(len(b)) — in place |
| Sort | `lst.sort()` | O(n log n), in place |
| Find index | `lst.index(x)` | O(n) |

```python
nums = [3, 1, 4, 1, 5]
nums.append(9)           # [3,1,4,1,5,9]
nums.insert(0, 0)        # [0,3,1,4,1,5,9] — slow for big lists
nums.pop()               # 9 — removes last
nums.pop(0)              # 0 — removes first (O(n)!)
nums.remove(1)           # removes FIRST 1
nums.sort()              # in-place ascending
sorted(nums)             # returns NEW sorted list, original untouched
nums.reverse()           # in-place
list(reversed(nums))     # new reversed list
nums.count(1)            # how many 1s
nums.index(5)            # index of first 5
```

## Slicing — copy or view?

```python
lst = [0, 1, 2, 3, 4]
lst[1:3]      # [1, 2] — a NEW list
lst[:]        # full shallow copy
lst[::-1]     # reversed copy
lst[::2]      # [0, 2, 4]

# Slices can be ASSIGNED — replaces a range
lst[1:3] = [10, 20, 30]    # [0,10,20,30,3,4]
lst[1:4] = []               # deletes a range
```

## The nested-list trap

```python
matrix = [[0] * 3] * 3
# Looks like [[0,0,0],[0,0,0],[0,0,0]] — but:
matrix[0][0] = 1
# matrix is [[1,0,0],[1,0,0],[1,0,0]] — three references to the SAME inner list!

# Correct:
matrix = [[0] * 3 for _ in range(3)]
```

## Aliasing vs copying

```python
a = [1, 2, 3]
b = a              # alias — same object
c = a[:]           # shallow copy — new object, same elements
d = list(a)        # shallow copy
e = a.copy()       # shallow copy

# Shallow = elements shared. For nested lists, inner lists are still shared:
outer = [[1], [2]]
import copy
deep = copy.deepcopy(outer)   # inner lists copied too
```

## Useful patterns

```python
# List of squares
squares = [x**2 for x in range(10)]

# First and last
first, last = lst[0], lst[-1]

# Split into chunks
chunks = [lst[i:i+3] for i in range(0, len(lst), 3)]

# Two-pointer style (from DSA) on sorted lists
# index from both ends:
i, j = 0, len(lst) - 1
while i < j:
    if lst[i] + lst[j] == target: ...
    elif lst[i] + lst[j] < target: i += 1
    else: j -= 1
```

---

**Setup:** Remove duplicates from a list while preserving order.

**Solution:**
```python
def dedupe(lst):
    seen = set()
    result = []
    for x in lst:
        if x not in seen:
            seen.add(x)
            result.append(x)
    return result
```

**Key insight:** `seen` as a set makes the membership check O(1) — the naive `if x not in result` is O(n) per element, O(n²) total. Set + list is the standard order-preserving dedupe.

---

**Setup:** Rotate a list to the right by k positions (in place).

**Solution:**
```python
def rotate(lst, k):
    k %= len(lst)                    # handle k > len
    lst[:] = lst[-k:] + lst[:-k]     # slice-assign back into same list

nums = [1, 2, 3, 4, 5]
rotate(nums, 2)   # [4, 5, 1, 2, 3]
```

**Key insight:** `lst[:] = ...` mutates the *original* list (all aliases see it), while `lst = ...` would only rebind the local name. Slicing with negative indices handles rotation elegantly.

---

**Setup:** Merge two sorted lists into one sorted list, in O(n) (the merge step of merge sort).

**Solution:**
```python
def merge(a, b):
    i = j = 0
    result = []
    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            result.append(a[i]); i += 1
        else:
            result.append(b[j]); j += 1
    result.extend(a[i:])     # leftover — both lists already sorted
    result.extend(b[j:])
    return result
```

**Key insight:** The two-pointer merge is the heart of merge sort (see DSA Divide & Conquer). Appends are O(1) amortized, so total is O(n). The `extend` of leftovers handles the unequal-length case in one line.

---

## Practice (try before peeking)

1. Find the second-largest element in a list in O(n).
2. Given `lst = [[1,2],[3,4]]`, what does `lst[1][0]` give? What about `[row[0] for row in lst]`?
3. Write a function that returns a new list with all zeros moved to the end (order of others preserved).

<details><summary>Answers</summary>

1. Two passes: `largest = max(lst)` then `max(x for x in lst if x != largest)` — or single pass tracking top two.
2. `lst[1][0]` is `3`. The comprehension gives `[1, 3]` — the first column.
3. `[x for x in lst if x != 0] + [0] * lst.count(0)`.

</details>

---

**Common traps:**
- `[[0]*n]*m` aliasing (see above) — always build nested lists with comprehensions
- `lst.sort()` vs `sorted(lst)`: sort mutates and returns None; sorted returns a new list — mixing them up silently loses data
- `lst += [x]` mutates in place; `lst = lst + [x]` creates a new list — matters when other references exist
- Using `pop(0)` or `insert(0, x)` in a loop → O(n²); use `collections.deque` for queue-style access
- IndexError: indexing past the end raises, slicing does not — don't catch IndexError to handle "empty", check `len(lst)` first

---
