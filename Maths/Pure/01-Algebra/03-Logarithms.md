---
date: 2026-07-19
type: concept
tags: [maths, pure, a-level, algebra, logarithms, logs]
parent: [[01-Algebra]]
proofs: [[Pure/Proofs/01-Algebra/03-Logarithms-Proofs]]
---

# 1.3 Logarithms

## Definition
For $a > 0$, $a \neq 1$, $x > 0$:
$\log_a x = y \iff a^y = x$

**Common bases:**
- $\log_{10} x$ or $\lg x$ — common logarithm
- $\log_e x$ or $\ln x$ — natural logarithm

## Formulae

| Formula | Expression |
|---------|------------|
| Product | $\log_a (xy) = \log_a x + \log_a y$ |
| Quotient | $\log_a \left(\frac{x}{y}\right) = \log_a x - \log_a y$ |
| Power | $\log_a (x^r) = r \log_a x$ |
| Change of Base | $\log_a b = \frac{\log_c b}{\log_c a}$ |
| Base Swap | $\log_a b = \frac{1}{\log_b a}$ |
| Log of 1 | $\log_a 1 = 0$ |
| Log of Base | $\log_a a = 1$ |
| Inverse | $a^{\log_a x} = x$ |
| Reciprocal | $\log_a \frac{1}{x} = -\log_a x$ |
| Power in base | $\log_{a^k} x = \frac{1}{k} \log_a x$ |

## Properties / Key Concepts

### Logarithmic Equations

**Type 1 — Single log:** Convert to exponential
$\log_a f(x) = c \implies f(x) = a^c$ (check $f(x) > 0$)

**Type 2 — Multiple logs:** Combine using laws
$\log_a f(x) + \log_a g(x) = c \implies \log_a (f(x)g(x)) = c$

**Type 3 — Variable in base:**
$\log_{f(x)} g(x) = c \implies f(x)^c = g(x)$ (check $f(x)>0, f(x)\neq 1, g(x)>0$)

### Logarithmic Inequalities
- For $a > 1$: $\log_a x > \log_a y \iff x > y$ (monotonic increasing)
- For $0 < a < 1$: $\log_a x > \log_a y \iff x < y$ (monotonic decreasing)
- **Always check domain:** arguments $> 0$

### Problem-Solving Patterns

| Pattern | Approach |
|---------|----------|
| Solve $\log_a f(x) = c$ | Convert to $f(x) = a^c$, check domain |
| Solve $\log_a f(x) = \log_a g(x)$ | $f(x) = g(x)$ with $f(x), g(x) > 0$ |
| Simplify expression | Apply laws to combine/expand |
| Change of base | Use $\log_a b = \frac{\ln b}{\ln a}$ or $\frac{\lg b}{\lg a}$ |
| Log inequality | Split cases $a>1$ vs $0<a<1$, apply monotonicity |

## Worked Examples

### Example 1: Solve $\log_2 (x+3) + \log_2 (x-1) = 3$
**Solution:**
Domain: $x+3>0$, $x-1>0 \Rightarrow x > 1$
$\log_2 ((x+3)(x-1)) = 3$
$(x+3)(x-1) = 2^3 = 8$
$x^2 + 2x - 3 = 8 \Rightarrow x^2 + 2x - 11 = 0$
$x = -1 \pm 2\sqrt{3}$
Only $x = -1 + 2\sqrt{3} \approx 2.46 > 1$ valid.

### Example 2: Solve $\log_x 8 = 3$
**Solution:**
Domain: $x > 0$, $x \neq 1$
$x^3 = 8 \Rightarrow x = 2$ (valid)

### Example 3: Evaluate $\log_3 5 \cdot \log_5 7 \cdot \log_7 9$
**Solution:**
$\log_3 5 \cdot \log_5 7 \cdot \log_7 9 = \frac{\ln 5}{\ln 3} \cdot \frac{\ln 7}{\ln 5} \cdot \frac{\ln 9}{\ln 7} = \frac{\ln 9}{\ln 3} = \log_3 9 = 2$

### Example 4: Solve $\log_2 (x-1) < 3$
**Solution:**
Domain: $x-1 > 0 \Rightarrow x > 1$
Base $2 > 1$ (increasing): $x-1 < 2^3 = 8 \Rightarrow x < 9$
Combined: $1 < x < 9$

## Common Traps
- ❌ $\log(a+b) = \log a + \log b$
- ❌ $\log(a-b) = \log a - \log b$
- ❌ Forgetting domain: argument MUST be positive
- ❌ Not checking base conditions ($a>0, a\neq 1$)
- ❌ Dividing by $\log_a x$ without considering $x=1$ ($\log_a 1 = 0$)
- ❌ In inequalities: forgetting to flip sign when $0<a<1$

## Cross-References
- [[Pure/01-Algebra/02-Indices|Indices]] — inverse relationship
- [[Pure/04-Calculus/01-Limits-Continuity|Limits & Continuity]] — $\lim_{n\to\infty}(1+1/n)^n = e$
- [[Pure/04-Calculus/07-Integration-Techniques|Integration Techniques]] — log integrals
- [[Physics/03-Thermal-Physics/03-Thermodynamics-Laws|Thermodynamics Laws]] — entropy $S = k \ln \Omega$

## Quick Reference
$\log_a(xy) = \log_a x + \log_a y$
$\log_a(x/y) = \log_a x - \log_a y$
$\log_a(x^r) = r \log_a x$
$\log_a b = \frac{\log_c b}{\log_c a}$
$a^{\log_a x} = x$, $\log_a 1 = 0$, $\log_a a = 1$

**Equation strategy:** Combine logs → convert to exponential → solve → check domain

---

*Status: Done*