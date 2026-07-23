---
date: 2026-07-20
type: concept
tags:
  - dsa
  - data-structures
  - hash-tables
---

# Hash Tables

## Definition
A data structure that maps keys to values using a hash function. Provides average $O(1)$ lookups.

## Components
- **Key** — unique identifier
- **Hash function** — maps key to bucket index
- **Hash table** — array of buckets (linked lists or trees)

## Collision Resolution
- **Chaining** — each bucket stores a linked list of entries
- **Open addressing** — probe next empty slot (linear, quadratic, double hashing)

## Time Complexity

| Operation | Average | Worst Case |
|-----------|---------|------------|
| Search | $O(1)$ | $O(n)$ |
| Insert | $O(1)$ | $O(n)$ |
| Delete | $O(1)$ | $O(n)$ |

Worst case occurs when all keys hash to the same bucket.

## Key Patterns
- [[06-Hash-Tables-Patterns.md#Frequency Counter|Frequency counter]]
- [[06-Hash-Tables-Patterns.md#Two Sum with Hash Map|Two sum]]
- [[06-Hash-Tables-Patterns.md#Contains Duplicate|Contains duplicate]]
- [[06-Hash-Tables-Patterns.md#Intersection of Two Arrays|Intersection of two arrays]]
- [[06-Hash-Tables-Patterns.md#Subarray Sum Equals K|Subarray sum equals K]]
- [[06-Hash-Tables-Patterns.md#Hash Set Usage|Hash set usage]]

## Applications
- Database indexing
- Caching (dictionaries)
- Counting / frequency problems
- De-duplication
- Symbol tables in compilers

---

**See also:** [[../01-Arrays/Arrays.md|Arrays]], [[../02-Strings/Strings.md|Strings]]
