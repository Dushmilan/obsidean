Divide and conquer is a strategy that solves a problem by splitting it into smaller subproblems, solving each recursively, then combining the results. It's the backbone of algorithms like merge sort and binary search.

**The Intuition:** Imagine you need to sort a huge pile of exams. Instead of sorting them all yourself, you split the pile in half, give each half to a friend, and when they hand back sorted piles, you merge them together. Each friend does the same thing recursively until the piles are small enough to sort directly.

**The Math:**

**Master Theorem:** $T(n) = aT(n/b) + f(n)$

| Recurrence | Result | Algorithm |
|-----------|--------|-----------|
| $T(n) = 2T(n/2) + O(n)$ | $O(n \log n)$ | Merge sort |
| $T(n) = T(n/2) + O(1)$ | $O(\log n)$ | Binary search |
| $T(n) = 2T(n/2) + O(1)$ | $O(n)$ | — |

**Key Patterns:** [[06-Divide-Conquer-Patterns]], [[06-Divide-Conquer-Patterns]], [[06-Divide-Conquer-Patterns]], [[06-Divide-Conquer-Patterns]], [[06-Divide-Conquer-Patterns]], [[06-Divide-Conquer-Patterns]]

**Applications:** Sorting (merge, quick), search (binary search), matrix multiplication (Strassen), fast Fourier transform (FFT).

---
**See also:** [[Recursion-Backtracking]], [[Sorting]]
