Regularization is any modification to a learning algorithm intended to reduce generalization error at the cost of training error. Ridge, Lasso, and Elastic Net are the canonical examples for linear models: they add a penalty on weight size to the loss, which shrinks coefficients, tames variance, and — in Lasso's case — performs automatic feature selection. Understanding *why penalizing size helps* is the key that unlocks dropout, early stopping, and weight decay in deep learning too.

> [!intuition] The Intuition
> Overfitted models are like conspiracy theorists: every feature gets an elaborate role ("the suspect's shoe size interacts with their postal code..."). Regularization enforces intellectual humility — "unless a feature earns its influence by strongly reducing error, keep its coefficient near zero." A model with small weights produces smooth, stable outputs: nudge the input slightly and the prediction barely moves. Big weights mean the model is exploiting fragile coincidences in the training data.

> [!math] The Math
> **The general recipe.** Add a norm penalty to the objective:
>
> $$J_{\text{reg}}(w) = \underbrace{\frac{1}{2N}\|Xw - y\|^2}_{\text{fit the data}} + \underbrace{\lambda\,\mathcal{R}(w)}_{\text{stay humble}}, \qquad \lambda \geq 0$$
>
> $\lambda$ is the dial between the two goals: $\lambda = 0$ recovers ordinary least squares; $\lambda \to \infty$ shrinks everything to zero. (Note: bias/intercept is conventionally *not* penalized — shifting the line doesn't affect wiggliness.)
>
> **Ridge regression ($L_2$ penalty).** $\mathcal{R}(w) = \|w\|_2^2 = \sum_j w_j^2$:
>
> $$J = \frac{1}{2N}\|Xw - y\|^2 + \lambda \|w\|^2 \quad\xrightarrow{\nabla = 0}\quad \boxed{\hat w_{\text{ridge}} = (X^\top X + 2\lambda N\, I)^{-1} X^\top \mathbf{y}}$$
>
> Compare with the normal equation from [[02-Linear-Regression]]: identical except $X^\top X$ gains a multiple of the identity before inversion. Two profound consequences:
> 1. **Invertibility guaranteed** — even with collinear features, $(X^\top X + cI)$ is always invertible. Ridge fixes the breakdown case of OLS.
> 2. **Shrinkage proportional to eigenvalues** — via SVD, ridge shrinks each principal direction's coefficient by $\frac{\sigma_i^2}{\sigma_i^2 + c}$: directions with strong signal pass through nearly untouched; weak/noisy directions get crushed. Ridge performs automatic, soft dimensionality reduction.
>
> Gradient form: $\nabla J = X^\top(Xw-y)/N + 2\lambda w$ — weights decay multiplicatively by factor $(1 - 2\alpha\lambda)$ every step *before* the data term corrects them. This "weight decay" reading is exactly what neural networks inherit ([[07-Regularization-in-DL]]).
>
> **Lasso ($L_1$ penalty).** $\mathcal{R}(w) = \|w\|_1 = \sum_j |w_j|$:
>
> $$J = \frac{1}{2N}\|Xw-y\|^2 + \lambda\sum_j |w_j|$$
>
> No closed form (the absolute value isn't differentiable at 0), but the crucial property: the $L_1$ penalty pushes *some* coefficients **exactly to zero**, producing sparse models. Geometry explains it: the $L_1$ constraint region is a diamond whose corners sit exactly on the axes — a solution pushed against a corner has all other coordinates zero. The $L_2$ ball is perfectly round, touching axes only at single points, hence no sparsity.
>
> Soft-thresholding gives the one-dimensional intuition: gradient descent step plus $L_1$ shrinkage moves a coefficient toward zero by fixed amount $\alpha\lambda$, clipping at zero:
>
> $$w \leftarrow S_{\alpha\lambda}\!\big(w - \alpha\nabla J_{\text{data}}\big), \qquad S_\tau(z) = \mathrm{sign}(z)\max(|z| - \tau, 0)$$
>
> Coefficients below the threshold are annihilated outright — that's feature selection.
>
> **Elastic Net.** Both penalties combined:
>
> $$J = \frac{1}{2N}\|Xw-y\|^2 + \lambda_1\|w\|_1 + \lambda_2\|w\|^2$$
>
> Keeps Lasso's sparsity while fixing its pathologies: with correlated features Lasso arbitrarily keeps one and discards the rest; Elastic Net keeps groups together.
>
> **Choosing $\lambda$.** Train error decreases monotonically as $\lambda \to 0$ — so you cannot tune $\lambda$ on training data. Sweep $\lambda$ over a log grid ($10^{-4} \dots 10^{2}$), evaluate each with cross-validation ([[08-Cross-Validation-and-Data-Splitting]]), pick the optimum (or the "1-standard-error" rule: simplest model within one SE of the best).
>
> **Probabilistic view (regularizers are priors).** Ridge = MAP estimation under Gaussian prior $w_j \sim \mathcal{N}(0, \tau^2)$; Lasso = MAP under Laplace prior (sharp peak at zero, heavy tails — encoding belief that many weights are *exactly* irrelevant). "Penalizing complexity" literally means "assuming small weights a priori." This Bayesian framing unifies regularization across all of ML.
>
> **Worked example — ridge vs. OLS on collinear features.** Predict house price from `size_m2` and `size_ft2` (= size_m2 × 10.76). OLS: $X^\top X$ is singular (perfect collinearity) — no unique answer; software returns arbitrary huge cancelling coefficients like $+5300, -493$. With ridge ($\lambda = 1$): solution exists, splits mass sensibly between duplicates, predictions stable. Variance eliminated at negligible bias cost — the tradeoff of [[05-Bias-Variance-and-Overfitting]] executed deliberately.
>
> **Worked example — watching lasso select features.** 20 features, but only 3 truly matter. As $\lambda$ grows from 0, plot each coefficient's path: the 17 noise features' paths hit exactly zero early and stay there; the 3 real signals survive longest. At moderate $\lambda$ the model is sparse *and* accurate — it discovered the true support from data alone. Caveat: when features are correlated copies of one real signal, lasso picks one at random; elastic net keeps both.

> [!context] AI Context
> This file derives ridge's closed form and its eigenvalue-shrinkage meaning, explains lasso sparsity via geometry and soft-thresholding, introduces elastic net, connects penalties to Bayesian priors (MAP), and prescribes cross-validated tuning of λ. It's the classical anchor for DL regularization techniques.

---
If you remember one thing: $L_2$ shrinks smoothly and stabilizes; $L_1$ zeroes out and selects; both are priors in disguise; λ must be chosen on validation data, never training data.
