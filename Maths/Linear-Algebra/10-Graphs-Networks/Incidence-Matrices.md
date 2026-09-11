
## Definition

The incidence matrix $A$ encodes a directed graph: $m$ edges (rows), $n$ nodes (columns). Entry $+1$ at the arriving node, $-1$ at the departing node, 0 otherwise.

$$(A\mathbf{x})_k = x_j - x_i \quad \text{(voltage drop across edge } k \text{ from } i \text{ to } j)$$

## The Intuition

A network of pipes (edges) connecting junctions (nodes). Each row is a rule: "potential at node $i$ minus potential at node $j$ = drop across edge $k$." The matrix turns abstract subspaces into Kirchhoff's laws.

## The Toolkit

| Fact | Statement |
|------|-----------|
| $A$ size | $m\times n$ (edges × nodes) |
| $A\mathbf{x} = \mathbf{0}$ | zero drop ⇒ all potentials equal |
| $\dim N(A)$ | 1 for a connected graph |
| $N(A^T)$ | loops (KCL: net current into each node = 0) |
| $\dim N(A^T)$ | $m - n + 1$ independent loops (connected) |
| Rank | $n - 1$ (connected graph) |

## Derivation

$A\mathbf{x} = \mathbf{0}$ forces $x_i = x_j$ along every edge, so all nodes share one potential — nullspace is the constant vector, dimension 1. $A^T\mathbf{y} = \mathbf{0}$ is current balance at each node; the loop count $m-n+1$ is Euler's formula. [Full derivations: Four-Fundamental-Subspaces]

## Method

1. Build $A$: one row per edge, $-1$ at departure, $+1$ at arrival.
2. Voltage drop: $(A\mathbf{x})_k$; potential consistency: $A\mathbf{x} = \mathbf{0}$.
3. Current law: solve $A^T\mathbf{y} = \mathbf{0}$ — the loops.

## Worked Examples

**Setup:** Triangle graph (3 edges, 3 nodes). What are $N(A)$ and $N(A^T)$?

**Solution:** Connected: $N(A)$ = constants (dim 1). $N(A^T)$: $m-n+1 = 1$ — one loop (the whole triangle).

**Key insight:** The loop basis is exactly the left nullspace.

## Common Traps

- Sign convention: $-1$ at departure, $+1$ at arrival (or the reverse, consistently)
- $\dim N(A) = 1$ only for *connected* graphs (each component adds one)
- Loops are $N(A^T)$, not $N(A)$

## Connections

- Networks-and-Kirchhoff-Laws · Four-Fundamental-Subspaces
- Orthogonal-Vectors-and-Subspaces — orthogonality of row/null spaces
