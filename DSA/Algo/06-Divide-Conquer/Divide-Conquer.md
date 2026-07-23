---
date: 2026-07-20
type: concept
tags:
  - dsa
  - algorithms
  - divide-and-conquer
---

# Divide & Conquer

## Definition
A strategy that solves a problem by:
1. **Divide** — split into smaller subproblems
2. **Conquer** — solve each subproblem recursively
3. **Combine** — merge the results

## Time Complexity
Typically $T(n) = aT(n/b) + f(n)$. Use Master Theorem:
- $T(n) = 2T(n/2) + O(n) \implies O(n \log n)$ (Merge sort)
- $T(n) = T(n/2) + O(1) \implies O(\log n)$ (Binary search)
- $T(n) = 2T(n/2) + O(1) \implies O(n)$

## Key Patterns
- [[06-Divide-Conquer-Patterns.md#Merge Sort|Merge sort]]
- [[06-Divide-Conquer-Patterns.md#Quick Sort|Quick sort]]
- [[06-Divide-Conquer-Patterns.md#Binary Search|Binary search]]
- [[06-Divide-Conquer-Patterns.md#Maximum Subarray (Divide & Conquer)|Maximum subarray]]
- [[06-Divide-Conquer-Patterns.md#Count Inversions|Count inversions]]
- [[06-Divide-Conquer-Patterns.md#Closest Pair of Points|Closest pair of points]]

## Applications
- Sorting (merge, quick)
- Search (binary search)
- Matrix multiplication (Strassen)
- Fast Fourier Transform (FFT)

---

**See also:** [[../05-Recursion-Backtracking/Recursion-Backtracking.md|Recursion]], [[../01-Sorting/Sorting.md|Sorting]]
