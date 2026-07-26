# Elimination and RREF

We know what $A\mathbf{x} = \mathbf{b}$ means — but how do you actually solve it? Gaussian elimination is the fundamental algorithm: reduce any matrix to upper triangular form $U$ by forward elimination, then to reduced row echelon form $R$ by back-substitution. The structure it reveals — pivots, free variables, special solutions — determines everything about the solution set. This is the engine behind [[04-LU-Factorization|LU factorization]] and the entry point to understanding [[06-Complete-Solutions-and-Rank|complete solutions]].

**The Intuition:** Think of elimination like sorting. You rearrange the equations so that each one introduces exactly one new unknown, working from top to bottom. It's like a staircase — each pivot steps down and to the right. Once you reach the bottom, the system is triangular and trivially solvable by back-substitution. Subtracting a multiple of one row from another does not change the solution set — this is the elementary row operation, and it's the engine of all elimination.

**The Math:** $A$ is $m \times n$. Elimination applies row operations (encoded as elementary matrices $E_{ij}$ and permutation matrices $P$) to produce $E \cdots E_2 E_1 A = U$, then continue to $R = \text{rref}(A)$. The number of pivots $r$ is the rank. The number of free variables is $n - r$, and each generates a special solution. The multipliers $l_{ij}$ — the factors you multiplied pivot rows by and subtracted from lower rows — become the entries of $L$ in the factorization $A = LU$. RREF is unique for any matrix, but different elimination sequences can produce different $U$'s; the rank and nullspace are the same regardless. If a zero appears in a pivot position and no row exchange can fix it, the matrix is singular — columns are linearly dependent. The failure is not a bug; it's a diagnosis.

**What does this mean for solving systems?** Eliminate forward to $U$, then read off pivots and free variables. If RREF is $I$, the nullspace is trivial. The nullspace matrix $N$ has columns that are the special solutions — one per free variable, set that free variable to 1 and the rest to 0.

**Setup:** $A = \begin{bmatrix} 1 & 2 & 1 \\ 2 & 6 & 1 \\ 1 & 2 & 4 \end{bmatrix}$, solve $A\mathbf{x} = \mathbf{0}$.

**Solution:** Forward elimination produces 3 pivots. RREF is $I$. No free variables.

**Key insight:** Full rank means the only solution to the homogeneous system is zero — the nullspace is trivial.

**Setup:** $A = \begin{bmatrix} 1 & 2 & 2 \\ 2 & 4 & 6 \end{bmatrix}$, solve $A\mathbf{x} = \mathbf{0}$.

**Solution:** Elimination gives $R = \begin{bmatrix} 1 & 2 & 0 \\ 0 & 0 & 1 \end{bmatrix}$. Column 2 is free. Set it to 1 and solve: special solution $\mathbf{x}_s = (-2, 1, 0)^T$.

**Key insight:** One free variable = one special solution. Set the free variable to 1, solve for the pivot variables.

**Setup:** $A = \begin{bmatrix} 1 & 2 & 3 \\ 2 & 4 & 6 \end{bmatrix}$, solve $A\mathbf{x} = \mathbf{0}$.

**Solution:** RREF is $\begin{bmatrix} 1 & 2 & 3 \\ 0 & 0 & 0 \end{bmatrix}$. Two free variables (columns 2 and 3). Two special solutions: $\mathbf{x}_{s1} = (-2, 1, 0)^T$ and $\mathbf{x}_{s2} = (-3, 0, 1)^T$. The nullspace is a plane in $\mathbb{R}^3$.

**Key insight:** The nullspace matrix $N$ has the special solutions as columns — one per free variable, with that free variable set to 1 and the rest to 0.

---
