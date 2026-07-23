---
date: 2026-07-20
type: concept
tags:
  - dsa
  - data-structures
  - arrays
---

# Arrays

## Definition
A contiguous block of memory storing elements of the same type. Each element is accessed via an index (offset from the base address).

## Properties
- **Contiguous memory** — elements are stored sequentially
- **Indexable** — $O(1)$ random access
- **Cache-friendly** — spatial locality means fast iteration

## Types
- **Static array** — fixed size at allocation
- **Dynamic array** (e.g. Python `list`, Java `ArrayList`) — resizable, amortized $O(1)$ append

## Time Complexity

| Operation | Static Array | Dynamic Array |
|-----------|-------------|---------------|
| Access | $O(1)$ | $O(1)$ |
| Search | $O(n)$ | $O(n)$ |
| Insert (at end) | — | $O(1)$ amortized |
| Insert (at index) | — | $O(n)$ |
| Delete (at end) | — | $O(1)$ |
| Delete (at index) | — | $O(n)$ |

## Key Patterns
- [[01-Arrays-Patterns.md#Traversal|Traversal]] — iterate forward/backward
- [[01-Arrays-Patterns.md#Reverse In-Place|Reverse in-place]]
- [[01-Arrays-Patterns.md#Rotate|Rotate left/right]]
- [[01-Arrays-Patterns.md#Prefix Sum|Prefix sum / cumulative sum]]
- [[01-Arrays-Patterns.md#Two Sum|Two-sum (brute force + hash map)]]
- [[01-Arrays-Patterns.md#Maximum Subarray (Kadane's)|Kadane's algorithm]]
- [[01-Arrays-Patterns.md#Dutch National Flag|Dutch national flag (3-way partition)]]

## Applications
- Foundation for all other data structures
- Matrix operations (2D arrays)
- Buffer / sliding window problems
- Lookup tables and caches

---

**See also:** [[../02-Strings/Strings.md|Strings]], [[../../Algo/02-Searching/Searching.md|Searching]]
