Recursion is a function calling itself to solve smaller instances of the same problem. Backtracking builds on this by systematically exploring all possibilities, abandoning branches that can't lead to a valid solution.

**The Intuition:** Think of recursion like Russian nesting dolls — you open one, find a smaller one inside, open that, and keep going until you hit the smallest one (the base case). Backtracking is like exploring a maze: you try a path, and if you hit a dead end, you undo your last choice and try a different route.

**The Math:**

**Recurrence relations for common patterns:**

| Recurrence | Complexity | Pattern |
|-----------|-----------|---------|
| $T(n) = T(n-1) + O(1)$ | $O(n)$ | Linear recursion |
| $T(n) = 2T(n/2) + O(n)$ | $O(n \log n)$ | Divide & conquer |
| $T(n) = 2T(n-1) + O(1)$ | $O(2^n)$ | Exponential branching |

**Backtracking template:**
1. **Choose** — pick a candidate for the current position
2. **Constraint** — check if the candidate is valid
3. **Recurse** — move to the next position
4. **Backtrack** — undo the choice

**Key Patterns:** [[05-Recursion-Backtracking-Patterns#Factorial|Factorial (basic recursion)]], [[05-Recursion-Backtracking-Patterns#Fibonacci|Fibonacci]], [[05-Recursion-Backtracking-Patterns#Subsets|Subsets / power set]], [[05-Recursion-Backtracking-Patterns#Permutations|Permutations]], [[05-Recursion-Backtracking-Patterns#Combinations|Combinations]], [[05-Recursion-Backtracking-Patterns#N-Queens|N-Queens]], [[05-Recursion-Backtracking-Patterns#Generate Parentheses|Generate parentheses]]

---
**See also:** [[../06-Divide-Conquer/Divide-Conquer.md|Divide & Conquer]], [[../08-Dynamic-Programming/Dynamic-Programming.md|DP]]
