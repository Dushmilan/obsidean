
## Definition

$A^{-1}$ undoes $A$: $A^{-1}A = AA^{-1} = I$. It exists iff $A$ is **square and full rank** — equivalently: independent columns, nonzero determinant, trivial nullspace, rank $n$.

**Gauss–Jordan:** form $[A \mid I]$, row-reduce until the left block is $I$; the right block becomes $A^{-1}$.

## The Intuition

The inverse is elimination run **in reverse** — a record of every step, played backwards. If $A^{-1}$ exists, $A\mathbf{x} = \mathbf{b}$ has the unique solution $\mathbf{x} = A^{-1}\mathbf{b}$. Think: the machine $A$ has an "un-machine" $A^{-1}$ that restores whatever was transformed.

## The Toolkit

| Property | Rule |
|----------|------|
| Product inverse | $(AB)^{-1} = B^{-1}A^{-1}$ |
| Transpose inverse | $(A^T)^{-1} = (A^{-1})^T$ |
| Diagonal | $\text{diag}(d_i)^{-1} = \text{diag}(1/d_i)$ |
| $(cA)^{-1}$ | $\frac{1}{c}A^{-1}$, $c \neq 0$ |

## Derivation

$[A\mid I] \xrightarrow{\text{eliminate}} [I \mid A^{-1}]$ because every elimination step on the left is mirrored by the same step on the right — the right block accumulates exactly $A^{-1}$. $(AB)^{-1} = B^{-1}A^{-1}$ follows from checking $B^{-1}A^{-1}\cdot AB = I$: the inner $A^{-1}A$ collapses. [Full derivations: Gaussian-Elimination]

## Method

1. Check square + full rank (no zero pivot).
2. Form $[A \mid I]$; Gauss–Jordan to $[I \mid A^{-1}]$.
3. Solve $A\mathbf{x} = \mathbf{b}$ via $\mathbf{x} = A^{-1}\mathbf{b}$ (or cheaper: elimination once, apply to $\mathbf{b}$).

## Worked Examples

**Setup:** Invert $A = \begin{bmatrix}1&2\\3&4\end{bmatrix}$.

**Solution:** $[A\mid I] = \begin{bmatrix}1&2&1&0\\3&4&0&1\end{bmatrix}$. Row 2 − 3·Row 1 → $\begin{bmatrix}1&2&1&0\\0&-2&-3&1\end{bmatrix}$. Row 1 − 1·Row 2 → $\begin{bmatrix}1&0&-2&1\\0&1&1.5&-0.5\end{bmatrix}$. So $A^{-1} = \begin{bmatrix}-2&1\\1.5&-0.5\end{bmatrix}$.

**Key insight:** Every row operation applies to both halves — the right half records the inverse.

---

**Setup:** Solve $\begin{bmatrix}1&2\\3&4\end{bmatrix}\mathbf{x} = \begin{bmatrix}5\\11\end{bmatrix}$.

**Solution:** $\mathbf{x} = A^{-1}\mathbf{b} = \begin{bmatrix}-2&1\\1.5&-0.5\end{bmatrix}\begin{bmatrix}5\\11\end{bmatrix} = \begin{bmatrix}1\\2\end{bmatrix}$.

**Key insight:** Check: $1(1,3)+2(2,4) = (5,11)$ ✓.

## Common Traps

- Inverting non-square or singular matrices (doesn't exist)
- Order reversal: $(AB)^{-1} = B^{-1}A^{-1}$, never $A^{-1}B^{-1}$
- Gauss–Jordan must use the *same* operations on both blocks
- Solving via $A^{-1}$ is conceptually clean but numerically costlier than elimination for big systems

## Connections

- Four-Ways-to-Multiply · Gaussian-Elimination
- LU-Decomposition — the practical solver
- Linear-Independence — why full rank matters
