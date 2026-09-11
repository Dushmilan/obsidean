# Quantifiers & Predicate Logic

Propositional logic reasons about whole statements. **Predicate logic** reasons about statements *about things*: "x is even", "for all x, ...", "there exists x such that ...". Quantifiers — $\forall$ (for all) and $\exists$ (there exists) — are how logic expresses statements about entire collections.

**The Intuition:** A predicate is a *template*: $P(x) =$ "x is even" has no truth value until you plug in an $x$. Quantifiers close the template into a statement. "∀x P(x)" claims *every* x satisfies it; "∃x P(x)" claims *some* x does. This is the difference between "all students passed" and "some student passed" — and learning to negate them correctly is a superpower.

## Predicates & domains

```text
P(x) = "x is even"          — a predicate (open sentence), no truth value
P(4) → true                 — plugging in closes it
P(7) → false

Domain of discourse = the set x ranges over.
"∀x P(x)" only means something relative to a domain.
∀x∈ℤ P(x)   — "every integer is even"  → false
∃x∈ℤ P(x)   — "some integer is even"   → true
```

## The two quantifiers

| Quantifier | Reads | Means | Proof strategy |
|-----------|-------|-------|----------------|
| $\forall x \, P(x)$ | "for all x, P(x)" | P holds for EVERY x in the domain | To **prove**: pick arbitrary x, show P(x). To **disprove**: one counterexample |
| $\exists x \, P(x)$ | "there exists x, P(x)" | P holds for AT LEAST ONE x | To **prove**: exhibit one x. To **disprove**: show none exist |

**The proof asymmetry — the most important fact:**
- Disproving a **∀** claim: ONE counterexample is enough
- Proving a **∃** claim: ONE example is enough
- Proving a **∀** claim: you must argue for *every* element (arbitrary x, general argument)
- Disproving a **∃** claim: you must show *no* element works (a general argument too)

## Negation — the De Morgan for quantifiers

```text
¬(∀x P(x))  ≡  ∃x ¬P(x)      "NOT all" = "some don't"
¬(∃x P(x))  ≡  ∀x ¬P(x)      "NOT some" = "none do"
```

**Every student passed** → "Not every student passed" = **some student failed**.
**There is a flying pig** → "There is no flying pig" = **every pig doesn't fly**.

**The rule:** negating flips the quantifier AND negates the predicate. Never just slap $\lnot$ in front — that leaves an unnegated interior.

## Nested quantifiers — order matters

```text
∀x ∀y P(x,y)     — for all x and all y, P holds (order doesn't matter)
∃x ∃y P(x,y)     — there exist x and y, P holds (order doesn't matter)
∀x ∃y P(x,y)     — for EVERY x, there is SOME y (y may depend on x!)
∃x ∀y P(x,y)     — there is ONE x that works for ALL y (a universal x!)
```

**Classic example** (domain = people, $Loves(x,y)$ = "x loves y"):
```text
∀x ∃y Loves(x,y)   — everyone loves someone   (each person may love a different person)
∃x ∀y Loves(x,y)   — someone loves everyone   (one person loves all) — MUCH stronger
```

The order of ∀ and ∃ is NOT swappable — swapping changes the meaning completely.

## Nested negation — flip every quantifier

```text
¬(∀x ∃y P(x,y))  ≡  ∃x ∀y ¬P(x,y)
¬(∃x ∀y P(x,y))  ≡  ∀x ∃y ¬P(x,y)
```
Every quantifier flips, every predicate negates, order stays.

## The three famous argument forms

**Modus ponens (affirming the antecedent):**
```text
p → q
p
∴ q
```
"If it rains, the ground is wet. It rains. Therefore the ground is wet." — valid.

**Modus tollens (denying the consequent):**
```text
p → q
¬q
∴ ¬p
```
"If it rains, ground is wet. Ground is dry. Therefore it didn't rain." — valid.

**The fallacy — affirming the consequent:**
```text
p → q
q
∴ p
```
"Ground is wet, therefore it rained" — invalid! (A sprinkler could have done it.)

---

**Setup:** Translate "Every even integer greater than 2 is the sum of two primes" (Goldbach's conjecture).

**Solution:** $\forall n \in \mathbb{Z} \big( (n \text{ even} \land n > 2) \to \exists p, q \text{ prime} (n = p + q) \big)$

**Key insight:** The $\forall$ with an implication inside is the "all elements with property X have property Y" pattern. The implication is *inside* the quantifier. This shape — $\forall x (A(x) \to B(x))$ — is how "every A is B" always translates.

---

**Setup:** Negate: "All birds can fly."

**Solution:** Let $B(x)$ = "x is a bird", $F(x)$ = "x can fly". Original: $\forall x (B(x) \to F(x))$. Negation: $\exists x \lnot(B(x) \to F(x)) \equiv \exists x (B(x) \land \lnot F(x))$ — "there exists a bird that cannot fly."

**Key insight:** Negating "all A are B" gives "some A is not B" — NOT "no A is B". The $\to$ inside the quantifier negates to an $\land$. This exact pattern trips people in every logic class.

---

**Setup:** Prove or disprove: "There is an integer that is greater than every other integer."

**Solution:** $\exists x \forall y (x > y)$. Disprove: for any candidate x, take $y = x + 1$ — then $y > x$, so x isn't greater than all. No such x exists.

**Key insight:** Disproving an ∃ claim requires a general argument — here, the elegant "always one bigger" construction. This is also how you'd reason about "there is a largest integer."

---

**Setup:** Order of quantifiers — "∀x ∃y (y = x²)" vs "∃y ∀x (y = x²)".

**Solution:** First: for every x there is a y equal to x² — TRUE (y depends on x: pick y = x²). Second: there's ONE y that equals every x² — FALSE (impossible: x=1 and x=2 need different squares).

**Key insight:** The difference is *dependency*: in ∀x∃y, y can depend on x (y = x²). In ∃y∀x, one fixed y must serve all x. This "can the second variable depend on the first?" question is the key to reading nested quantifiers.

---

## Practice (try before peeking)

1. Negate: "Every dog has a tail."
2. Negate: ∃x ∀y P(x,y).
3. Which is stronger: ∀x∃y(x < y) or ∃y∀x(x < y)?

<details><summary>Answers</summary>

1. ∃x(Dog(x) ∧ ¬hasTail(x)) — "some dog has no tail." (Not "no dog has a tail.")
2. ∀x ∃y ¬P(x,y) — flip the quantifier, negate the predicate.
3. ∀x∃y(x<y) — for every x there's something bigger (true for integers: x+1). ∃y∀x(x<y) claims a single y bigger than everything — false. First is weaker and true.

</details>

---

**Common traps:**
- Swapping the order of $\forall$ and $\exists$ — changes the meaning
- Negating $\forall x (A \to B)$ to $\forall x (A \land \lnot B)$ instead of $\exists x (A \land \lnot B)$
- "Not all" vs "none" — different negations
- Forgetting the domain — $\forall x P(x)$ is meaningless without it
- Proving ∀ claims with examples — one example proves nothing for a "for all"

---
