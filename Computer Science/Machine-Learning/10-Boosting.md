Boosting converts a sequence of weak learners — models barely better than random guessing — into one strong learner by training each new model to fix the mistakes of the ensemble so far. It is the most consistently winning approach on tabular business data, powering XGBoost and LightGBM in production everywhere. The core insight: instead of averaging many independent models (bagging), make models *sequential and corrective*, each concentrating on what its predecessors got wrong.

> [!intuition] The Intuition
> A student reviewing for an exam keeps an error log. First pass through practice problems: 30% wrong. Second pass focuses *only* on those missed, weighted heavier. Third pass re-focuses on remaining misses. After several passes the hard cases have been drilled to near-perfection while easy ones stay mastered from round one. Each "pass" is a weak learner; the final skill is the weighted combination of all passes. Crucially, later passes see a *re-weighted world* where previous failures loom large.

> [!math] The Math
> **AdaBoost (the original, 1997).** For binary classification with $n$ samples, initial weights $w_i = 1/n$. Round $t$, with current sample weights:
>
> 1. Train weak learner $h_t$ minimizing weighted error $\varepsilon_t = \sum_{i:\, h_t(x_i) \ne y_i} w_i$ (must be $< 0.5$).
> 2. Set its say: $\alpha_t = \frac{1}{2}\ln\frac{1-\varepsilon_t}{\varepsilon_t}$ — small error → loud voice.
> 3. Reweight: correct samples ×$e^{-\alpha_t}$, wrong ×$e^{+\alpha_t}$; renormalize.
>
> Final classifier — a weighted vote:
>
> $$H(x) = \mathrm{sign}\!\Big(\sum_{t=1}^{T}\alpha_t h_t(x)\Big)$$
>
> **Why this actually works:** AdaBoost secretly minimizes exponential loss $J = \sum_i e^{-y_i F(x_i)}$ where $F(x) = \sum_t \alpha_t h_t(x)$ is the running score. Adding each new $(h_t, \alpha_t)$ performs exact coordinate-descent minimization of that loss. Exponential loss punishes confident wrong answers brutally ($e^{+large}$), so late rounds obsess over persistent misfits.
>
> **Gradient boosting (the generalization).** Reframe: at round $t$ we have cumulative model $F_{t-1}(x)$; we want a new tree that reduces any differentiable loss $L$. The trick of genius-level simplicity — fit the new tree $g_t$ not to targets but to **negative gradients** (pseudo-residuals):
>
> $$r_i^{(t)} = -\Big[\frac{\partial L(y_i, F(x_i))}{\partial F(x_i)}\Big]_{F=F_{t-1}}, \qquad g_t \approx r^{(t)}, \qquad F_t(x) = F_{t-1}(x) + \nu\, g_t(x)$$
>
> It's gradient descent ([[03-Gradient-Descent]]) where the "parameter" being stepped is the function itself — hence *gradient* boosting. Squared loss → residuals are literally $y - F$; absolute loss → signs; deviance → class-probability gradients. One framework, every task. Shallow trees (depth 3–6) serve as the weak learners because each only needs local corrections, and shrinkage ν ≈ 0.1 ("learning rate") prevents any single tree from dominating — smaller ν needs more trees but generalizes better.
>
> **XGBoost/LightGBM additions** (why they won): second-order Newton steps using Hessian information; built-in $L_1/L_2$ regularization on leaf weights ([[06-Regularization]]); column/row subsampling (borrowed from forests); sparsity-aware split finding for missing values; histogram-based splits for speed (LightGBM's leaf-wise growth). Conceptually unchanged: sequential residual-fitting with regularization.
>
> **Boosting vs. bagging** (contrast with random forests):
>
> | | Bagging / Random Forest | Boosting |
> |---|---|---|
> | Training | Parallel, independent trees | Sequential, each fixes predecessors |
> | Focus | Equal attention to all samples | Increasingly on hard samples |
> | Bias/variance attack | Variance reduction (averaging) | Bias reduction (iterative correction) |
> | Overfit risk | Low (robust to depth) | Real (too many trees overfit without shrinkage) |
> | Tuning burden | Light | Heavier (ν, depth, n_trees interact) |
>
> **Worked example — AdaBoost by hand, two rounds.** Data: five points $x=1..5$, labels $+,+,-,-,-$; weak learners = single threshold stumps.
> Round 1, weights all 0.2. Best stump: "$x < 2.5 \Rightarrow +$" misclassifies $x_5$: $\varepsilon_1 = 0.2$, so $\alpha_1 = \frac12\ln(4) ≈ 0.693$. Reweight: $x_5$ jumps to $0.2e^{0.693}=0.4$; others drop to $0.2 e^{-0.693}=0.1$. Normalized: $[0.125, 0.125, 0.125, 0.125, 0.5]$.
> Round 2: best stump under these weights: "$x > 4.5 \Rightarrow -$" catches $x_5$ but misses $x_4$: error $= 0.125$, $\alpha_2 = \frac12 \ln(7) ≈ 0.973$ — louder voice, as promised by lower error.
> Ensemble after two rounds scores points as $\alpha_1 h_1 + \alpha_2 h_2$: check $x_4$: $h_1(+):+0.693$, $h_2$ says $+$ too (since $4<4.5$): total positive ✓. All five now classified correctly by just two stumps — including $x_5$, which *no single stump gets right together with* the others. That's the emergence that makes boosting more than the sum of its parts.
>
> **Worked example — reading gradient boosting on squared loss.** One feature, three points $(1, 2), (2, 4), (3, 6.5)$; start $F_0 = \bar y = 4.17$ (constant minimizer). Residuals: $-2.17, -0.17, +2.33$. Tree fitting residuals splits at 1.5/2.5 → leaf means $-2.17, -0.17, +2.33$ (each point alone). With ν=0.5: $F_1(1) = 4.17 - 1.085 = 3.08$, etc. New residuals halve in magnitude; repeat. Watch predictions converge toward data with each round — literally gradient descent in function space, visible in three lines of arithmetic.
>
> **Practical wisdom:** tune `learning_rate` × `n_estimators` jointly (early stopping on validation picks n); keep trees shallow; always use built-in regularization; give it raw-ish tabular features (trees don't need scaling — [[11-Feature-Engineering-and-Scaling]]) but do encode categoricals sensibly. When your dataset is < ~1M rows of mixed-quality tabular features, gradient-boosted trees remain the strongest default in ML.

> [!context] AI Context
> This file derives AdaBoost's reweighting and α formula, proves its exponential-loss descent interpretation, generalizes to gradient boosting via pseudo-residuals (function-space gradient descent), summarizes XGBoost-era improvements, and contrasts bagging vs boosting. Prerequisite context: [[09-Decision-Trees]] for the weak learners themselves.

---
If you remember one thing: boosting = sequential bias reduction — each weak learner fits the ensemble's current errors (exponentially reweighted or via negative gradients), and shallow trees plus slow learning rates turn thousands of stumps into state-of-the-art tabular predictors.
