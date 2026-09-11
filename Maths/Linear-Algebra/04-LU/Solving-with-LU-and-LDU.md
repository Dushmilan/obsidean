
## Definition

Once $A = LU$ (or $PA = LU$), solving $A\mathbf{x} = \mathbf{b}$ is **two triangular solves**:

$$L\mathbf{y} = \mathbf{b} \quad \text{(forward substitution)} \qquad U\mathbf{x} = \mathbf{y} \quad \text{(back substitution)}$$

**LDU form:** $A = LDU$ with $L$ and $U$ unit triangular, $D$ holding the pivots. Used for symmetric matrices ($A = LDL^T$) and for reading off pivots directly.

## The Intuition

Factor once, solve many times. Elimination costs $n^3/3$; each new right-hand side costs only $n^2$ with the factorization in hand — a massive saving for multiple $\mathbf{b}$'s. It's why every numerical linear algebra package is built on this.

## The Toolkit

| Step | Operation | Cost |
|------|-----------|------|
| Factor | $A = LU$ | $n^3/3$ |
| Solve $L\mathbf{y} = \mathbf{b}$ | forward substitution | $n^2/2$ |
| Solve $U\mathbf{x} = \mathbf{y}$ | back substitution | $n^2/2$ |
| Total per $\mathbf{b}$ | — | $n^2$ |

## Derivation

$A\mathbf{x} = LU\mathbf{x} = \mathbf{b}$: set $\mathbf{y} = U\mathbf{x}$; solve $L\mathbf{y} = \mathbf{b}$ top-down (each row has one unknown), then $U\mathbf{x} = \mathbf{y}$ bottom-up. The $LDU$ split moves the pivot diagonal out of $U$: $U = DU'$. [Full derivations: LU-Decomposition]

## Method

1. Factor once: $A = LU$ (or $PA = LU$).
2. For each $\mathbf{b}$: solve $L\mathbf{y} = \mathbf{b}$, then $U\mathbf{x} = \mathbf{y}$.
3. For symmetric $A$: use $A = LDL^T$ — half the storage, half the work.

## Worked Examples

**Setup:** Solve $A\mathbf{x} = \mathbf{b}$ where $A = \begin{bmatrix}2&1\\4&3\end{bmatrix}$, $\mathbf{b} = \begin{bmatrix}3\\10\end{bmatrix}$.

**Solution:** $L = \begin{bmatrix}1&0\\2&1\end{bmatrix}$, $U = \begin{bmatrix}2&1\\0&1\end{bmatrix}$. Forward: $y_1 = 3$, $y_2 = 10 - 6 = 4$. Back: $x_2 = 4$, $x_1 = (3-4)/2 = -0.5$.

**Key insight:** Two triangular solves — no re-elimination.

---

**Setup:** Write $A = \begin{bmatrix}2&1\\4&3\end{bmatrix}$ in $LDU$ form.

**Solution:** $D = \begin{bmatrix}2&0\\0&1\end{bmatrix}$ (pivots), $U' = \begin{bmatrix}1&0.5\\0&1\end{bmatrix}$: $A = \begin{bmatrix}1&0\\2&1\end{bmatrix}\begin{bmatrix}2&0\\0&1\end{bmatrix}\begin{bmatrix}1&0.5\\0&1\end{bmatrix}$.

**Key insight:** Pivots visible in $D$ — the diagonal of $U$ divided out.

## Common Traps

- Solving both triangles as one elimination pass (loses the point)
- Forward vs back substitution direction
- Forgetting $P$ when $PA = LU$
- $LDL^T$ only for symmetric (and then only without pivoting in the basic form)

## Connections

- LU-Decomposition · Inverses-and-Gauss-Jordan
- Transposes-and-Symmetric-Matrices — the $LDL^T$ case
- Complete-Solution
