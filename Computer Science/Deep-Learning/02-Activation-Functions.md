Activation functions are the nonlinearities sandwiched between layers — the entire reason deep networks aren't just one big linear regression. The choice looks minor (it's one line of code) but it decides whether your network trains at all: the wrong activation produces dead neurons, vanishing gradients, or slow convergence. This note covers the standard zoo, the mathematics of why ReLU won, and how modern variants (GELU, SiLU) power today's transformers.

> [!intuition] The Intuition
> Each neuron is a tiny valve deciding how much of its computation flows downstream. A sigmoid valve never fully opens or closes — it always leaks, and near saturation it's frozen shut (gradients vanish). ReLU is a light switch with a twist: fully off for negative input, perfectly linear passthrough for positive — crude, but *always honest* about its gradient (1 or 0, never a vanishing fraction). Deep stacks need valves that don't strangle their own learning signal.

> [!math] The Math
> **Sigmoid:** $\sigma(z) = \frac{1}{1+e^{-z}}$, output $(0,1)$, derivative $\sigma' = \sigma(1-\sigma)$.
> **Tanh:** $\tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}$, output $(-1, 1)$, derivative $1 - \tanh^2(z)$.
>
> Both **saturate**: for large $|z|$, derivative → 0. In backprop ([[03-Forward-Pass-and-Backpropagation]]) gradients multiply across layers; a chain of saturated units multiplies by ~0 repeatedly — the **vanishing gradient problem**. Ten saturated sigmoid layers shrink gradient by factor $\prod_i \sigma'(z_i) < 0.25^{10} \approx 10^{-6}$: early layers stop learning entirely. Tanh saturates more slowly and centers at zero (better than sigmoid's all-positive outputs, which make weight gradients zigzag), but shares the fatal saturation.
>
> **ReLU:** $\mathrm{ReLU}(z) = \max(0, z)$, derivative: 1 if $z>0$, else 0.
>
> Why it won:
> 1. **No vanishing gradient on the positive side** — derivative is exactly 1 regardless of depth.
> 2. **Sparsity** — roughly half a layer's units output zero per input: cheaper compute, decorrelated representations.
> 3. **Biological flavor + cheapness** — no exponentials; a comparison and a copy.
> 4. **Effective gradient through active paths only** — information flows through a sparse sub-network each forward pass.
>
> The cost: **dying ReLUs.** If a unit's weights drift so its pre-activation is negative for *all* training inputs, its gradient is permanently 0 — the unit is dead forever (no data-dependent signal can revive it). Large learning rates make this epidemic: a big step shoves many units into permanent negative territory.
>
> **Leaky ReLU / PReLU:** $\mathrm{LeakyReLU}(z) = \max(0.01 z,\ z)$ — tiny slope on the negative side keeps dead units minimally alive (gradient 0.01 instead of 0). PReLU learns the negative slope. Cheap insurance; common default in CNN practice.
>
> **GELU (Gaussian Error Linear Unit)** — the transformer standard:
>
> $$\mathrm{GELU}(z) = z\,\Phi(z) \approx 0.5\, z\big(1 + \tanh[\sqrt{2/\pi}(z + 0.044715\,z^3)]\big)$$
>
> where $\Phi$ is the Gaussian CDF. Interpretation: stochastic regularization in expectation — each input is multiplied by a random 0/1 gate whose probability grows smoothly with $z$. Smooth (nonzero derivative everywhere), self-gating, non-monotonic dip near $z \approx -0.75$ that empirically helps representational quality. Powers BERT, GPT, ViT.
>
> **SiLU/Swish:** $\mathrm{swish}(z) = z \cdot \sigma(\beta z)$ — similar smooth self-gating family ($\beta \to \infty$ recovers ReLU); used in EfficientNet and LLaMA-style MLP blocks (often as "SiLU" in SwiGLU pairings).
>
> **Softplus:** $\log(1 + e^z)$ — the smooth ReLU approximation, useful when you need $C^\infty$ smoothness or positive-constrained outputs.
>
> **Choosing — practical map:**
>
> | Context | Standard choice | Why |
> |---------|----------------|-----|
> | Hidden layers, CNNs | ReLU / LeakyReLU | Speed, proven, sparse |
> | Transformer FFN blocks | GELU (or SiLU/SwiGLU) | Smoothness helps optimization at scale |
> | Binary output layer | Sigmoid | Produces probability |
> | Multiclass output | Softmax | Valid probability distribution |
> | Bounded control policies | tanh | Output range $(-1,1)$ matches action spaces |
>
> Never put saturating activations in hidden layers of deep nets unless you have a specific reason.
>
> **Worked example — watching saturation kill a gradient.** Chain of 5 layers, all sigmoids, all pre-activations $z = 4$ (mildly confident): each derivative $\sigma'(4) = \sigma(4)(1-\sigma(4)) ≈ 0.982 \cdot 0.018 ≈ 0.0177$. Product over 5 layers: $0.0177^5 ≈ 1.7 \times 10^{-9}$. The learning signal reaching layer 1 is a billionth of the loss's gradient — layer 1 effectively frozen while output layer trains. Same network with ReLU at $z=4$: derivatives all exactly 1, gradient arrives undiminished. This arithmetic is why pre-2010 deep sigmoid networks didn't train and post-2010 ReLU networks did.
>
> **Worked example — dying ReLU, traced precisely.** Single unit, inputs $x \in [0, 10]$ (all nonnegative!), weight $w = -2$, bias $b = 1$: pre-activation $z = -2x + 1 < 0$ for every sample with $x > 0.5$. Output always 0; backward: $\partial z/\partial w = x \neq 0$ but upstream gradient $\times \mathbb{1}[z > 0] = 0$ — both weight and bias receive exactly zero gradient, forever. Prevention: smaller learning rates, Leaky ReLU, careful initialization ([[06-Initialization-and-Normalization]]), monitoring the fraction of zero activations during training (healthy: well under ~50%; alarming: climbing toward 90%+).

> [!context] AI Context
> This file catalogs sigmoid/tanh/ReLU/leaky-ReLU/GELU/SiLU with exact derivatives, derives vanishing gradients from saturation multiplication, explains dying ReLUs mechanically, and gives context-appropriate selection guidance. Direct prerequisites for backpropagation and initialization notes.

---
If you remember one thing: activations trade expressiveness against gradient flow — saturation kills gradients multiplicatively with depth, which is why ReLU-family (and now GELU) dominate hidden layers, while sigmoids survive only at outputs.
