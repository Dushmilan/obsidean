---
date: 2026-07-20
type: concept
tags:
  - dsa
  - algorithms
  - recursion
  - backtracking
---

# Recursion & Backtracking

## Recursion
A function that calls itself to solve smaller instances of the same problem.

### Components
- **Base case** — stops the recursion
- **Recursive case** — calls itself with modified input
- **State** — parameters that change with each call

### Time Complexity
Often $O(\text{branches}^{\text{depth}})$ for recursive trees. Analyze with recurrence relations:
- $T(n) = T(n-1) + O(1) \implies O(n)$
- $T(n) = 2T(n/2) + O(n) \implies O(n \log n)$
- $T(n) = 2T(n-1) + O(1) \implies O(2^n)$

## Backtracking
A systematic way to try all possibilities by building candidates incrementally and abandoning them ("backtracking") when they can't lead to a valid solution.

### Template
1. Choose — pick a candidate for the current position
2. Constraint — check if the candidate is valid
3. Recurse — move to the next position
4. Backtrack — undo the choice

## Key Patterns
- [[05-Recursion-Backtracking-Patterns.md#Factorial|Factorial (basic recursion)]]
- [[05-Recursion-Backtracking-Patterns.md#Fibonacci|Fibonacci]]
- [[05-Recursion-Backtracking-Patterns.md#Subsets|Subsets / power set]]
- [[05-Recursion-Backtracking-Patterns.md#Permutations|Permutations]]
- [[05-Recursion-Backtracking-Patterns.md#Combinations|Combinations]]
- [[05-Recursion-Backtracking-Patterns.md#N-Queens|N-Queens]]
- [[05-Recursion-Backtracking-Patterns.md#Generate Parentheses|Generate parentheses]]

---

**See also:** [[../06-Divide-Conquer/Divide-Conquer.md|Divide & Conquer]], [[../08-Dynamic-Programming/Dynamic-Programming.md|DP]]
