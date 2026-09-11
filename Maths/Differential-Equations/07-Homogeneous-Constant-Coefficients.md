# Homogeneous Constant-Coefficient Equations

Here is the miracle that makes linear ODEs with constant coefficients solvable by hand: guess an exponential, and calculus turns into algebra. Differentiating $e^{rx}$ just multiplies it by $r$, so the entire differential equation collapses into a polynomial equation for $r$.

**The Intuition:** Exponentials are eigenfunctions of differentiation — the derivative operator can't change their shape, only their size. Feed $e^{rx}$ into $y'' + py' + qy$ and it comes out as $(r^2 + pr + q)e^{rx}$: same function, scaled. The equation asks that scale factor to vanish — a polynomial in $r$. Each root of this "characteristic polynomial" is a growth rate the system can sustain; complex roots are oscillations wearing exponential clothes (rotation = $e^{i\theta}$).

**The Math:** For

$$ay'' + by' + cy = 0,$$

substitute $y = e^{rx}$ to get the **characteristic equation**

$$ar^2 + br + c = 0, \qquad r_{\pm} = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}.$$

Three cases:

| Discriminant | Roots | General solution |
|---|---|---|
| $b^2 - 4ac > 0$ | distinct real $r_1 \ne r_2$ | $y = c_1e^{r_1x} + c_2e^{r_2x}$ |
| $= 0$ | repeated $r$ | $y = (c_1 + c_2x)e^{rx}$ |
| $< 0$ | $\alpha \pm i\beta$ | $y = e^{\alpha x}(c_1\cos\beta x + c_2\sin\beta x)$ |

Why $xe^{rx}$ appears in the repeated-root case: one solution is "used up" by $e^{rx}$, and reduction of order (06-Second-Order-Linear-Theory-Wronskian) manufactures $y_2 = xe^{rx}$. The complex case uses Euler's formula $e^{(\alpha+i\beta)x} = e^{\alpha x}(\cos\beta x + i\sin\beta x)$ and takes real and imaginary parts.

**Setup:** Solve $y'' - 5y' + 6y = 0$, $y(0) = 1$, $y'(0) = 0$.

**Solution:** Characteristic: $r^2 - 5r + 6 = (r-2)(r-3) = 0$. So $y = c_1e^{2x} + c_2e^{3x}$. Conditions: $c_1 + c_2 = 1$, $2c_1 + 3c_2 = 0 \Rightarrow c_2 = -2, c_1 = 3$. Answer: $y = 3e^{2x} - 2e^{3x}$.

**Key insight:** Factor first, constants second — initial conditions always become two linear equations in two unknowns.

**Setup:** Solve $y'' + 4y' + 4y = 0$.

**Solution:** $r^2 + 4r + 4 = (r+2)^2 = 0$: repeated $r = -2$. Solution: $y = (c_1 + c_2x)e^{-2x}$.

**Key insight:** Critical damping lives here — the fastest possible return to zero without oscillation; the extra $x$ factor is the fingerprint of the double root.

**Setup:** Solve $y'' + 2y' + 5y = 0$ and describe the motion.

**Solution:** $r = \dfrac{-2 \pm \sqrt{4-20}}{2} = -1 \pm 2i$. So
$$y = e^{-x}\big(c_1\cos 2x + c_2 \sin 2x\big).$$
This is under-damped oscillation: frequency $2$, envelope $e^{-x}$ decaying to rest.

**Key insight:** Real part of $r$ controls the envelope (growth/decay), imaginary part controls angular frequency — read stability straight off the roots.

---

### Additional Notes

Connect each case to spring physics ($my'' + \gamma y' + ky = 0$): over-damped ($b^2>4ac$), critically damped ($=$), under-damped ($<$). The same table then scales up: higher-order equations have more roots — real ones give exponentials, repeats add powers of $x$, complex pairs add sines/cosines — exactly the bookkeeping expanded in 10-Higher-Order-and-Cauchy-Euler. To handle nonhomogeneous equations like $y'' + 4y = \sin 3x$, you need a particular solution on top of this homogeneous core: 08-Undetermined-Coefficients.

---
