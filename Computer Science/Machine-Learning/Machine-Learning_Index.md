Machine Learning is the discipline of getting computers to learn patterns from data instead of being explicitly programmed with rules. This sub-vault covers the full classical ML stack: the mathematical foundations (regression, optimization, generalization theory) and the workhorse algorithms that powered the field before deep learning — and that still win on tabular data today.

**The Intuition:** Think of learning ML like learning to cook. The foundations are knife skills and heat control (linear models, gradient descent, evaluation) — boring to practice, but every dish afterward depends on them. The classical algorithms are recipes: decision trees and boosting are the reliable crowd-pleasers you'll actually cook again and again. Feature engineering is mise en place — prepping ingredients well matters more than fancy technique.

**The Math:**

| Track | Notes | Focus |
|-------|-------|-------|
| Foundations | [[01-What-is-Machine-Learning]] → [[02-Linear-Regression]] → [[03-Gradient-Descent]] | The supervised learning setup, least squares, iterative optimization |
| Generalization | [[04-Logistic-Regression]] → [[05-Bias-Variance-and-Overfitting]] → [[06-Regularization]] → [[07-Model-Evaluation-and-Metrics]] → [[08-Cross-Validation-and-Data-Splitting]] | Classification, why models fail out-of-sample, and how we measure success honestly |
| Classical Algorithms | [[09-Decision-Trees]] → [[10-Boosting]] → [[11-Feature-Engineering-and-Scaling]] | Tree-based ensembles (the tabular-data champions) and data preparation |

**Learning Path:** Read 01–03 in order — they form a single story (model → loss → optimizer). 04–08 can be read as one block on generalization. 09–11 are self-contained algorithm deep dives.

**Prerequisites:** Linear algebra (matrices, vectors), basic calculus (derivatives, chain rule), probability fundamentals ([[Stats/Probability/Probability_Index]]), Python.

**Continues into:** Deep Learning — [[Computer Science/Deep-Learning/Deep-Learning_Index]]

**AI Context:** This file is the entry point for the Machine Learning notes. It maps the three tracks (foundations, generalization, classical algorithms), states prerequisites, and links every note in reading order. Use it to navigate or to check what's been covered before asking for new material.

---
**Key References:** [An Introduction to Statistical Learning](https://www.statlearning.com/) — James, Witten, Hastie, Tibshirani · [Pattern Recognition and Machine Learning](https://www.microsoft.com/en-us/research/publication/pattern-recognition-machine-learning/) — Bishop

---

**Up:** [[Vault-Index]]
