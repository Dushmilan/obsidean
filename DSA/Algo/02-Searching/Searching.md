---
date: 2026-07-20
type: concept
tags:
  - dsa
  - algorithms
  - searching
---

# Searching

## Definition
Finding the position of a target element within a collection.

## Types

### Linear Search
Sequentially checks each element.

| Case | Time |
|------|------|
| Best | $O(1)$ |
| Average | $O(n)$ |
| Worst | $O(n)$ |

### Binary Search
Repeatedly divides the search space in half. Requires a **sorted** array.

| Case | Time |
|------|------|
| Best | $O(1)$ |
| Average | $O(\log n)$ |
| Worst | $O(\log n)$ |

## Key Patterns
- [[02-Searching-Patterns.md#Linear Search|Linear search]]
- [[02-Searching-Patterns.md#Binary Search — Basic|Binary search (basic)]]
- [[02-Searching-Patterns.md#Binary Search — First/Last Occurrence|First/last occurrence]]
- [[02-Searching-Patterns.md#Binary Search — Closest Element|Closest element]]
- [[02-Searching-Patterns.md#Binary Search on Rotated Sorted Array|Rotated array]]
- [[02-Searching-Patterns.md#Binary Search — Square Root|Square root (binary search on answer)]]

## Applications
- Lookup in databases (indexed)
- Debugging (git bisect)
- Range queries
- Finding boundaries

---

**See also:** [[../01-Sorting/Sorting.md|Sorting]], [[../03-Two-Pointers/Two-Pointers.md|Two Pointers]]
