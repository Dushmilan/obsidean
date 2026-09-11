Dynamic programming solves problems by breaking them into overlapping subproblems and storing results so you never compute the same thing twice. It's the go-to when recursion alone would re-solve the same subproblem exponentially many times.

**The Intuition:** Imagine you're climbing stairs and someone asks how many ways you can reach step $n$. You could take 1 step or 2 steps at a time. Instead of recomputing "ways to reach step $n-1$" and "ways to reach step $n-2$" over and over (like naive recursion does), you write each answer down once and reuse it. That's DP — trade memory for time.

**The Math:**

- **Time:** $O(\text{states} \times \text{transitions})$ — number of states times cost per transition
- **Two approaches:**

| Approach | How | Trade-off |
|----------|-----|-----------|
| Top-down (memoization) | Recursive + cache | Natural but stack overhead |
| Bottom-up (tabulation) | Iterative DP table | Faster, no recursion stack |

**When to use:** (1) overlapping subproblems — same subproblem solved multiple times, (2) optimal substructure — optimal solution built from optimal sub-solutions.

**Key Patterns:** [[08-Dynamic-Programming-Patterns]], [[08-Dynamic-Programming-Patterns]], [[08-Dynamic-Programming-Patterns]], [[08-Dynamic-Programming-Patterns]], [[08-Dynamic-Programming-Patterns]], [[08-Dynamic-Programming-Patterns]], [[08-Dynamic-Programming-Patterns]], [[08-Dynamic-Programming-Patterns]]

**Applications:** Optimization problems, sequence alignment (bioinformatics), resource allocation, text justification, pathfinding (Bellman-Ford, Floyd-Warshall).

---
**See also:** [[Recursion-Backtracking]], [[Greedy]]
