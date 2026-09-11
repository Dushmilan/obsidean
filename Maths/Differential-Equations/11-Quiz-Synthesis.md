# Quiz & Synthesis

The course compresses into two toolboxes and one architecture. First-order: classify, then pick separable / linear / exact / substitution (01-ODEs-Terminology-and-Classification through 04-Exact-Equations-and-Substitutions). Linear of any order: homogeneous basis from the characteristic equation (06-Second-Order-Linear-Theory-Wronskian, 07-Homogeneous-Constant-Coefficients, 10-Higher-Order-and-Cauchy-Euler), particular solution by guessing or varying (08-Undetermined-Coefficients, 09-Variation-of-Parameters), then $y = y_c + y_p$. Applications translate sentences into rate laws (05-Applications-of-First-Order-ODEs).

**The Intuition:** Think of solving an ODE as triage. Question 1: what order? Order 1 → toolbox A (four tricks). Order ≥ 2 linear → architecture B. Nonlinear order 2 → outside this course's reach (and mostly beyond closed forms entirely). The exam is won at the classification step: most lost marks come from forcing the wrong method onto a well-behaved equation.

**The Math:** The full decision tree:

$$
\text{Order 1:}\quad
\begin{cases}
y' = f(x)g(y) & \text{separate} \\
y' + p\,y = q & \text{integrating factor } e^{\int p} \\
M_y = N_x & \text{potential } \Psi = C \\
\text{degree-0 RHS / Bernoulli} & v = y/x \text{ or } u = y^{1-n}
\end{cases}
$$

$$
\text{Linear } n \ge 2:\quad
r^n + \cdots = 0 \;\to\; y_c \;\to\; y_p \;(\text{UC or VoP}) \;\to\; y = y_c + y_p
$$

Work the six problems below cold before reading solutions.

**Setup (Q1 — first order, mixed):** Solve $\dfrac{dy}{dx} = \dfrac{y}{x} + x\tan\dfrac{y}{x}$... or simpler: solve $xy' = 2y + x^3\cos x$.

**Solution:** Standard form: $y' - \tfrac{2}{x}y = x^2\cos x$. Integrating factor $x^{-2}$: $(x^{-2}y)' = \cos x$, so $x^{-2}y = \sin x + C$, i.e. $y = x^2(\sin x + C)$.

**Key insight:** Divide to standard form *first* — the integrating factor is invisible until you do.

**Setup (Q2 — exactness):** Solve $(3x^2y + y^{-2})dx - (2yx^{-3})\cdots$ no — solve $(3x^2 y)\,dx + (x^3 + 2y)\,dy = 0$.

**Solution:** $M_y = 3x^2$, $N_x = 3x^2$: exact. $\Psi = \int 3x^2y\,dx = x^3y + h(y)$; $\Psi_y = x^3 + h' = x^3 + 2y \Rightarrow h = y^2$. Answer: $x^3y + y^2 = C$.

**Key insight:** Verify exactness on paper first; then integrate $M$ in $x$ and patch with $h(y)$ — mechanical once ordered.

**Setup (Q3 — modeling):** A tank holds 100 L of pure water. Brine with 0.2 kg/L enters at 5 L/min; the well-mixed contents leave at the same rate. Find the salt amount after 20 minutes.

**Solution:** $S' = 1 - \tfrac{5}{100}S = 1 - \tfrac{S}{20}$. Integrating factor $e^{t/20}$: $(e^{t/20}S)' = e^{t/20}$, so $S = 20 + Ce^{-t/20}$. From $S(0) = 0$: $C = -20$. At $t=20$: $S = 20(1 - e^{-1}) \approx 12.64$ kg.

**Key insight:** "in − out" always; equilibrium is inflow concentration × volume, reached exponentially.

**Setup (Q4 — characteristic cases):** Solve $y'' + 4y' + 8y = 0$ and state whether motion is over-, critically-, or under-damped.

**Solution:** $r = \dfrac{-4 \pm \sqrt{16-32}}{2} = -2 \pm 2i$. So $y = e^{-2x}(c_1\cos2x + c_2\sin2x)$. Discriminant negative ⇒ under-damped: decaying oscillations at frequency 2.

**Key insight:** Sign of the discriminant classifies damping instantly; real part gives decay rate, imaginary part frequency.

**Setup (Q5 — resonance):** Solve $y'' - y' = x + e^{x}$.

**Solution:** Homogeneous: $r(r-1)=0$: $y_c = c_1 + c_2e^{x}$. Split inputs. For $x$: trial $Ax + B$. For $e^x$: duplicates $c_2e^x$, so try $Axe^{x}$. Compute for $y_{p1} = Ax+B$: $y'' - y' = -A \stackrel{!}{=} x$?? Mismatch — polynomial input needs degree matching: use $y_{p1} = -\tfrac{x^2}{2} - x$: check $y' = -x - 1$, $y'' = -1$, so $y'' - y' = -1 + x + 1 = x$. ✓ For $y_{p2} = Axe^x$: $y' = Ae^x(x+1)$, $y'' = Ae^x(x+2)$, difference: $Ae^x \stackrel{!}{=} e^x \Rightarrow A = 1$. Total:
$$y = c_1 + c_2e^{x} - \frac{x^2}{2} - x + xe^{x}.$$

**Key insight:** Match each additive input separately (superposition), and bump trials that collide with $y_c$ by powers of $x$.

**Setup (Q6 — Cauchy–Euler):** Solve $x^2y'' - 3xy' + 3y = 0$, $x > 0$.

**Solution:** Trial $x^r$: $r(r-1) - 3r + 3 = r^2 - 4r + 3 = (r-1)(r-3)$. Distinct roots:
$$y = c_1x + c_3x^{3}.$$

**Key insight:** Same pipeline as constant coefficients, but the trial is $x^r$ and repeated roots bring $\ln|x|$ instead of $x$.

---

### Additional Notes

Final self-audit: can you (1) classify any ODE in under ten seconds, (2) produce $\mu = e^{\int p}$ without thinking, (3) test exactness before touching an integral, (4) write all three root-case templates from memory, (5) detect resonance against $y_c$ before substituting a trial, and (6) run variation-of-parameters when $g$ contains $\ln$ or $\tan$? If yes to all six, this module is banked. Where it goes next — Laplace transforms, series solutions, systems — each reuses this exact skeleton with a bigger engine.

---
