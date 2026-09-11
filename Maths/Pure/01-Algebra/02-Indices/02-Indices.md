
## Definition

Indices are shorthand for repeated multiplication:

$$a^n = \underbrace{a \cdot a \cdots a}_{n \text{ times}}, \qquad a \in \mathbb{R},\ n \in \mathbb{N}$$

The definition extends to every real exponent while preserving the index laws:
$$a^0 = 1 \ (a \neq 0), \qquad a^{-n} = \frac{1}{a^n} \ (a \neq 0), \qquad a^{m/n} = \sqrt[n]{a^m} \ (a \ge 0 \text{ for even } n)$$

## The Intuition

An exponent is a "multiplication counter." $a^3$ counts three multiplications of $a$. The rule $a^m \cdot a^n = a^{m+n}$ is just *concatenating the two counters*: $a^2 \cdot a^3 = (a \cdot a)(a \cdot a \cdot a) = a^5$. Adding exponents is the only rule consistent with counting multiplications.

## The Toolkit

| Result | Formula | Valid when |
|--------|---------|-----------|
| Product | $a^m \cdot a^n = a^{m+n}$ | $a > 0$ (or $a \neq 0$ with integer exponents) |
| Quotient | $\dfrac{a^m}{a^n} = a^{m-n}$ | $a \neq 0$ |
| Power of a power | $(a^m)^n = a^{mn}$ | $a > 0$ or integer exponents |
| Power of a product | $(ab)^n = a^n b^n$ | — |
| Power of a quotient | $\left(\dfrac{a}{b}\right)^n = \dfrac{a^n}{b^n}$ | $b \neq 0$ |
| Zero exponent | $a^0 = 1$ | $a \neq 0$ |
| Negative exponent | $a^{-n} = \dfrac{1}{a^n}$ | $a \neq 0$ |
| Fractional exponent | $a^{m/n} = \sqrt[n]{a^m}$ | $a \ge 0$ if $n$ even |
| Growth/decay | $N(t) = N_0 e^{kt}$ | $k$ constant |
| Half-life | $t_{1/2} = \dfrac{\ln 2}{|k|}$ | decay $k < 0$ |

## Derivation

Product law from counting: $a^m \cdot a^n$ expands to $m + n$ copies of $a$, hence $a^{m+n}$. Quotient law: cancelling $n$ of the $m$ copies leaves $m-n$. Fractional exponents: $(a^{1/n})^n = a^{n \cdot 1/n} = a^1 = a$, so $a^{1/n}$ must be $\sqrt[n]{a}$ — the extension is *forced* by the product law. [Full derivations: 02-Indices-Proofs]

## Method

**Same-base exponential equations**: rewrite both sides with one base, equate exponents — never solve by isolation.

**"Quadratic in disguise"**: seeing $a^{2x}$ and $a^x$ together, substitute $y = a^x$ ($y > 0$), solve the quadratic, back-substitute $x = \log_a y$.

**Exponential models**: "doubles in time $t$" means the ratio is $2$ — solve $2 = e^{kt}$ for $k$ with $\ln$.

## Worked Examples

**Setup:** Solve $2^{x+1} = 8^{2x-3}$.

**Solution:** Rewrite $8 = 2^3$: $2^{x+1} = 2^{3(2x-3)} = 2^{6x-9}$. Equate exponents: $x+1 = 6x-9 \Rightarrow 5x = 10 \Rightarrow x = 2$.

**Key insight:** If you can write both sides with the same base, just set the exponents equal.

---

**Setup:** Solve $4^x - 5 \cdot 2^x + 4 = 0$.

**Solution:** Let $y = 2^x$ (so $4^x = y^2$): $y^2 - 5y + 4 = 0 \Rightarrow (y-1)(y-4) = 0$. Then $y=1 \Rightarrow x=0$; $y=4 \Rightarrow x=2$.

**Key insight:** When you see $a^{2x}$ and $a^x$ together, substitute $y = a^x$.

---

**Setup:** A population doubles in 5 years. Find the growth constant $k$.

**Solution:** At $t=5$: $2 = e^{5k} \Rightarrow \ln 2 = 5k \Rightarrow k = \frac{\ln 2}{5} \approx 0.1386$.

**Key insight:** "Doubling" means the ratio is 2. Take natural logs to solve for $k$.

## Common Traps

- $(a^m)^n \neq a^{m^n}$ — exponents multiply, they don't tower
- $(a+b)^n \neq a^n + b^n$ — powers don't distribute over addition
- $a^0 = 1$ only when $a \neq 0$ — $0^0$ is undefined
- Negative base with fractional exponents ($(-8)^{2/3}$ needs care — real only for certain forms)
- Forgetting to check "quadratic in disguise" roots are positive before back-substituting

## Connections

- 03-Logarithms — logs are the inverse of indices
- 01-Real-Numbers-Surds — $a^{m/n} = \sqrt[n]{a^m}$
- 07-Binomial-Theorem — expanding $(1+x)^n$
- 04.2-Differentiation — $\frac{d}{dx}a^x$


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

> Original links unrecoverable; topic-level mappings live in [[Maths-Cross_Index]].
