---
date: 2026-07-20
type: concept
tags:
  - dsa
  - algorithms
  - bit-manipulation
---

# Bit Manipulation

## Definition
Operations performed directly on the binary representation of integers. Extremely fast — single CPU instruction each.

## Common Bitwise Operators

| Operator | Symbol | Example |
|----------|--------|---------|
| AND | `&` | `5 & 3 = 1` (101 & 011 = 001) |
| OR | `|` | `5 | 3 = 7` (101 | 011 = 111) |
| XOR | `^` | `5 ^ 3 = 6` (101 ^ 011 = 110) |
| NOT | `~` | `~5 = -6` |
| Left Shift | `<<` | `5 << 1 = 10` (101 → 1010) |
| Right Shift | `>>` | `5 >> 1 = 2` (101 → 010) |

## Key Patterns
- [[10-Bit-Manipulation-Patterns.md#Check if Bit is Set|Check if bit is set]]
- [[10-Bit-Manipulation-Patterns.md#Set / Clear / Toggle Bit|Set / clear / toggle bit]]
- [[10-Bit-Manipulation-Patterns.md#Count Set Bits (Brian Kernighan's)|Count set bits]]
- [[10-Bit-Manipulation-Patterns.md#Power of Two Check|Power of two]]
- [[10-Bit-Manipulation-Patterns.md#Find the Single Non-Repeating Element|Single non-repeating element]]
- [[10-Bit-Manipulation-Patterns.md#Two Non-Repeating Elements|Two non-repeating elements]]
- [[10-Bit-Manipulation-Patterns.md#Subsets via Bitmask|Subsets via bitmask]]

## Applications
- Performance-critical code (embedded, graphics)
- Flags and permissions (Unix file modes)
- Compression and encoding
- Cryptography
- Competitive programming

---

**See also:** [[../05-Recursion-Backtracking/Recursion-Backtracking.md|Subsets (bitmask)]], [[../../DS/06-Hash-Tables/Hash-Tables.md|Hash Tables]]
