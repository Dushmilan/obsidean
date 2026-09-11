Gradient descent is the engine under almost everything you'll train — linear regression, logistic regression, and every neural network in your Deep Learning vault. The idea is disarmingly simple: to minimize a loss, repeatedly step downhill. The richness is in *why* it works, what the learning rate really controls, and the three flavors (batch, stochastic, mini-batch) that define the speed/memory tradeoff of modern training.

> [!intuition] The Intuition
> You're standing on a foggy mountainside trying to reach the valley. You can't see the whole terrain, but you can feel the slope under your feet. Strategy: feel which direction steepest descends, take a step that way, repeat. Step size matters enormously — baby steps take forever; giant leaps overshoot the valley floor and bounce between walls forever; a well-chosen stride gets you down efficiently. The fog = you only ever compute local information (the gradient at your current point).

> [!math] The Math
> **The update rule.** For differentiable loss $J(w)$, first-order Taylor expansion around current point:
>
> $$J(w + \Delta) \approx J(w) + \nabla J(w)^\top \Delta$$
>
> To guarantee decrease, choose $\Delta$ opposite the gradient: $\Delta = -\alpha \nabla J(w)$. Then $J(w+\Delta) \approx J(w) - \alpha\|\nabla J(w)\|^2 < J(w)$ for small enough $\alpha$. This gives:
>
> $$\boxed{w \leftarrow w - \alpha\, \nabla_w J(w)}$$
>
> Why *negative* gradient specifically? It's the direction of **steepest descent** — among all unit directions $v$, the directional derivative $\nabla J \cdot v$ is minimized by $v = -\nabla J / \|\nabla J\|$ by Cauchy–Schwarz.
>
> **Deriving it concretely for linear regression.** With $J(w) = \frac{1}{2N}\|Xw - y\|^2$:
>
> $$\nabla_w J = \frac{1}{N}X^\top(Xw - y)$$
>
> so each iteration computes residuals $(Xw - y)$, correlates them with features via $X^\top(\cdot)$, and nudges weights against that correlation. Intuition: if feature $j$ consistently co-occurs with large positive errors, its weight should shrink. Gradient descent automates exactly this reasoning.
>
> **Convergence for convex quadratic.** Linear regression's loss has Hessian $H = \frac{1}{N}X^\top X$. GD on quadratics converges iff $0 < \alpha < 2/L$ where $L$ is the largest eigenvalue of $H$, and error shrinks per step like $(1 - \alpha \lambda_i)$ along eigendirection $i$. Consequence: convergence rate is throttled by the *condition number* $\kappa = \lambda_{\max}/\lambda_{\min}$ — elongated bowls (features at very different scales!) converge painfully slowly. This is the mathematical reason [[11-Feature-Engineering-and-Scaling]] insists on standardizing inputs before training.
>
> **Learning rate behavior** (worth visualizing mentally): too small → monotone crawl; good → smooth exponential approach; too big → oscillation across the valley with growing amplitude; way too big → divergence, loss goes to NaN. On real losses: decay schedules ($\alpha_t = \alpha_0/(1+kt)$, step decay, cosine) start large for fast progress, end small for precise settling.
>
> **Three flavors:**
>
> | Variant | Update uses | Pros | Cons |
> |---------|-------------|------|------|
> | Batch GD | Full dataset $\nabla J$ | Exact direction, stable | One update costs full pass; huge data = slow |
> | Stochastic GD | One random sample | Near-free updates, noise escapes local traps | Noisy path, needs decaying $\alpha$ |
> | Mini-batch GD | Random batch (32–1024) | Best of both; GPU-friendly vectorization | Batch size is another hyperparameter |
>
> Mini-batch is the modern default. Its noise isn't a bug — for nonconvex neural-net losses, noise helps escape saddles and shallow bad minima ([[05-Optimizers]] in DL builds directly on this).
>
> **Worked example — two steps by hand.** Minimize $J(w) = w^2$, start at $w_0 = 4$, learning rate $\alpha = 0.25$. Gradient: $J'(w) = 2w$.
>
> Step 1: $w_1 = 4 - 0.25(2 \cdot 4) = 4 - 2 = 2$. Loss: $16 \to 4$.
> Step 2: $w_2 = 2 - 0.25(2 \cdot 2) = 2 - 1 = 1$. Loss: $4 \to 1$.
> Pattern: each step halves $w$ — because $\alpha = 0.25$ gives multiplier $(1 - 2\alpha) = 0.5$. After $k$ steps $w_k = 4 \cdot 0.5^k$: exponential convergence, exactly as the eigenvalue analysis predicts ($\lambda = 2$ here).
>
> Now retry with $\alpha = 1.01$: $w_1 = 4 - 1.01 \cdot 8 = -4.08$. You've overshot to the other side *and grown* ($|-4.08| > 4$). Each subsequent step amplifies: divergence. The critical threshold is $\alpha = 2/L = 2/2 = 1$ — matching theory perfectly.
>
> **Worked example — linear regression in code-shaped pseudocode.**
> ```
> w ← zeros(d)
> repeat:
>     preds   ← X @ w                    # forward
>     error   ← preds − y                # residuals
>     grad    ← (X.T @ error) / N        # ∇J
>     w       ← w − α · grad             # update
> until ‖grad‖ small or patience exceeded
> ```
> This exact skeleton — forward, residual, gradient, update — survives unchanged into deep learning, where "gradient" comes from backprop instead of algebra ([[03-Forward-Pass-and-Backpropagation]]).
>
> **Failure modes to know:** saddle points (gradient zero, not minimum — momentum rescues), plateaus (vanishing gradients in deep nets), local minima (rarely the practical problem people fear in high dimensions), and ill-conditioning (fix via scaling). Modern optimizers ([[05-Optimizers]]) address all four with accumulated history.

> [!context] AI Context
> This file derives the GD update from Taylor expansion, proves why negative gradient is steepest descent, connects condition number to feature scaling, demonstrates convergence/divergence numerically, and defines batch/stochastic/mini-batch variants. It's the direct prerequisite for DL optimizers and backpropagation notes.

---
If you remember one thing: $w \leftarrow w - \alpha\nabla J$ is guaranteed descent *locally*, converges iff $\alpha$ respects curvature, and mini-batch noise is a feature, not a bug.
