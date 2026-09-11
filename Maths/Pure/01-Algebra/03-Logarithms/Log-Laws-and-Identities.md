
## Definition

$\log_a x = y \iff a^y = x$, for $a > 0$, $a \neq 1$, $x > 0$.

| Law | Formula | Valid when |
|-----|---------|-----------|
| Product | $\log_a(xy) = \log_a x + \log_a y$ | $x, y > 0$ |
| Quotient | $\log_a\frac{x}{y} = \log_a x - \log_a y$ | $x, y > 0$ |
| Power | $\log_a x^r = r\log_a x$ | $x > 0$ |
| Change of base | $\log_a b = \frac{\ln b}{\ln a}$ | $a, b > 0$, $a \neq 1$ |
| Base swap | $\log_a b = \frac{1}{\log_b a}$ | both bases $\neq 1$ |
| Inverse | $a^{\log_a x} = x$, $\log_a a^x = x$ | $x > 0$ |

## The Intuition

Logs are "power detectives" — they unwind exponentiation. Each law mirrors an index law: products become sums, powers become products. The change-of-base formula is the universal translator.

## The Toolkit

| Identity | Form |
|----------|------|
| Special values | $\log_a 1 = 0$, $\log_a a = 1$ |
| Natural log | $\ln = \log_e$, $\frac{d}{dx}\ln x = \frac1x$ |
| Telescoping | $\log_a b\cdot\log_b c = \log_a c$ |

## Derivation

All laws follow from the index laws: $a^{p+q} = a^pa^q$ becomes $\log_a(xy) = \log_a x + \log_a y$. Change of base: $a^y = x \Rightarrow \ln(a^y) = \ln x \Rightarrow y = \frac{\ln x}{\ln a}$. [Full derivations: 03-Logarithms-Proofs]

## Method

1. **Simplify:** expand products/quotients/powers into sums.
2. **Change base:** $\log_a b = \ln b/\ln a$ — always works.
3. **Evaluate:** convert to exponential form.

## Worked Examples

**Setup:** Evaluate $\log_3 5\cdot\log_5 7\cdot\log_7 9$.

**Solution:** $\frac{\ln5}{\ln3}\cdot\frac{\ln7}{\ln5}\cdot\frac{\ln9}{\ln7} = \frac{\ln9}{\ln3} = \log_3 9 = 2$.

**Key insight:** Change of base telescopes — intermediates cancel.

## Common Traps

- $\log(a+b) \neq \log a + \log b$ — only products split
- Argument must be strictly positive
- Base must be positive, not 1
- Base swap $\log_a b = 1/\log_b a$

## Connections

- Logarithmic-Equations-and-Inequalities · Indices
- 04.2-Differentiation — $\frac{d}{dx}\ln x$


## Cross-Track Connections

*Reconstructed 2026-08-24 after the registry-loss incident — see [[Maths-MOC]].*

> Original links unrecoverable; topic-level mappings live in [[Maths-Cross_Index]].
