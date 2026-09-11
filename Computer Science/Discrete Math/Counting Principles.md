# Counting Principles

Counting is the foundation of probability, algorithm analysis, and combinatorics. Two principles — the **multiplication rule** (sequential choices) and the **addition rule** (disjoint alternatives) — handle most counting problems. The **pigeonhole principle** gives cheap existence guarantees.

**The Intuition:** Counting is about *decomposing* a decision into stages. "How many 3-letter codes?" → choose letter 1 (26 ways), letter 2 (26), letter 3 (26) → multiply. "How many ways to pick a vowel OR a consonant?" → disjoint alternatives → add. Most counting errors are multiplication-vs-addition confusion, which decomposing into stages fixes.

## The multiplication rule — sequential choices

If a task can be done in $n_1$ ways, then $n_2$ ways, ..., then $n_k$ ways, the whole task can be done in $n_1 \times n_2 \times \cdots \times n_k$ ways.

**Setup:** How many 4-digit PINs? (digits 0-9, repeats allowed)
**Solution:** $10 \times 10 \times 10 \times 10 = 10^4 = 10{,}000$.

**Key insight:** "Repeats allowed" = same number of choices at every stage. "No repeats" = decreasing choices: $10 \cdot 9 \cdot 8 \cdot 7$.

## The addition rule — disjoint alternatives

If a task can be done in $n_1$ ways *or* $n_2$ ways *or* ..., and the ways are disjoint, the total is the sum.

**Setup:** Choose one book from 6 fiction or 4 non-fiction. How many?
**Solution:** $6 + 4 = 10$ — disjoint sets, add.

**The "or" test:** if the choices can't happen together (disjoint), add. If they're sequential stages, multiply. "AND" → multiply; "OR" (exclusive) → add.

## Inclusion-exclusion — overlapping alternatives

When "or" sets overlap, adding double-counts:

```text
|A ∪ B| = |A| + |B| - |A ∩ B|
|A ∪ B ∪ C| = |A|+|B|+|C| - |A∩B| - |A∩C| - |B∩C| + |A∩B∩C|
```

**Setup:** Students: 20 play cricket, 15 play football, 8 play both. How many play at least one?
**Solution:** $20 + 15 - 8 = 27$.

## The pigeonhole principle — the free guarantee

> If $n$ items are placed in $m$ boxes and $n > m$, some box has at least 2 items.

**Generalized:** some box has at least $\lceil n/m \rceil$ items.

**Setup:** In a room of 13 people, two share a birth month.
**Solution:** 13 items (people), 12 boxes (months) → some month has ≥ ⌈13/12⌉ = 2 people.

**Setup:** In a group of 367 people, two share a birthday.
**Solution:** 366 possible birthdays (incl. Feb 29) → 367 > 366 → guaranteed.

**Setup:** Show any 5 integers contain two with the same remainder mod 4.
**Solution:** 4 boxes (remainders 0..3), 5 items → some box has 2.

**Advanced form:** "If you choose 3 pairs from... " — the *generalized* pigeonhole with thresholds: at least $\lceil n/m \rceil$.

## Complement counting — count the opposite

When "at least one" is hard, count the total and subtract "none".

**Setup:** How many 3-bit strings have at least one 1?
**Solution:** Total $2^3 = 8$ minus "all zeros" (1) = 7.

**Setup:** A password has 6 lowercase letters. How many have at least one 'a'?
**Solution:** $26^6 - 25^6$ — total minus "no a at all".

---

**Setup:** A restaurant menu: 4 appetizers, 8 mains, 3 desserts. A 3-course meal?

**Solution:** $4 \times 8 \times 3 = 96$ — sequential choices, multiply.

**Key insight:** Each stage is independent (any appetizer + any main + any dessert). If choices became linked (e.g., "mains that pair with your appetizer"), the counts change — stage analysis exposes that.

---

**Setup:** How many 5-letter "words" (strings) have no repeated letters?

**Solution:** $26 \cdot 25 \cdot 24 \cdot 23 \cdot 22 = 26!/21!$ — decreasing choices.

**Key insight:** This is the permutation count $P(26, 5)$ — order matters (strings). Same answer via the formula $n!/(n-k)!$.

---

**Setup:** In a class of 40, show at least 4 share a birth month.

**Solution:** $\lceil 40/12 \rceil = 4$ — generalized pigeonhole.

**Key insight:** Generalized pigeonhole computes the *guaranteed minimum*. It doesn't tell you *which* month or *who* — just that the bound holds. Pure existence argument.

---

**Setup:** 6 numbers are chosen from {1..10}. Show two sum to 11.

**Solution:** Pair the numbers: {1,10}, {2,9}, {3,8}, {4,7}, {5,6} — 5 boxes. Choosing 6 numbers puts two in one box → they sum to 11.

**Key insight:** The *art* of pigeonhole is designing the boxes so "two in a box" implies the property you want. The pairing partition is the trick here — same style as graph-coloring proofs.

---

## Practice (try before peeking)

1. How many 2-letter codes from {a,b,c} with no repeats? With repeats?
2. 10 socks in a drawer, 6 black, 4 white. How many must you pull to guarantee a matching pair?
3. How many 4-bit strings contain exactly... two 1s? (Count them directly.)

<details><summary>Answers</summary>

1. No repeats: 3×2 = 6. With repeats: 3×3 = 9.
2. 3 — two colors = 2 boxes; pulling 3 puts two in one box (pigeonhole). 
3. Choose which 2 of 4 positions hold the 1s: $\binom{4}{2} = 6$.

</details>

---

**Common traps:**
- Multiplying when you should add (disjoint vs sequential)
- Addition rule with overlapping sets — use inclusion-exclusion
- Treating "at least one" directly — use complement
- Pigeonhole with $n \le m$ — no guarantee
- Counting ordered vs unordered as the same — order matters for permutations, not combinations

---
