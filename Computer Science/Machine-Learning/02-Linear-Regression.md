Linear regression is the "hello world" of machine learning, and treating it as trivial is the fastest way to stay confused later. It is the complete supervised-learning pipeline in miniature — model, loss, optimizer, evaluation — and every deep learning network you will ever train reduces to it plus nonlinearities. Master the two ways to solve it (closed form and iterative) and you've mastered the template all of ML follows.

> [!intuition] The Intuition
> You're fitting the best straight line through a scatter of points, like laying a rigid ruler through a cloud of pushpins so total deviation is smallest. Each data point tugs on the ruler with a spring; the springs pull harder the further the point sits from the line; at equilibrium you have the least-squares fit. That's literally what minimizing squared error does — the solution *is* the force balance.

> [!math] The Math
> **Model.** Given features $x \in \mathbb{R}^{d}$ and parameters $w$ (weights), $b$ (bias/intercept):
>
> $$\hat{y} = h_{w,b}(x) = w^\top x + b$$
>
> Stack $N$ examples into matrix form: $\hat{\mathbf{y}} = Xw + b\mathbf{1}$. Convention trick: append a constant 1 column to $X$, fold $b$ into $w$, and write simply $\hat{\mathbf{y}} = Xw$.
>
> **Loss.** Mean Squared Error over $N$ training examples:
>
> $$J(w) = \frac{1}{2N}\sum_{i=1}^{N}\left(w^\top x^{(i)} - y^{(i)}\right)^2 = \frac{1}{2N}\|Xw - \mathbf{y}\|^2$$
>
> Why squared? Three honest reasons: (1) it penalizes large errors disproportionately, (2) it's differentiable everywhere, unlike absolute error at zero, (3) under Gaussian noise assumptions, squared loss is the *maximum likelihood* choice ([[04-Loss-Functions]] derives this). The $\tfrac{1}{2}$ is cosmetic — it cancels when differentiating.
>
> **Solution 1 — Normal Equation (closed form).** Set the gradient to zero:
>
> $$\nabla_w J = \frac{1}{N}X^\top(Xw - \mathbf{y}) = 0 \quad\Longrightarrow\quad X^\top X w = X^\top \mathbf{y} \quad\Longrightarrow\quad \boxed{w = (X^\top X)^{-1}X^\top \mathbf{y}}$$
>
> This is exact — no iterations, no learning rate. Costs: $X^\top X$ is $d \times d$ and inverting costs $O(d^3)$, fine for hundreds of features, impossible for millions. Also requires $X^\top X$ invertible — collinear features break it (fix: [[06-Regularization]]).
>
> **Solution 2 — Gradient Descent.** When $d$ is huge, iterate instead:
>
> $$w \leftarrow w - \alpha \cdot \frac{1}{N} X^\top(Xw - \mathbf{y})$$
>
> Full derivation of the update rule lives in [[03-Gradient-Descent]]. Linear regression's loss surface is convex — a single bowl — so gradient descent always converges to the same answer the normal equation gives, just approximately and iteratively.
>
> **Probabilistic view (why squares are principled).** Assume targets are truly linear plus Gaussian noise:
>
> $$y = w^\top x + \varepsilon, \quad \varepsilon \sim \mathcal{N}(0, \sigma^2) \quad\Longrightarrow\quad p(y \mid x; w) = \mathcal{N}(y;\ w^\top x,\ \sigma^2)$$
>
> Maximize likelihood → minimize negative log-likelihood:
>
> $$-\log L(w) = \frac{1}{2\sigma^2}\sum_i (y^{(i)} - w^\top x^{(i)})^2 + \text{const}$$
>
> The first term *is* MSE. So least squares isn't arbitrary — it's the maximum-likelihood estimator when noise is Gaussian. Change the noise distribution and you change the loss: Laplace noise → absolute error, Bernoulli outputs → cross-entropy via [[04-Logistic-Regression]].
>
> **Worked example — tiny dataset by hand.** Data: $(1, 2), (2, 3), (3, 5)$ (no bias term, pure slope). Then $X = \begin{pmatrix}1\\2\\3\end{pmatrix},\ \mathbf{y} = \begin{pmatrix}2\\3\\5\end{pmatrix}$. Compute: $X^\top X = 14$, $X^\top\mathbf{y} = 2+6+15 = 23$. So $w = 23/14 \approx 1.643$. Check predictions: $\hat{y}(3) = 4.93$ vs actual 5 — residuals $\approx 0.36, -0.29, 0.07$. Sum of squared residuals: $\approx 0.21$; no other slope beats it. Verify with calculus for one parameter: $J(w) = \frac{1}{6}\sum(wx_i - y_i)^2$, $J'(w) = \frac{2}{6}\sum x_i(wx_i - y_i)$, setting zero gives $w\sum x_i^2 = \sum x_i y_i$, i.e., $14w = 23$. Same answer — normal equation and calculus agree because they're the same operation.
>
> **Worked example — interpreting coefficients.** Fit apartment price (in \$1000s) from size ($m^2$): get $w = 4.2$, $b = 30$. Read: each extra $m^2$ adds \$4,200; baseline (0 m², meaningless physically but needed for calibration) \$30k. If you add feature "district avg income" and $w_{\text{size}}$ drops to 3.1, that's *confounding in action* — richer districts have bigger pricier flats, and part of size's apparent effect belonged to income. Regression coefficients are *conditional* effects: effect of a feature holding others fixed. This is why blindly trusting coefficients misleads.
>
> **Evaluation hook:** Goodness of fit is measured with $R^2 = 1 - \frac{\text{SS}_\text{res}}{\text{SS}_\text{tot}}$ — fraction of variance explained. But high $R^2$ on *training* data means nothing about generalization — see [[07-Model-Evaluation-and-Metrics]] and [[05-Bias-Variance-and-Overfitting]] before trusting any number.

> [!context] AI Context
> This file establishes the linear model, MSE, the normal equation derivation via $\nabla J = 0$, gradient descent as alternative, and the Gaussian-noise MLE justification for squared loss. The probabilistic-view pattern (pick noise → derive loss → optimize) recurs in logistic regression and neural nets.

---
If you remember one thing: linear regression = model $w^\top x + b$, MSE loss, solved exactly by $(X^\top X)^{-1}X^\top\mathbf{y}$ or iteratively by gradient descent — and squared loss is justified, not assumed.
