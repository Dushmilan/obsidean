# Relations & Functions

Relations connect elements between sets; functions are the special relations that map each input to exactly one output. Relations give you equivalence classes and orderings; functions give you injectivity, surjectivity, and bijections — the concepts behind hash tables, sorting, database keys, and counting arguments.

**The Intuition:** A relation is a set of pairs — "a is less than b", "x is a friend of y", "this student took that course". A function is a stricter relation: each input picks *exactly one* output. Equivalence relations group things into buckets (equivalence classes); partial orders arrange things into hierarchies (like file systems or task dependencies).

## Relations

A relation $R \subseteq A \times B$ is a set of ordered pairs.

```text
A = {1, 2, 3}, B = {1, 2}
R = {(1,1), (1,2), (2,1)}       — a relation: 3 pairs
1 R 1, 1 R 2, 2 R 1             — "related to" notation
2 R 2?                          — NO, (2,2) not in R
```

**Properties (when $R$ is on a set $A$, i.e., $R \subseteq A \times A$):**

| Property | Definition | Example on people |
|----------|-----------|-------------------|
| Reflexive | $\forall a: (a,a) \in R$ | "has the same age as" |
| Symmetric | $(a,b) \in R \Rightarrow (b,a) \in R$ | "is a sibling of" |
| Antisymmetric | $(a,b)$ and $(b,a)$ only if $a=b$ | "$\le$" |
| Transitive | $(a,b), (b,c) \Rightarrow (a,c)$ | "is an ancestor of" |

## Equivalence relations — the bucketing structure

A relation that is **reflexive + symmetric + transitive** is an **equivalence relation**. It partitions $A$ into **equivalence classes** — disjoint buckets where everything in a bucket is related to everything else.

```text
Modulo 3 on integers:  a ≡ b (mod 3)
Classes: [0] = {...,-3,0,3,...}, [1] = {...,-2,1,4,...}, [2] = {...,-1,2,5,...}
- 3 classes partition ℤ
- 7 ∈ [1], 4 ∈ [1] (7 ≡ 4 mod 3)  → same class
- [1] = [4] = [7]                  → classes have many names
```

**Key facts:**
- Equivalence classes are disjoint and cover $A$ (a *partition*)
- Two elements are equivalent iff they're in the same class
- $[a] = [b]$ iff $a \sim b$

## Partial orders — the hierarchy structure

A relation that is **reflexive + antisymmetric + transitive** is a **partial order** (often written $\preceq$).

```text
The subset relation ⊆ on sets — a partial order.
The ≤ relation on numbers — a partial order (and a total order).
The "divides" relation on positive integers — a partial order.
```

