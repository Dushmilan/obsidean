---
date: 2026-07-20
type: concept
tags:
  - dsa
  - algorithms
  - dynamic-programming
---

# Dynamic Programming

## Definition
A method for solving problems by breaking them into overlapping subproblems and storing results to avoid redundant computation.

## Approaches

### Top-Down (Memoization)
Recursive + caching. Start from the target and recurse down.

### Bottom-Up (Tabulation)
Iterative. Build solutions from base cases upward. Often uses an array (DP table).

## When to Use DP
1. **Overlapping subproblems** — same subproblem solved multiple times
2. **Optimal substructure** — optimal solution built from optimal sub-solutions

## Time Complexity
$O(\text{states} \times \text{transitions})$ — number of states times transition cost.

## Key Patterns
- [[08-Dynamic-Programming-Patterns.md#Fibonacci (DP)|Fibonacci]]
- [[08-Dynamic-Programming-Patterns.md#Climbing Stairs|Climbing stairs]]
- [[08-Dynamic-Programming-Patterns.md#0/1 KnapSack|0/1 knapsack]]
- [[08-Dynamic-Programming-Patterns.md#Longest Common Subsequence|LCS]]
- [[08-Dynamic-Programming-Patterns.md#Longest Increasing Subsequence|LIS]]
- [[08-Dynamic-Programming-Patterns.md#Coin Change (DP)|Coin change (minimum coins)]]
- [[08-Dynamic-Programming-Patterns.md#Edit Distance|Edit distance]]
- [[08-Dynamic-Programming-Patterns.md#Unbounded KnapSack|Unbounded knapsack]]

## Applications
- Optimization problems
- Sequence alignment (bioinformatics)
- Resource allocation
- Text justification
- Pathfinding (Bellman-Ford, Floyd-Warshall)

---

**See also:** [[../05-Recursion-Backtracking/Recursion-Backtracking.md|Recursion]], [[../07-Greedy/Greedy.md|Greedy]]
