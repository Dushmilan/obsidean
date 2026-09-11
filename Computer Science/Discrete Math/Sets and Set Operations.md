# Sets & Set Operations

A **set** is an unordered collection of distinct objects. Sets are the vocabulary of all discrete mathematics — relations, functions, graphs, formal languages, and database theory are all built on them. This note covers the definitions, operations, and laws that appear everywhere.

**The Intuition:** A set is a bag of distinct things with no order and no duplicates: `{2, 3, 5}` is the same set as `{3, 5, 2}`. Membership is the fundamental question — "is this in the set?" The operations (union, intersection, difference) combine bags by the same logic as `OR`, `AND`, and `NOT` — and that correspondence is the key to learning the laws.

## Notation & basic definitions

```text
A = {1, 2, 3}          — roster notation
B = {x | x is even}    — set-builder: "the set of all x such that..."
∅  or {}               — empty set (NOT the same as {0})
ℤ, ℕ, ℝ, ℚ             — integers, naturals, reals, rationals

2 ∈ A                  — 2 is an element of A (TRUE)
5 ∈ A                  — FALSE
5 ∉ A                  — 5 is not an element
A ⊆ B                  — A is a SUBSET of B: every element of A is in B
A ⊂ B                  — proper subset: A ⊆ B but A ≠ B
|A|                    — cardinality: number of elements
```

**The subset trap:** the empty set is a subset of *every* set ($\emptyset \subseteq A$ always), and every set is a subset of itself ($A \subseteq A$).

## Operations

| Operation | Definition | Example (A={1,2}, B={2,3}) |
|-----------|-----------|------------------------------|
| Union | $A \cup B = \{x \mid x \in A \lor x \in B\}$ | {1,2,3} |
| Intersection | $A \cap B = \{x \mid x \in A \land x \in B\}$ | {2} |
| Difference | $A \setminus B = \{x \mid x \in A \land x \notin B\}$ | {1} |
| Complement | $\bar{A} = \{x \mid x \notin A\}$ (within a universe U) | depends on U |
| Symmetric difference | $A \triangle B = (A \setminus B) \cup (B \setminus A)$ | {1,3} |
| Cartesian product | $A \times B = \{(a,b) \mid a \in A, b \in B\}$ | {(1,2),(1,3),(2,2),(2,3)} |
| Power set | $\mathcal{P}(A) = \{X \mid X \subseteq A\}$ | {∅,{1},{2},{1,2}} |

## The key laws (logic in disguise)

| Law | Set form | Logic analog |
|-----|----------|--------------|
| Identity | $A \cup \emptyset = A$, $A \cap U = A$ | $p \lor F = p$, $p \land T = p$ |
| Domination | $A \cup U = U$, $A \cap \emptyset = \emptyset$ | $p \lor T = T$, $p \land F = F$ |
| Idempotence | $A \cup A = A$, $A \cap A = A$ | $p \lor p = p$ |
| Double complement | $\overline{\bar{A}} = A$ | $\lnot\lnot p = p$ |
| Commutativity | $A \cup B = B \cup A$, $A \cap B = B \cap A$ | $p \lor q = q \lor p$ |
| Associativity | $(A \cup B) \cup C = A \cup (B \cup C)$ | — |
| Distributivity | $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$ | $p \land (q \lor r)$ |
| De Morgan | $\overline{A \cup B} = \bar{A} \cap \bar{B}$; $\overline{A \cap B} = \bar{A} \cup \bar{B}$ | $\lnot(p \lor q) = \lnot p \land \lnot q$ |
| Absorption | $A \cup (A \cap B) = A$, $A \cap (A \cup B) = A$ | — |

**The trick:** every set law mirrors a logic law via $\cup \leftrightarrow \lor$, $\cap \leftrightarrow \land$, $\bar{\cdot} \leftrightarrow \lnot$. If you know one, you know the other.

## Cardinality formulas

```text
|A ∪ B| = |A| + |B| - |A ∩ B|           (inclusion-exclusion, 2 sets)
|A ∪ B ∪ C| = |A| + |B| + |C| - |A∩B| - |A∩C| - |B∩C| + |A∩B∩C|
|A × B| = |A| · |B|
|P(A)| = 2^|A|
```

---

**Setup:** Given $A = \{1,2,3\}$, $B = \{2,3,4\}$, $U = \{1,2,3,4,5\}$, compute $\bar{A} \cap B$.

**Solution:** $\bar{A} = \{4,5\}$; $\bar{A} \cap B = \{4\}$.

**Key insight:** Break into steps — compute the complement *within the universe* first, then intersect. The universe matters: without $U$, "complement" is undefined.

---

**Setup:** Prove $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$ by element argument.

**Solution:**
```text
x ∈ LHS ⇔ x ∈ A ∧ (x ∈ B ∨ x ∈ C)
        ⇔ (x ∈ A ∧ x ∈ B) ∨ (x ∈ A ∧ x ∈ C)    (distributivity of ∧)
        ⇔ x ∈ (A∩B) ∪ (A∩C) = RHS
```

**Key insight:** An element argument shows both inclusions ($\subseteq$ and $\supseteq$) — each $\iff$ step covers both directions. This "pick an arbitrary element, follow definitions, re-group" style is *the* method for set identity proofs.

---

**Setup:** In a class of 30, 18 take Python, 15 take Java, and 8 take both. How many take neither?

**Solution:** $|P \cup J| = 18 + 15 - 8 = 25$. Neither = $30 - 25 = 5$.

**Key insight:** Inclusion-exclusion subtracts the overlap once — the union double-counts the 8 students in both. The "neither" count is total minus union.

---

**Setup:** List $\mathcal{P}(\{a, b\})$ and confirm its size.

**Solution:** $\{\emptyset, \{a\}, \{b\}, \{a,b\}\}$ — $2^2 = 4$ subsets. Each element independently chosen (in/out): 2 choices × 2 choices = 4.

**Key insight:** The power set is "choose for each element: include or not" — $2^n$ subsets. This is exactly the bitmask idea from DSA Bit Manipulation: an $n$-bit number selects a subset.

---

## Practice (try before peeking)

1. Is $\emptyset = \{\emptyset\}$? Why not?
2. $|A \cup B|$ when $A \subseteq B$?
3. How many subsets does a set of 5 elements have?

<details><summary>Answers</summary>

1. No — $\emptyset$ has no elements; $\{\emptyset\}$ has one element (the empty set itself). $|\emptyset| = 0$, $|\{\emptyset\}| = 1$.
2. $|A \cup B| = |B|$ — if A is a subset of B, the union is just B.
3. $2^5 = 32$.

</details>

---

**Common traps:**
- $\emptyset$ vs $\{\emptyset\}$ vs $\{0\}$ — three different sets
- Membership ($\in$) vs subset ($\subseteq$): $1 \in \{1,2\}$ but $1 \not\subseteq \{1,2\}$ (though $\{1\} \subseteq \{1,2\}$)
- Complement needs a specified universe
- Duplicates and order don't matter — $\{1,2\} = \{2,1\} = \{1,1,2\}$
- $A \setminus B \ne B \setminus A$ — difference isn't symmetric

---