**Hasse diagram:** draw the order with only covering relations (edges that aren't implied by transitivity) — a compact picture of the hierarchy.

## Functions

$f : A \to B$ assigns each $a \in A$ *exactly one* $b \in B$. $A$ is the **domain**, $B$ the **codomain**; the set of actual outputs is the **range** (image).

```text
f: ℤ → ℤ, f(x) = x²
domain = ℤ, codomain = ℤ, range = {0, 1, 4, 9, ...} (perfect squares)

f(3) = 9, f(-3) = 9         — two inputs, one output (allowed for functions)
```

## The three classification properties

| Property | Meaning | Test |
|----------|---------|------|
| **Injective** (one-to-one) | No two inputs share an output | $f(a) = f(b) \Rightarrow a = b$; horizontal line test |
| **Surjective** (onto) | Every codomain element is hit | range = codomain; every $b$ has some $a$ with $f(a)=b$ |
| **Bijective** | Both injective and surjective | has an inverse $f^{-1}$ |

```text
f(x) = 2x on ℝ → ℝ:        injective? yes. surjective? yes. bijective ✓
f(x) = x² on ℝ → ℝ:        injective? no (f(2)=f(-2)). surjective? no (no preimage of -1)
f(x) = x² on ℝ → ℝ⁺∪{0}:   surjective? yes. injective? no.
```

**Bijection = counting tool:** if there's a bijection between $A$ and $B$, they have the same size. This is how you count without counting: find a bijection to something you know.

## Compositions

```text
(g ∘ f)(x) = g(f(x))
f: A → B, g: B → C  ⇒  g ∘ f: A → C

Composition of two bijections is a bijection.
f and g invertible ⇒ (g ∘ f)⁻¹ = f⁻¹ ∘ g⁻¹    (note: order REVERSES!)
```

---

**Setup:** Is $R = \{(a,b) \mid a + b \text{ is even}\}$ on integers an equivalence relation?

**Solution:** Reflexive: $a + a = 2a$ even ✓. Symmetric: $a+b$ even implies $b+a$ even ✓. Transitive: $a \sim b$ (a+b even) and $b \sim c$ (b+c even) → add: $a + 2b + c$ even → $a + c$ even ✓. Equivalence relation. Classes: evens and odds (two classes).

**Key insight:** Transitivity for parity: the sum $a + 2b + c$ is even iff $a+c$ is even — the $2b$ vanishes mod 2. The equivalence classes are exactly the parity buckets.

---

**Setup:** Let $f: \mathbb{Z} \to \mathbb{Z}$, $f(n) = 2n$. Injective? Surjective?

**Solution:** Injective: $2a = 2b \Rightarrow a = b$ ✓. Surjective: is every integer $2n$ for some $n$? Only evens — odd integers have no preimage. Not surjective.

**Key insight:** The distinction between codomain and range matters for surjectivity. As $f: \mathbb{Z} \to \mathbb{Z}$ it's not onto; as $f: \mathbb{Z} \to \text{evens}$ it is. The codomain is part of the function's definition.

---

**Setup:** Show $\mathbb{Z}^+ \times \mathbb{Z}^+$ (positive integer pairs) has the same cardinality as $\mathbb{Z}^+$.

**Solution:** Bijection — zigzag (Cantor pairing). Order pairs by $a+b$ sum, then by $a$: (1,1), (1,2), (2,1), (1,3), (2,2), (3,1), ... Every pair appears exactly once — a bijection with the naturals.

**Key insight:** Bijections let you count infinite sets. The existence of this pairing means $\mathbb{Z}^+ \times \mathbb{Z}^+$ is **countable** — same size as $\mathbb{Z}^+$. This is the result that underlies "a union of countably many countable sets is countable."

---

**Setup:** How many functions $f: \{1,2,3\} \to \{a,b\}$ exist? How many are surjective?

**Solution:** Each of 3 inputs picks 1 of 2 outputs: $2^3 = 8$ functions. Surjective ones must hit both a and b: total 8 minus the 2 non-surjective (all-a and all-b) = 6.

**Key insight:** Counting functions = counting choices per input (domain size ^ |choices|). "At least one of each" → complement counting — the same idea as counting "at least one" events in combinatorics.

---

## Practice (try before peeking)

1. Is "is a divisor of" on positive integers a partial order?
2. A function from a 3-element set to a 2-element set — can it be injective?
3. Are $\mathbb{Z}$ and the evens the same size?

<details><summary>Answers</summary>

1. Yes — reflexive (n|n), antisymmetric (a|b and b|a implies a=b), transitive (a|b, b|c implies a|c).
2. No — 3 inputs, 2 outputs; by the pigeonhole principle two inputs share an output. Injection needs |domain| ≤ |codomain|.
3. Yes — $f(n) = 2n$ is a bijection. Infinite sets can be the same size as proper subsets (the definition of infinite!).

</details>

---

**Common traps:**
- Confusing codomain with range — surjectivity depends on the codomain
- A function is not injective if *any* two inputs collide — one collision destroys it
- "Antisymmetric" ≠ "not symmetric" — a relation can be both (equality) or neither
- $\subseteq$ is a partial order, $\in$ is not transitive: $1 \in \{1\} \in \{\{1\}\}$ but $1 \notin \{\{1\}\}$
- Assuming "less than" is a partial order — it's transitive but NOT reflexive, so no

---
