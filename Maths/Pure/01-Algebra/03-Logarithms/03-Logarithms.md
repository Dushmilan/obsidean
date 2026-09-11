
## Definition

A logarithm answers: "What power must I raise the base to, in order to get this number?"

$$\log_a x = y \iff a^y = x, \qquad a > 0,\ a \neq 1,\ x > 0$$

- $a$ — the **base** (positive, not 1)
- $x$ — the **argument** (strictly positive)
- $y$ — the **exponent** (any real)
- $\ln x = \log_e x$ — natural logarithm, base $e$

Domain rules: the argument of a log must be positive; the base must be positive and $\neq 1$.

## The Intuition

A logarithm is a "power detective." If you know the base and the result, the log tells you which exponent was used. $\log_2 8 = 3$ means "2 raised to what power gives 8? Answer: 3." Exponentiation winds up ($2^3 = 8$); logarithms unwind ($\log_2 8 = 3$). They're the inverse operation to indices.

## The Toolkit

| Result | Formula | Valid when |
|--------|---------|-----------|
| Definition | $\log_a x = y \iff a^y = x$ | $a > 0, a \neq 1, x > 0$ |
| Product | $\log_a (xy) = \log_a x + \log_a y$ | $x, y > 0$ |
| Quotient | $\log_a \left(\tfrac{x}{y}\right) = \log_a x - \log_a y$ | $x, y > 0$ |
| Power | $\log_a (x^r) = r \log_a x$ | $x > 0$, $r$ real |
| Change of base | $\log_a b = \dfrac{\ln b}{\ln a}$ | $a, b > 0$, $a \neq 1$ |
| Base swap | $\log_a b = \dfrac{1}{\log_b a}$ | $a, b > 0$, both $\neq 1$ |
| Inverse | $a^{\log_a x} = x$, $\log_a(a^x) = x$ | $x > 0$ |
| Special values | $\log_a 1 = 0$, $\log_a a = 1$ | $a > 0$, $a \neq 1$ |
| Derivative | $\frac{d}{dx}\ln x = \frac{1}{x}$ | $x > 0$ |

## Derivation

Change of base, from first principles: $a^y = x \implies \ln(a^y) = \ln x \implies y\ln a = \ln x \implies y = \frac{\ln x}{\ln a}$.

The product rule: $a^{p+q} = a^p a^q$ — taking $\log_a$ of both sides gives $\log_a(xy) = \log_a x + \log_a y$ (with $x = a^p$, $y = a^q$). Every other rule follows the same way from the index laws. [Full derivations: 03-Logarithms-Proofs]

## Method

**Solving log equations** $a > 1$:
1. If there are multiple logs, combine with product/quotient rules (check domain first).
2. Convert log form → exponential form: $\log_a f(x) = c \implies f(x) = a^c$.
3. Solve the resulting algebraic equation.
4. **Check every root against the domain** — discard extraneous ones.

**Logarithmic inequalities** (base $b$):
- $b > 1$ (increasing): $\log_b f < c \implies f < b^c$, direction preserved.
- $0 < b < 1$ (decreasing): direction **flips**.
- Always intersect with the domain $f > 0$.

**Telescoping products**: rewrite each term with change of base — intermediate logs cancel: $\log_a b \cdot \log_b c = \log_a c$.

## Worked Examples

**Setup:** Solve $\log_2 (x+3) + \log_2 (x-1) = 3$.

**Solution:** Domain: $x > 1$. Combine: $\log_2((x+3)(x-1)) = 3$. Convert: $(x+3)(x-1) = 8 \Rightarrow x^2 + 2x - 11 = 0 \Rightarrow x = -1 \pm 2\sqrt{3}$. Only $x = -1 + 2\sqrt{3} \approx 2.46 > 1$ is valid.

**Key insight:** Always check domain restrictions after solving — one root may be extraneous.

---

**Setup:** Evaluate $\log_3 5 \cdot \log_5 7 \cdot \log_7 9$.

**Solution:** Change of base: $\frac{\ln 5}{\ln 3} \cdot \frac{\ln 7}{\ln 5} \cdot \frac{\ln 9}{\ln 7} = \frac{\ln 9}{\ln 3} = \log_3 9 = 2$.

**Key insight:** Change of base creates a telescoping product — intermediate terms cancel.

---

**Setup:** Solve $\log_2 (x-1) < 3$.

**Solution:** Domain: $x > 1$. Base $2 > 1$ (increasing): $x-1 < 8 \Rightarrow x < 9$. Combined: $1 < x < 9$.

**Key insight:** For $a > 1$ the inequality direction is preserved; for $0 < a < 1$ it flips.

## Common Traps

- $\log(a+b) \neq \log a + \log b$ — only *products* and *quotients* split
- Argument must be strictly positive — $\log(0)$ and $\log(\text{negative})$ are undefined
- Base must be positive and $\neq 1$ — $\log_1 x$ is meaningless
- Extraneous roots from log equations — always re-check against the domain
- Inequalities with base $0 < a < 1$ — direction flips
- Dropping the absolute value: $\ln|x|$ is the antiderivative of $1/x$, not $\ln x$

## Connections

- 02-Indices — logs are the inverse operation
- 07-Binomial-Theorem — logs handle large powers
- 04.2-Differentiation — $\frac{d}{dx}\ln x = \frac{1}{x}$
- 04.4-Integration — $\int \frac{1}{x}\,dx = \ln|x| + C$


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

- [[Shannon Entropy H(X)]] — entropy measured in bits via log base 2
