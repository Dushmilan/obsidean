The loss function is the definition of success your optimizer chases — every gradient computed by backpropagation ([[03-Forward-Pass-and-Backpropagation]]) points "downhill" relative to *this one scalar*. Choose a loss misaligned with your actual goal and you will efficiently optimize the wrong thing. This note covers MSE, MAE, Huber, binary/multiclass cross-entropy, and the maximum-likelihood principle that unifies them all.

> [!intuition] The Intuition
> A loss is a scoring rubric. Squared error is a strict judge who punishes blunders quadratically — one catastrophic miss costs as much as four moderate ones. Absolute error judges linearly — tolerant of the occasional disaster. Cross-entropy is a probability teacher: it doesn't just want the right answer, it wants *justified confidence*, and it savagely punishes confident wrongness. Picking the rubric is picking what "better" means — do it before training, not after.

> [!math] The Math
> **Regression losses.**
>
> $$L_{\text{MSE}} = \frac{1}{N}\sum (y_i - \hat y_i)^2 \qquad L_{\text{MAE}} = \frac{1}{N}\sum |y_i - \hat y_i| \qquad L_{\text{Huber}} = \begin{cases} \frac{1}{2}(y-\hat y)^2 & |e| \leq \delta \\ \delta(|e| - \tfrac{\delta}{2}) & \text{else} \end{cases}$$
>
> - **MSE** — smooth, differentiable everywhere, strong gradient on large errors (fast correction); but quadratic sensitivity means outliers dominate training.
> - **MAE** — outlier-robust; median-seeking rather than mean-seeking; gradient has constant magnitude $\pm 1$ (no urgency signal near zero → slower late-stage convergence).
> - **Huber** — quadratic near zero (smooth convergence), linear beyond $\delta$ (outlier containment): the principled compromise.
>
> **Maximum likelihood: where losses come from.** Every canonical loss is negative log-likelihood of some output distribution:
>
> | Noise/output model | Likelihood $p(y|\hat y)$ | Negative log-likelihood |
> |---|---|---|
> | Gaussian $\mathcal{N}(\hat y, 1)$ | $\propto e^{-\frac12(y-\hat y)^2}$ | **MSE** |
> | Laplace $∝ e^{-|y-\hat y|}$ | heavy tails | **MAE** |
> | Bernoulli($\hat p$) for $y∈\{0,1\}$ | $\hat p^{\,y}(1-\hat p)^{1-y}$ | **binary cross-entropy** |
> | Categorical($\hat{\mathbf p}$), K classes | $\prod_k \hat p_k^{\,y_k}$ | **categorical cross-entropy** |
>
> Deriving the last row: take logs of likelihood summed over data:
>
> $$J = -\frac{1}{N}\sum_{i=1}^{N}\sum_{k=1}^{K} y_{i,k}\log \hat p_{i,k}$$
>
> where $\mathbf y_i$ is one-hot truth. If truth is class 3 and model says $\hat p_3 = 0.001$: loss $= -\log(0.001) = 6.9$ nats. Confidently wrong → effectively infinite penalty as $\hat p_{\text{true}} \to 0$. The loss literally measures surprise in bits/nats — information theory's self-information, connecting to your Information-Theory-formatted notes across this vault.
>
> **Softmax** produces the distribution cross-entropy consumes (from logits $\mathbf z$):
>
> $$\hat p_k = \mathrm{softmax}(\mathbf z)_k = \frac{e^{z_k}}{\sum_j e^{z_j}}, \qquad \sum_k \hat p_k = 1$$
>
> Exponentials enforce positivity; normalization enforces a distribution; the max-logit dominates exponentially ("soft argmax").
>
> **The magical combined gradient.** Differentiate cross-entropy through softmax w.r.t. logits — the messy terms cancel:
>
> $$\frac{\partial J}{\partial z_k} = \hat p_k - y_k$$
>
> Exactly the logistic-regression form from [[Computer Science/Machine-Learning/04-Logistic-Regression]], generalized to K classes. This cancellation is why everyone trains softmax+CE as one fused unit — and why implementations use `log_softmax` + `nll_loss` internally (computing softmax separately then logging it loses precision when probabilities underflow).
>
> **Numerical stability — non-negotiable details:** sigmoid via $\log(\sigma(z))$ must be computed as $-\mathrm{softplus}(-z)$; softmax subtracts the max logit first ($e^{z_k - z_{\max}}$ — identical result, no overflow). Frameworks bake this in; hand-rolled versions break at half-precision or extreme logits.
>
> **Worked example — CE vs MSE for classification, numerically.** Binary case, true label 1, model outputs:
> Model A: $\hat p = 0.6$. Model B: $\hat p = 0.9$.
> Cross-entropy: A $= -\ln 0.6 = 0.511$; B $= -\ln 0.9 = 0.105$. B rewarded strongly.
> MSE on probabilities: A $= (0.4)^2 = 0.16$; B $= (0.1)^2 = 0.01$ — similar ordering here, but now the failure case:
> Model C: $\hat p = 0.02$ (confidently wrong!). CE $= -\ln 0.02 = 3.912$ — enormous, gradient screams. MSE $= (0.98)^2 ≈ 0.960$ — barely worse than A's 0.16? No—worse, but *bounded and tame*: with MSE+sigmoid the gradients also vanish exactly when the model is confidently wrong (sigmoid saturated), so the worst errors generate the weakest learning signal. CE's gradient $\hat p - y = -0.98$: full-strength correction precisely when needed most. That asymmetry is why classification uses CE, full stop.
>
> **Worked example — reading a loss curve like a diagnostician.** Training loss 2.30 stuck (10-class problem): $\ln 10 ≈ 2.303$ — model predicts uniform distribution; learning signal isn't propagating (suspect LR, initialization, or label pipeline bug). Training loss *below* $\approx 0.05$ while validation rises: memorization onset ([[Computer Science/Machine-Learning/05-Bias-Variance-and-Overfitting]]) — early stopping ([[07-Regularization-in-DL]]). Loss NaNs: exploding gradients/LR too high/numerical instability. Loss curves encode diagnoses; knowing each loss's floor value ($\ln K$ for uniform guessing over K classes) gives you absolute reference points.
>
> **Loss engineering in practice:** class imbalance → weighted CE ($w_k$ inversely proportional to class frequency) or focal loss $(1-\hat p_t)^\gamma$·CE which down-weights easy examples; label smoothing ($y \to 0.9/0.1$ mix) prevents pathological overconfidence and improves calibration; multi-task models sum per-task losses with balancing weights — an active tuning art.

> [!context] AI Context
> This file derives regression losses from noise assumptions, presents the MLE table unifying all canonical losses, works the softmax-CE gradient cancellation, demonstrates why CE beats MSE for classification via saturation analysis, and teaches loss-curve diagnostics plus imbalance/smoothing variants. Prerequisite for optimizers and training-practice notes.

---
If you remember one thing: pick the loss whose implicit probabilistic model matches your data (Gaussian→MSE, Bernoulli/Categorical→CE); cross-entropy + softmax yields the clean $(\hat p - y)$ gradient and punishes confident wrongness exactly when correction matters.
