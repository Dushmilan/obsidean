
## Definition

Gaussian elimination recorded as a product:

$$A = LU \quad (PA = LU \text{ when row exchanges are needed})$$

- $L$: lower triangular, 1s on the diagonal, holding the multipliers $l_{ij}$.
- $U$: upper triangular, pivots on the diagonal.
- $LDU$ form: factor pivots into diagonal $D$, so $L$ and $U$ both have 1s on the diagonal.

Requires all pivots nonzero (or made so by permutation).

## The Intuition

$L$ is the set of *undo instructions* — to recover $A$ from $U$, apply the elimination steps in reverse. $L$ is the recipe, $U$ is the result; together they reconstruct $A$ exactly. Each elimination step is an elementary matrix, and the product of their inverses is $L$.

## The Toolkit

| Form | When |
|------|------|
| $A = LU$ | no row exchanges needed |
| $PA = LU$ | pivots need swaps |
| $A = LDU$ | pivots factored into $D$ |

**Cost:** elimination $n^3/3$ operations; solving per $\mathbf{b}$ only $n^2$.

## Derivation

$E_2E_1A = U \Rightarrow A = E_1^{-1}E_2^{-1}U = LU$. Inverses of elementary matrices are elementary, so their product is lower triangular with the multipliers in place — the key fact is that elimination never messes up the already-done columns, so $L$'s entries are exactly the recorded $l_{ij}$. [Full derivations: Gaussian-Elimination]

## Method

1. Eliminate $A \to U$, recording each multiplier into $L$.
2. If a pivot is zero, swap rows and record in $P$: $PA = LU$.
3. Solve via $L\mathbf{y} = \mathbf{b}$ then $U\mathbf{x} = \mathbf{y}$ (two triangular solves).

## Worked Examples

**Setup:** $A = \begin{bmatrix}2&1&1\\4&3&3\\8&7&9\end{bmatrix}$. Find $LU$.

**Solution:** $l_{21}=2$, $l_{31}=4$, $l_{32}=2$. $U = \begin{bmatrix}2&1&1\\0&1&1\\0&0&2\end{bmatrix}$, $L = \begin{bmatrix}1&0&0\\2&1&0\\4&2&1\end{bmatrix}$.

**Key insight:** The multipliers *are* $L$ — no extra computation.

---

**Setup:** Why permute? $A = \begin{bmatrix}0&1\\1&2\end{bmatrix}$.

**Solution:** First pivot is 0 — swap rows: $P = \begin{bmatrix}0&1\\1&0\end{bmatrix}$, $PA = \begin{bmatrix}1&2\\0&1\end{bmatrix} = LU$.

**Key insight:** Zero pivot → mandatory permutation; without it, no $LU$ exists.

## Common Traps

- Recording a multiplier into the wrong position of $L$
- Forgetting the permutation when a pivot is zero
- $L$ has 1s on the diagonal; $U$ holds the pivots — don't swap their roles
- Using $LU$ when $PA = LU$ is required

## Connections

- Solving-with-LU-and-LDU · Gaussian-Elimination
- Inverses-and-Gauss-Jordan — the alternative
