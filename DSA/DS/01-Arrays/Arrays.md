An array is a contiguous block of memory storing elements of the same type, accessed via an index (offset from the base address). It's the most fundamental data structure — everything else is built on top of it.

**The Intuition:** Think of an array like a row of mailboxes in an apartment building, each numbered sequentially. If you know the mailbox number, you can walk straight there — that's $O(1)$ access. But if you want to insert a new mailbox in the middle, you'd have to shift every mailbox after it down by one — that's $O(n)$.

**The Math:**

| Operation         | Static Array | Dynamic Array    |
| ----------------- | ------------ | ---------------- |
| Access            | $O(1)$       | $O(1)$           |
| Search            | $O(n)$       | $O(n)$           |
| Insert (at end)   | —            | $O(1)$ amortized |
| Insert (at index) | —            | $O(n)$           |
| Delete (at end)   | —            | $O(1)$           |
| Delete (at index) | —            | $O(n)$           |

**Properties:** Contiguous memory (cache-friendly, fast iteration), indexable ($O(1)$ random access). Dynamic arrays (Python `list`, Java `ArrayList`) are resizable with amortized $O(1)$ append.

**Key Patterns:** [[01-Arrays-Patterns#Traversal|Traversal]], [[01-Arrays-Patterns#Reverse In-Place|Reverse in-place]], [[01-Arrays-Patterns#Rotate|Rotate left/right]], [[01-Arrays-Patterns#Prefix Sum|Prefix sum / cumulative sum]], [[01-Arrays-Patterns#Two Sum|Two-sum (brute force + hash map)]], [[01-Arrays-Patterns#Maximum Subarray (Kadane's)|Kadane's algorithm]], [[01-Arrays-Patterns#Dutch National Flag|Dutch national flag (3-way partition)]]

**Applications:** Foundation for all other data structures, matrix operations (2D arrays), buffer / sliding window problems, lookup tables and caches.

---
**See also:** [[../02-Strings/Strings.md|Strings]], [[../../Algo/02-Searching/Searching.md|Searching]]
