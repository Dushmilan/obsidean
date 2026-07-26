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

**Key Patterns:** [[02-Strings-Patterns#Palindrome Check|Palindrome check]], [[02-Strings-Patterns#Anagram Check|Anagram check]], [[02-Strings-Patterns#Character Frequency Count|Character frequency]], [[02-Strings-Patterns#String Reversal|String reversal]], [[02-Strings-Patterns#Substring Search (KMP)|KMP substring search]], [[02-Strings-Patterns#Longest Substring Without Repeating Characters|Longest substring without repeating]], [[02-Strings-Patterns#Group Anagrams|Group anagrams]]

**Applications:** Text processing and parsing, pattern matching, encoding/decoding, input validation.

---
**See also:** [[../01-Arrays/Arrays.md|Arrays]], [[../../Algo/04-Sliding-Window/Sliding-Window.md|Sliding Window]]
