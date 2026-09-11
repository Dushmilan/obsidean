## Definition

The **cofactor** of entry $a_{ij}$ is $C_{ij} = (-1)^{i+j}\,M_{ij}$, where $M_{ij}$ (the *minor*) is the determinant of the $(n-1)\times(n-1)$ matrix left by deleting row $i$ and column $j$. Cofactors unlock four closed-form results: the big determinant formula, the explicit inverse, Cramer's rule, and volume.

## The Intuition

The full $n\times n$ determinant formula has $n!$ products — hopeless by hand beyond $n=3$. Cofactors are the recursion that tames it: peel off one row, shrink the problem to size $n-1$, repeat until you reach easy $2\times2$s. Cramer's rule reads each unknown straight off a ratio of determinants — each system component gets its own "area ratio." And $|\det A|$ finally cashes the geometric promise: it *is* area (2D) or volume (3D) of the box spanned by the rows.

## The Formulas

| Result | Formula |
|--------|---------|
| 2×2 base case | $\begin{vmatrix}a&b\\ c&d\end{vmatrix} = ad-bc$ |
| Cofactor expansion | $\det A = \sum_{j} a_{ij} C_{ij}$ (expand along **any** row $i$ or column $j$) |
| Explicit inverse | $A^{-1} = \dfrac{C^T}{\det A}$ (transpose of cofactor matrix) |
| Cramer's rule | $x_j = \dfrac{\det B_j}{\det A}$, where $B_j$ = $A$ with column $j$ replaced by $\mathbf{b}$ |
| Volume | $|\det A|$ = area (2D) / volume (3D) of the box spanned by rows |

## Method

1. Expand along the **emptiest** row/column — zeros kill their cofactor terms for free.
2. For $A^{-1}$ by hand: compute all cofactors, transpose, divide by $\det A$.
3. Use Cramer's rule only when $\det A \neq 0$ and you need *one* component — elimination is faster for full solutions.
4. Area/volume questions → build the matrix of edge vectors, take $|\det|$.

## Worked Examples

**Example 1 — full 3×3 cofactor expansion.** $A=\begin{bmatrix}2&1&0\\1&3&2\\0&1&4\end{bmatrix}$. Expand along row 1 (contains a zero):

$$\det A = 2\begin{vmatrix}3&2\\1&4\end{vmatrix} - 1\begin{vmatrix}1&2\\0&4\end{vmatrix} + 0 = 2(12-2) - 1(4-0) = 20-4 = 16$$

**Example 2 — Cramer's rule on a 2×2.** Solve $x+y=3,\; 2x-y=0$. Here $A=\begin{bmatrix}1&1\\2&-1\end{bmatrix}$, $\det A = -1-2 = -3$. Replace column 1 by $\mathbf{b}=(3,0)$: $\det B_x = -3$ → $x = \frac{-3}{-3}=1$. Replace column 2: $\det B_y = -6$ → $y = \frac{-6}{-3}=2$. Check: $1+2=3$ ✓, $2-2=0$ ✓.

**Example 3 — area from a determinant.** Parallelogram spanned by $\mathbf{u}=(1,2)$, $\mathbf{v}=(3,1)$: $\left|\begin{smallmatrix}1&2\\3&1\end{smallmatrix}\right| = |1-6| = 5$ square units. Negative sign would have meant orientation flip — area keeps the absolute value.

**Practical honesty:** for solving real systems, elimination beats Cramer ($O(n^3)$ vs far worse). Cramer's value is *structural* — it expresses solutions as pure ratios of volumes, no process required.

**Key insight:** the determinant is three tools in one — a singularity test, an inversion formula denominator, and a literal volume — all connected by the cofactor recursion. [Foundation: Ten-Properties-of-Determinants]
