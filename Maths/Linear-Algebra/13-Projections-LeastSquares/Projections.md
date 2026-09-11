
## Definition

The **projection** of $\mathbf{b}$ onto a subspace $V$ is the closest point in $V$ to $\mathbf{b}$. Onto a line spanned by $\mathbf{a}$:

$$\hat{x} = \frac{\mathbf{a}^T\mathbf{b}}{\mathbf{a}^T\mathbf{a}}, \qquad \mathbf{p} = \frac{\mathbf{a}\mathbf{a}^T}{\mathbf{a}^T\mathbf{a}}\mathbf{b}$$

Onto $C(A)$: $\mathbf{p} = A\hat{\mathbf{x}}$ where $\hat{\mathbf{x}}$ solves $A^TA\hat{\mathbf{x}} = A^T\mathbf{b}$.

**Projection matrix:** $P = A(A^TA)^{-1}A^T$ — symmetric ($P^T = P$) and idempotent ($P^2 = P$).

## The Intuition

$\mathbf{b}$ is a point in the air, $C(A)$ is the floor. $\mathbf{p}$ is the shadow — the closest floor point; the error $\mathbf{e} = \mathbf{b} - \mathbf{p}$ is the vertical drop. The shortest distance to the floor is along the perpendicular; any other direction is longer.

## The Toolkit

| Quantity | Formula |
|----------|---------|
| Projection onto a line | $\mathbf{p} = \frac{\mathbf{a}\mathbf{a}^T}{\mathbf{a}^T\mathbf{a}}\mathbf{b}$ |
| Projection onto $C(A)$ | $\mathbf{p} = A(A^TA)^{-1}A^T\mathbf{b}$ |
| Normal equations | $A^TA\hat{\mathbf{x}} = A^T\mathbf{b}$ |
| Projection matrix | $P = A(A^TA)^{-1}A^T$ |
| Properties | $P^T = P$, $P^2 = P$ |

## Derivation

The error must be ⊥ to the subspace: $\mathbf{a}^T(\mathbf{b} - \hat{x}\mathbf{a}) = 0$ gives the 1D formula. For $C(A)$: $A^T(\mathbf{b} - A\hat{\mathbf{x}}) = \mathbf{0}$ gives the normal equations. $P^2 = P$ because projecting an already-projected vector changes nothing. [Full derivations: Orthogonal-Complements]

## Method

1. 1D: dot-product formula.
2. General: form $A^TA$ and $A^T\mathbf{b}$; solve the normal equations.
3. Verify: error $\mathbf{e} = \mathbf{b} - \mathbf{p}$ is ⊥ to $C(A)$.

## Worked Examples

**Setup:** Project $\mathbf{b} = (1,2,2)$ onto the line through $\mathbf{a} = (1,1,1)$.

**Solution:** $\hat{x} = \frac{5}{3}$, $\mathbf{p} = \frac53(1,1,1)$, $\mathbf{e} = (-\frac23,\frac13,\frac13)$, and $\mathbf{a}^T\mathbf{e} = -\frac23+\frac13+\frac13 = 0$ ✓.

**Key insight:** The error is perpendicular — that's the defining property.

## Common Traps

- Projection ≠ projection matrix when $A^TA$ isn't invertible (dependent columns)
- $\mathbf{p} \in C(A)$ always; $\hat{\mathbf{x}}$ is in the *parameter* space
- $P$ projects onto $C(A)$, and $I - P$ projects onto $N(A^T)$

## Connections

- Least-Squares · Orthogonal-Vectors-and-Subspaces
- Four-Fundamental-Subspaces — where $\mathbf{p}$ lives
