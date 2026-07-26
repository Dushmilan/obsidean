A hash table maps keys to values using a hash function, providing average O(1) lookups. It's the most commonly used data structure for fast key-value access — the backbone of dictionaries, caches, and sets.

**The Intuition:** Think of a hash table like a library with numbered shelves. When you want to store a book (key), you run its title through a formula (hash function) that tells you exactly which shelf number to put it on. To find it later, you run the same formula — no searching needed. But if two books hash to the same shelf (collision), you need a backup plan like chaining them together.

**The Math:**

| Operation | Average | Worst Case |
|-----------|---------|------------|
| Search | $O(1)$ | $O(n)$ |
| Insert | $O(1)$ | $O(n)$ |
| Delete | $O(1)$ | $O(n)$ |

Worst case occurs when all keys hash to the same bucket.

**Collision resolution:** Chaining (each bucket stores a linked list) or open addressing (probe next empty slot — linear, quadratic, double hashing).

**Key Patterns:** [[06-Hash-Tables-Patterns#Frequency Counter|Frequency counter]], [[06-Hash-Tables-Patterns#Two Sum with Hash Map|Two sum]], [[06-Hash-Tables-Patterns#Contains Duplicate|Contains duplicate]], [[06-Hash-Tables-Patterns#Intersection of Two Arrays|Intersection of two arrays]], [[06-Hash-Tables-Patterns#Subarray Sum Equals K|Subarray sum equals K]], [[06-Hash-Tables-Patterns#Hash Set Usage|Hash set usage]]

**Applications:** Database indexing, caching (dictionaries), counting / frequency problems, de-duplication, symbol tables in compilers.

---
**See also:** [[../01-Arrays/Arrays.md|Arrays]], [[../02-Strings/Strings.md|Strings]]
