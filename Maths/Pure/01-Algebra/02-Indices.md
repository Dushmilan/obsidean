# 1.2 Indices (Exponents)

Indices are a shorthand for repeated multiplication: $a^n$ means $a$ multiplied by itself $n$ times. They compress repeated multiplication into compact expressions and, crucially, turn multiplication into addition ($a^m \cdot a^n = a^{m+n}$). This property is the reason logarithms work and why exponential models describe so many natural phenomena — from radioactive decay to population growth. The index laws extend naturally from positive integers to zero, negative, and fractional exponents, each extension preserving the core algebraic rules.

**The Intuition:** Think of an exponent as a "multiplication counter." $a^3$ counts three multiplications of $a$. The rule $a^m \cdot a^n = a^{m+n}$ is just concatenating the two counters. If $a^2 \cdot a^3 = (a \cdot a)(a \cdot a \cdot a) = a^5$, then adding exponents is the only rule that works.

**The Math:** For $a \in \mathbb{R}$, $n \in \mathbb{N}$: $a^n = \underbrace{a \cdot a \cdots a}_{n \text{ times}}$. Extended: $a^0 = 1$ ($a \neq 0$), $a^{-n} = \frac{1}{a^n}$ ($a \neq 0$), $a^{m/n} = \sqrt[n]{a^m}$ ($a \ge 0$ for even $n$).

- **Product:** $a^m \cdot a^n = a^{m+n}$
- **Quotient:** $\frac{a^m}{a^n} = a^{m-n}$
- **Power of power:** $(a^m)^n = a^{mn}$ (valid when $a > 0$ or exponents are integers)
- **Power of product/quotient:** $(ab)^n = a^n b^n$, $\left(\frac{a}{b}\right)^n = \frac{a^n}{b^n}$
- **Growth/decay:** $N(t) = N_0 e^{kt}$, half-life $t_{1/2} = \frac{\ln 2}{|k|}$

**What does this mean for Pure Mathematics?** The index laws are the foundation of exponential growth/decay models, logarithms, and simplifying algebraic expressions. Exponential equations require a fundamentally different solving strategy — you either match bases or take logarithms, never standard algebraic isolation.

### Example 1: Solve $2^{x+1} = 8^{2x-3}$

**Setup:** Exponential equation with related bases.

**Solution:** Rewrite $8 = 2^3$: $2^{x+1} = 2^{3(2x-3)} = 2^{6x-9}$. Equate exponents: $x+1 = 6x-9 \Rightarrow 5x = 10 \Rightarrow x = 2$.

**Key insight:** If you can write both sides with the same base, just set the exponents equal.

### Example 2: Solve $4^x - 5 \cdot 2^x + 4 = 0$

**Setup:** Quadratic in disguise.

**Solution:** Let $y = 2^x$: $y^2 - 5y + 4 = 0 \Rightarrow (y-1)(y-4) = 0$. $y=1 \Rightarrow x=0$; $y=4 \Rightarrow x=2$.

**Key insight:** When you see $a^{2x}$ and $a^x$ together, substitute $y = a^x$.

### Example 3: Population doubles in 5 years. Find $k$.

**Setup:** Exponential growth model $N(t) = N_0 e^{kt}$.

**Solution:** At $t=5$: $2 = e^{5k} \Rightarrow \ln 2 = 5k \Rightarrow k = \frac{\ln 2}{5} \approx 0.1386$.

**Key insight:** "Doubling" means the ratio is 2. Take natural log to solve for $k$.

---
