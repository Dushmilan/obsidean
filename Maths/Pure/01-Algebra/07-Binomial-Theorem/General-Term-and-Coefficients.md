
## Definition

$(x+y)^n = \sum_{r=0}^{n}\binom{n}{r}x^{n-r}y^r$ with general term

$$T_{r+1} = \binom{n}{r}x^{n-r}y^r \quad (r = 0,1,\dots,n)$$

The **$k$-th term uses $r = k-1$.** Coefficient of $x^p$: set the exponent of $x$ in $T_{r+1}$ equal to $p$, solve for $r$, evaluate.

## The Intuition

$\binom{n}{r}$ counts the configurations with exactly $r$ copies of $y$ from $n$ brackets. The general term is the machinery for "find the coefficient of $x^4$" problems — match the power, then compute.

## The Toolkit

| Fact | Formula |
|------|---------|
| General term | $T_{r+1} = \binom{n}{r}x^{n-r}y^r$ |
| Sum of coefficients | $(1+1)^n = 2^n$ |
| Alternating sum | $(1-1)^n = 0$ |
| Symmetry | $\binom{n}{r} = \binom{n}{n-r}$ |
| Pascal | $\binom{n}{r} = \binom{n-1}{r-1} + \binom{n-1}{r}$ |

## Derivation

Combinatorial: choose which $r$ of $n$ factors contribute $y$. [Full derivations: 07-Binomial-Theorem-Proofs]

## Method

1. Write the general term with the variable powers.
2. Set the exponent of the target variable equal to the desired power.
3. Solve for $r$, compute the coefficient (watch signs).

## Worked Examples

**Setup:** Coefficient of $x^4$ in $\left(2x - \frac3x\right)^{10}$.

**Solution:** $T_{r+1} = \binom{10}{r}2^{10-r}(-3)^r x^{10-2r}$; set $10-2r = 4 \Rightarrow r = 3$; coefficient $= \binom{10}{3}2^7(-3)^3 = -414720$.

**Key insight:** Match the exponent to find which term contributes.

## Common Traps

- Off-by-one: the $k$-th term uses $r = k-1$
- Missing the sign of a negative second term
- $\binom{n}{r} = \binom{n}{n-r}$ — use it to save computation
- Forgetting the factor $2^{10-r}$ when the base isn't $x$

## Connections

- Binomial-Approximations · Permutations & Combinations
- 04.7-Series-Expansions — generalised binomial
