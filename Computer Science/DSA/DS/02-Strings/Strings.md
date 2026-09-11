A string is a sequence of characters — implemented as an array of characters under the hood. Strings are everywhere in programming, and most operations on them are O(n) because of their immutable nature in most languages.

**The Intuition:** Think of a string like a necklace where each bead is a character. You can grab any bead by counting from the start (O(1) access). But if you want to change one bead, you can't just pop it out — you have to create an entirely new necklace (immutability). That's why string concatenation in a loop is expensive: you're rebuilding the whole necklace each time.

**The Math:**

| Operation | Time Complexity |
|-----------|-----------------|
| Access by index | $O(1)$ |
| Search (char) | $O(n)$ |
| Concatenation | $O(n + m)$ |
| Substring | $O(n)$ |
| Comparison | $O(\min(n, m))$ |

**Key insight:** Strings are immutable in Python and Java — every operation creates a new string. Use `StringBuilder` (Java) or list joins (Python) for repeated concatenation.

**Key Patterns:** [[02-Strings-Patterns]], [[02-Strings-Patterns]], [[02-Strings-Patterns]], [[02-Strings-Patterns]], [[02-Strings-Patterns]], [[02-Strings-Patterns]], [[02-Strings-Patterns]]

**Applications:** Text processing and parsing, pattern matching, encoding/decoding, input validation.

---
**See also:** [[Arrays]], [[Sliding-Window]]
