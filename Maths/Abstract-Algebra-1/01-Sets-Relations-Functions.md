# Sets, Relations & Functions

Before we can define a group, we need a shared language: sets to hold elements, relations to compare them, and functions to move between them. Almost every definition in this course is secretly one of these three wearing a costume.

**The Intuition:** Think of a set as a bag of objects with no order and no duplicates. A relation is any rule that says "these two are related" — like two people being siblings. A function is the disciplined version: every input gets exactly one output, like a vending machine (one button, one snack, every time). An equivalence relation is the mathematician's idea of "same category": it chops a set into non-overlapping bins called *equivalence classes*, and those bins later become the congruence classes of 03-Modular-Arithmetic.

**The Math:** For sets $A$ and $B$:

- $A \subseteq B$ means every element of $A$ lies in $B$. The **power set** $\mathcal{P}(A)$ is the set of all subsets; if $|A| = n$ then $|\mathcal{P}(A)| = 2^n$.
- The **Cartesian product** $A \times B = \{(a,b) : a \in A,\ b \in B\}$ pairs every element of $A$ with every element of $B$. A **relation** on $A$ is just a subset $R \subseteq A \times A$, written $a \sim b$.
- A **relation** $\sim$ on $A$ is an **equivalence relation** if it is:
  - **Reflexive:** $a \sim a$
  - **Symmetric:** $a \sim b \Rightarrow b \sim a$
  - **Transitive:** $a \sim b$ and $b \sim c \Rightarrow a \sim c$
- The equivalence class of $a$ is $[a] = \{x \in A : x \sim a\}$. Equivalence classes form a **partition**: they are non-empty, pairwise disjoint, and cover $A$.
- A function $f : A \to B$ is:
  - **Injective (one-to-one):** $f(a_1) = f(a_2) \Rightarrow a_1 = a_2$
  - **Surjective (onto):** every $b \in B$ equals $f(a)$ for some $a \in A$
  - **Bijective:** both at once — exactly one input per output
- Composition obeys $(g \circ f)(a) = g(f(a))$, and $f$ has an inverse iff $f$ is bijective, in which case $f^{-1} \circ f = \mathrm{id}_A$ and $f \circ f^{-1} = \mathrm{id}_B$.

Key counting facts: if there is a bijection $A \to B$ then $|A| = |B|$; for finite sets, $|A \times B| = |A|\,|B|$, and the number of functions $A \to B$ is $|B|^{|A|}$.

**Setup:** On $\mathbb{Z}$, define $a \sim b$ iff $a - b$ is even. Show $\sim$ is an equivalence relation and find its classes.

**Solution:** Reflexive: $a - a = 0$ is even. Symmetric: if $a-b$ is even then so is $b-a = -(a-b)$. Transitive: if $a-b$ and $b-c$ are even then $a-c = (a-b)+(b-c)$ is even. There are exactly two classes: the evens $[0]$ and the odds $[1]$.

**Key insight:** This is congruence mod 2 in disguise — equivalence relations defined by "$n$ divides the difference" are exactly what produces $\mathbb{Z}_n$ in 03-Modular-Arithmetic.

**Setup:** Let $f: \mathbb{R} \to \mathbb{R}$, $f(x) = 2x + 3$. Prove $f$ is bijective and find $f^{-1}$.

**Solution:** Injective: $f(a)=f(b) \Rightarrow 2a+3=2b+3 \Rightarrow a=b$. Surjective: given any $y$, take $a = (y-3)/2$, then $f(a)=y$. Inverse: $f^{-1}(y) = \dfrac{y-3}{2}$.

**Key insight:** Linear maps with nonzero slope are always bijections $\mathbb{R} \to \mathbb{R}$ — undo them by reversing each arithmetic step in reverse order.

**Setup:** How many functions $f: \{1,2,3\} \to \{a,b\}$ are surjective?

**Solution:** Total functions: $2^3 = 8$. Not onto: the two constant ones ($f \equiv a$, $f \equiv b$). Answer: $8 - 2 = 6$.

**Key insight:** Complementary counting ("total minus bad") beats direct construction when checking surjectivity on small sets.

---

### Additional Notes

Two facts get used silently throughout algebra:

1. **Composition of bijections is a bijection**, and $(g \circ f)^{-1} = f^{-1} \circ g^{-1}$ — reverse the order, then reverse each step.
2. **Partitions ↔ equivalence relations:** every partition defines an equivalence relation ("same box"), and every equivalence relation yields a partition into classes. We exploit this when defining $\mathbb{Z}_n$ (03-Modular-Arithmetic) and again whenever we slice a set into equal-sized pieces by symmetry.

---
