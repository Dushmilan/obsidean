A linked list is a linear data structure where elements (nodes) are connected via pointers. Each node contains data and a reference to the next node — giving you O(1) insertions at the head, but no random access.

**The Intuition:** Think of a scavenger hunt. Each clue (node) tells you where to go next. You can't jump to clue #5 directly — you have to follow the chain from the start. That's the trade-off: insertion is cheap (just redirect a pointer), but access by index requires walking the whole list.

**The Math:**

| Operation | Singly | Doubly |
|-----------|--------|--------|
| Access by index | $O(n)$ | $O(n)$ |
| Search | $O(n)$ | $O(n)$ |
| Insert at head | $O(1)$ | $O(1)$ |
| Insert at tail | $O(n)$ / $O(1)$* | $O(1)$ |
| Delete at head | $O(1)$ | $O(1)$ |
| Delete by value | $O(n)$ | $O(n)$ |

\* $O(1)$ with tail pointer

**Types:** Singly linked (next only), doubly linked (next + prev), circular linked (last points to head).

**Key Patterns:** [[03-Linked-Lists-Patterns]], [[03-Linked-Lists-Patterns]], [[03-Linked-Lists-Patterns]], [[03-Linked-Lists-Patterns]], [[03-Linked-Lists-Patterns]], [[03-Linked-Lists-Patterns]], [[03-Linked-Lists-Patterns]]

**Applications:** Dynamic memory allocation (free lists), undo/redo in editors (doubly linked), LRU cache, adjacency lists for graphs.

---
**See also:** [[Stacks]], [[Graphs]]
