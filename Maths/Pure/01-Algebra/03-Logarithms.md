# 1.3 Logarithms

A logarithm answers: "What power must I raise the base to, in order to get this number?" $\log_a x = y \iff a^y = x$. They are the inverse operation to exponentiation — if indices answer "what is $a^n$?", logarithms answer "what power gives me $x$?". The natural logarithm $\ln x$ (base $e$) is particularly important because $\frac{d}{dx} \ln x = \frac{1}{x}$. Logarithms turn multiplication into addition and exponentiation into multiplication, making complex expressions manageable and enabling solution of exponential equations.

**The Intuition:** A logarithm is like a "power detective." If you know the base and the result, the log tells you what exponent was used. $\log_2 8 = 3$ means "2 raised to what power gives 8? Answer: 3." Think of it as unwinding a power — exponentiation winds up ($2^3 = 8$), logarithms unwind ($\log_2 8 = 3$).

**The Math:** For $a > 0$, $a \neq 1$, $x > 0$: $\log_a x = y \iff a^y = x$.

- **Product:** $\log_a (xy) = \log_a x + \log_a y$
- **Quotient:** $\log_a \left(\frac{x}{y}\right) = \log_a x - \log_a y$
- **Power:** $\log_a (x^r) = r \log_a x$
- **Change of Base:** $\log_a b = \frac{\ln b}{\ln a}$
- **Base Swap:** $\log_a b = \frac{1}{\log_b a}$
- **Inverse:** $a^{\log_a x} = x$

Critical: $\log(a+b) \neq \log a + \log b$. Only products and quotients split. Domain: argument must be strictly positive, base must be positive and $\neq 1$. For $0 < a < 1$, inequalities flip direction.

**What does this mean for Pure Mathematics?** Logarithms are essential for calculus ($\int \frac{1}{x} dx = \ln|x| + C$), information theory (entropy in bits via $\log_2$), and solving any equation where the variable sits in the exponent. The change of base formula is your universal tool — $\log_3 5 = \frac{\ln 5}{\ln 3}$.

### Example 1: Solve $\log_2 (x+3) + \log_2 (x-1) = 3$

**Setup:** Sum of two logs with the same base.

**Solution:** Domain: $x > 1$. Combine: $\log_2((x+3)(x-1)) = 3$. Convert: $(x+3)(x-1) = 8 \Rightarrow x^2 + 2x - 11 = 0 \Rightarrow x = -1 \pm 2\sqrt{3}$. Only $x = -1 + 2\sqrt{3} \approx 2.46 > 1$ is valid.

**Key insight:** Always check domain restrictions after solving — one root may be extraneous.

### Example 2: Evaluate $\log_3 5 \cdot \log_5 7 \cdot \log_7 9$

**Setup:** Product of logs with different bases.

**Solution:** Apply change of base: $\frac{\ln 5}{\ln 3} \cdot \frac{\ln 7}{\ln 5} \cdot \frac{\ln 9}{\ln 7} = \frac{\ln 9}{\ln 3} = \log_3 9 = 2$.

**Key insight:** Change of base creates a telescoping product — intermediate terms cancel.

### Example 3: Solve $\log_2 (x-1) < 3$

**Setup:** Logarithmic inequality.

**Solution:** Domain: $x > 1$. Base $2 > 1$ (increasing): $x-1 < 8 \Rightarrow x < 9$. Combined: $1 < x < 9$.

**Key insight:** For $a > 1$, the inequality direction is preserved. For $0 < a < 1$, it flips.

---
