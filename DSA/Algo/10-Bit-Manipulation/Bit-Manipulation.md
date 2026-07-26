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

**Key Patterns:** [[10-Bit-Manipulation-Patterns#Check if Bit is Set|Check if bit is set]], [[10-Bit-Manipulation-Patterns#Set / Clear / Toggle Bit|Set / clear / toggle bit]], [[10-Bit-Manipulation-Patterns#Count Set Bits (Brian Kernighan's)|Count set bits]], [[10-Bit-Manipulation-Patterns#Power of Two Check|Power of two]], [[10-Bit-Manipulation-Patterns#Find the Single Non-Repeating Element|Single non-repeating element]], [[10-Bit-Manipulation-Patterns#Two Non-Repeating Elements|Two non-repeating elements]], [[10-Bit-Manipulation-Patterns#Subsets via Bitmask|Subsets via bitmask]]

**Applications:** Performance-critical code (embedded, graphics), flags and permissions (Unix file modes), compression and encoding, cryptography, competitive programming.

---
**See also:** [[../05-Recursion-Backtracking/Recursion-Backtracking.md|Subsets (bitmask)]], [[../../DS/06-Hash-Tables/Hash-Tables.md|Hash Tables]]
