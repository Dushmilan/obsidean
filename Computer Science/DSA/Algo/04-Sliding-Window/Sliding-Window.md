The sliding window technique maintains a range (window) over a sequence and updates it incrementally as it moves, avoiding redundant recomputation. It's the go-to for substring/subarray problems with a contiguous constraint.

**The Intuition:** Imagine a window sliding across a row of houses, counting how many have their lights on. Instead of recounting every time you move one house over, you just subtract the house leaving the window and add the house entering it. That's the core idea — reuse work from the previous window position.

**The Math:**

- **Time:** $O(n)$ — each element is added and removed at most once
- **Space:** $O(1)$ or $O(k)$ depending on what you track

**Two flavors:**

| Type | When to Use | Examples |
|------|-------------|----------|
| Fixed size | Window size $k$ is given | Max sum of $k$ elements, anagram count |
| Variable size | Window grows/shrinks on condition | Longest substring without repeating chars, smallest subarray with sum $\geq$ target |

**Key Patterns:** [[04-Sliding-Window-Patterns]], [[04-Sliding-Window-Patterns]], [[04-Sliding-Window-Patterns]], [[04-Sliding-Window-Patterns]], [[04-Sliding-Window-Patterns]]

---
**See also:** [[Two-Pointers]], [[Queues]]
