# Linear Systems and the Meaning of $A\mathbf{x} = \mathbf{b}$

Every linear system in science and engineering reduces to the equation $A\mathbf{x} = \mathbf{b}$, and the key to the whole course is choosing the right way to look at it. The row picture treats each equation as a hyperplane and finds their intersection. But there's a deeper way — the column picture — that asks: can we assemble $\mathbf{b}$ from the columns of $A$ using the right weights? This reframing is the foundation of everything that follows, from 02-Elimination-and-RREF to 07-Independence-Basis-Dimension to 13-Projections-Least-Squares.

**The Intuition:** Think of $A$'s columns as ingredients. The equation asks: what recipe (weights $\mathbf{x}$) combines the ingredients into the dish $\mathbf{b}$? In 2D, two lines crossing at a point (row picture) vs. two arrows scaling and adding to reach a target (column picture) — same answer, different geometry. The column picture makes it explicit that $\mathbf{b}$ must be reachable from the columns of $A$, which is the most direct way to understand solvability.

**The Math:** $A$ is an $m \times n$ matrix, $\mathbf{x} \in \mathbb{R}^n$ is the column vector of unknowns, and $\mathbf{b} \in \mathbb{R}^m$ is the constants vector. The column picture expands to $x_1 \mathbf{a}_1 + x_2 \mathbf{a}_2 + \cdots + x_n \mathbf{a}_n = \mathbf{b}$ — you're finding weights that build $\mathbf{b}$ from columns. When $A$ is square ($m = n$), the question "is $\mathbf{b}$ in the column space?" becomes "is $A$ invertible?" When $m \neq n$, the column space is a proper subspace of $\mathbb{R}^m$ and most $\mathbf{b}$ are unreachable. The row picture tells you about each equation individually; the column picture tells you about the whole system at once. Treating $A$ as an operator (mapping $\mathbf{x}$ to $\mathbf{b}$) is what makes it possible to talk about inverses, eigenvalues, and transformations later — it's the bridge from algebra to geometry.

**What does this mean for solving systems?** Start with the column picture when analyzing solvability (it's the fastest way to see if a solution exists), but use the row picture when performing elimination (it's the algorithmic workhorse). When $A$ is square, check invertibility. When rectangular, check the rank.

**Setup:** Solve $2x + y = 5$ and $x + 3y = 7$.

**Solution:** Row picture: find where two lines intersect. Column picture: find weights on $(2, 1)^T$ and $(1, 3)^T$ that give $(5, 7)^T$. The answer is $x = 1.6$, $y = 1.8$.

**Key insight:** Both pictures give the same answer, but the column picture shows you the vectors being combined.

**Setup:** Solve $x + y = 3$ and $x + y = 5$.

**Solution:** Row picture: parallel lines never meet. Column picture: $(1, 1)^T$ only spans a line in $\mathbb{R}^2$, and $(3, 5)^T$ is off that line. No solution exists — $\mathbf{b} \notin C(A)$.

**Key insight:** The column picture immediately tells you the system is inconsistent. You can see $\mathbf{b}$ is not reachable.

**Setup:** Solve $x + 2y + 3z = 6$ and $2x + 4y + 6z = 12$.

**Solution:** The second equation is twice the first. One equation, three unknowns, two free variables. Column picture: $(1, 2)^T$, $(2, 4)^T$, and $(3, 6)^T$ are all multiples — they span only a line. Infinitely many solutions along a line in $\mathbb{R}^3$.

**Key insight:** The column picture reveals the dependency directly — all columns are multiples of one.

---
