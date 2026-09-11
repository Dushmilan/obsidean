Optimizers are the algorithms that take the gradients backpropagation produces and decide *how* to turn them into parameter updates. Plain gradient descent ([[Computer Science/Machine-Learning/03-Gradient-Descent]]) treats every step identically; modern optimizers accumulate memory — of past directions (momentum) and past per-parameter magnitudes (adaptive scaling) — which is what makes deep networks trainable at practical speed. The lineage SGD → Momentum → RMSProp → Adam is a story of fixing one pathology at a time.

> [!intuition] The Intuition
> Descending a foggy valley with a heavy ball instead of footsteps: momentum is the ball's inertia — it rolls through small ditches, averages out the zigzag from noisy steps, and speeds along consistent downhills. Adaptive methods add per-foot terrain sensing: rocky directions (large, erratic gradients) get careful small steps; smooth gentle slopes get confident long strides. Adam combines both: an inertia ball *and* a per-direction stride controller.

> [!math] The Math
> **SGD + Momentum.** Velocity accumulates exponentially-decayed history of gradients:
>
> $$v_t = \beta v_{t-1} + g_t, \qquad w_t = w_{t-1} - \alpha\, v_t \qquad (\beta \approx 0.9)$$
>
> Unroll: $v_t = g_t + \beta g_{t-1} + \beta^2 g_{t-2} + \dots$ — weights move along an exponentially-weighted average direction. Effects: (1) consistent gradient components reinforce ($1/(1-\beta)$ amplification ≈ 10× for $\beta=0.9$); oscillating components cancel — ravines traversed straight instead of zigzag; (2) noise from mini-batches averages toward the true gradient; (3) the ball coasts across flat regions and rolls out of shallow bad minima. Nesterov variant looks ahead ($\nabla J(w + \beta v)$) before committing — slightly better curvature response, same idea.
>
> **RMSProp — per-parameter scaling.** Track second-moment (magnitude) of gradients with another EMA:
>
> $$s_t = \beta_2 s_{t-1} + (1-\beta_2)\, g_t^2, \qquad w_t = w_{t-1} - \alpha\, \frac{g_t}{\sqrt{s_t} + \epsilon}$$
>
> Parameters with consistently large gradients get their effective step shrunk; quiet parameters get amplified steps. The update magnitude becomes roughly scale-invariant (~α regardless of raw gradient size) — crucial when loss surfaces mix steep and flat directions, and for sparse settings where some parameters rarely receive gradient (embedding rows for rare words).
>
> **Adam — momentum + adaptive scaling combined:**
>
> $$m_t = \beta_1 m_{t-1} + (1-\beta_1)g_t \qquad\quad v_t = \beta_2 v_{t-1} + (1-\beta_2)g_t^2$$
> $$\hat m_t = \frac{m_t}{1-\beta_1^t}, \qquad \hat v_t = \frac{v_t}{1-\beta_2^t} \qquad\quad \boxed{w_t = w_{t-1} - \alpha\, \frac{\hat m_t}{\sqrt{\hat v_t} + \epsilon}}$$
>
> Defaults: $\beta_1 = 0.9$, $\beta_2 = 0.999$, $\epsilon = 10^{-8}$, α ≈ $10^{-3}$. The bias correction matters at start-up: with $m_0 = 0$, early estimates are biased toward zero; dividing by $(1-\beta^t)$ exactly unbiases them (at $t=1$: $\hat m_1 = m_1 / 0.1 = g_1$ — correct immediately). Without correction, first steps are tiny and warmup is slow. **AdamW** fixes a subtle flaw: Adam's $L_2$ regularization gets entangled with the adaptive denominator (over-penalized only for high-gradient params); AdamW decouples it — weight decay applied directly to weights, separate from gradient adaptation — and is the default for transformer training.
>
> | Optimizer | Memory | Tuning burden | Best for |
> |-----------|--------|---------------|----------|
> | SGD | $1\times$ params | High (LR schedule critical) | Final-point generalization in vision; large budgets |
> | SGD+Momentum | $2\times$ | High | Same, faster |
> | RMSProp | $2\times$ | Medium | RNNs (its origin), non-stationary objectives |
> | Adam/AdamW | $3\times$ | Low (robust defaults) | Transformers, NLP, quick experiments |
>
> **Worked example — momentum kills zigzag, numerically.** Loss $J(w) = 5w_1^2 + 0.1w_2^2$ (steep in $w_1$, shallow in $w_2$), α=0.08, start $(5, 1)$.
> Plain GD: gradient $(10w_1, 0.2w_2)$. Step 1: $(5{-}4,\ 1{-}0.016) = (1, 0.984)$. Step 2: $(1{-}0.8, 0.968) = (0.2, ...)$ — wait, recompute: gradient at $(1, 0.984)$: $(10, 0.197)$ → new $(0.2, 0.968)$. $w_1$ converges fast (steep), but watch what happens near the valley floor... at $(0.2, 0.968)$: gradient $(2, 0.194)$ → next $w_1 = 0.04$: monotone here because we started aligned with axes; now perturb the path — the classic failure appears when steps overshoot $w_1 = 0$ (try α=0.15: $5 \to 5(1{-}1.5) = -2.5 \to 6.25 \to \dots$ divergence along $w_1$ while $w_2$ still crawls). The condition-number problem from [[Computer Science/Machine-Learning/03-Gradient-Descent]] in action.
> Momentum ($\beta = 0.9$, α=0.02): $v_1 = 0.9v_0 + g$. Along $w_1$, big alternating gradient signs cancel inside $v$ (average ≈ true downhill pull without overshoot); along $w_2$, small consistent gradients accumulate ×10. Result: no divergence even at effective LR ~10× higher on the steep axis, while the flat axis finally moves at useful speed. That's both benefits simultaneously — this toy *is* why momentum is never worse in practice.
>
> **Worked example — reading Adam's adaptive behavior.** Embedding table: word "the" appears in 60% of batches; word "axolotl" in 0.01%. With shared LR SGD, "the"'s embedding takes huge cumulative movement while "axolotl" barely drifts. Under Adam, "the" has large $v_t$ → updates normalized to modest size; "axolotl"'s rare but informative gradients hit a tiny $v_t$ → each occurrence moves it meaningfully. Per-parameter normalization equalizes learning progress across frequencies — the mechanism behind Adam's dominance in NLP.
>
> **Schedules & warmup (part of optimizer design):** step decay (÷10 at milestones), cosine decay to zero over total steps, linear warmup over first ~few thousand steps then cosine — the transformer standard that prevents early-training blowups when $v_t$ estimates are still unreliable.

> [!context] AI Context
> This file derives momentum as gradient EMA, RMSProp as second-moment normalization, Adam as their fusion with bias correction, covers AdamW's decoupled weight decay, demonstrates zigzag suppression numerically, and prescribes schedules/warmup. It assumes [[03-Forward-Pass-and-Backpropagation]] gradients and feeds directly into training practice.

---
If you remember one thing: momentum smooths direction, RMSProp/Adam rescale magnitude per-parameter, Adam fuses both with startup-corrected estimates — and AdamW's decoupled decay is today's transformer default.
