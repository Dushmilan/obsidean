# Linear First-Order & Integrating Factor

Not every first-order ODE separates. But a huge and important class — $y' + p(x)y = q(x)$ — always yields to one universal trick: multiply by the right magic factor and the left side becomes a perfect derivative.

**The Intuition:** A first-order linear equation says "your rate of change = (leak proportional to current amount) + (external tap)." The trouble is the leak couples with $y$. The integrating factor $\mu(x) = e^{\int p\,dx}$ acts like a volume knob that grows/shrinks exactly fast enough to cancel the leak: after multiplication, the left-hand side is precisely $(\mu y)'$, and you integrate once. One trick, no case analysis, works for *every* equation of this shape.

**The Math:** Standard form:

$$y' + p(x)\,y = q(x).$$

Multiply by the **integrating factor**:

$$\mu(x) = e^{\int p(x)\,dx}.$$

Then

$$\mu y' + \mu p y = (\mu y)' = \mu q \quad\Longrightarrow\quad y = \frac{1}{\mu}\left(\int \mu q\, dx + C\right).$$

Why it works: $\mu' = p\,\mu$ by construction, so $\mu y' + \mu p y = \mu y' + \mu' y = (\mu y)'$ — the product rule in reverse.

Procedure:
1. Divide by the coefficient of $y'$ to reach standard form.
2. Compute $\mu$; constants of integration may be dropped ($C=0$ suffices).
3. Multiply through; recognize $(\mu y)'$; integrate.
4. Solve for $y$; apply initial conditions.

**Setup:** Solve $y' + 2y = e^{3x}$.

**Solution:** $\mu = e^{\int 2dx} = e^{2x}$. Then $(e^{2x}y)' = e^{2x}e^{3x} = e^{5x}$, so $e^{2x} y = \tfrac{1}{5}e^{5x} + C$, giving $y = \tfrac{1}{5}e^{3x} + Ce^{-2x}$.

**Key insight:** The answer splits structurally: one particular solution ($e^{3x}/5$) driven by the input, plus the general homogeneous solution ($Ce^{-2x}$) — this split becomes the whole theory for higher-order equations (06-Second-Order-Linear-Theory-Wronskian).

**Setup:** Solve $xy' - 4y = x^6 e^x$ for $x > 0$.

**Solution:** Standard form: $y' - \tfrac{4}{x}y = x^5 e^x$. Then $\mu = e^{-\int 4/x\,dx} = e^{-4\ln x} = x^{-4}$. So $(x^{-4}y)' = x e^x$. Integrate by parts twice: $\int xe^x dx = (x-1)e^x + C$. Hence $y = x^4\big((x-1)e^x + C\big)$.

**Key insight:** Coefficients like $-\frac{4}{x}$ integrate to logarithms, so $\mu$ collapses back to powers of $x$ — keep an eye out for this pattern.

**Setup:** Mixing tank: 200 L brine, salt amount $S(t)$, inflow 10 L/min at 0.3 kg/L, outflow 10 L/min well-mixed. Find $S(t)$ if $S(0)=20$ kg.

**Solution:** Rate: $S' = \text{in} - \text{out} = 3 - 10\cdot\frac{S}{200} = 3 - \frac{S}{20}$. Standard form: $S' + \tfrac{1}{20}S = 3$. With $\mu = e^{t/20}$: $(e^{t/20}S)' = 3e^{t/20}$, so $S = 60 + Ce^{-t/20}$. From $S(0) = 20$: $C = -40$. Thus $S = 60 - 40e^{-t/20}$, rising toward 60 kg.

**Key insight:** "in − out" bookkeeping produces a linear equation almost every time; the long-term limit kills the transient term.

---

### Additional Notes

Sanity check your $\mu$: after multiplying, the left side *must* be exactly the derivative of a single product. If it isn't, you divided wrong or integrated $p$ wrong. And note the division-of-labor between techniques: separable handles products $f(x)g(y)$, linear handles sums with a $p(x)y$ term — when both apply, either path reaches the same answer. When neither applies, try substitutions or exactness next: 04-Exact-Equations-and-Substitutions.

---
