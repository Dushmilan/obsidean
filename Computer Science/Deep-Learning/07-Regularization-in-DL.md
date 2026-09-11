Deep networks have enough capacity to memorize their entire training set — millions of parameters happily storing label noise. Regularization in DL is the collection of techniques that force these networks to spend capacity on *patterns* instead of memorization: dropout randomly disables neurons, weight decay shrinks them, early stopping quits while generalization still improves, and data augmentation multiplies the effective dataset. All four manage the bias-variance balance from [[Computer Science/Machine-Learning/05-Bias-Variance-and-Overfitting]] at neural scale.

> [!intuition] The Intuition
> A basketball team that knows one star player will carry every game falls apart when he's injured. Dropout is the coach randomly benching players each practice: nobody can specialize into a single critical dependency, so the team develops redundant, robust plays. Weight decay is a budget constraint — fancy complicated strategies must justify their complexity. Early stopping is quitting practice at peak form instead of over-drilling until you pick up bad habits. Augmentation is practicing in wind, rain, and on bad courts so real games feel easy.

> [!math] The Math
> **Dropout.** During training, each unit is kept with probability $p$ (typically 0.5 hidden layers):
>
> $$\tilde a_i = \frac{m_i}{p}\, a_i, \qquad m_i \sim \mathrm{Bernoulli}(p)$$
>
> (the $1/p$ "inverted" scaling keeps expected activation constant — inference needs no adjustment). Why it regularizes:
> 1. **No co-adaptation** — no neuron can rely on a specific partner existing; features must be independently useful.
> 2. **Ensemble interpretation** — each mini-batch trains a different thinned subnetwork; sharing weights across subnets means one training run approximates averaging $2^{\text{units}}$ models.
> 3. **Noise injection** — variance added to activations ≈ multiplicative noise, smoothing the loss surface.
>
> Apply to *inputs* lightly ($p \approx 0.1$) and hidden layers more heavily; skip on the final logits layer. Disable entirely at inference (`model.eval()`) — otherwise predictions become stochastic garbage.
>
> **Weight decay ($L_2$).** Add $\frac{\lambda}{2}\|w\|^2$ to loss; gradient step becomes:
>
> $$w_t = (1 - \alpha\lambda)\, w_{t-1} - \alpha\,\nabla_w J$$
>
> — weights shrink multiplicatively each step before the data gradient corrects them ([[Computer Science/Machine-Learning/06-Regularization]] derives this). Deep-learning specifics: decay only *weights*, not biases/norm parameters (they don't affect output scale); prefer **AdamW's decoupled decay** when using Adam ([[05-Optimizers]]) — entangled $L_2$ under Adam penalizes high-gradient parameters unevenly.
>
> **Early stopping.** Validation error is U-shaped even while training error falls monotonically. Keep the parameter snapshot at validation minimum:
>
> $$w^* = w_{t^*}, \qquad t^* = \arg\min_t J_{\text{val}}(w_t)$$
>
> with "patience" (stop after k evaluations without improvement). It doubles as free regularization — limiting effective model complexity by truncating training — and as your main defense against wasting GPU-hours.
>
> **Data augmentation.** Construct label-preserving transformations $(x, y) \to (T(x), y)$ and train on originals + transforms: images — flips/crops/color jitter/mixup ($\lambda x_i + (1-\lambda)x_j$, soft labels mixed identically); audio — time-stretch/pitch-shift/noise; text — paraphrase/back-translation. Mathematically it encodes *invariance priors* directly into the input distribution — telling the network which variations are meaningless before it wastes capacity discovering them.
>
> **Worked example — dropout's ensemble arithmetic.** Single layer, two units, ideal feature = average of both ($a_1 = a_2 = v$), p=0.5. With inverted dropout, expected output: $\mathbb{E}[\tilde a_1 + \tilde a_2] = 2v$ preserved ✓. But any single training pass sees one of: $(2v, 0), (0, 2v), (v, v)$-scaled variants... The downstream neuron must find weights working across all patterns → it learns distributed reliance rather than latching onto whichever unit currently fires harder. At test time both units active deterministically: prediction averages what training explored stochastically — literally ensemble-bagged behavior from one network.
>
> **Worked example — early stopping catching overfitting live.** Training curve (typical): epoch 10 val-loss 0.42, epoch 20: 0.35, epoch 30: 0.33 ← best, epoch 40: 0.34, 50: 0.37, 60: 0.41 (train-loss still falling: 0.30→0.18→0.09). Patience=10 restores epoch 30 weights. The gap between train (0.09!) and restored-val (0.33) quantifies memorized-but-not-generalized capacity. Without early stopping you'd ship the epoch-60 model: 24% worse where it matters.
>
> **Worked example — augmentation teaching invariance.** Cat photo rotated 5°: same label. Before augmentation the network may allocate capacity to "rotation-of-ears" as predictive (it correlates with this training set's camera positions); after showing thousands of rotation-pairs, that correlation breaks — capacity redirects to fur texture/face geometry. Mixup goes further: blend cat+dog pixels 70/30 with soft label 0.7/0.3 — decision boundaries become linear between examples, dramatically improving calibration and robustness to label noise.
>
> **Combining techniques (standard recipes):** vision CNNs — augmentation + weight decay + early stop; dropout optional/moderate; transformers — dropout inside attention/FFN + AdamW decay + warmup/early-stop; small-data regimes — everything, aggressively. Monitor honestly via [[08-Training-Deep-Nets-in-Practice]]'s discipline.

> [!context] AI Context
> This file derives inverted-dropout mechanics and its ensemble/noise interpretations, weight-decay shrinkage and AdamW coupling, early stopping with patience logic, and augmentation as encoded invariance priors including mixup. It completes the stabilization toolkit; next note assembles everything into end-to-end training workflow.

---
If you remember one thing: deep nets default toward memorization — inject noise (dropout/augmentation), constrain magnitude (decay), or halt at the validation optimum (early stopping) to convert capacity into generalization.
