Feature engineering is the craft of transforming raw data into representations that make patterns easy for models to find; scaling is its most universally-required piece. The old slogan — "better data beats fancier algorithms" — survives every benchmark cycle. This note covers the standard transformations (encoding, scaling, target transformation), why each works, and the leakage traps that make naive preprocessing the most common silent bug in applied ML.

> [!intuition] The Intuition
> A model is like a student with perfect arithmetic but no world knowledge. Hand it "city = Tokyo, temp = 290" and it can't connect them. You are the translator: turn cities into comparable coordinates, temperatures into centered numbers, dates into "days since holiday," and suddenly the arithmetic finds real structure. Scaling is seating everyone at the same-sized desk: without it, a feature measured in millimeters shouts down a feature measured in kilometers during gradient descent.

> [!math] The Math
> **Scaling transforms** (applied per feature column):
>
> $$\text{Standardize: } x' = \frac{x - \mu}{\sigma} \qquad\qquad \text{Min-max: } x' = \frac{x - \min}{\max - \min} \qquad\qquad \text{Robust: } x' = \frac{x - \text{median}}{\text{IQR}}$$
>
> Standardized features have mean 0, std 1 — the default for linear models, logistic regression, SVMs, neural nets. Min-max squeezes into $[0,1]$ — good when you need bounded inputs (image pixels), terrible with heavy outliers (one extreme value compresses everything else). Robust scaling uses median/IQR — built for outlier-ridden columns.
>
> *Why scaling matters mathematically:* recall from [[03-Gradient-Descent]] that convergence speed depends on the loss Hessian's condition number $\kappa = \lambda_{\max}/\lambda_{\min}$. Features at wildly different scales create elongated elliptical loss bowls — $\kappa$ explodes and GD zigzags. Standardization makes the bowl round-ish ($\kappa \approx 1$), so descent marches straight to the minimum. Distance-based models (k-NN, k-means) have an even harder dependency: Euclidean distance is scale-dominated — unscaled, income (0–200,000) completely drowns age (0–100). Trees are the exception: splits test one feature at a time ($x_j \le t$), invariant to any monotone rescaling — which is exactly why XGBoost pipelines skip scalers.
>
> **Categorical encoding.**
> - *One-hot:* category → vector of 0/1 dummy columns; drop one to avoid collinearity in linear models. Correct for nominal categories, but explodes dimensionality on high-cardinality columns.
> - *Ordinal mapping:* only when a true order exists ("small < medium < large").
> - *Target encoding:* replace category with mean of target over that category — powerful for high cardinality, extremely leak-prone (see worked example below).
> - Rare-category handling: bucket infrequent levels into "other" to avoid overfitting single observations.
>
> **Nonlinear feature maps.** Linear models + engineered features = curved decision boundaries ([[04-Logistic-Regression]]'s boundary discussion): add $x^2$ terms for parabolic effects, interaction terms $x_i x_j$ for synergy effects, $\log(x)$ for multiplicative relationships. Basis expansions predate deep learning — neural nets ([[Computer Science/Deep-Learning/01-Perceptrons-to-Neural-Networks]]) simply learn these transforms automatically instead of requiring you to hand-pick them.
>
> **Target transformation.** Skewed positive targets (income, prices) violate Gaussian-noise assumptions behind squared loss ([[02-Linear-Regression]]): train on $\log(y)$, predict $\hat z$, invert with $\exp(\hat z)$. Bonus: log-space errors become relative errors, usually matching business meaning better.
>
> **Worked example — scaling rescues gradient descent, numerically.** Features: income ∈ [20k, 200k], age ∈ [18, 80]. True relation involves both. With raw scales, $X^\top X$ eigenvalue ratio ≈ $(10^5/10)^2 = 10^8$ — GD would need ~$\kappa$ iterations-scale steps to converge. After standardization both columns have unit variance, $\kappa$ drops to near 1–10, convergence takes dozens of epochs instead of millions. Same model, same data, same optimizer — the only change is representation.
>
> **Worked example — target-encoding leakage, quantified.** 1000 rows; category "store_id" has 500 stores. Naive target encoding computed on the *full dataset*: store 7 appears once, with target 1 → encoded as 1.0. Any row from store 7 carries "mean target = 1.0" — literally its own answer inside its own feature. Cross-validated accuracy: inflated 91%. Fix — compute encodings within training folds only (or smoothed: $\frac{n_c \bar y_c + m \cdot \bar y}{n_c + m}$ shrinking toward global mean): honest accuracy 79%. Twelve phantom points of accuracy, zero real skill — caught only by pipeline discipline ([[08-Cross-Validation-and-Data-Splitting]]).
>
> **Worked example — datetime decomposition.** Raw timestamp "2026-03-15 18:42" is nearly useless numerically. Decompose: hour-of-day → cyclical encoding $(\sin 2\pi h/24, \cos 2\pi h/24)$ so that 23:00 neighbors 00:00; is_weekend flag; days-since-last-purchase (recency). Each derived feature gives the model a direct handle on a real mechanism (commute-time traffic, weekly cycles, churn recency) — this is engineering knowledge becoming representational advantage.
>
> **Ordering rule for pipelines:** split first, then fit preprocessing *on train only*, then transform val/test with those fitted parameters — never fit on full data before splitting. sklearn `Pipeline` + `ColumnTransformer` enforce this mechanically and prevent every leakage bug above by construction.

> [!context] AI Context
> This file covers standardize/min-max/robust scaling with the condition-number justification, categorical encoding strategies including target encoding's leakage math, nonlinear basis expansions, log-target transformation, and pipeline ordering rules. It closes the classical ML track; the Deep Learning track assumes these instincts as baseline hygiene.

---
If you remember one thing: fit preprocessing on training folds only, scale for anything gradient- or distance-based (skip it for trees), encode categories with cardinality-awareness, and remember that representation quality bounds what any algorithm can extract.
