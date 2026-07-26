---
date: 2026-07-19
type: proof
tags: [maths, pure, proof, algebra, logarithms]
parent: [[Pure/01-Algebra/03-Logarithms]]
---

# Logarithms — Full Derivations

## Definition
For $a > 0$, $a \neq 1$, $x > 0$:
$\log_a x = y \iff a^y = x$

## Proof of Logarithm Laws

### 1. Product Rule: $\log_a(xy) = \log_a x + \log_a y$

**Proof:**
Let $\log_a x = u$ and $\log_a y = v$.
Then $a^u = x$ and $a^v = y$.
$xy = a^u \cdot a^v = a^{u+v}$.
By definition, $\log_a(xy) = u + v = \log_a x + \log_a y$. $\square$

### 2. Quotient Rule: $\log_a\left(\frac{x}{y}\right) = \log_a x - \log_a y$

**Proof:**
Let $\log_a x = u$, $\log_a y = v \Rightarrow a^u = x, a^v = y$.
$\frac{x}{y} = \frac{a^u}{a^v} = a^{u-v}$.
$\log_a(x/y) = u - v = \log_a x - \log_a y$. $\square$

### 3. Power Rule: $\log_a(x^r) = r \log_a x$

**Proof:**
Let $\log_a x = u \Rightarrow a^u = x$.
$x^r = (a^u)^r = a^{ru}$.
$\log_a(x^r) = ru = r \log_a x$. $\square$

### 4. Change of Base: $\log_a b = \frac{\log_c b}{\log_c a}$ ($c > 0, c \neq 1$)

**Proof:**
Let $\log_a b = u \Rightarrow a^u = b$.
Take $\log_c$ of both sides: $\log_c(a^u) = \log_c b$.
By power rule: $u \log_c a = \log_c b$.
Therefore $u = \frac{\log_c b}{\log_c a}$. $\square$

**Corollary:** $\log_a b = \frac{1}{\log_b a}$ (set $c = b$).

---

### 5. Log of 1: $\log_a 1 = 0$

**Proof:** $a^0 = 1 \Rightarrow \log_a 1 = 0$. $\square$

### 6. Log of Base: $\log_a a = 1$

**Proof:** $a^1 = a \Rightarrow \log_a a = 1$. $\square$

---

### 7. Inverse Property: $a^{\log_a x} = x$ and $\log_a(a^x) = x$

**Proof:**
$\log_a x = y \iff a^y = x$.
So $a^{\log_a x} = a^y = x$.
And $\log_a(a^x) = x$ since $a^x$ is the number whose log base $a$ is $x$. $\square$

---

## Derivative of $\log_a x$

$\frac{d}{dx} \log_a x = \frac{1}{x \ln a}$

**Proof:**
$\log_a x = \frac{\ln x}{\ln a}$ (change of base with $c = e$).
$\frac{d}{dx} \frac{\ln x}{\ln a} = \frac{1}{\ln a} \cdot \frac{1}{x} = \frac{1}{x \ln a}$. $\square$

**Special case:** $\frac{d}{dx} \ln x = \frac{1}{x}$.

---

## Integral of $\frac{1}{x}$

$\int \frac{1}{x} dx = \ln |x| + C$

**Proof:**
$\frac{d}{dx} \ln x = \frac{1}{x}$ for $x > 0$.
For $x < 0$, let $u = -x > 0$. $\frac{d}{dx} \ln(-x) = \frac{1}{-x} \cdot (-1) = \frac{1}{x}$.
So $\frac{d}{dx} \ln |x| = \frac{1}{x}$ for all $x \neq 0$. $\square$

---

## Logarithmic Differentiation

For $y = f(x)^{g(x)}$:
$\ln y = g(x) \ln f(x)$
Differentiate: $\frac{y'}{y} = g'(x) \ln f(x) + g(x) \frac{f'(x)}{f(x)}$
$y' = y \left[ g'(x) \ln f(x) + g(x) \frac{f'(x)}{f(x)} \right]$

---

## Taylor Series for $\ln(1+x)$

For $|x| < 1$:
$\ln(1+x) = x - \frac{x^2}{2} + \frac{x^3}{3} - \frac{x^4}{4} + \cdots = \sum_{n=1}^\infty \frac{(-1)^{n-1}}{n} x^n$

**Proof:**
$\frac{1}{1+x} = 1 - x + x^2 - x^3 + \cdots = \sum_{n=0}^\infty (-1)^n x^n$ (geometric series, $|x|<1$)
Integrate term by term: $\int \frac{1}{1+x} dx = \ln(1+x) = C + \sum_{n=0}^\infty \frac{(-1)^n}{n+1} x^{n+1}$.
At $x=0$: $\ln 1 = 0 = C$. So $C=0$.
$\ln(1+x) = \sum_{n=1}^\infty \frac{(-1)^{n-1}}{n} x^n$. $\square$

---

## Logarithmic Inequality Proof

**For $a > 1$:** $x > y > 0 \iff \log_a x > \log_a y$

**Proof:**
$f(x) = \log_a x$ has derivative $f'(x) = \frac{1}{x \ln a} > 0$ for $x > 0$, $a > 1$.
So $f$ is strictly increasing.
Therefore $x > y \iff f(x) > f(y)$. $\square$

**For $0 < a < 1$:** $f'(x) = \frac{1}{x \ln a} < 0$ (since $\ln a < 0$).
So $f$ is strictly decreasing.
$x > y \iff f(x) < f(y)$. $\square$

---

## Limit: $\lim_{x \to 0} \frac{\ln(1+x)}{x} = 1$

**Proof 1 (L'Hôpital):**
$\lim_{x \to 0} \frac{\ln(1+x)}{x} \overset{0/0}{=} \lim_{x \to 0} \frac{1/(1+x)}{1} = 1$. $\square$

**Proof 2 (Definition of derivative):**
$\frac{d}{dx} \ln(1+x)\big|_{x=0} = \frac{1}{1+0} = 1$.
But derivative = $\lim_{h \to 0} \frac{\ln(1+h) - \ln 1}{h} = \lim_{h \to 0} \frac{\ln(1+h)}{h}$. $\square$

**Proof 3 (Substitution $x = 1/n$):**
$\lim_{n \to \infty} n \ln(1+1/n) = \lim_{n \to \infty} \ln(1+1/n)^n = \ln e = 1$. $\square$