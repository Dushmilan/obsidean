# Second-Order Linear Theory & Wronskians

From here on, the course changes character: less trick-hunting, more architecture. For linear second-order equations we prove a complete structural picture — solutions form a two-dimensional space, and every solution is built from just two independent pieces. The Wronskian is the tool that certifies independence.

**The Intuition:** A linear homogeneous equation $y'' + p y' + q y = 0$ behaves like a vector space problem: solutions add and scale to give new solutions (superposition). So the whole solution set is a flat plane inside function-space — find any two non-parallel directions and you own the entire plane. The Wronskian is a "parallelism detector": it vanishes exactly when your two candidate directions are secretly one direction in disguise.

**The Math:**

**Superposition.** If $y_1, y_2$ solve the homogeneous equation, so do $c_1 y_1 + c_2 y_2$. Consequence: for nonhomogeneous $L[y] = g$, if $y_p$ is *any* particular solution, then

$$y = c_1 y_1 + c_2 y_2 + y_p$$

covers all solutions — general $=$ complementary $+$ particular.

**Existence & uniqueness.** With continuous $p, q, g$ on an interval, the IVP ($y(x_0) = a,\ y'(x_0) = b$) has exactly one solution.

**Linear independence & the Wronskian.** Functions are **linearly dependent** if some nontrivial combination is identically zero. Define

$$W(y_1, y_2)(x) = \begin{vmatrix} y_1 & y_2 \\ y_1' & y_2' \end{vmatrix} = y_1y_2' - y_2y_1'.$$

For solutions of $y'' + py' + qy = 0$: $y_1, y_2$ are independent **iff** $W \ne 0$ at some point — in fact Abel's identity says

$$W(x) = C\,e^{-\int p\,dx},$$

so $W$ is either always zero or never zero. Then $\{y_1, y_2\}$ spans: every solution is $c_1y_1 + c_2y_2$ (**general solution**).

**Reduction of order.** Knowing one solution $y_1$, hunt the second as $y = v(x)y_1$: the ODE reduces to first order in $v'$, yielding
$$y_2 = y_1 \int \frac{e^{-\int p\,dx}}{y_1^2}\,dx.$$

**Setup:** Verify $y_1 = e^{3x}$, $y_2 = e^{-3x}$ are independent for $y'' - 9y = 0$ and write the general solution.

**Solution:** Both solve it: $9e^{\pm3x} - 9e^{\pm3x} = 0$. ✓ Wronskian: $W = e^{3x}(-3e^{-3x}) - (-3e^{3x})e^{-3x} = -6 \neq 0$ everywhere. General solution: $y = c_1e^{3x} + c_2e^{-3x}$.

**Key insight:** A nonzero Wronskian at even *one* point settles independence forever (Abel).

**Setup:** Given $y_1 = x$ solves $x^2y'' - 2xy' + 2y = 0$, find the full general solution.

**Solution:** Reduction of order with $p = -2/x$: $y_2 = x \displaystyle\int \frac{e^{-\int(-2/x)dx}}{x^2}dx = x\int \frac{x^2}{x^2}dx = x^2$. Check: $(x^2)'' - 2x(2x) + 2x^2 = 2x^2 - 4x^2 + 2x^2 = 0$. ✓ So $y = c_1x + c_2x^2$.

**Key insight:** One known solution unlocks the other mechanically — no guessing needed; the formula is a machine.

**Setup:** Show that $x^2$ and $x|x|$ have identically-zero Wronskian yet are not linearly dependent — why doesn't this break the theory?

**Solution:** On $(0,\infty)$, $x|x| = x^2$: proportional. On $(-\infty,0)$, $x|x| = -x^2$: proportional. Piecewise they differ, but $W = 0$ everywhere while neither is a global constant multiple of the other. The escape: they don't both solve any ODE $y'' + py' + qy = 0$ with continuous coefficients — Abel's guarantee only covers genuine solution pairs.

**Key insight:** "$W=0 \Rightarrow$ dependent" requires both functions to solve the same nice ODE; outside that setting, $W$ can mislead — a favorite exam trap.

---

### Additional Notes

This note is pure scaffolding but pays off immediately: 07-Homogeneous-Constant-Coefficients manufactures the two basis solutions from a characteristic polynomial, 08-Undetermined-Coefficients and 09-Variation-of-Parameters then build $y_p$, and 10-Higher-Order-and-Cauchy-Euler shows everything survives verbatim in higher dimensions — Wronskians become $n\times n$ determinants, spaces become $n$-dimensional.

---
