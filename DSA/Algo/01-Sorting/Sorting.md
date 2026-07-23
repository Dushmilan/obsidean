---
date: 2026-07-20
type: concept
tags:
  - dsa
  - algorithms
  - sorting
---

# Sorting

## Definition
Rearranging elements into a specific order (typically ascending or descending).

## Comparison Sorts

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes |
| Selection | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | No |
| Insertion | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes |
| Merge | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | Yes |
| Quick | $O(n \log n)$ | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ | No |
| Heap | $O(n \log n)$ | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ | No |

## Key Patterns
- [[01-Sorting-Patterns.md#Bubble Sort|Bubble sort]]
- [[01-Sorting-Patterns.md#Selection Sort|Selection sort]]
- [[01-Sorting-Patterns.md#Insertion Sort|Insertion sort]]
- [[01-Sorting-Patterns.md#Merge Sort|Merge sort]]
- [[01-Sorting-Patterns.md#Quick Sort|Quick sort]]
- [[01-Sorting-Patterns.md#Heap Sort|Heap sort]]
- [[01-Sorting-Patterns.md#Built-in Sort|Built-in sort usage]]

## Applications
- Data processing and reporting
- Binary search prerequisite
- Database ORDER BY operations
- Finding duplicates efficiently

---

**See also:** [[../02-Searching/Searching.md|Searching]], [[../06-Divide-Conquer/Divide-Conquer.md|Divide & Conquer]]
