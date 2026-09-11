# Separable Equations

The first solving technique of the course, and the one you should always try first: if you can herd all the $y$'s to one side and all the $x$'s to the other, the problem collapses to two integrals.

**The Intuition:** A separable ODE says "the rate depends only on where you are and when, but in a split way" — growth rate $=$ (function of $y$) $\times$ (function of $x$). Since each factor only knows its own variable, you can sort them onto opposite sides like laundry: darks left, lights right. Integrating then just accumulates change on each side independently. It feels almost too easy — that's why checking separability is step one of every first-order encounter.

**The Math:** An ODE is **separable** if it can be written as

$$\frac{dy}{dx} = f(x)\,g(y).$$

Move everything $y$-ish left, everything $x$-ish right:

$$\frac{dy}{g(y)} = f(x)\,dx \quad\Longrightarrow\quad \int \frac{dy}{g(y)} = \int f(x)\,dx + C.$$

Procedure:
1. Rewrite into $\frac{dy}{dx} = f(x)g(y)$ form (factor!).
2. Separate; integrate both sides.
3. Solve for $y$ if possible; apply initial conditions *after* integrating (or use definite integrals).
4. Note lost solutions: dividing by $g(y)$ discards constant roots of $g(y) = 0$ — recover them separately.

The exponential model $\dfrac{dy}{dt} = ky$ integrates to $y = Ce^{kt}$ — the single most important solution shape in applied mathematics.

**Setup:** Solve $\dfrac{dy}{dx} = xy^2$, with $y(0) = 1$.

**Solution:** Separate: $\int y^{-2}\,dy = \int x\,dx \Rightarrow -y^{-1} = \tfrac{x^2}{2} + C$. Apply $y(0)=1$: $-1 = C$. So $\dfrac{1}{y} = 1 - \dfrac{x^2}{2}$, i.e. $y = \dfrac{1}{1 - x^2/2}$.

**Key insight:** Apply initial conditions to the *implicit* integrated form immediately — cleaner than carrying constants through algebra.

**Setup:** Solve $\dfrac{dy}{dx} = e^{x+y}$.

**Solution:** Factor: $e^{x+y} = e^x e^y$. Then $\int e^{-y} dy = \int e^x dx \Rightarrow -e^{-y} = e^x + C$. So $e^{-y} = -e^x - C$ and $y = -\ln(C_1 - e^x)$ after renaming constants. Also $y \equiv$ const solutions? Here $e^y \neq 0$ ever, so none are lost.

**Key insight:** Sums in an exponent usually mean separability via factoring — $e^{x+y}$, not $e^x + y$, is your trigger.

**Setup:** Solve $x\dfrac{dy}{dx} + y = y^2$, noting all constant solutions. (Initial condition $y(1) = 2$.)

**Solution:** Constants: $y \equiv 0$ works; $y \equiv 1$ works ($0 = 1 - 1$). For $y \ne 0, 1$: rewrite $x y' = y^2 - y$, separate: $\int \frac{dy}{y(y-1)} = \int \frac{dx}{x}$. Partial fractions: $\frac{1}{y(y-1)} = \frac{1}{y-1} - \frac{1}{y}$. Integrate: $\ln|y-1| - \ln|y| = \ln|x| + C$, so $\frac{y-1}{y} = Kx$. With $y(1) = 2$: $K = \tfrac12$. Hence $\frac{y-1}{y} = \frac{x}{2} \Rightarrow y = \frac{2}{2 - x}$.

**Key insight:** Partial fractions live inside half of all separable problems — brush up on them before exams.

---

### Additional Notes

Two cautions. First, separation is a formal trick — treating $dy/dx$ as a fraction is justified rigorously by the chain rule (substituting $u = g(y(x))$), so your conscience can stay clean. Second, always ask whether dividing by $g(y)$ deleted constant solutions: exam graders love docking that mark. Next technique up: when $y'$ appears alone with a linear-in-$y$ right-hand side, skip separation entirely and reach for the integrating factor in 03-Linear-First-Order-and-Integrating-Factor.

---
