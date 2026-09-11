# Conditional Probability

## Definition

The probability of $A$ given that $B$ has occurred:

$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}, \quad P(B) > 0$$

**Independent** if $P(A\mid B) = P(A)$, equivalently $P(A\cap B) = P(A)P(B)$.
**Multiplication rule:** $P(A\cap B) = P(A\mid B)P(B)$.
**Bayes' theorem:** $P(A\mid B) = \frac{P(B\mid A)P(A)}{P(B)}$.

## The Intuition

You've rolled a die but hidden it: $P(3) = 1/6$. Told it's odd, $P(3)$ jumps to $1/3$ — conditioning narrows the sample space from 6 outcomes to 3. Conditioning updates beliefs by shrinking the possibilities.

## The Toolkit

| Rule | Formula |
|------|---------|
| Conditional | $P(A\mid B) = \frac{P(A\cap B)}{P(B)}$ |
| Multiplication | $P(A\cap B) = P(A\mid B)P(B)$ |
| Bayes | $P(A\mid B) = \frac{P(B\mid A)P(A)}{P(B)}$ |
| Law of total probability | $P(B) = P(B\mid A)P(A) + P(B\mid A^c)P(A^c)$ |

## Derivation

The conditional formula defines a probability measure on the reduced sample space $B$. Bayes' theorem follows from the multiplication rule applied both ways: $P(A\cap B) = P(A\mid B)P(B) = P(B\mid A)P(A)$. [Full derivations: [[Introduction to Probability]]]

## Method

1. Identify the conditioning event — which direction does the question run?
2. Apply the multiplication rule for sequences (especially without replacement).
3. Bayes: expand the denominator with the law of total probability.

## Worked Examples

**Setup:** 1% have a disease; a test is 99% accurate both ways. Test positive → probability of disease?

**Solution:** $P(D\mid+) = \frac{0.99\times0.01}{0.99\times0.01 + 0.01\times0.99} = 0.5$.

**Key insight:** Despite a 99% test, only 50% — most positives are false positives because the disease is rare. Base rates matter enormously.

---

**Setup:** 3 red, 2 blue; draw two without replacement. Both red?

**Solution:** $P(R_1\cap R_2) = P(R_1)P(R_2\mid R_1) = \frac35\cdot\frac24 = 0.3$.

**Key insight:** Without replacement the draws are dependent — the second probability changes.

## Common Traps

- Confusing $P(A\mid B)$ with $P(B\mid A)$ — often wildly different
- Independence does not imply mutual exclusivity
- Forgetting to condition on the correct event in the denominator
- Bayes without the law of total probability for $P(B)$

## Connections

- [[Introduction to Probability]] · [[Random Variables]]
- [[Maths/Pure/01-Algebra/06-Permutations-Combinations/06-Permutations-Combinations]] — counting
- [[Physics/01-Measurement/01.2-Errors-Uncertainties]] — false positives in measurement
