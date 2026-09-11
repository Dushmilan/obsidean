A queue is a FIFO (First In, First Out) data structure — elements are added at the rear and removed from the front. It's the backbone of BFS, task scheduling, and buffering systems.

**The Intuition:** Think of a checkout line at a grocery store. The first person in line gets served first. New people join at the back. That's FIFO. A priority queue is like a hospital ER — the most critical patient (highest priority) gets seen next, regardless of arrival order.

**The Math:**

| Operation | Queue | Deque | Priority Queue |
|-----------|-------|-------|----------------|
| Enqueue / Offer | $O(1)$ | $O(1)$ | $O(\log n)$ |
| Dequeue / Poll | $O(1)$ | $O(1)$ | $O(\log n)$ |
| Peek (front) | $O(1)$ | $O(1)$ | $O(1)$ |
| Search | $O(n)$ | $O(n)$ | $O(n)$ |

**Types:** Simple FIFO queue, circular queue (rear wraps to front for efficient space), deque (double-ended, insert/remove at both ends), priority queue (elements ordered by priority).

**Key Patterns:** [[05-Queues-Patterns]], [[05-Queues-Patterns]], [[05-Queues-Patterns]], [[05-Queues-Patterns]], [[05-Queues-Patterns]]

**Applications:** BFS on graphs/trees, task scheduling, buffering (I/O, streaming), cache (LRU with deque), breadth-first traversal.

---
**See also:** [[Stacks]], [[Dynamic-Programming]], [[Sliding-Window]]
