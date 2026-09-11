
## Definition

Every solution of a consistent system has the form

$$\mathbf{x}_{\text{complete}} = \mathbf{x}_p + \underbrace{c_1\mathbf{x}_{s1} + \cdots + c_{n-r}\mathbf{x}_{sn-r}}_{\text{any nullspace vector}}$$

$\mathbf{x}_p$ = a **particular solution** (set free variables to 0); the nullspace part spans all homogeneous solutions. Existence requires $\mathbf{b} \in C(A)$.

## The Intuition

$\mathbf{x}_p$ is the base camp — one easy solution. The nullspace is the range of wandering: directions you can move without changing the output. If $A\mathbf{x}_p = \mathbf{b}$ and $A\mathbf{x}_n = \mathbf{0}$, then $A(\mathbf{x}_p + \mathbf{x}_n) = \mathbf{b}$ — base camp plus any wander stays a solution. The solution set is an **affine subspace** (a translate), not a subspace — it misses the origin unless $\mathbf{b} = \mathbf{0}$.

## The Toolkit

| Case | Solution count |
|------|----------------|
| $\mathbf{b} \notin C(A)$ | none |
| $r = n$ (full column rank) | at most one |
| $r < n$ | infinitely many (if consistent) |
| $\mathbf{b} = \mathbf{0}$ | always solvable (nullspace itself) |

## Derivation

Linearity: $A(\mathbf{x}_p + \mathbf{x}_n) = A\mathbf{x}_p + A\mathbf{x}_n = \mathbf{b} + \mathbf{0}$. Conversely, if $\mathbf{x}$ is any solution, $\mathbf{x} - \mathbf{x}_p$ solves $A\mathbf{x} = \mathbf{0}$, so every solution is $\mathbf{x}_p + N(A)$. [Full derivations: RREF-Free-Variables-and-Special-Solutions]

## Method

1. Check $\mathbf{b} \in C(A)$ (eliminate; consistent = no $0 = \text{nonzero}$ row).
2. Find $\mathbf{x}_p$: set free variables to 0, solve for pivots.
3. Find the special solutions (one per free variable).
4. Write the complete solution.

## Worked Examples

**Setup:** $x + 2y + 3z = 6$ with $A = \begin{bmatrix}1&2&3\end{bmatrix}$.

**Solution:** One equation, free variables $y, z$. $\mathbf{x}_p = (6, 0, 0)$. Nullspace: $y = 1, z = 0$ → $(-2,1,0)$; $y=0,z=1$ → $(-3,0,1)$. Complete: $(6,0,0) + c_1(-2,1,0) + c_2(-3,0,1)$.

**Key insight:** Free variables → $n - r = 2$ wandering directions.

---

**Setup:** $A\mathbf{x} = \mathbf{0}$ always has how many solutions?

**Solution:** At least $\mathbf{x} = \mathbf{0}$; the nullspace is the whole solution set (a subspace).

**Key insight:** Homogeneous systems are never inconsistent.

## Common Traps

- The solution set is an affine space, not a subspace — don't call it one
- $\mathbf{x}_p$ isn't unique — any particular solution works
- If $\mathbf{b} \notin C(A)$, there is no $\mathbf{x}_p$ — check first

## Connections

- Rank-and-Free-Variables · RREF-Free-Variables-and-Special-Solutions
- Four-Fundamental-Subspaces
- Linear-Independence
