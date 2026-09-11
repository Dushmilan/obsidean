Logistic regression has a misleading name — it's a *classification* algorithm, not a regression one. It takes the linear model you know from [[02-Linear-Regression]] and squashes it through the sigmoid function to output probabilities, then trains by maximizing likelihood with cross-entropy loss. It remains the first serious baseline for any classification problem, and its machinery (sigmoid, log-loss, decision boundaries) carries over verbatim into neural networks.

> [!intuition] The Intuition
> Linear regression answers "how much?"; logistic regression answers "how likely?" Imagine a dimmer switch instead of a light switch: instead of jumping 0/1, you get a smooth dial between "definitely no" and "definitely yes." The sigmoid is that dial — extreme inputs slam it to either end, inputs near the boundary hover around uncertainty. The model draws a straight line through feature space; everything on one side leans toward class 1, on the other toward class 0, and confidence grows with distance from the line.

> [!math] The Math
> **Model.** Apply sigmoid to the linear score $z = w^\top x + b$:
>
> $$\sigma(z) = \frac{1}{1 + e^{-z}}, \qquad \hat{p} = \sigma(w^\top x + b) = P(y=1 \mid x; w)$$
>
> Sigmoid properties worth memorizing: $\sigma(0) = 0.5$, $\sigma(-z) = 1 - \sigma(z)$ (symmetry), and for large $|z|$ it saturates to 0/1. Its derivative has the beautiful form:
>
> $$\sigma'(z) = \sigma(z)(1 - \sigma(z))$$
>
> — which makes backpropagation clean and also reveals the **vanishing gradient problem**: when $z$ is very negative or positive, both factors approach zero, so gradients die in saturated neurons (this exact issue reappears in [[02-Activation-Functions]]).
>
> **Why can't we just use MSE on $\sigma$?** Because with MSE + sigmoid the loss becomes *non-convex* in $w$ — gradient descent can get stuck. Cross-entropy fixes this: composed with sigmoid, it's convex, and the gradient takes a shockingly clean form.
>
> **Loss via maximum likelihood.** One training example contributes:
>
> $$p(y \mid x; w) = \hat{p}^{\,y}(1-\hat{p})^{\,1-y}$$
>
> Take negative log across the dataset:
>
> $$J(w) = -\frac{1}{N}\sum_{i=1}^{N}\Big[\, y^{(i)}\log \hat{p}^{(i)} + (1-y^{(i)})\log(1-\hat{p}^{(i)}) \,\Big]$$
>
> This is binary **cross-entropy**. Read it as a strict teacher: if $y=1$ but the model says $\hat{p}=0.01$, the penalty $-\log 0.01 \approx 4.6$; if the model says $0.9999$, penalty $\approx 0.0001$. Confidently wrong hurts enormously — exactly the behavior you want.
>
> **The beautiful gradient.** For a single example, using $\hat{p} - y$ as shorthand:
>
> $$\frac{\partial J}{\partial w} = (\hat{p} - y)\,x, \qquad \text{update: } w \leftarrow w - \alpha(\hat{p}-y)x$$
>
> Compare with linear regression's update $w \leftarrow w - \alpha(\hat{y}-y)x$ — *identical form*. The $(\text{prediction} - \text{truth})$ error signal driving the update is universal in ML.
>
> **Decision boundary.** The model predicts class 1 iff $\hat{p} > 0.5$, i.e., $w^\top x + b > 0$. So the boundary is the hyperplane $w^\top x + b = 0$ — linear in the features. Non-linear boundaries require feature engineering ([[11-Feature-Engineering-and-Scaling]]): add $x_1^2, x_1x_2$ terms and the boundary becomes curved while remaining linear *in the expanded features*.
>
> **Worked example — exam pass/fail.** Data: hours studied $x$ vs. pass ($y=1$) or fail ($y=0$). Suppose training converges to $\hat{p} = \sigma(1.5x - 5)$. Questions this model answers:
> - Probability of passing after 4 hours: $z = 1.5(4)-5 = 1$, $\hat p = \sigma(1) \approx 0.731$.
> - The 50% point ("decision boundary"): $1.5x - 5 = 0 \Rightarrow x \approx 3.33$ hours.
> - Odds interpretation: since $\log\frac{\hat p}{1-\hat p} = 1.5x - 5$, each extra hour multiplies odds of passing by $e^{1.5} \approx 4.48$. Logistic regression coefficients are *log-odds effects* — this is why it dominates fields like medicine and credit scoring where odds ratios are the language of inference.
> - Confidence check at 10 hours: $z = 10$, $\hat p \approx 0.99995$ — saturation means "very sure," and note how the gradient there is nearly zero: this example barely moves weights during further training.
>
> **Worked example — hand computation of one gradient step.** Single example $x = 2$, $y = 1$; current $w = 0$, $b = 0$, learning rate $\alpha = 1$.
> Forward: $z = 0$, $\hat p = 0.5$. Loss: $-\log 0.5 \approx 0.693$.
> Gradients: $\partial_w J = (\hat p - y)x = (-0.5)(2) = -1$; $\partial_b J = -0.5$.
> Update: $w = 0 - (1)(-1) = 1$; $b = 0.5$.
> Second pass: $z = 1(2)+0.5 = 2.5$, $\hat p = \sigma(2.5) \approx 0.924$, loss dropped $0.693 \to 0.079$. The model moved decisively toward "predict pass" because truth was pass and it was initially clueless — gradient descent working as intended.
>
> **Multiclass extension:** two standard routes. *One-vs-rest*: train $K$ independent binary classifiers, predict the most confident. *Softmax regression* (multinomial): generalize sigmoid to output a full probability distribution — this is exactly what neural nets use in their final layer ([[04-Loss-Functions]] derives softmax cross-entropy).

> [!context] AI Context
> This file introduces sigmoid, derives cross-entropy from maximum likelihood, proves the clean gradient $(\hat p - y)x$, interprets coefficients as log-odds, and sets up decision boundaries and multiclass extensions. It's the conceptual bridge from classical ML into neural network classification.

---
If you remember one thing: logistic regression = linear model + sigmoid + cross-entropy; convexity comes free from that pairing, and the update rule looks identical to linear regression's.
