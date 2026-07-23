---
date: 2026-07-21
type: linear-algebra-cluster
tags: [linear-algebra, strang]
lectures: [2, 7]
prereq_clusters: ["01"]
status: complete
source: manual
---

# 02 — Elimination and RREF

## Concept Statement
Run Gaussian elimination forward into $U$ (upper triangular) and backward into $R$ (reduced echelon form). Read pivots, free variables, and special solutions off $R$ without solving by hand.

## Lecture Sources
- Strang MIT 18.06, Lecture 2: *Elimination with Matrices*
- Strang MIT 18.06, Lecture 7: *Solving $A\mathbf{x} = \mathbf{0}$ — Pivot Variables, Special Solutions*

## Core Material

### Gaussian Elimination (Forward, $A \to U$)

Clearing sub-diagonal entries by subtracting multiples of pivot rows.

- **Pivot** — the leading non-zero entry of a row used to eliminate below. Cannot be zero (would force a row exchange).
- **Multiplier** — $l_{ij} = (\text{entry to eliminate}) / (\text{pivot})$. These are subtracted.
- **Result** — an *upper triangular* $U$ with the same nullspace and column space as $A$.

**Failure modes:**
- *Temporary*: zero appears in a pivot position — fix with row exchange (permutation).
- *Permanent*: no non-zero pivot available after all row swaps → singular matrix.

### Elimination Matrices ($E$) and Permutation Matrices ($P$)

Row operations as left-multiplication:
- $E_{21}$: subtracts $\ell \times$ row 1 from row 2.
- $P$: identity with rows reordered. Exchanging two rows is left-multiplying by the matching $P$.

The full elimination sequence factors as $E \cdots E_2 E_1 A = U$.

### RREF — Reduced Row Echelon Form

Continue elimination *upwards* and divide each pivot row by its pivot. The result $R = \text{rref}(A)$ has:
- Pivots exactly $1$, isolated.
- Zero rows pushed to the bottom.
- Other entries in pivot columns are $0$.

### Reading Solutions Off $R$

For $A\mathbf{x} = \mathbf{0}$, when $R$ is reorganised with pivot columns first:
$$R = \begin{bmatrix} I & F \\ 0 & 0 \end{bmatrix}$$

- **Pivot columns** ($r$ of them) — pivot variables determined.
- **Free columns** ($n - r$ of them) — free variables, can take any value.

**Special solutions** — set one free variable to $1$, the rest to $0$, solve for pivot variables. There are exactly $n - r$ special solutions, one per free column.

**Nullspace matrix $N$:**
$$N = \begin{bmatrix} -F \\ I \end{bmatrix}$$
Columns are the special solutions. $RN = 0$ always.

## Cross-Cluster Links
- **Prereq**: [[01-Linear-Systems-and-Axb]]
- **Next**: [[04-LU-Factorization]] (condenses elimination as $A = LU$)
- **Forward**: [[06-Complete-Solutions-and-Rank]] (extends $R\mathbf{x} = \mathbf{0}$ to $R\mathbf{x} = \mathbf{c}$)
- **Operator lens**: [[03-Matrix-Multiplication-and-Inverses]] (eliminators as matrices)

## Thematic Summary
The first half of the course is dominated by elimination. L2 hands you the forward sweep; L7 turns it around 180° and shows that the same algorithm simultaneously encodes *both* pivot-determined variables *and* free-variables-parameterised solutions. RREF is the canonical compressed form every textbook and computer uses to read out the structure of a linear system.

## Glossary

| Term | Definition |
|------|------------|
| **Pivot** | First non-zero entry of a row in $R$, used to eliminate entries beneath it. |
| **Multiplier** | The scalar $l_{ij}$ multiplied into a pivot row and subtracted from a lower row to zero entry $(i,j)$. |
| **Upper Triangular Matrix ($U$)** | $u_{ij} = 0$ for $i > j$; result of forward elimination. |
| **Permutation Matrix ($P$)** | Identity with rows reordered. $P^T = P^{-1}$. |
| **Elementary Matrix ($E_{ij}$)** | An identity matrix with $-l_{ij}$ inserted at $(i,j)$, applied via left-multiplication. |
| **Row Reduced Echelon Form (RREF)** | Pivots are 1, isolated by zero rows/columns, zero rows at the bottom. |
| **Rank ($r$)** | Number of pivots in $R$. Also equals column-space dimension. |
| **Free Variable** | Variable corresponding to a non-pivot column — assignable to any value. |
| **Special Solution** | A nullspace basis vector: one free var set to 1, rest to 0. |
