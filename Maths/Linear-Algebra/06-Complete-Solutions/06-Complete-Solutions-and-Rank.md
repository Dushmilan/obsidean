# Complete Solutions and the Rank Cases

We know how to eliminate and factor — but what does the solution to $A\mathbf{x} = \mathbf{b}$ actually look like? The complete solution is a particular solution plus any nullspace vector: $\mathbf{x}_{\text{complete}} = \mathbf{x}_p + c_1 \mathbf{x}_{s1} + \cdots$. The number of solutions depends entirely on the rank $r$ of $A$. This is the complete answer to "what does $A\mathbf{x} = \mathbf{b}$ look like?" — it tells you both whether solutions exist and how many there are.

**The Intuition:** $\mathbf{x}_p$ is the "base camp" — one solution you can find easily by setting all free variables to zero. The nullspace is the "range of wandering" — directions you can move from base camp without leaving the solution set. Think of the solution set as a flat surface (line, plane, hyperplane) floating in $\mathbb{R}^n$, offset from the origin by $\mathbf{x}_p$. If $A\mathbf{x}_p = \mathbf{b}$ and $A\mathbf{x}_n = \mathbf{0}$, then $A(\mathbf{x}_p + \mathbf{x}_n) = \mathbf{b} + \mathbf{0} = \mathbf{b}$ — any combination of a particular solution and a nullspace vector is also a solution.

**The Math:** $A$ is $m \times n$ with rank $r$. $\mathbf{b}$ must be in $C(A)$ for solutions to exist. If $\mathbf{b}$ is not in $C(A)$, no solutions exist. If $r = n$, the nullspace is trivial and there is at most one solution. The solution set is an affine subspace — a translate of a subspace — not a subspace itself (it doesn't contain the origin unless $\mathbf{b} = \mathbf{0}$). There are infinitely many particular solutions (any $\mathbf{x}_p +$ nullspace vector works), but the one with free variables $= 0$ is the easiest to compute. Every matrix falls into exactly one of four rank cases: full rank ($m = n$, $r = n$) gives exactly 1 solution for every $\mathbf{b}$; full column rank ($m > n$, $r = n$) gives 0 or 1; full row rank ($m < n$, $r = m$) gives infinite solutions for every $\mathbf{b}$; defective ($r < m$, $r < n$) gives 0 or infinite.

**Worked Examples:**

**Example 1: Full rank, unique solution.** $A = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$, $\mathbf{b} = (3, 5)^T$. RREF is $I$. No free variables. $\mathbf{x}_p = (3, 5)^T$. The solution is unique.

**Key insight:** Full rank means the nullspace is trivial — the only way to stay in the solution set is to not move at all.

**Example 2: Full column rank, one solution.** $A = \begin{bmatrix} 1 & 2 \\ 2 & 4 \\ 1 & 1 \end{bmatrix}$, $\mathbf{b} = (5, 10, 3)^T$. Eliminate on $[A \mid \mathbf{b}]$ to check if $\mathbf{b}$ is in $C(A)$. It is. $r = 2 = n$, so no free variables. $\mathbf{x}_p = (1, 2)^T$ (unique).

**Key insight:** Full column rank means at most one solution — existence depends entirely on whether $\mathbf{b}$ is reachable.

**Example 3: Full row rank, infinite solutions.** $A = \begin{bmatrix} 1 & 2 & 3 \\ 2 & 4 & 6 \end{bmatrix}$, $\mathbf{b} = (1, 2)^T$. $r = 1 < n = 3$. Two free variables. $\mathbf{x}_p = (1, 0, 0)^T$. Complete solution: $\mathbf{x} = (1, 0, 0)^T + c_1(-2, 1, 0)^T + c_2(-3, 0, 1)^T$.

**Key insight:** Full row rank means solutions exist for every $\mathbf{b}$, but there are infinitely many — the nullspace gives you the freedom.

Always check if $\mathbf{b}$ is in $C(A)$ before looking for solutions. Find $\mathbf{x}_p$ by setting free variables to 0, find the nullspace by solving $A\mathbf{x} = \mathbf{0}$, then combine: $\mathbf{x}_{\text{complete}} = \mathbf{x}_p + N(A)$.
