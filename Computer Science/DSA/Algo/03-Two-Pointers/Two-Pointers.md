Two pointers is a technique where you use two indices to traverse a data structure — typically from opposite ends or at different speeds — to solve problems in linear time with constant space.

**The Intuition:** Think of two people walking towards each other on a hallway. If the hallway is sorted (shortest room numbers on one end, longest on the other), they can meet in the middle and find pairs that sum to a target. One pointer chases while the other leads — together they scan the array in a single pass instead of nested loops.

**The Math:**

- **Time:** $O(n)$ — one pass through the array
- **Space:** $O(1)$ — just two indices

**When to use:**
- **Sorted input** — find pairs, triplets, or ranges
- **In-place modification** — partition, deduplicate
- **Slow-fast** — cycle detection, middle of linked list
- **Opposite ends** — palindrome check, two-sum in sorted array

**Key Patterns:** [[03-Two-Pointers-Patterns]], [[03-Two-Pointers-Patterns]], [[03-Two-Pointers-Patterns]], [[03-Two-Pointers-Patterns]], [[03-Two-Pointers-Patterns]]

---
**See also:** [[Sliding-Window]], [[Linked-Lists]]
