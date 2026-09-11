# Exact Equations & Substitutions

When an equation neither separates nor is linear, two escape hatches remain: check whether the left side is secretly a perfect total differential (exactness), or transform the variable so the equation becomes one of the types you already know (substitution).

**The Intuition:** An exact equation is a level-set condition in disguise. If some landscape function $\Psi(x, y)$ exists with $\Psi_x = M$ and $\Psi_y = N$, then $M\,dx + N\,dy = 0$ just says "walk along a contour of constant altitude." Solving the ODE means finding the terrain — integrate the slope in one direction, then patch it using the other. Substitutions are lens changes: a gnarly equation in $y$ may be tame when viewed through $v = y/x$, because many first-order equations really only care about *ratios*, not levels.

**The Math:**

**Exactness.** For $M(x,y)\,dx + N(x,y)\,dy = 0$: if

$$\frac{\partial M}{\partial y} = \frac{\partial N}{\partial x}$$

(on a simply connected region), then $\Psi$ exists with $\Psi_x = M,\ \Psi_y = N$, and solutions are the level curves

$$\Psi(x, y) = C.$$

Procedure: integrate $M$ in $x$ → get $\Psi$ up to a function $h(y)$; match $\Psi_y = N$ to find $h'$; integrate.

**Homogeneous substitution.** If $f(tx, ty) = f(x, y)$ (degree-0 right side), set $v = y/x$. Then $y' = v + xv'$ and the ODE becomes separable in $v$ and $x$ (02-Separable-Equations).

**Bernoulli.** $y' + p(x)y = q(x)y^n$ ($n \ne 0, 1$): substitute $u = y^{1-n}$, turning it linear (03-Linear-First-Order-and-Integrating-Factor):

$$u' + (1-n)p(x)u = (1-n)q(x).$$

**Setup:** Solve $(2xy + 1)\,dx + (x^2 - 1)\,dy = 0$.

**Solution:** Check: $M_y = 2x$, $N_x = 2x$. Equal ⇒ exact. Integrate $M$: $\Psi = \int(2xy+1)dx = x^2y + x + h(y)$. Then $\Psi_y = x^2 + h'(y)$ must equal $N = x^2 - 1$, so $h'(y) = -1$, $h = -y$. Answer: $x^2y + x - y = C$.

**Key insight:** The test comes first — never start integrating until $M_y = N_x$ is verified on paper.

**Setup:** Solve $xy' = y + x\tan(y/x)$.

**Solution:** Right side depends only on $y/x$: substitute $v = y/x$, so $y' = v + xv'$. Then $x(v + xv') \cdot \frac{1}{x}\cdots$ — carefully: $v + xv' = \tfrac{y}{x} + \tan v = v + \tan v$, hence $xv' = \tan v$. Separate: $\cot v\,dv = \frac{dx}{x}$. Integrate: $\ln|\sin v| = \ln|x| + C$, so $\sin(y/x) = Kx$.

**Key insight:** The tell for homogeneous equations is that $x$ and $y$ appear only through ratios like $y/x$ or powers like $x^2+xy+y^2$ (all same total degree).

**Setup:** Solve $y' + \dfrac{y}{x} = x^2 y^3$.

**Solution:** Bernoulli with $n = 3$: let $u = y^{-2}$, so $u' = -2y^{-3}y'$. Multiply the ODE by $-2y^{-3}$: $u' - \dfrac{2}{x}u = -2x^2$. Linear! $\mu = e^{\int -2/x\,dx} = x^{-2}$: $(x^{-2}u)' = -2$, so $x^{-2}u = -2x + C$, i.e. $u = -2x^3 + Cx^2$. Back-substitute: $y^{-2} = Cx^2 - 2x^3$.

**Key insight:** The exponent $n$ dictates the power $u = y^{1-n}$ — memorize the formula, then it's a linear problem you've already mastered.

---

### Additional Notes

Decision order for first-order ODEs: (1) separable? (2) linear? (3) exact? (4) homogeneous/Bernoulli substitution? Most course exams draw from these four boxes. If all fail, the honest answer is "no elementary solution" — most ODEs have none, which is exactly why numerical methods and existence theory exist. With first-order tools complete, we graduate to the structural theory of linear second-order equations in 06-Second-Order-Linear-Theory-Wronskian.

---
