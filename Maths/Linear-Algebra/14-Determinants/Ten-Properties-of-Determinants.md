## Definition

The **determinant** $\det A$ (written $|A|$) is a single number attached to every square matrix. Three equivalent views:

1. **Algebraic** — the product of the pivots (with a sign correction for row swaps).
2. **Geometric** — the signed volume of the box whose edges are the rows of $A$. Sign $=$ whether the transformation flips orientation.
3. **Detective** — $\det A = 0 \iff A$ is singular (no inverse, dependent rows/columns).

## The Intuition

A matrix transforms space; the determinant measures how much it scales *volume*. A $3\times3$ matrix with $\det = 5$ turns unit cubes into boxes of volume 5. $\det = -2$ means doubled volume *and* a mirror flip (orientation reversal). And if $\det = 0$, the box was flattened — space collapsed onto a lower dimension — which is exactly singularity. Every property below is obvious through this lens.

## The Ten Properties (Strang's list)

| # | Property | Statement |
|---|----------|-----------|
| 1 | Identity | $\det I = 1$ (unit cube stays unit cube) |
| 2 | Swap | Exchanging two rows multiplies $\det$ by $-1$ |
| 3 | Linearity | $\det$ is linear in each row **separately**: scaling one row by $k$ scales $\det$ by $k$ |
| 4 | Dependence | Two equal rows (or any dependent rows) $\Rightarrow \det A = 0$ |
| 5 | Elimination-safe | Adding $\lambda\,\text{row}_i$ to $\text{row}_j$ leaves $\det$ **unchanged** |
| 6 | Zero row | A zero row $\Rightarrow \det A = 0$ (flattened box) |
| 7 | Triangular | $\det T = t_{11}t_{22}\cdots t_{nn}$ (product of diagonal) |
| 8 | Singularity | $\det A = 0 \iff A$ singular |
| 9 | Product rule | $\det(AB) = \det A \cdot \det B$, so $\det(A^{-1}) = 1/\det A$ |
| 10 | Transpose | $\det A^T = \det A$ |

## Why Property 5 is the workhorse

Elimination only uses row *replacement* ("add multiple of one row to another") — property 5 says this never changes the determinant. So the cleanest way to compute any determinant: run elimination, track swaps (each flips sign), then multiply the pivots of the resulting triangular matrix.

## Worked Examples

**Example 1 — elimination bookkeeping.** $A = \begin{bmatrix}2&3\\4&7\end{bmatrix}$. Step: $\text{row}_2 \leftarrow \text{row}_2 - 2\,\text{row}_1$ gives $\begin{bmatrix}2&3\\0&1\end{bmatrix}$ — no det change (P5). Triangular (P7): $\det = 2\cdot1 = 2$. Direct check: $2\cdot7 - 3\cdot4 = 14-12 = 2$ ✓.

**Example 2 — instant singularity test.** $B = \begin{bmatrix}1&2\\2&4\end{bmatrix}$: row 2 is exactly $2\times$ row 1 → P4 → $\det B = 0$, so $B$ is singular without doing any elimination.

**Example 3 — why swaps matter.** Swapping the rows of $\begin{bmatrix}0&1\\1&0\end{bmatrix}$… it *is* already a swap matrix: $\det = -1$ (P2), even though its entries are positive. Orientation flip made visible.

**Key insight:** the determinant compresses everything about invertibility and volume into one computable number — and properties 1–7 turn computing it into plain elimination. [Full formula machinery: Cofactors-Cramers-Rule-and-Volume]
