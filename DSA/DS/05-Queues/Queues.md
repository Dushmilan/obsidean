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

**Key Patterns:** [[05-Queues-Patterns#Basic Queue Operations|Basic operations]], [[05-Queues-Patterns#Circular Queue|Circular queue]], [[05-Queues-Patterns#Deque as Stack / Queue|Deque usage]], [[05-Queues-Patterns#First Non-Repeating Character in Stream|First non-repeating character]], [[05-Queues-Patterns#Sliding Window Maximum|Sliding window maximum]]

**Applications:** BFS on graphs/trees, task scheduling, buffering (I/O, streaming), cache (LRU with deque), breadth-first traversal.

---
**See also:** [[../04-Stacks/Stacks.md|Stacks]], [[../../Algo/08-Dynamic-Programming/Dynamic-Programming.md|DP]], [[../../Algo/04-Sliding-Window/Sliding-Window.md|Sliding Window]]
