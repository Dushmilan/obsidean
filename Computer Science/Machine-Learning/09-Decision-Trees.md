A decision tree classifies by asking a sequence of yes/no questions about features, exactly the way the game "20 Questions" narrows down an answer. Despite their simplicity, trees are the foundation of the most successful algorithms on tabular data — random forests and gradient boosting ([[10-Boosting]]) — so understanding how they greedily choose splits, why they overfit, and how pruning tames them is essential background for modern practice.

> [!intuition] The Intuition
> Diagnosing a car that won't start: "Does the engine crank? No → check battery. Yes → do you smell fuel? ..." Each question splits the space of possibilities into two smaller, purer sub-problems. Good mechanics ask high-information questions first ("does it crank?" eliminates half of all causes at once); bad ones ask trivia ("is the radio working?"). A decision tree learns from data *which questions split possibilities most cleanly* and in what order.

> [!math] The Math
> **The model.** Internal nodes test a feature ($x_j \leq t$?); branches carry outcomes; leaves carry predictions (majority class for classification, mean value for regression). Prediction = walk from root to leaf. The tree partitions feature space into axis-aligned rectangles; each rectangle (leaf) gets one prediction.
>
> **Choosing splits — entropy and information gain.** Measure node impurity with Shannon entropy:
>
> $$H(t) = -\sum_{c=1}^{K} p_c \log_2 p_c$$
>
> where $p_c$ is the fraction of class-$c$ samples at node $t$. Pure node (all one class): $H = 0$. Two-class coin-flip node: $H = 1$ bit, maximal. For a candidate split into left/right children with fractions $\alpha_L, \alpha_R$ of samples:
>
> $$\text{Gain} = H(\text{parent}) - \big[\alpha_L H(L) + \alpha_R H(R)\big]$$
>
> The greedy algorithm evaluates every feature × every threshold (sort values, consider midpoints), picks maximum Gain, recurses on children. Gini impurity $G(t) = 1 - \sum_c p_c^2$ is a computationally cheaper near-equivalent (no logarithms) and is CART's default; results rarely differ.
>
> **Stopping & overfitting.** Unconstrained growth drives training error to zero — each leaf can become pure by isolating individual points. That's memorization ([[05-Bias-Variance-and-Overfitting]] in its purest form). Controls: `max_depth`, `min_samples_split` (don't split small nodes), `min_samples_leaf` (don't create tiny leaves), `min_impurity_decrease` (split must earn it).
>
> **Pruning.** Cost-complexity pruning formalizes the simplicity tradeoff:
>
> $$J_\gamma(T) = J_{\text{train}}(T) + \gamma\,|T|$$
>
> with $|T|$ the number of leaves. Grow full tree, then increase γ: subtrees collapse when their training-error reduction no longer justifies their leaf count; pick γ by cross-validation.
>
> **Worked example — hand-building a tiny tree.** 10 loan applicants: default (yes/no) by income and debts.
>
> | Income | Debts | Default |
> |--------|-------|---------|
> | low | low | no |
> | low | high | yes |
> | low | high | yes |
> | mid | low | no |
> | mid | low | no |
> | mid | high | yes |
> | high | low | no |
> | high | low | no |
> | high | high | no |
> | high | low | no |
>
> Parent impurity: 4 yes / 6 no → $H = -(0.4\log_2 0.4 + 0.6\log_2 0.6) ≈ 0.971$ bits.
> Candidate "Debts = high?" — high branch: 3y/1n → $H ≈ 0.811$; low branch: 1y/5n → $H ≈ 0.650$. Weighted: $0.4(0.811)+0.6(0.650)=0.714$. **Gain ≈ 0.257.**
> Candidate "Income = high?" — high branch: 0y/4n → $H=0$ (pure!); other: 4y/2n → $H=0.918$. Weighted: $0.4(0)+0.6(0.918)=0.551$. **Gain ≈ 0.420** ← wins.
> Root splits on income. Under "high income": all no — pure leaf. Under rest: 4y/2n, split on debts: high→3y/1n (predict yes), low→1y/5n (predict no). The algorithm discovered "high income is protective" purely from impurity arithmetic — no domain knowledge injected.
>
> **Worked example — regression tree sketch.** Predict apartment price by size. Root sorts sizes, tests thresholds; best split (say 60 m², Gain = largest variance drop) separates studios from family flats. Recursion continues within each side until `min_samples_leaf` stops it. Leaf predicts the mean price of its members; predictions form a staircase function — piecewise-constant, which is why single regression trees predict poorly at extrapolation and get boosted instead.
>
> **Why single trees lose to ensembles:** deep trees = high variance (retrain on slightly different sample → different structure entirely); shallow trees = high bias (can't express interactions). Individually weak, but as forest/boosting ensembles they dominate tabular ML — the subject of [[10-Boosting]]. Advantages trees keep even alone: interpretability (read rules off the paths), zero feature scaling required ([[11-Feature-Engineering-and-Scaling]] explains why), native handling of mixed types and missing values.

> [!context] AI Context
> This file defines tree structure, derives entropy/information-gain/Gini split selection with a fully worked numeric example, covers stopping criteria and cost-complexity pruning, shows the axis-aligned-partition geometry, and sets up the variance story motivating ensembles next.

---
If you remember one thing: trees greedily maximize impurity reduction per split; unconstrained, they memorize (variance!); depth/pruning controls the bias-variance dial; ensembles fix what single trees lack.
