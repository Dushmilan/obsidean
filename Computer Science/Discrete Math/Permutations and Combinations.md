# Permutations & Combinations

Permutations count *ordered* arrangements; combinations count *unordered* selections. The single question that separates them — **does order matter?** — decides the formula. These counts underpin probability (sample spaces), algorithm analysis, and the binomial theorem.

**The Intuition:** Picking a team of 3 from 5 people is a combination — order doesn't matter, {Ada, Bob, Cy} is one pick. Assigning 3 people to positions (captain, treasurer, secretary) is a permutation — order matters, each arrangement is different. A combination is a permutation divided by the ways the picked items could be reordered.

## The formulas

```text
PERMUTATION — ordered selection of k from n distinct items:
  P(n, k) = n! / (n - k)!        = n·(n-1)···(n-k+1)

COMBINATION — unordered selection of k from n distinct items:
  C(n, k) = n! / (k!·(n-k)!)     = P(n, k) / k!
  Read: "n choose k", written ⌊n k⌋ or binom(n, k)

FULL PERMUTATION — arranging all n items:
  n!

PERMUTATIONS WITH REPETITION — arranging n items where some repeat:
  n! / (n₁! · n₂! · ...)         (n₁ identical of kind 1, etc.)
```

## Key identities

```text
C(n, k) = C(n, n-k)               symmetry: choosing 3 to keep = choosing n-3 to drop
C(n, k) = C(n-1, k-1) + C(n-1, k)  Pascal's identity
C(n, 0) = C(n, n) = 1
Sum over k: Σ C(n,k) = 2ⁿ          (each element in or out)
```

## When to use which

| Problem | Order matters? | Formula |
|---------|---------------|---------|
| Arrange 5 books on a shelf | yes | $5!$ |
| Top-3 finishers in a race | yes | $P(n, 3)$ |
| Committee of 3 from 10 | no | $C(10, 3)$ |
| PIN with distinct digits | yes | $P(10, 4)$ |
| Deal 5 cards from 52 | no | $C(52, 5)$ |
| Permute "MISSISSIPPI" | yes, repeats | $11!/(4!·4!·2!)$ |

## Stars and bars — distributing identical items

**Identical items into distinct bins.** The number of ways to distribute $n$ identical items into $k$ distinct bins:
```text
Non-negative (bins may be empty):  C(n + k - 1, k - 1)
Positive (each bin ≥ 1):           C(n - 1, k - 1)
```

**Setup:** Distribute 10 identical candies to 3 children (each ≥ 1).
**Solution:** $C(10-1, 3-1) = C(9,2) = 36$.

**Setup:** Non-negative solutions to $x_1 + x_2 + x_3 + x_4 = 20$.
**Solution:** $C(20+4-1, 4-1) = C(23, 3) = 1771$.

## The binomial theorem

```text
(x + y)ⁿ = Σ_{k=0..n} C(n, k) x^k y^(n-k)

(x + y)³ = C(3,0)x³ + C(3,1)x²y + C(3,2)xy² + C(3,3)y³
         = x³ + 3x²y + 3xy² + y³
```

**Why:** each term picks x from some factors and y from the rest — choosing *which* $k$ factors contribute x is $C(n, k)$.

---

**Setup:** How many ways to arrange the letters of "BOOK"? "BOOO"?

**Solution:** BOOK: 4 distinct → $4! = 24$. BOOO: 3 O's identical → $4!/3! = 4$ (B, O, O, O arrangements = position of B).

**Key insight:** Identical items divide out the arrangements that look the same. $4!/3!$: total arrangements over the identical O's' arrangements.

---

**Setup:** From a deck, how many 5-card hands? How many *flushes* (all same suit)?

**Solution:** Hands: $C(52,5) = 2{,}598{,}960$. Flushes: pick a suit (4) then 5 cards from it: $4 \cdot C(13,5) = 4 \cdot 1287 = 5148$.

**Key insight:** Sequential decisions (choose suit, then cards) multiply — the multiplication rule in action. $C(52,5)$ is why poker probabilities are precise fractions of 2.6M.

---

**Setup:** Expand $(2x - 1)^4$.

**Solution:**
```text
Σ C(4,k)(2x)^k(-1)^(4-k)
= (2x)⁴ - 4(2x)³ + 6(2x)² - 4(2x) + 1
= 16x⁴ - 32x³ + 24x² - 8x + 1
```

**Key insight:** The binomial coefficients $1, 4, 6, 4, 1$ come straight from Pascal's triangle / $C(4,k)$. Signs alternate because of $(-1)^{4-k}$.

---

**Setup:** A committee of 4 from 6 men and 5 women must include at least 2 women. How many?

**Solution:** Split into disjoint cases:
```text
2 women:  C(5,2)·C(6,2) = 10·15 = 150
3 women:  C(5,3)·C(6,1) = 10·6  = 60
4 women:  C(5,4)         = 5
Total = 150 + 60 + 5 = 215
```

**Key insight:** "At least" → case analysis (disjoint alternatives, add). The naive $C(11,4) - C(6,4)$ (all minus 0-women-minus-1-women) also works — complement. Two roads, same answer.

---

## Practice (try before peeking)

1. A pizza place: 10 toppings, choose any 3?
2. How many 6-digit codes with digits all distinct?
3. 12 people, 3 distinct prizes? 3 identical prizes?

<details><summary>Answers</summary>

1. $C(10,3) = 120$ — order doesn't matter.
2. $P(10,6) = 10·9·8·7·6·5 = 151{,}200$ — order matters, no repeats.
3. Distinct prizes: $P(12,3) = 1320$. Identical: $C(12,3) = 220$ — the prizes' ordering disappears.

</details>

---

**Common traps:**
- Permuting when order doesn't matter (and vice versa) — ask "would swapping two items change the outcome?"
- Forgetting to divide by $k!$ for combinations — $P(n,k)$ counts each group $k!$ times
- Stars and bars on *distinct* items — it's only for identical items
- Dividing by factorials for non-identical items — each distinct item contributes its own factor
- $C(n,k)$ vs $C(n,n-k)$ confusion — they're equal; use the smaller for easier arithmetic

---
