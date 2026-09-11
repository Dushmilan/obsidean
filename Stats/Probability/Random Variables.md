# Random Variables

## Definition

A random variable assigns a number to each outcome of a random process.

- **Discrete:** PMF $P(X=x)$; $E[X] = \sum xP(X=x)$, $\text{Var}(X) = \sum (x-\mu)^2P(X=x)$.
- **Continuous:** PDF $f(x)$; $E[X] = \int x f(x)dx$, $\text{Var}(X) = \int (x-\mu)^2 f(x)dx$.
- **CDF** (both): $F(x) = P(X \le x)$.

**Linearity:** $E[aX+b] = aE[X]+b$; $\text{Var}(aX+b) = a^2\text{Var}(X)$.

## The Intuition

A rule assigning numbers to outcomes: "number of heads in 10 flips" (0–10), "height of a random student". The *distribution* describes which values are likely and which are rare — the full uncertainty picture.

## The Toolkit

| Quantity | Discrete | Continuous |
|----------|----------|-----------|
| Expected value | $\sum xP(X=x)$ | $\int xf(x)dx$ |
| Variance | $\sum (x-\mu)^2P(X=x)$ | $\int (x-\mu)^2f(x)dx$ |
| Shortcut | $\text{Var} = E[X^2] - (E[X])^2$ | same |
| CDF | $\sum_{x\le k}P(X=x)$ | $\int_{-\infty}^x f(t)dt$ |

## Derivation

$\text{Var}(X) = E[(X-\mu)^2] = E[X^2] - 2\mu E[X] + \mu^2 = E[X^2] - \mu^2$. The linearity of $E$ follows from the definition; the $a^2$ in variance comes from squaring the deviation. [Full derivations: [[Introduction to Probability]]]

## Method

1. Identify discrete vs continuous → use sums or integrals.
2. Variance via the shortcut $E[X^2] - (E[X])^2$.
3. Linear transforms: mean shifts by $b$, spread scales by $|a|$ (variance by $a^2$).

## Worked Examples

**Setup:** A fair die, $X$ = number shown. Find $E[X]$, $\text{Var}(X)$.

**Solution:** $E[X] = 3.5$. $E[X^2] = 91/6$. $\text{Var}(X) = 91/6 - 12.25 \approx 2.92$; $\text{SD} \approx 1.71$.

**Key insight:** The expected value 3.5 isn't a possible outcome — means are averages, not typical values.

---

**Setup:** $E[X] = 10$, $\text{Var}(X) = 4$. Mean and variance of $Y = 3X + 2$?

**Solution:** $E[Y] = 32$; $\text{Var}(Y) = 9\times4 = 36$; $\text{SD}(Y) = 6$.

**Key insight:** Adding a constant shifts the mean but not the spread; multiplying scales the SD by $|a|$.

## Common Traps

- Treating a PDF value as a probability — $P(X=x) = 0$ for continuous; only intervals count
- $f(x)$ can exceed 1 — the *area* must be 1
- $\text{Var}(aX+b) = a^2\text{Var}(X)$ — the $b$ vanishes
- Using PMF formulas for continuous variables

## Connections

- [[Introduction to Probability]] · [[Common Distributions]] · [[Central Limit Theorem]]
- [[Maths/Pure/04-Calculus/04-Integration/04.4-Integration]] — continuous expectations
