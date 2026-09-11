# Groups — Definition & Examples

A group is the smallest structure in which you can "undo" operations. One set, one operation, four axioms — and yet this single definition captures clock arithmetic, matrix multiplication, symmetries of a square, and addition of integers, all at once.

**The Intuition:** A group is a self-contained universe of moves. Every move can be performed (closure), doing nothing is a move (identity), every move can be undone (inverses), and chaining moves respects brackets (associativity). Chess pieces on a board, rotations of a Rubik's cube, shuffling a deck — any system where moves compose and undo cleanly is whispering "I'm a group." Drop any axiom and whole theorems collapse: without inverses there is no "undo", and Lagrange-style counting later becomes impossible.

**The Math:** A **group** $(G, \ast)$ is a set $G$ with a binary operation $\ast : G \times G \to G$ such that:

1. **Closure:** $a \ast b \in G$ for all $a, b \in G$
2. **Associativity:** $(a \ast b) \ast c = a \ast (b \ast c)$
3. **Identity:** there exists $e \in G$ with $e \ast a = a \ast e = a$ for all $a$
4. **Inverses:** for each $a \in G$ there exists $a^{-1}$ with $a \ast a^{-1} = a^{-1} \ast a = e$

If additionally $a \ast b = b \ast a$ for all $a,b$, the group is **abelian**. The **order** of $G$, written $|G|$, is its number of elements; the **order** of an element $a$ is the least $k > 0$ with $a^k = e$ (or $\infty$ if none exists).

Immediate consequences worth memorizing — all proved by playing the axioms against each other:

- The identity is **unique**; each inverse is **unique**.
- **Cancellation:** $ab = ac \Rightarrow b = c$ and $ba = ca \Rightarrow b = c$ (multiply by $a^{-1}$).
- $(a^{-1})^{-1} = a$ and $(ab)^{-1} = b^{-1}a^{-1}$ — shoes and socks: to undo "$ab$", undo $b$ first.
- $ax = b$ has the unique solution $x = a^{-1}b$; likewise $ya = b$.

Standard examples to test every new theorem against:

| Group | Operation | Identity | Notes |
|-------|-----------|----------|-------|
| $(\mathbb{Z}, +)$ | addition | $0$ | abelian, infinite |
| $(\mathbb{Z}_n, +_n)$ | addition mod $n$ | $[0]$ | abelian, finite (03-Modular-Arithmetic) |
| $(\mathbb{R}^\*, \times)$ | nonzero reals | $1$ | $0$ excluded: no inverse |
| $(M_{n\times n}(\mathbb{R}), \cdot)$ | matrix mult | $I_n$ | non-abelian for $n \ge 2$ |
| $D_4$: symmetries of a square | composition | $\mathrm{id}$ | 8 elements, non-abelian |

Non-examples teach as much: $(\mathbb{Z}, \times)$ fails inverses ($2$ has none), $(\mathbb{R}, \times)$ fails for the same reason, subtraction on $\mathbb{Z}$ fails associativity.

**Setup:** Prove the identity element of a group is unique.

**Solution:** Suppose $e$ and $e'$ are both identities. Then $e \ast e' = e'$ (since $e$ is an identity) but also $e \ast e' = e$ (since $e'$ is an identity). Hence $e = e'$.

**Key insight:** Uniqueness proofs in group theory follow this template forever: compute the same expression two ways using two axioms, then cancel.

**Setup:** In a group, show $(ab)^{-1} = b^{-1}a^{-1}$.

**Solution:** Check by multiplying back: $(b^{-1}a^{-1})(ab) = b^{-1}(a^{-1}a)b = b^{-1}eb = e$, and $(ab)(b^{-1}a^{-1}) = aeb a^{-1}\!\cdots = a(b b^{-1})a^{-1} = aa^{-1} = e$. Since inverses are unique, $(ab)^{-1} = b^{-1}a^{-1}$.

**Key insight:** To prove "$x$ is *the* inverse of $y$", verify $xy = yx = e$ and invoke uniqueness — no searching required.

**Setup:** Is $(\mathbb{Z}_6, \times_6)$ a group?

**Solution:** No. Closure holds, $[1]$ is the identity, but $[2] \cdot [3] = [6] = [0]$ and $[0]$ has no inverse: $[0] \cdot x = [0] \ne [1]$ always. Only the units $[1], [5]$ invert (03-Modular-Arithmetic), so the full set fails axiom 4.

**Key insight:** When testing group axioms, hunt specifically for elements that annihilate — zero divisors are where inverses go to die.

---

### Additional Notes

Notation warning: multiplicative notation ($a^{-1}$, $a^k$) is default when the operation is abstract or non-commutative; additive notation ($-a$, $ka$, identity $0$) is common when the group is abelian, especially $\mathbb{Z}$ and $\mathbb{Z}_n$. Same theorems, different costume. Element order becomes the central character in 05-Subgroups-and-Cyclic-Groups.

---
