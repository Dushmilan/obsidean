# ODEs — Terminology & Classification

A differential equation is an equation for an unknown *function* written in terms of its own derivatives. It is how mathematics encodes change: population growth, cooling coffee, vibrating springs, current in a circuit. Before solving anything, we need a vocabulary for what kind of equation we are facing — because the type tells you which tool to reach for.

**The Intuition:** Think of an ODE as a set of rules about your speed, not your position. "Your velocity is proportional to how far you are from home" doesn't name the journey, but it squeezes all possible journeys down to one family. Solving the ODE means finding every itinerary obeying the rules; an initial condition picks out the single trip you actually took. Classification (order, linearity) is just reading the rules carefully before driving.

**The Math:** An **ordinary differential equation (ODE)** relates an unknown $y(x)$ and its derivatives:

$$F\big(x,\ y,\ y',\ y'',\ \dots,\ y^{(n)}\big) = 0.$$

- **Order:** highest derivative present. $\ y' + 2y = e^x$ has order 1; $y'' + 4y = 0$ has order 2.
- **Linear vs nonlinear:** linear in $y$ and its derivatives — no $y^2$, no $yy'$, no $\sin(y)$:
$$a_n(x)y^{(n)} + a_{n-1}(x)y^{(n-1)} + \cdots + a_1(x)y' + a_0(x)y = g(x).$$
If $g \equiv 0$ it is **homogeneous**; otherwise **nonhomogeneous**.
- **Solution:** a function $y(x)$ that satisfies the identity on some interval.
- **Initial-value problem (IVP):** ODE + conditions at one point, e.g. $y(x_0) = y_0$, $y'(x_0) = y_1$. A solution exists and is unique near $x_0$ when $F$ and $\partial F/\partial y$ are continuous (Picard's theorem).
- **General solution of an order-$n$ linear ODE** contains $n$ independent constants; each initial condition fixes one constant.

Why derivatives encode physics: Newton's second law $m x'' = F$ is itself an ODE — acceleration is a derivative, so mechanics *is* differential equations.

**Setup:** Classify: (a) $y'' + 3y' + 2y = \sin x$, (b) $(y')^2 = x$, (c) $x^3 y''' + xy' - 5y = 0$.

**Solution:**
(a) Order 2, linear, nonhomogeneous ($g = \sin x$).
(b) Order 1, **nonlinear** ($(y')^2$).
(c) Order 3, linear (coefficients may depend on $x$ freely), homogeneous.

**Key insight:** Linearity is about *how $y$ appears*, never about how $x$ appears — $x^3 y'''$ is perfectly linear.

**Setup:** Verify $y = C_1 e^{2x} + C_2 e^{-2x}$ solves $y'' - 4y = 0$. What kind of family is this?

**Solution:** $y'' = 4C_1e^{2x} + 4C_2 e^{-2x} = 4y$, so $y'' - 4y = 0$ for any constants. Two constants for a second-order equation: this is the general solution.

**Key insight:** Checking a proposed solution is always differentiation + substitution — even before you can derive solutions yourself.

**Setup:** IVP: $y' = y$, $y(0) = 3$. Find the unique solution.

**Solution:** $y = Ce^t$ solves $y' = y$. The condition gives $Ce^0 = 3 \Rightarrow C = 3$, so $y = 3e^t$. Uniqueness holds since $f(t,y)=y$ has continuous partials everywhere.

**Key insight:** The general solution is the whole family; the initial condition selects the member passing through your point.

---

### Additional Notes

The course map: first-order equations get three techniques sorted by equation shape — separable (02-Separable-Equations), linear integrating factor (03-Linear-First-Order-and-Integrating-Factor), exact plus substitutions (04-Exact-Equations-and-Substitutions) — then applications (05-Applications-of-First-Order-ODEs). Linear equations of any order get a structural theory (Wronskians, 06-Second-Order-Linear-Theory-Wronskian) plus two construction methods for particular solutions (08-Undetermined-Coefficients, 09-Variation-of-Parameters), extended to higher orders and Cauchy–Euler equations in 10-Higher-Order-and-Cauchy-Euler. When you meet a new ODE, classify first: order → linear? → homogeneous? That decision tree picks the method every time.

---
