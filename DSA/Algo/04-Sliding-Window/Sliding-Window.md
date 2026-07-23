---
date: 2026-07-20
type: concept
tags:
  - dsa
  - algorithms
  - sliding-window
---

# Sliding Window

## Definition
A technique that maintains a range (window) over a sequence and updates it incrementally as it moves, avoiding redundant recomputation.

## Types

### Fixed Size Window
The window size $k$ is given. Slide one step at a time.

- Maximum / minimum sum of $k$ elements
- Average of subarrays of size $k$
- Count occurrences of anagram in a string

### Variable Size Window
The window grows or shrinks based on a condition.

- Longest substring without repeating characters
- Smallest subarray with sum $\geq$ target
- Longest substring with at most $k$ distinct characters

## Time Complexity
$O(n)$ — each element is added and removed at most once.

## Key Patterns
- [[04-Sliding-Window-Patterns.md#Fixed Size — Maximum Sum|Fixed window: max sum]]
- [[04-Sliding-Window-Patterns.md#Variable Size — Longest Substring Without Repeating|Variable: longest substring]]
- [[04-Sliding-Window-Patterns.md#Variable Size — Minimum Window Substring|Variable: min window substring]]
- [[04-Sliding-Window-Patterns.md#Fixed Size — Count Anagrams|Fixed: count anagrams]]
- [[04-Sliding-Window-Patterns.md#Variable Size — Longest Substring with K Distinct|Variable: K distinct chars]]

---

**See also:** [[../03-Two-Pointers/Two-Pointers.md|Two Pointers]], [[../../DS/05-Queues/Queues.md|Queues]]
