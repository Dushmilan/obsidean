Machine Learning is programming by example: instead of hand-writing rules that map inputs to outputs, you show an algorithm many input–output pairs (or just raw inputs) and it tunes itself until its own internal rules reproduce the pattern. Arthur Samuel's 1959 definition still holds — "the field of study that gives computers the ability to learn without being explicitly programmed" — and Tom Mitchell's operational version tells you *what* learning means: a program improves its performance measure $T$ on task $P$ with experience $E$.

> [!intuition] The Intuition
> Traditional programming is teaching a child every single chess move in advance; machine learning is letting them play ten thousand games and improve from each one. Or: a spam filter written with `if` statements breaks the moment spammers change one word ("fr*ee v1agra"), while a spam filter trained on examples notices the shift on its own because it learned *what spam looks like statistically*, not a brittle list of keywords.

> [!math] The Math
> **The supervised learning setup.** You assume there is some unknown function $f$ mapping inputs to outputs, and you observe noisy samples of it:
>
> $$\mathcal{D} = \{(x^{(1)}, y^{(1)}), (x^{(2)}, y^{(2)}), \dots, (x^{(n)}, y^{(n)})\}, \quad y^{(i)} = f(x^{(i)}) + \varepsilon^{(i)}$$
>
> Your job: pick a hypothesis space $\mathcal{H}$ (a family of candidate functions) and find the $h \in \mathcal{H}$ that best generalizes — predicts well on data *not* in $\mathcal{D}$. Formally we minimize expected risk:
>
> $$R(h) = \mathbb{E}_{(x,y) \sim P(x,y)}[\ell(h(x), y)]$$
>
> but since $P$ is unknown, we minimize *empirical risk* — the average loss over training samples — and hope (via [[05-Bias-Variance-and-Overfitting]]) the gap between them is small.
>
> $$\hat{h} = \arg\min_{h \in \mathcal{H}} \frac{1}{n}\sum_{i=1}^{n} \ell(h(x^{(i)}), y^{(i)})$$
>
> Every supervised algorithm you'll ever meet decomposes into four design choices:
>
> | Component | Question it answers | Examples |
> |-----------|--------------------|----------|
> | **Model** $\mathcal{H}$ | What family of functions? | Linear, trees, neural nets |
> | **Loss** $\ell$ | What does "wrong" cost? | MSE, cross-entropy |
> | **Optimizer** | How to search for the best parameters? | Gradient descent, tree-greedy split |
> | **Regularization** | Which candidates do we prefer when ties? | Penalties, depth limits, priors |
>
> **The three paradigms:**
>
> | Paradigm | Training signal | Task examples |
> |----------|----------------|---------------|
> | **Supervised** | Labeled pairs $(x, y)$ | Price prediction, spam vs. ham, tumor classification |
> | **Unsupervised** | No labels — find structure | Clustering, dimensionality reduction |
> | **Reinforcement** | Delayed reward from actions | Game playing, robotics |
>
> Regression vs. classification is about the *type* of output: continuous ($y \in \mathbb{R}$, e.g., house price) vs. categorical ($y \in \{1,\dots,K\}$, e.g., digit 0–9). The same machinery applies; only the loss changes ([[04-Loss-Functions]] in DL covers this deeply).
>
> **Worked example — spotting the components:** "Predict tomorrow's electricity demand from temperature, day-of-week, and past demand." Task $T$: predict demand. Experience $E$: historical daily records. Performance $P$: mean absolute error on held-out future days. Model choice: start linear ([[02-Linear-Regression]]). Loss: squared error because demand errors are roughly symmetric and Gaussian-ish. Optimizer: gradient descent or closed form. This decomposition habit — naming all four components before coding — is what separates practitioners who debug models from those who merely run them.
>
> **Worked example — why not program the rules?** Suppose you try rule-based house pricing: price = 5000 × bedrooms + 200 × sqm + ... You'll tune constants forever and miss interactions (a bedroom matters more in the city center). With ML, you fit $w_1(\text{bedrooms}) + w_2(\text{sqm}) + w_3(\text{bedrooms} \times \text{city-center}) + b$ from data and the coefficients *discover themselves*. The catch: you've traded writing rules for curating data, and your model inherits every bias your data contains.
>
> **Parametric vs. non-parametric:** Parametric models (linear regression, neural nets) fix the number of parameters in advance — learning means setting values. Non-parametric models (k-NN, trees) grow complexity with data — learning can mean memorizing structure. This distinction drives the bias-variance behavior you'll meet in [[05-Bias-Variance-and-Overfitting]].

> [!context] AI Context
> This file defines ML operationally, sets up the notation $(x^{(i)}, y^{(i)})$, empirical risk, and the model/loss/optimizer/regularization decomposition used across this whole sub-vault. It's the vocabulary note — everything later refers back to it.

---
If you remember one thing: ML is *empirical risk minimization under a chosen hypothesis class* — every famous algorithm is just a different answer to those four component questions.
