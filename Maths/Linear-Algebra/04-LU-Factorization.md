# A = LU Factorization

Gaussian elimination can be recorded as a product of two matrices: $L$ (lower triangular, holding the multipliers) and $U$ (upper triangular, holding the pivots), so that $A = LU$. Once you factor $A$, solving $A\mathbf{x} = \mathbf{b}$ becomes two cheap triangular solves instead of redoing elimination every time. This is the foundation of every modern numerical linear algebra package, and it's where [[02-Elimination-and-RREF|elimination]] becomes a reusable tool.

**The Intuition:** Think of $L$ as a set of undo instructions — to recover $A$ from $U$, apply the elimination steps in reverse. $L$ is the recipe of elimination (which rows to subtract from which), and $U$ is the result (the staircase form). Together they reconstruct $A$ exactly. Each elimination step is a matrix, and multiplying all their inverses together gives $L$ — the multipliers line up cleanly because the inverses of elementary matrices are also elementary matrices.

**The Math:** $A$ is $m \times n$. $L$ is lower triangular with 1s on the diagonal; the multiplier $l_{ij}$ used to eliminate entry $(i,j)$ goes directly into position $(i,j)$ of $L$. $U$ is upper triangular with pivots on the diagonal. When row exchanges are needed, $PA = LU$ where $P$ is a [[05-Transposes-Permutations-Spaces|permutation matrix]] recording the swaps. The factorization requires all pivots to be non-zero (or made non-zero by permutation). You can factor out the pivots into a diagonal matrix $D$ to get $A = LDU$, where $L$ and $U$ both have 1s on the diagonal. The cost of elimination is $n^3/3$ operations; once $L$ and $U$ are computed, solving for any new $\mathbf{b}$ costs only $n^2$ operations — a massive savings for multiple right-hand sides.

**What does this mean for computation?** Factor once, solve many times. When the first pivot is zero, a permutation is mandatory — you need $PA = LU$, not just $A = LU$.

**Setup:** $A = \begin{bmatrix} 2 & 1 & 1 \\ 4 & 3 & 3 \\ 8 & 7 & 9 \end{bmatrix}$. Find the LU factorization.

**Solution:** Elimination gives multipliers $l_{21} = 2$, $l_{31} = 4$, $l_{32} = 2$. So $U = \begin{bmatrix} 2 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 0 & 2 \end{bmatrix}$ and $L = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 4 & 2 & 1 \end{bmatrix}$.

**Key insight:** The multipliers go directly into the slots of $L$. That's the memory of elimination.

**Setup:** Using $A = LU$ from above, solve $A\mathbf{x} = \mathbf{b}$ where $\mathbf{b} = (1, 3, 5)^T$.

**Solution:** First solve $L\mathbf{y} = \mathbf{b}$ (forward substitution): $\mathbf{y} = (1, 1, -1)^T$. Then solve $U\mathbf{x} = \mathbf{y}$ (back substitution): $\mathbf{x} = (1, 2, -0.5)^T$.

**Key insight:** Two triangular solves (each $n^2$ cost) instead of redoing elimination.

**Setup:** $A = \begin{bmatrix} 0 & 1 \\ 2 & 3 \end{bmatrix}$. Factor with row exchanges.

**Solution:** The first pivot is zero, so swap rows: $P = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$, $PA = \begin{bmatrix} 2 & 3 \\ 0 & 1 \end{bmatrix}$. Now $L = I$ and $U = PA$.

---
