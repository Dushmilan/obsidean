---
date: 2026-07-20
type: concept
tags:
  - dsa
  - data-structures
  - queues
---

# Queues

## Definition
A FIFO (First In, First Out) data structure. Elements are added at the rear and removed from the front.

## Types
- **Simple Queue** — FIFO
- **Circular Queue** — rear wraps to front, efficient space usage
- **Deque (Double-Ended Queue)** — insert/remove at both ends
- **Priority Queue** — elements ordered by priority

## Time Complexity

| Operation | Queue | Deque | Priority Queue |
|-----------|-------|-------|----------------|
| Enqueue / Offer | $O(1)$ | $O(1)$ | $O(\log n)$ |
| Dequeue / Poll | $O(1)$ | $O(1)$ | $O(\log n)$ |
| Peek (front) | $O(1)$ | $O(1)$ | $O(1)$ |
| Search | $O(n)$ | $O(n)$ | $O(n)$ |

## Key Patterns
- [[05-Queues-Patterns.md#Basic Queue Operations|Basic operations]]
- [[05-Queues-Patterns.md#Circular Queue|Circular queue]]
- [[05-Queues-Patterns.md#Deque as Stack / Queue|Deque usage]]
- [[05-Queues-Patterns.md#First Non-Repeating Character in Stream|First non-repeating character]]
- [[05-Queues-Patterns.md#Sliding Window Maximum|Sliding window maximum]]

## Applications
- BFS on graphs/trees
- Task scheduling
- Buffering (I/O, streaming)
- Cache (LRU with deque)
- Breadth-first traversal

---

**See also:** [[../04-Stacks/Stacks.md|Stacks]], [[../../Algo/08-Dynamic-Programming/Dynamic-Programming.md|DP]], [[../../Algo/04-Sliding-Window/Sliding-Window.md|Sliding Window]]
