# Variation of Parameters

Undetermined coefficients only handles inputs built from polynomials, exponentials, and waves. Variation of parameters is the universal method: given *any* continuous forcing $g(x)$ — $\tan x$, $\ln x$, $e^x/x$ — it manufactures the particular solution from the homogeneous ones. Slower, but never refuses.

**The Intuition:** Take the complementary solution $y = c_1y_1 + c_2y_2$ and promote the constants to functions: $y_p = u_1(x)y_1 + u_2(x)y_2$. You have two unknown functions but one equation, so you may impose a second condition of your choosing — pick the condition that kills the $u''$ terms. What survives is a tiny algebraic system for $u_1', u_2'$ whose right-hand sides involve only $g$: integrate, done. The method "varies" what used to be parameters; hence the name.

**The Math:** For

$$y'' + p(x)y' + q(x)y = g(x),$$

with independent homogeneous solutions $y_1, y_2$, seek

$$y_p = u_1 y_1 + u_2 y_2 \qquad \text{where}$$
$$u_1' = \frac{-y_2\, g}{W}, \qquad u_2' = \frac{y_1\, g}{W}, \qquad W = W(y_1,y_2) \neq 0.$$

Then integrate to get $u_1, u_2$, and

$$y = c_1y_1 + c_2y_2 + y_p.$$

Derivation in one breath: impose $u_1'y_1 + u_2'y_2 = 0$ (the chosen condition); then computing $y_p''$ and substituting into the ODE, all terms without primes cancel because $y_{1,2}$ solve the homogeneous equation, leaving exactly the system above via Cramer's rule.

**Setup:** Solve $y'' - 4y' + 4y = e^{2x}\ln x$ (resonant input).

**Solution:** Homogeneous: $(r-2)^2 = 0$, $y_c = (c_1 + c_2x)e^{2x}$; take $y_1 = e^{2x}$, $y_2 = xe^{2x}$. Compute the Wronskian directly:
$W = y_1y_2' - y_2y_1' = e^{2x}(e^{2x} + 2xe^{2x}) - xe^{2x}(2e^{2x}) = e^{4x}$.
So
$$u_1' = \frac{-xe^{2x}\cdot e^{2x}\ln x}{e^{4x}} = -x\ln x, \qquad u_2' = \frac{e^{2x}\cdot e^{2x}\ln x}{e^{4x}} = \ln x.$$
Integrate by parts: $u_1 = -\tfrac{x^2}{2}\ln x + \tfrac{x^2}{4}$, $u_2 = x\ln x - x$. Then
$$y_p = \Big(-\tfrac{x^2}{2}\ln x + \tfrac{x^2}{4}\Big)e^{2x} + (x\ln x - x)\,xe^{2x} = \frac{x^2 e^{2x}}{2}\ln x - \frac{3x^2e^{2x}}{4}.$$
(Dropping pieces that solve the homogeneous equation is allowed.) Full answer:
$$y = (c_1 + c_2x)e^{2x} + \frac{x^2e^{2x}}{2}\ln x - \frac{3x^2e^{2x}}{4}.$$

**Key insight:** $\ln x$ inputs are impossible for undetermined coefficients but routine here — the integrals are the only work.

**Setup:** Solve $y'' + y = \tan x$.

**Solution:** $y_1 = \cos x$, $y_2 = \sin x$, $W = 1$. Then $u_1' = -\sin x\tan x = -\dfrac{\sin^2 x}{\cos x} = \cos x - \sec x$, so $u_1 = \sin x - \ln|\sec x + \tan x|$. And $u_2' = \cos x \tan x = \sin x$, so $u_2 = -\cos x$. Particular:
$$y_p = (\cos x)(\sin x - \ln|\sec x + \tan x|) + (\sin x)(-\cos x) = -\cos x \ln|\sec x + \tan x|.$$
(The $\pm\sin x\cos x$ pair cancels.)

**Key insight:** Trig identities do heavy lifting mid-problem; simplify before integrating or the integrals balloon.

---

### Additional Notes

Order of attack for nonhomogeneous problems: if $g$ lives in the polynomial–exponential–wave family, use 08-Undetermined-Coefficients (fast); otherwise variation of parameters (universal). Both require the homogeneous basis first (07-Homogeneous-Constant-Coefficients). One warning: the formulas above are for *standard form* — leading coefficient normalized to 1 — so divide through first or every $W$-quotient inherits the wrong denominator.

---
