
## Definition

For $A$ ($m\times n$) and $B$ ($n\times p$), the product $C = AB$ is $m\times p$. **Four equivalent views:**

1. **Dot-product way:** $c_{ij} = \text{row } i \text{ of } A \cdot \text{column } j \text{ of } B$.
2. **Column way:** column $j$ of $C$ = combination of columns of $A$ weighted by column $j$ of $B$.
3. **Row way:** row $i$ of $C$ = combination of rows of $B$ weighted by row $i$ of $A$.
4. **Block way:** partition into conformable blocks; multiply blocks as if scalars.

## The Intuition

Multiplication is **composition of transformations**: if $A$ rotates and $B$ scales, $AB$ scales first then rotates — read right to left. Each view exposes different structure: the dot-product way for entries, the column way for "where does this output column come from".

## The Toolkit

| View | Sees | Use |
|------|------|-----|
| Dot product | single entry $c_{ij}$ | entry-by-entry computation |
| Column | outputs as combinations of $A$'s columns | $AB$ acting on $B$'s columns |
| Row | outputs as combinations of $B$'s rows | row space structure |
| Block | big matrices as small ones | partition tricks |

## Derivation

All four views compute the same sum $\sum_k a_{ik}b_{kj}$ rearranged. The column way is the definition of $A$ applied to each column of $B$; the row way is the transpose of that statement. Block multiplication works because multiplication is bilinear. [Full derivations: Row-and-Column-Pictures]

## Method

1. Check inner dimensions match: $A$ ($m\times n$), $B$ ($n\times p$).
2. Choose the view that reveals the structure you need.
3. Compute: dot-product per entry, or build whole columns/rows.

## Worked Examples

**Setup:** $A = \begin{bmatrix}1&2\\3&4\end{bmatrix}$, $B = \begin{bmatrix}5&6\\7&8\end{bmatrix}$. Find $AB$.

**Solution:** $c_{11} = 1\cdot5+2\cdot7 = 19$; $c_{12} = 1\cdot6+2\cdot8 = 22$; $c_{21} = 3\cdot5+4\cdot7 = 43$; $c_{22} = 3\cdot6+4\cdot8 = 50$. So $AB = \begin{bmatrix}19&22\\43&50\end{bmatrix}$.

**Key insight:** Column way: column 1 of $AB$ = $5(1,3) + 7(2,4) = (19,43)$ — same answer.

---

**Setup:** Show $AB \neq BA$ for these matrices.

**Solution:** $BA = \begin{bmatrix}5&6\\7&8\end{bmatrix}\begin{bmatrix}1&2\\3&4\end{bmatrix} = \begin{bmatrix}23&34\\31&46\end{bmatrix} \neq AB$.

**Key insight:** Multiplication is **not commutative** — order matters because it's composition.

## Common Traps

- Wrong inner dimensions (undefined product)
- Assuming commutativity $AB = BA$
- Cancellation: $AB = AC$ doesn't imply $B = C$ unless $A$ is invertible
- $(AB)^T = B^TA^T$ — order reverses

## Connections

- Inverses-and-Gauss-Jordan · Transposes-and-Symmetric-Matrices
- LU-Decomposition — multiplication as recorded elimination
