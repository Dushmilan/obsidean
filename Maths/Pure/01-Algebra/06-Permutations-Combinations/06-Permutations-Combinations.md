
## Definition

- **Permutation** $^nP_r$ — ordered arrangements: choosing $r$ items from $n$ where **order matters**.
- **Combination** $\binom{n}{r}$ — unordered selections: choosing $r$ items from $n$ where **order doesn't matter**.

$$^nP_r = \frac{n!}{(n-r)!}, \qquad \binom{n}{r} = \frac{n!}{r!(n-r)!} = \frac{^nP_r}{r!}, \qquad 0! = 1$$

The only difference is the $r!$ — the number of ways to reorder each selection.

## The Intuition

Permutations are assigning seats in a row — who sits where matters. Combinations are forming a committee — who is in it matters, not the seating order. $^nP_r$ fills $r$ slots from $n$ options, each slot shrinking the pool by one. $\binom{n}{r}$ then divides by $r!$ to discard the overcounting from different orderings of the same group.

## The Toolkit

| Count | Formula | Use when |
|-------|---------|----------|
| Factorial | $n! = n(n-1)\cdots 1$ | arranging all $n$ distinct items |
| Permutation | $^nP_r = \dfrac{n!}{(n-r)!}$ | ordered arrangements of $r$ from $n$ |
| Combination | $\binom{n}{r} = \dfrac{n!}{r!(n-r)!}$ | unordered selections of $r$ from $n$ |
| Circular | $(n-1)!$ | seating around a circle |
| Circular (reflections same) | $\dfrac{(n-1)!}{2}$ | necklaces / bracelets |
| Identical objects | $\dfrac{n!}{n_1! n_2! \cdots n_k!}$ | arranging $n$ items with repeats |
| Stars and bars | $\binom{n+r-1}{r-1}$ | $n$ identical objects into $r$ bins |
| Grid paths $(0,0)\to(m,n)$ | $\binom{m+n}{m}$ | right/up moves only |

## Derivation

$^nP_r = n(n-1)\cdots(n-r+1)$ — each slot has one fewer option. Dividing by $r!$ gives combinations because each unordered group of $r$ appears $r!$ times in the ordered count. Stars and bars: arranging $n$ stars and $r-1$ bars, so $\binom{n+r-1}{r-1}$. Grid paths: $m+n$ moves, choose which $m$ are "right", so $\binom{m+n}{m}$. [Full derivations: 06-Permutations-Combinations-Proofs]

## Method

1. **AND → multiply; OR (mutually exclusive) → add** — break the problem into independent choices.
2. Ask: *does order matter?* → permutations ($^nP_r$) or combinations ($\binom{n}{r}$).
3. Ask: *are items identical?* → divide by $n_i!$ for each repeated group.
4. **"Together" trick:** treat the forced group as one unit, arrange units, then arrange inside the group.
5. **"At least" trick:** split into exact cases and sum, or total-minus-forbidden.

## Worked Examples

**Setup:** Arrange the letters of "MATHEMATICS" with the vowels together. Letters: M(2), A(2), T(2), H, E, I, C, S.

**Solution:** Vowels A, A, E, I form 1 block. Block + 7 consonants = 8 items: $\frac{8!}{2!2!}$ arrangements. Inside the block: $\frac{4!}{2!}$. Total: $\frac{8!}{2!2!} \times \frac{4!}{2!} = 120{,}960$.

**Key insight:** "Together" problems: treat the group as a unit, then arrange within the group.

---

**Setup:** A committee of 5 from 7 men and 6 women, with **at least 3 women**.

**Solution:** Cases: exactly 3, 4, or 5 women.
$$\binom{6}{3}\binom{7}{2} + \binom{6}{4}\binom{7}{1} + \binom{6}{5}\binom{7}{0} = 420 + 105 + 6 = 531$$

**Key insight:** "At least" problems: list all valid cases and sum them (OR → add).

---

**Setup:** Paths on a grid from $(0,0)$ to $(m,n)$, moving only right or up.

**Solution:** $m+n$ moves total, choose which $m$ are right: $\binom{m+n}{m}$.

**Key insight:** Grid path problems reduce to choosing positions for one type of move.

## Common Traps

- Confusing order-matters vs not — decide this **first**, it changes everything
- Using $\binom{n}{r}$ where $^nP_r$ is needed (and vice versa)
- Forgetting to divide by $n_i!$ for repeated/identical items
- "At least one" done as exactly one — use total minus zero
- Circular arrangements: $(n-1)!$, not $n!$

## Connections

- 08-Mathematical-Induction — proving $\binom{n}{r}$ identities
- 07-Binomial-Theorem — the coefficients $\binom{n}{r}$
- 03-Logarithms — factorials grow fast; logs help compare


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

- [[Discrete_Math_Index]] — counting principles formalized on the CS side
