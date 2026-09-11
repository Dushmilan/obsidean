
## Definition

Kirchhoff's laws are linear algebra statements about the incidence matrix $A$:

- **KCL (current):** $A^T\mathbf{y} = \mathbf{0}$ — net current entering each node is zero. Basis of $N(A^T)$ = independent loops, count $m-n+1$.
- **KVL (voltage):** potentials $\mathbf{x}$ satisfy $A\mathbf{x} = \mathbf{v}$ (drops); consistency requires $\mathbf{v} \perp N(A^T)$.

## The Intuition

Current can't pile up at a node (KCL); around any loop, the voltage drops sum to zero (KVL). The matrix form says: the row space holds the "true" potentials, the left nullspace holds the impossible drop patterns.

## The Toolkit

| Law | Matrix form | Space |
|-----|-------------|-------|
| KCL | $A^T\mathbf{y} = \mathbf{0}$ | $N(A^T)$ = loops |
| KVL | $A\mathbf{x} = \mathbf{v}$ solvable iff $\mathbf{v} \perp N(A^T)$ | $C(A)$ = row space |
| Ohm (optional) | $\mathbf{y} = C\mathbf{v}$ diagonal | — |

## Derivation

$A^T\mathbf{y}$ at node $i$ sums the signed currents of incident edges — setting it to zero is KCL. Consistency of $A\mathbf{x} = \mathbf{v}$ requires $\mathbf{v} \in C(A)$, which by orthogonality is $\mathbf{v} \perp N(A^T)$ — the loop condition of KVL. [Full derivations: Incidence-Matrices]

## Method

1. Write the incidence matrix.
2. KCL: solve $A^T\mathbf{y} = \mathbf{0}$ → loop currents.
3. KVL: solve $A\mathbf{x} = \mathbf{v}$ → node potentials.

## Worked Examples

**Setup:** 3 edges, 3 nodes in a triangle. A drop vector along the closed loop must satisfy?

**Solution:** The loop vector $\mathbf{y} = (1,1,1)$ (going around) satisfies $A^T\mathbf{y} = \mathbf{0}$ — KCL. Any consistent $\mathbf{v}$ must be orthogonal to it (KVL).

**Key insight:** KVL is exactly "drop vectors lie in $C(A)$."

## Common Traps

- KCL vs KVL space mix-up: currents in $N(A^T)$, potentials in row space
- Sign conventions must be consistent between $A$ and $\mathbf{y}$
- Adding independent loops carelessly — the loop basis has exactly $m-n+1$

## Connections

- Incidence-Matrices · Four-Fundamental-Subspaces
- Orthogonal-Vectors-and-Subspaces
