Before a network receives a single gradient, its fate is partly sealed by how its weights were initialized and whether normalization layers stabilize activations during training. Both techniques attack the same enemy: uncontrolled activation/gradient scale as signals traverse depth — exploding or vanishing before learning can even begin. Initialization sets the starting conditions; batch/layer norm keeps every intermediate distribution civilized throughout training.

> [!intuition] The Intuition
> A 100-layer pipeline of amplifiers: if each stage multiplies the signal by 1.05, output = $1.05^{100} \approx 130\times$ input; by 0.95, it attenuates to $0.006\times$. Networks are exactly this: each layer's weights scale activations multiplicatively. Random initialization is dialing every amplifier blind — you want unity gain on average. Normalization layers are automatic gain control installed between stages, continuously re-centering and re-scaling so no stage saturates.

> [!math] The Math
> **Why zero or naive initialization fails.** All-zero weights → all neurons identical → symmetric gradients forever ("symmetry breaking" failure). Naive $\mathcal{N}(0, 1)$ (or uniform $[0,1]$) → variance grows multiplicatively with fan-in:
>
> $$\mathrm{Var}(z^{(l)}) = n_{l-1}\,\mathrm{Var}(w)\,\mathrm{Var}(a^{(l-1)})$$
>
> With $n_{l-1} = 1000$ inputs and $\mathrm{Var}(w) = 1$: pre-activation variance ×1000 per layer — instant explosion (or with small weights, implosion).
>
> **Xavier/Glorot init** (tanh-appropriate). Demand variance preserved *forward* through a layer: $\mathrm{Var}(z) \approx \mathrm{Var}(a)$ requires
>
> $$\mathrm{Var}(w) = \frac{2}{n_{\text{in}} + n_{\text{out}}}$$
>
> (average of forward-preservation $\frac{1}{n_\text{in}}$ and backward-preservation $\frac{1}{n_\text{out}}$ requirements).
>
> **He/Kaiming init** (ReLU-appropriate). ReLU zeroes half the activations, halving variance; compensate by doubling:
>
> $$\boxed{\mathrm{Var}(w) = \frac{2}{n_{\text{in}}}} \qquad w \sim \mathcal{N}\!\left(0, \frac{2}{n_\text{in}}\right)$$
>
> Rule to remember: **match the init to the activation's variance-halving behavior** — Glorot for symmetric activations, He for ReLU-family. Transformers extend this: GPT-2 scales residual-branch projections by $1/\sqrt{2L}$ ($L$ = depth) because residual streams *accumulate* variance across layers ([[09-CNNs]]' ResNet connection).
>
> **Batch Normalization** (networks with spatial/batch structure). For a mini-batch of activations at some channel, normalize then learnably rescale:
>
> $$\hat x_i = \frac{x_i - \mu_B}{\sqrt{\sigma_B^2 + \epsilon}}, \qquad y_i = \gamma\, \hat x_i + \beta$$
>
> $\gamma, \beta$ are learned — the network can undo normalization if useful, but must actively choose scale/mean rather than inherit drift. Effects:
> 1. **Internal covariate shift tamed** — each layer sees standardized inputs regardless of what earlier layers are doing; optimization landscape smooths measurably (gradient predictive of step direction over longer distances).
> 2. **Higher effective learning rates** survive without divergence.
> 3. **Regularization side-effect** — batch statistics are noisy estimates → slight per-example jitter ≈ implicit noise injection.
> 4. **At inference**, frozen running averages replace batch stats (must set `model.eval()` — a classic silent-bug source).
>
> Costs: couples examples within a batch (small batches → noisy stats → poor results), awkward for sequences. Enter:
>
> **Layer Normalization.** Normalize across the *feature dimension* of each example independently — no batch statistics at all:
>
> $$\hat x = \frac{x - \mu_{\text{features}}}{\sqrt{\sigma^2_{\text{features}} + \epsilon}}, \qquad y = \gamma \odot \hat x + \beta$$
>
> Identical behavior whether batch size is 1 or 512, no train/inference distinction in statistics. This independence is precisely why **transformers standardize on LayerNorm** ([[10-Attention-and-Transformers]]): variable-length sequences and small-per-GPU batches make batch stats unreliable. RMSNorm (LLaMA-era) drops mean-centering for speed: just divide by RMS.
>
> **Worked example — init variance cascade, numerically.** Ten tanh layers, $n = 500$ per layer.
> Naive $\mathrm{Var}(w)=1$: layer-1 pre-activation std ≈ $\sqrt{500} ≈ 22$ → deep saturation; tanh′ there ≈ $e^{-2\cdot22}$-scale ≈ 0 — gradients die in the *first* backward step.
> Glorot $\mathrm{Var}(w)=2/1000=0.002$, std ≈ 0.045: each layer preserves scale — activations exit layer 10 with roughly entry-level magnitude, gradients return likewise. One formula choice separates "loss never moves" from "training works."
>
> **Worked example — batch norm rescuing mid-network drift.** Suppose layer 7's weights grow 3× during an aggressive update phase. Without norm: every downstream layer's inputs shift/scale 3× → downstream gradients misaligned → loss spikes. With BatchNorm after layer 7: downstream sees re-standardized input (μ≈0, σ≈1) *regardless* — the disturbance is quarantined at layer 8's own parameters instead of cascading. This containment is why BN nets tolerate α values that would blow up plain networks, and why BN placement (after affine, before/after activation — literature supports both) matters less than its presence.
>
> **Order-of-operations note (modern practice):** original paper: conv → BN → ReLU. Later "post-LN vs pre-LN" transformer debate: GPT-style **pre-norm** (norm inside the residual branch, before attention/FFN) trains more stably without warmup fragility; post-LN achieves slightly better final loss when it converges. Modern LLMs overwhelmingly choose pre-norm/RMSNorm for trainability.

> [!context] AI Context
> This file derives Xavier/He variances from forward/backward preservation requirements, explains BatchNorm mechanics and its four effects, contrasts LayerNorm/RMSNorm for sequence models, demonstrates the variance-cascade numerically, and covers pre/post-norm placement. It assumes [[02-Activation-Functions]] saturation analysis and enables [[08-Training-Deep-Nets-in-Practice]].

---
If you remember one thing: keep signal variance ≈ constant across depth — He-init for ReLU nets, Glorot otherwise — then let LayerNorm (transformers) or BatchNorm (CNNs) enforce it continuously during training.
