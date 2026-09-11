
## Definition

When $A\mathbf{x} = \mathbf{b}$ has no solution, solve the closest problem instead:

$$\text{minimise } \|A\mathbf{x} - \mathbf{b}\|^2 \iff A^TA\hat{\mathbf{x}} = A^T\mathbf{b}$$

$\hat{\mathbf{x}}$ = least-squares solution; $\mathbf{p} = A\hat{\mathbf{x}}$ = best approximation in $C(A)$; $\mathbf{e}$ = residual. Used for fitting lines to data.

**Linear fit:** $y = c_1 + c_2 t$ from $n$ points → columns $(1, \dots, 1)$ and $(t_1, \dots, t_n)$.

## The Intuition

$\mathbf{b}$ off the floor: the least-squares answer drops the perpendicular and reports where it lands. "Least squares" because you minimise the *sum of squared vertical errors* — the standard regression criterion.

## The Toolkit

| Quantity | Formula |
|----------|---------|
| Normal equations | $A^TA\hat{\mathbf{x}} = A^T\mathbf{b}$ |
| Best fit $\mathbf{p}$ | $A\hat{\mathbf{x}}$ |
| Residual | $\mathbf{e} = \mathbf{b} - \mathbf{p}$, $\mathbf{e} \perp C(A)$ |
| Projection matrix | $P = A(A^TA)^{-1}A^T$ |
| Fit parameters | $\hat{\mathbf{x}} = (A^TA)^{-1}A^T\mathbf{b}$ |

## Derivation

$A^T\mathbf{e} = \mathbf{0}$ (perpendicularity) → $A^T(\mathbf{b} - A\hat{\mathbf{x}}) = \mathbf{0}$ → normal equations. If columns are independent, $A^TA$ is invertible and $\hat{\mathbf{x}}$ is unique. [Full derivations: Projections]

## Method

1. Build $A$ (basis vectors as columns) and $\mathbf{b}$.
2. Form $A^TA$ and $A^T\mathbf{b}$.
3. Solve the normal equations for $\hat{\mathbf{x}}$.
4. $\mathbf{p} = A\hat{\mathbf{x}}$; check residual ⊥ column space.

## Worked Examples

**Setup:** Fit $y = c_1 + c_2t$ through $(1,1), (2,2), (3,4)$.

**Solution:** $A = \begin{bmatrix}1&1\\1&2\\1&3\end{bmatrix}$, $\mathbf{b} = (1,2,4)$. $A^TA = \begin{bmatrix}3&6\\6&14\end{bmatrix}$, $A^T\mathbf{b} = (7,17)$. Solve: $c_2 = \frac{3}{2}$, $c_1 = -\frac13$. Best line $y = -\frac13 + \frac32 t$.

**Key insight:** The line minimises the sum of squared vertical errors.

## Common Traps

- $A^TA$ singular when columns are dependent — need independent columns
- Least squares minimises *vertical* error (for $y$ on $t$), not perpendicular distance
- Don't solve $A\mathbf{x} = \mathbf{b}$ directly when inconsistent — use the normal equations
- The residual has zero dot product with every column

## Connections

- Projections · Orthogonal-Vectors-and-Subspaces
- Transposes-and-Symmetric-Matrices — $A^TA$ symmetric


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

- [[02-Linear-Regression]] — normal equation = least squares in closed form
