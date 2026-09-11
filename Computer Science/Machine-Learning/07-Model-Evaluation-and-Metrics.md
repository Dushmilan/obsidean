Choosing the right metric is choosing what your model optimizes its design decisions toward — pick wrongly and you'll confidently ship a useless model. Accuracy, precision, recall, F1, ROC-AUC, and the confusion matrix form the core toolkit for classification; regression has its own set. The unifying principle: a single number never captures model quality — you must know *which errors hurt* in your application.

> [!intuition] The Intuition
> A hospital screening test and a spam filter should not be judged by the same standard. For cancer screening, a missed tumor (false negative) is catastrophic while a false alarm costs one extra biopsy — so maximize *recall*. For spam filtering, sending a real job offer to trash (false positive) is worse than letting ten spam emails through — so prioritize *precision*. Accuracy asks "how often right overall?" but these applications care about specific quadrants of right/wrong — that's why the confusion matrix comes first.

> [!math] The Math
> **The confusion matrix** (binary classification):
>
> $$\begin{array}{c|cc} & \text{Predicted } + & \text{Predicted } - \\ \hline \text{Actual } + & TP & FN \\ \text{Actual } - & FP & TN \end{array}$$
>
> Every metric is an algebraic combination of these four numbers:
>
> $$\text{Accuracy} = \frac{TP+TN}{TP+TN+FP+FN} \qquad\quad \text{Precision} = \frac{TP}{TP+FP}$$
>
> $$\text{Recall (sensitivity)} = \frac{TP}{TP+FN} \qquad\quad \text{Specificity} = \frac{TN}{TN+FP}$$
>
> - **Precision** answers: "of things I flagged positive, how many really were?" — measures false-alarm rate.
> - **Recall** answers: "of all real positives, how many did I catch?" — measures miss rate.
>
> They trade off against each other via the decision threshold: lower the threshold → catch more positives (recall↑) but flag more junk (precision↓).
>
> **The accuracy trap.** Class imbalance destroys accuracy's meaning. 99% of transactions are legitimate: a model predicting "not fraud" always scores 99% accuracy — and catches zero fraud. F1 patches this by harmonically averaging precision and recall:
>
> $$F_1 = \frac{2}{\frac{1}{P}+\frac{1}{R}} = \frac{2PR}{P+R}$$
>
> Harmonic mean punishes imbalance between P and R: $P=1.0, R=0.01$ gives arithmetic mean 0.505 but F1 ≈ 0.0198. A model must do *both* decently to score well. Generalization for weighted cases: $F_\beta = (1+\beta^2)\frac{PR}{\beta^2 P + R}$ — β > 1 weights recall more (use β=2 for screening).
>
> **ROC curve and AUC.** Sweep the classification threshold from strict to loose; at each point plot True Positive Rate ($= $ recall) vs. False Positive Rate ($= FP/(FP+TN)$). The **ROC curve** traces this path; **AUC** is its area:
>
> $$\text{AUC} = P(\text{model scores random } + \text{ higher than random } -)$$
>
> — a threshold-free interpretation as ranking quality. Baseline random guessing gives AUC 0.5 (diagonal); perfect separation gives 1.0. Use ROC-AUC when you care about ranking across all thresholds and classes are roughly balanced; when classes are heavily imbalanced, Precision–Recall curves are more informative because FPR barely moves when negatives dominate.
>
> **Regression metrics:**
>
> $$\text{MAE} = \frac{1}{N}\sum|y_i - \hat y_i| \qquad \text{RMSE} = \sqrt{\tfrac{1}{N}\sum(y_i - \hat y_i)^2} \qquad R^2 = 1 - \frac{\sum(y_i-\hat y_i)^2}{\sum(y_i - \bar y)^2}$$
>
> RMSE penalizes large errors quadratically (matches MSE-trained models); MAE is robust to outliers and interpretable in target units ("off by \$4,300 on average"). $R^2$ compares your model against the dumb baseline "predict the mean": 0 means no better than baseline, 1 is perfect.
>
> **Worked example — full metric computation.** Model flags 100 transactions; 80 were truly fraudulent, 20 were legit customers falsely accused. Meanwhile 50 frauds slipped through (predicted legit).
> Matrix: TP=80, FP=20, FN=50. Suppose total dataset 10,000 with TN=9850.
> - Accuracy = (80+9850)/10000 = 99.3% — looks superb!
> - Precision = 80/100 = 80% — flagged accounts are usually truly fraudulent.
> - Recall = 80/130 ≈ 61.5% — nearly 4 in 10 frauds escaped.
> - F1 = 2(0.8)(0.615)/(0.8+0.615) ≈ 69.6%.
> Now ask which number matters: if each missed fraud costs \$5000 and each false accusation costs \$50 in support time, misses cost ~\$250k vs false alarms \$1k — recall is the metric to optimize, and "99.3% accurate" was pure theater.
>
> **Worked example — threshold surgery.** Same probabilities, two deployments. Cancer screening: move threshold from 0.5 down to 0.15 — patients above 15% risk get biopsied. Recall jumps 70%→95%, precision falls 60%→35% (more unnecessary biopsies — acceptable). Spam filter: raise threshold to 0.9 — only near-certain spam gets trashed. Precision 95%, recall drops to 55% (some spam survives in inbox — acceptable). One trained model, different operating points; the threshold is a business decision, not part of the model.

> [!pitfall] Common pitfalls
> computing metrics on training data (meaningless — see [[05-Bias-Variance-and-Overfitting]]); tuning hyperparameters on the test set (leaks test information — see [[08-Cross-Validation-and-Data-Splitting]]); comparing AUC across datasets with different class ratios; ignoring calibration (a model can rank perfectly yet output garbage probabilities — check reliability curves before using outputs as probabilities).

> [!context] AI Context
> This file defines the confusion matrix and derives accuracy/precision/recall/specificity/Fβ/ROC-AUC plus regression metrics, demonstrates the accuracy trap under class imbalance, shows threshold tuning as deployment-level control, and lists evaluation pitfalls that motivate honest data splitting next.

---
If you remember one thing: metrics encode error costs — pick the quadrant that hurts most in your application, then choose precision, recall, or F1 accordingly; never trust accuracy alone on imbalanced data.
