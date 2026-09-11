Searching is the problem of finding the position of a target element within a collection. It ranges from the trivial linear scan to the efficient binary search that cuts the search space in half with each step.

**The Intuition:** Imagine looking up a word in a dictionary. You don't start at page 1 — you open roughly to the middle, see if your word is before or after, and repeat. That's binary search. Linear search, by contrast, is reading every page from the start until you find it.

**The Math:**

| Method | Best | Average | Worst | Prerequisite |
|--------|------|---------|-------|-------------|
| Linear Search | $O(1)$ | $O(n)$ | $O(n)$ | None |
| Binary Search | $O(1)$ | $O(\log n)$ | $O(\log n)$ | Sorted array |

**Key Patterns:** [[02-Searching-Patterns]], [[02-Searching-Patterns]], [[02-Searching-Patterns]], [[02-Searching-Patterns]], [[02-Searching-Patterns]], [[02-Searching-Patterns]]

**Applications:** Lookup in databases (indexed), debugging (git bisect), range queries, finding boundaries.

---
**See also:** [[Sorting]], [[Two-Pointers]]
