---
date: 2026-07-20
type: concept
tags:
  - dsa
  - data-structures
  - strings
---

# Strings

## Definition
A sequence of characters. Implemented as an array of characters under the hood.

## Properties
- **Immutable in Java** — every operation creates a new string
- **Immutable in Python** — strings cannot be modified in-place
- Indexable — $O(1)$ character access
- Length stored explicitly

## Time Complexity

| Operation | Python / Java |
|-----------|---------------|
| Access by index | $O(1)$ |
| Search (char) | $O(n)$ |
| Concatenation | $O(n + m)$ |
| Substring | $O(n)$ (Python), $O(n)$ (Java) |
| Comparison | $O(\min(n, m))$ |

## Key Patterns
- [[02-Strings-Patterns.md#Palindrome Check|Palindrome check]]
- [[02-Strings-Patterns.md#Anagram Check|Anagram check]]
- [[02-Strings-Patterns.md#Character Frequency Count|Character frequency]]
- [[02-Strings-Patterns.md#String Reversal|String reversal]]
- [[02-Strings-Patterns.md#Substring Search (KMP)|KMP substring search]]
- [[02-Strings-Patterns.md#Longest Substring Without Repeating Characters|Longest substring without repeating]]
- [[02-Strings-Patterns.md#Group Anagrams|Group anagrams]]

## Applications
- Text processing and parsing
- Pattern matching
- Encoding/decoding
- Input validation

---

**See also:** [[../01-Arrays/Arrays.md|Arrays]], [[../../Algo/04-Sliding-Window/Sliding-Window.md|Sliding Window]]
