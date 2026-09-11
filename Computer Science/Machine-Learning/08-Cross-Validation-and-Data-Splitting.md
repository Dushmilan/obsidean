Cross-validation is how you get honest estimates of generalization performance from limited data — and honest hyperparameter tuning without burning your test set. The discipline is simple: data you train on cannot be the same data you judge on, and the final test set must be touched exactly once. Nearly every "my model gets 99% accuracy" embarrassment traces back to violating one of these rules.

> [!intuition] The Intuition
> A teacher can't grade students on the exact homework problems they memorized solutions to — you need fresh problems. With only a small problem bank, the clever compromise: rotate through the bank — each round, most problems serve as practice and a different few as the quiz; every problem gets used for both purposes but never both at once. That rotation *is* k-fold cross-validation.

> [!math] The Math
> **The three-way split (the non-negotiable foundation):**
>
> $$\mathcal{D} \longrightarrow \underbrace{\mathcal{D}_{\text{train}}}_{\sim 70\%} + \underbrace{\mathcal{D}_{\text{val}}}_{\sim 15\%} + \underbrace{\mathcal{D}_{\text{test}}}_{\sim 15\%}$$
>
> - **Train** — fit parameters $w$.
> - **Validation** — select among models/hyperparameters $\lambda$. It simulates "new data" *during development*.
> - **Test** — final, untouched estimate of deployed performance. Used once.
>
> Why three sets? Because once you've tuned against validation, its score is optimistic too — selection leaks information. The hierarchy of trust: test > val > train. Rule: if any decision about the model (features, architecture, threshold, λ) used a dataset's answers, that dataset can no longer measure true generalization.
>
> **k-fold cross-validation.** Split training pool into $k$ equal folds. Train $k$ models; each holds out a different fold:
>
> $$CV_k = \frac{1}{k}\sum_{j=1}^{k}\; J\big(h^{(-j)},\; \mathcal{D}_j\big)$$
>
> where $h^{(-j)}$ is the model trained on everything except fold $j$, evaluated on fold $j$. Every point serves as training data $(k{-}1)$ times and validation once. Typical k = 5 or 10:
> - **Variance reduction:** the CV mean averages k correlated estimates → far more stable than a single split.
> - **Cost:** k trainings instead of 1 — expensive with big models (that's why deep learning usually uses a single held-out validation set instead).
>
> **Stratification.** For classification with imbalance, plain random folds can end up with few minority examples in some folds. Stratified k-fold preserves class proportions in every fold — strictly better, near-zero cost, default choice.
>
> **Nested cross-validation** (the fully honest version). Model selection itself needs validation data; if you use the same CV loop for selecting *and* reporting performance, your reported score inherits optimism. Nested CV: inner loop selects hyperparameters per outer-training-split; outer loop reports untouched-fold performance:
>
> $$\text{Outer: } \mathcal{D} = \bigcup_j \mathcal{D}_j,\qquad h_j^* = \underset{h}{\arg\min}\; CV_{\text{inner}}(h;\, \mathcal{D}\setminus\mathcal{D}_j),\qquad \text{score} = \frac{1}{k}\sum_j J(h_j^*; \mathcal{D}_j)$$
>
> Expensive ($k_{\text{outer}} \times k_{\text{inner}}$ fits) but the gold standard for small datasets where every scrap of signal matters.
>
> **Time-series splits.** Random shuffling on temporal data commits **lookahead leakage**: training on 2025 to predict 2024. Walk-forward validation respects chronology — train on months 1–12, validate on month 13; slide forward. Any dataset with time structure (sales, sensor logs, financial series) demands this.
>
> **Worked example — choosing k by hand.** 100 examples, 5-fold CV, accuracy as metric. Folds of 20. Round 1: train on folds 2–5 (80 examples), test fold 1 → 88% correct. Rounds 2–5 give 92%, 84%, 90%, 86%. Mean ≈ 88.0%, standard deviation ≈ 3.0%. Report "88% ± 3%". Compare models by this mean; prefer a simpler model within noise ([[07-Model-Evaluation-and-Metrics]]'s 1-SE logic). Contrast: a single lucky 80/20 split might have shown 94% — pure variance. That instability across hypothetical splits is precisely what averaging removes.
>
> **Worked example — leakage, the silent killer.** Fraud dataset: preprocess by standardizing all features using the *full dataset's* means/stds, then run 10-fold CV → CV says 96%. Correct pipeline — fit scaler inside each fold on train portion only — gives 89%. The 7-point gap was the scaler peeking at test-fold statistics. Worse variants: duplicate rows spanning train/test, target-encoded features computed before splitting, random splits over grouped data (same customer in train and test). Defense: treat preprocessing as part of the pipeline and fit it exclusively within each training fold (sklearn `Pipeline` exists precisely for this).
>
> **Practical defaults worth internalizing:** stratified 5- or 10-fold for tabular classification; single train/val/test split for large deep-learning datasets; group-aware splits when entities repeat (GroupKFold); walk-forward for anything temporal; keep a locked-away test set even during CV experiments — tune on CV, report on test, once.

> [!context] AI Context
> This file defines train/val/test responsibilities, derives k-fold CV and its variance-reduction rationale, covers stratified/grouped/nested/temporal variants, and demonstrates leakage through concrete numeric gaps. It completes the evaluation methodology begun in the previous note.

---
If you remember one thing: performance estimates are only as honest as the separation between the data that influenced the model and the data that judged it — when in doubt, nest your loops.
