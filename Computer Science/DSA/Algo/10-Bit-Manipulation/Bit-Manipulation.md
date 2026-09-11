Bit manipulation operates directly on the binary representation of integers. Every operation is a single CPU instruction, making it extremely fast — useful in performance-critical code, cryptography, and competitive programming.

**The Intuition:** Imagine you have a light switch panel where each switch represents a bit (0 or 1). Bitwise AND checks if both switches are on. OR turns on a switch if either is on. XOR toggles a switch if the other is on. Shift is like sliding the entire panel left or right — everything moves, new slots fill with zeros.

**The Math:**

| Operator | Symbol | Example | Meaning |
|----------|--------|---------|---------|
| AND | `&` | `5 & 3 = 1` | 101 & 011 = 001 |
| OR | `\|` | `5 \| 3 = 7` | 101 \| 011 = 111 |
| XOR | `^` | `5 ^ 3 = 6` | 101 ^ 011 = 110 |
| NOT | `~` | `~5 = -6` | Bitwise complement |
| Left Shift | `<<` | `5 << 1 = 10` | 101 → 1010 |
| Right Shift | `>>` | `5 >> 1 = 2` | 101 → 010 |

**Key Patterns:** [[10-Bit-Manipulation-Patterns]], [[10-Bit-Manipulation-Patterns]], [[10-Bit-Manipulation-Patterns]], [[10-Bit-Manipulation-Patterns]], [[10-Bit-Manipulation-Patterns]], [[10-Bit-Manipulation-Patterns]], [[10-Bit-Manipulation-Patterns]]

**Applications:** Performance-critical code (embedded, graphics), flags and permissions (Unix file modes), compression and encoding, cryptography, competitive programming.

---
**See also:** [[Recursion-Backtracking]], [[Hash-Tables]]
