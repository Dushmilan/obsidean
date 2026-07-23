---
date: 2026-07-19
type: concept
tags: [maths, pure, a-level, algebra, indices, exponents]
parent: [[01-Algebra]]
proofs: [[Pure/Proofs/01-Algebra/02-Indices-Proofs]]
---

# 1.2 Indices (Exponents)

## Definition
For $a \in \mathbb{R}$, $n \in \mathbb{N}$: $a^n = \underbrace{a \cdot a \cdots a}_{n \text{ times}}$

Extended definitions:
- $a^0 = 1$ ($a \neq 0$)
- $a^{-n} = \frac{1}{a^n}$ ($a \neq 0$)
- $a^{m/n} = \sqrt[n]{a^m} = (\sqrt[n]{a})^m$ ($a \ge 0$ for even $n$)
- $a^x$ for irrational $x$: limit of rational exponents

## Formulae

| Formula | Expression |
|---------|------------|
| Product | $a^m \cdot a^n = a^{m+n}$ |
| Quotient | $\frac{a^m}{a^n} = a^{m-n}$ |
| Power of power | $(a^m)^n = a^{mn}$ |
| Power of product | $(ab)^n = a^n b^n$ |
| Power of quotient | $\left(\frac{a}{b}\right)^n = \frac{a^n}{b^n}$ |
| Growth/decay | $N(t) = N_0 e^{kt}$ or $N(t) = N_0 a^t$ |
| Half-life | $t_{1/2} = \frac{\ln 2}{k}$ (decay) |
| Doubling time | $t_2 = \frac{\ln 2}{k}$ (growth) |

**Critical:** $(a^m)^n = a^{mn}$ fails for negative $a$ with fractional exponents!
Example: $((-1)^2)^{1/2} = 1^{1/2} = 1 \neq (-1)^1 = -1$

## Properties / Key Concepts

### Exponential Equations

**Type 1 — Same Base:**
$a^{f(x)} = a^{g(x)} \Rightarrow f(x) = g(x)$ ($a > 0, a \neq 1$)

**Type 2 — Different Bases, Related:**
$a^{f(x)} = b^{g(x)}$ — take logs: $f(x)\ln a = g(x)\ln b$

**Type 3 — Quadratic in Exponential:**
$a^{2x} + b a^x + c = 0$ — substitute $y = a^x$

**Type 4 — Sum of Exponentials:**
$a^x + a^{-x} = k$ — multiply by $a^x$, solve quadratic

### Exponential Functions
$f(x) = a^x$ ($a > 0, a \neq 1$)
- Domain: $\mathbb{R}$, Range: $(0, \infty)$
- $y$-intercept: $(0, 1)$
- Horizontal asymptote: $y = 0$ (as $x \to -\infty$ if $a>1$, $x \to \infty$ if $0<a<1$)
- Monotonic: increasing if $a>1$, decreasing if $0<a<1$

### Growth & Decay Models
- $k > 0$ or $a > 1$: exponential growth
- $k < 0$ or $0 < a < 1$: exponential decay

### Problem-Solving Patterns

| Pattern | Example | Approach |
|---------|---------|----------|
| Same base | $2^{x+1} = 8^{2x-3}$ | Rewrite: $8=2^3 \Rightarrow 2^{x+1}=2^{6x-9}$ |
| Quadratic in $a^x$ | $4^x - 5\cdot 2^x + 4 = 0$ | Let $y=2^x \Rightarrow y^2 - 5y + 4 = 0$ |
| Symmetric sum | $2^x + 2^{-x} = 3$ | Let $y=2^x \Rightarrow y + 1/y = 3$ |
| Logarithmic form | $3^{2x} = 7$ | $2x\ln 3 = \ln 7 \Rightarrow x = \frac{\ln 7}{2\ln 3}$ |
| Growth/decay | Population doubles in 5 years | $2 = e^{5k} \Rightarrow k = \frac{\ln 2}{5}$ |

## Worked Examples

### Example 1: Solve $2^{x+1} = 8^{2x-3}$
Rewrite $8 = 2^3$: $2^{x+1} = 2^{3(2x-3)} = 2^{6x-9}$
Equate exponents: $x+1 = 6x-9 \Rightarrow 5x = 10 \Rightarrow x = 2$

### Example 2: Solve $4^x - 5 \cdot 2^x + 4 = 0$
Let $y = 2^x$: $y^2 - 5y + 4 = 0 \Rightarrow (y-1)(y-4) = 0$
$y = 1 \Rightarrow x = 0$; $y = 4 \Rightarrow x = 2$

### Example 3: Solve $2^x + 2^{-x} = 3$
Multiply by $2^x$: $(2^x)^2 - 3(2^x) + 1 = 0$
$y = \frac{3 \pm \sqrt{5}}{2}$, so $x = \log_2\left(\frac{3 \pm \sqrt{5}}{2}\right)$

### Example 4: Population doubles in 5 years. Find $k$.
$N(t) = N_0 e^{kt}$, at $t=5$: $2N_0 = N_0 e^{5k} \Rightarrow k = \frac{\ln 2}{5} \approx 0.1386$

## Common Traps
- ❌ $a^{m+n} = a^m + a^n$ (confusing with log property)
- ❌ $(a+b)^n = a^n + b^n$ (only $n=1$)
- ❌ Taking log of negative/zero
- ❌ Forgetting $a^x > 0$ for all $x$
- ❌ Extraneous solutions from squaring/substitution

## Cross-References
- [[Pure/01-Algebra/01-Real-Numbers-Surds|Real Numbers & Surds]] — fractional exponents as surds
- [[Pure/01-Algebra/03-Logarithms|Logarithms]] — inverse relationship
- [[Pure/04-Calculus/01-Limits-Continuity|Limits & Continuity]] — limit $e = \lim_{n\to\infty}(1+1/n)^n$
- [[Physics/02-Mechanics/03-Work-Energy-Power|Work, Energy & Power]] — exponential decay in damped oscillations

## Quick Reference
**Laws:** $a^m \cdot a^n = a^{m+n}$, $\frac{a^m}{a^n} = a^{m-n}$, $(a^m)^n = a^{mn}$
**Exponential equation:** Same base → equate exponents; different base → take logs
**Growth/decay:** $N(t) = N_0 e^{kt}$, half-life $t_{1/2} = \frac{\ln 2}{|k|}$

---

*Status: Done*