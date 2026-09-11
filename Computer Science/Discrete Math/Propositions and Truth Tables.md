# Propositions & Truth Tables

Logic is the mathematics of precise reasoning. A **proposition** is a statement that is either true or false. **Connectives** combine propositions, and **truth tables** define exactly what each connective means — the foundation for proofs, type systems, and circuit design.

**The Intuition:** Logic is a game with strict rules. Every proposition is a claim ("it is raining") that has exactly one truth value. Connectives build compound claims ("it is raining AND the road is wet"). A truth table is a complete specification — it says what the compound claim is for *every* combination of its parts. There's no ambiguity left for interpretation.

## Propositions vs non-propositions

```text
PROPOSITIONS (have a truth value):
  "2 + 2 = 4"                          → true
  "The moon is made of cheese"         → false
  "x is even" (when x is fixed)        → true or false, one of them

NOT propositions:
  "Close the door!"                    → command, no truth value
  "What time is it?"                   → question
  "x + 1"                              → not a claim, just an expression
  "This statement is false"            → paradox, no consistent truth value
```

## The connectives

| Name | Symbol | Read as | True when |
|------|--------|---------|-----------|
| Negation | $\lnot p$ | "not p" | p is false |
| Conjunction | $p \land q$ | "p and q" | both true |
| Disjunction | $p \lor q$ | "p or q" | at least one true |
| Exclusive or | $p \oplus q$ | "p xor q" | exactly one true |
| Implication | $p \to q$ | "if p then q" | p false OR q true |
| Biconditional | $p \leftrightarrow q$ | "p iff q" | both same |

## Truth tables — the definitions

```text
p  q | p∧q  p∨q  p⊕q  p→q  p↔q
T  T |  T    T    F    T    T
T  F |  F    T    T    F    F
F  T |  F    T    T    T    F
F  F |  F    F    F    T    T
```

**The implication row that surprises everyone:** $p \to q$ is TRUE when $p$ is false. "If it rains, the ground is wet" is *not* falsified by a dry sunny day — implication only promises something *when the hypothesis holds*.

## Compound truth tables — the method

```c
// To build a truth table for (p ∧ q) → (p ∨ q):
// 1. List all 2ⁿ rows for n variables (2 rows × 2 rows = 4)
// 2. Compute the subexpressions left to right
// 3. Finish with the outermost connective

p  q | p∧q  p∨q | (p∧q)→(p∨q)
T  T |  T    T   |     T
T  F |  F    T   |     T
F  T |  F    T   |     T
F  F |  F    F   |     T
```

Every row is T → this is a **tautology** (always true, regardless of p and q).

## The three classifications

| Type | Meaning | Example |
|------|---------|---------|
| **Tautology** | True in every row | $p \lor \lnot p$ (law of excluded middle) |
| **Contradiction** | False in every row | $p \land \lnot p$ |
| **Contingency** | True in some rows, false in others | $p \land q$ |

## Logical equivalence — same truth table

Two expressions are **logically equivalent** ($\equiv$) if their truth tables match. The important ones to know cold:

```text
De Morgan's laws:
  ¬(p ∧ q) ≡ ¬p ∨ ¬q
  ¬(p ∨ q) ≡ ¬p ∧ ¬q
  ¬(p → q) ≡ p ∧ ¬q          ← the negation of an implication

Implication rewrites:
  p → q ≡ ¬p ∨ q             ← implication as disjunction
  p → q ≡ ¬q → ¬p            ← contrapositive (same table!)

Distributive:
  p ∧ (q ∨ r) ≡ (p ∧ q) ∨ (p ∧ r)
  p ∨ (q ∧ r) ≡ (p ∨ q) ∧ (p ∨ r)

Double negation:
  ¬¬p ≡ p
```

## The four implications of $p \to q$

```text
p → q        (implication)      — original
q → p        (converse)         — NOT equivalent to original
¬q → ¬p      (contrapositive)   — EQUIVALENT to original
¬p → ¬q      (inverse)          — NOT equivalent to original
```

**The trap:** people routinely treat "if p then q" as "if q then p" (the converse). "If it's a dog, it's an animal" does NOT imply "if it's an animal, it's a dog." But "if it's NOT an animal, it's NOT a dog" (contrapositive) IS equivalent.

---

**Setup:** Prove $p \to q \equiv \lnot p \lor q$ using truth tables.

**Solution:**
```text
p  q | p→q  ¬p  ¬p∨q
T  T |  T    F    T
T  F |  F    F    F
F  T |  T    T    T
F  F |  T    T    T
```
Columns `p→q` and `¬p∨q` match on every row → equivalent.

**Key insight:** This single equivalence is the engine of many proofs — an implication is just "either the premise is false, or the conclusion holds." It also explains why `if (p) q` in code compiles to a conditional jump: implication is a disjunction with a guard.

---

**Setup:** Simplify $\lnot(p \lor \lnot q)$ using equivalence laws.

**Solution:**
```text
¬(p ∨ ¬q)
≡ ¬p ∧ ¬¬q            De Morgan
≡ ¬p ∧ q              double negation
```

**Key insight:** Equivalence laws let you *rewrite* logic like algebra — no truth table needed for each step. This is term rewriting, the same idea behind algebraic simplification in math and expression optimization in compilers.

---

**Setup:** Write a truth table for the "exclusive or" and show $p \oplus q \equiv (p \lor q) \land \lnot(p \land q)$.

**Solution:**
```text
p  q | p⊕q  p∨q  p∧q  ¬(p∧q) | (p∨q)∧¬(p∧q)
T  T |  F    T    T     F    |      F
T  F |  T    T    F     T    |      T
F  T |  T    T    F     T    |      T
F  F |  F    F    F     T    |      F
```
Columns match → equivalent.

**Key insight:** XOR = "OR, but not both." This is exactly how the `^` operator works in C/Java bitwise logic, and why `a ^ b` toggles bits — the truth table is the hardware specification.

---

## Practice (try before peeking)

1. Is "If 1 + 1 = 3, then pigs fly" true or false? (Careful!)
2. Which are equivalent: converse, inverse, contrapositive?
3. Negate: "Every student passed" using a formula.

<details><summary>Answers</summary>

1. **True** — the premise is false, so the implication is vacuously true (regardless of the conclusion).
2. Only the contrapositive ($\lnot q \to \lnot p$) is equivalent to the original. The converse and inverse are equivalent to *each other* but not the original.
3. Let P(x) = "x passed"; the negation of ∀x P(x) is ∃x ¬P(x) — "some student failed."

</details>

---

**Common traps:**
- Treating $p \to q$ as $q \to p$ (converse confusion) — the most common logic error
- Thinking $p \to q$ is false when p is false — it's *vacuously* true
- Negating $p \to q$ as $\lnot p \to \lnot q$ — the correct negation is $p \land \lnot q$
- Assuming "or" is exclusive — in logic and most programming, $\lor$ is *inclusive* unless stated
- Building truth tables with missing rows — $n$ variables needs exactly $2^n$ rows

---
