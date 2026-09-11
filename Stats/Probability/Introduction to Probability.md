# Introduction to Probability

## Definition

Probability measures how likely an event is, on a scale 0–1. For equally likely outcomes:

$$P(A) = \frac{\text{number of outcomes in } A}{\text{total number of outcomes}}$$

**Axioms:** (1) $P(A) \ge 0$; (2) $P(S) = 1$; (3) mutually exclusive $A, B \Rightarrow P(A\cup B) = P(A) + P(B)$.

## The Intuition

Probability is about the *process* that generates outcomes, not any single outcome. 0 = never, 1 = always, 0.5 = half the time in the long run. It's the mathematical language of uncertainty underpinning every statistics topic.

## The Toolkit

| Rule | Formula |
|------|---------|
| Complement | $P(A^c) = 1 - P(A)$ |
| General addition | $P(A\cup B) = P(A) + P(B) - P(A\cap B)$ |
| Mutual exclusivity | $P(A\cup B) = P(A) + P(B)$ |
| Independence | $P(A\cap B) = P(A)P(B)$ |

## Derivation

The axioms are Kolmogorov's — probability is a measure on the sample space. The addition rule subtracts the overlap to avoid double-counting; the complement rule follows from $P(A) + P(A^c) = 1$. [Full derivations: [[Conditional Probability]]]

## Method

1. Define the sample space and the event(s).
2. "At least one" problems → complement first.
3. Overlapping events → general addition rule (subtract the intersection).

## Worked Examples

**Setup:** A deck of 52. Probability of a heart or a face card?

**Solution:** $P = 13/52 + 12/52 - 3/52 = 22/52 \approx 0.423$.

**Key insight:** The subtraction prevents double-counting the three heart face cards.

---

**Setup:** Flip a coin 3 times. Probability of at least one head?

**Solution:** $P = 1 - (1/2)^3 = 1 - 1/8 = 7/8$.

**Key insight:** Complement turns three cases into one product.

## Common Traps

- "Probability 0.5" doesn't guarantee an event soon — it's long-run frequency
- Mutual exclusivity ≠ independence — different concepts entirely
- Forgetting the $-P(A\cap B)$ in the addition rule
- Adding probabilities for overlapping events

## Connections

- [[Conditional Probability]] · [[Random Variables]] · [[Central Limit Theorem]]
- [[Maths/Pure/01-Algebra/06-Permutations-Combinations/06-Permutations-Combinations]] — counting outcomes
