An autoencoder is a neural network trained to copy its input to its output through a constrained internal code — the constraint forces it to learn useful structure rather than the identity function, turning unsupervised copying into representation learning.

> [!intuition] The Intuition
> Copying a textbook by hand teaches you nothing, but copying it onto a single index card forces you to discover what matters. An autoencoder is that index-card bottleneck: the encoder compresses `x → h` (the card), the decoder expands `h → r ≈ x`. If the card is small (undercomplete), you keep only the essence — like PCA keeping principal directions. If the card is large (overcomplete), you could cheat by transcribing verbatim, so you add rules: "use few words" (sparsity), "still work if I smudge the original" (denoising), or "don't change the summary if I wiggle the input slightly" (contractive). Stochastic variants say "write a *distribution* of plausible cards, not one card." The denoising/contractive tricks make the reconstruction vector field point toward the data manifold — arrows that also estimate the data distribution's score `∇ log p(x)`, i.e., which direction is more probable.

> [!math] The Math
> **General structure — Fig.14.1 in Goodfellow.** Encoder `f`, decoder `g`:
>
> $$h = f(x), \qquad r = g(h) = g(f(x)), \qquad J = \mathbb{E}_{x \sim \hat p_{data}}[L(x, r)]$$
>
> where `L` is typically squared error `‖x−r‖²` for real values or cross-entropy for binary. The hidden code `h` is the learned representation. Two-part training view: encoder extracts features, decoder is a generative model `p_decoder(x|h)`. Without constraints, `g∘f` learns the identity — useful for representation only when constrained.
>
> **Recirculation (historical root).** Hinton & McClelland (1988), Hinton & Zemel (1994): train autoencoder by *recirculating* the reconstruction as if it were a new input and comparing intermediate codes — an early form of backpropagation for unsupervised learning. Modern training simply minimizes `L(x, g(f(x)))` by SGD ([[03-Forward-Pass-and-Backpropagation]]), but the name survives: autoencoders "circulate" information through a bottleneck and back. The key insight persists — forcing reconstruction through `h` makes `h` a compressed generative code.
>
> **1. Undercomplete Autoencoders — §14.1.** Constraint: `dim(h) < dim(x)`. Objective:
>
> $$J_{under} = \mathbb{E}_x [L(x, g(f(x)))], \quad f:\mathbb{R}^n→\mathbb{R}^k, k<n$$
>
> This bottleneck *is* the regularizer. Linear encoder `f(x)=W x`, linear decoder `g(h)=Wᵀh`, squared loss → recovers PCA subspace: `W` spans the top-`k` eigenvectors of the data covariance (Goodfellow Thm, cf. `[[Maths/Linear-Algebra]]`). With nonlinear `f,g` and sufficient capacity, undercomplete AEs learn a nonlinear manifold's principal coordinates. If capacity is too high, even with `k<n`, it can memorize — then need further regularization. Preventing copying is the whole game.
>
> **2. Regularized Autoencoders — §14.2.** Allow `dim(h) ≥ dim(x)` (overcomplete) but add penalty `Ω`:
>
> $$J_{reg} = J + λ Ω(h, x) \quad \text{or} \quad J + λ Ω(W, J_f)$$
>
> The regularizer, not dimension, prevents copying. Sparse, denoising, and contractive AEs are all instances — they learn *overcomplete* yet useful codes (like V1 simple cells) because `Ω` biases toward sparse/stable codes. This is the unsupervised analogue of [[07-Regularization-in-DL]] and [[Computer Science/Machine-Learning/06-Regularization]].
>
> **3. Sparse Autoencoders — §14.2.1.** Penalty encourages few active units:
>
> $$Ω(h) = λ Σ_i |h_i| \quad (L₁) \qquad \text{or} \qquad Ω(h) = Σ_j \mathrm{KL}(ρ ‖ \hat ρ_j)$$
>
> where `ρ` is target sparsity (e.g., 0.05) and `\hat ρ_j = E_x[h_j]` is empirical mean activation. KL form from sparse coding literature: `\mathrm{KL}(ρ‖\hat ρ)=ρ\log(ρ/\hat ρ)+(1−ρ)\log((1−ρ)/(1−\hat ρ))` blows up unless `\hat ρ≈ρ`. Training: `J = L(x,g(f(x))) + Σ_j KL(ρ‖\hat ρ_j)`. At inference, `h` is sparse — feature selection emerges. Connection to Laplace prior `p(h)∝exp(−λ‖h‖₁)`.
>
> **4. Denoising Autoencoders (DAE) — §14.2.2 & §14.5.** Corrupt then reconstruct (Vincent et al. 2008). Corruption process `C(\tilde x|x)`:
>
> $$\tilde x ∼ C(\tilde x|x), \quad h = f(\tilde x), \quad r = g(h), \quad J_{DAE} = \mathbb{E}_{x,\tilde x}[L(x, g(f(\tilde x)))]$$
>
> Common `C`: isotropic Gaussian `\tilde x = x + ε, ε∼N(0,σ²I)`, masking noise (set fraction `ν` of inputs to 0), salt-and-pepper. DAE cannot copy because `\tilde x ≠ x`; it must capture `p_data` structure to undo noise. Loss is really `−\log p_{decoder}(x|h=f(\tilde x))` with factorial decoder `p_{decoder}(x|h)=N(x; g(h), σ²I)` or Bernoulli for binary. Fig.14.3 computational graph: `x → C → \tilde x → f → h → g → r`, loss compares `r` to clean `x`.
>
> *Why DAE is not just augmentation:* [[07-Regularization-in-DL]]'s augmentation trains `x→y` with label-preserving `T(x)`; DAE trains `\tilde x → x` where target *is* the clean input — unsupervised.
>
> **5. Regularizing by penalizing derivatives — Contractive Autoencoder (CAE) — §14.7.** Explicitly penalize sensitivity of `h` to `x` (Rifai et al. 2011):
>
> $$Ω_{CAE}(h,x) = λ ‖J_f(x)‖_F² = λ Σ_{ij} \left( \frac{∂h_j}{∂x_i} \right)², \quad J_f = ∂f/∂x$$
>
> For `h = σ(Wx+b)`: `J_f = \mathrm{diag}(σ'(Wx+b)) W`, so
>
> $$‖J_f‖_F² = Σ_j [σ'(z_j)² Σ_i W_{ji}²]$$
>
> (elementwise sigmoid derivative `σ'(z)=σ(z)(1−σ(z))` → saturated units have `σ'≈0` and contribute little; penalty drives `W` small *or* units saturated — both contract). Name "contractive": neighborhood of `x` maps to *smaller* neighborhood in `h`. Alain & Bengio (2013): for small Gaussian noise `σ→0`,
>
> $$\mathbb{E}[‖g(f(x+ε))−x‖²] ≈ ‖g(f(x))−x‖² + σ² ‖J_{g∘f}(x)‖_F²$$
>
> so DAE with infinitesimal noise ≡ CAE penalty on *reconstruction* `g∘f`. Best classification features come from penalizing `J_f` (code) not `J_{g∘f}` (reconstruction) — code robustness transfers better.
>
> **6. Representational Power, Layer Size and Depth — §14.3.** Universal approximation applies: one hidden-layer AE can approximate any continuous `g∘f` arbitrarily well, but may need exponential width `O(2^n)` for `n`-dim inputs — depth is exponentially more efficient (same argument as [[01-Perceptrons-to-Neural-Networks]] XOR + [[06-Initialization-and-Normalization]] depth). Empirical notes: (a) overcomplete + linear `f,g` without `Ω` → learns identity even with large `k`; (b) undercomplete + nonlinear needs sufficient depth to disentangle manifolds — deep AEs (stacked) build hierarchy `edges→parts→objects` like [[09-CNNs]] receptive fields. Practical: depth helps reconstruction quality but complicates optimization ([[08-Training-Deep-Nets-in-Practice]] workflow applies).
>
> **7. Stochastic Encoders and Decoders — §14.4.** Generalize `h=f(x)` to distributions (Fig.14.2):
>
> $$h ∼ p_{encoder}(h|x), \qquad x ∼ p_{decoder}(x|h)$$
>
> Loss is negative log-likelihood:
>
> $$J = \mathbb{E}_{x∼\hat p_{data}} \mathbb{E}_{h∼p_{encoder}(h|x)} [−\log p_{decoder}(x|h)]$$
>
> Simple case: `p_{encoder}(h|x)=N(h; f(x), σ²I)`, `p_{decoder}(x|h)=N(x; g(h), σ²I)` or Bernoulli `Bern(x; g(h))` for binary. Sampling `h` during training ≈ injecting noise into code — bridges to variational autoencoders (not in Ch.14, but predecessor). Allows direct sampling/markov chain: `x₀ → h₁∼p_{enc}(·|x₀) → x₁∼p_{dec}(·|h₁) → ...` converges to joint model if ergodic.
>
> **8. Estimating the Score — §14.5.1 & §14.6 (Manifolds).** Key result (Alain & Bengio 2014): optimal DAE reconstruction estimates the score `∇_x \log p_{data}(x)`:
>
> $$g^*(f^*(\tilde x)) = \tilde x + σ² ∇_{\tilde x} \log p(\tilde x) + o(σ²)$$
>
> or equivalently `r(x)−x ≈ σ² ∇_x \log p(x)`. For Gaussian corruption variance `σ²`. Score points toward higher-density regions — perpendicular to manifold, then along it near data. CAE with `‖J_f‖_F²` has same connection via `r(x)−x ∝ ∇ \log p(x)`.
>
> *Manifold intuition — §14.6:* High-dim data concentrates near low-dim manifold. Autoencoders learn vector field `v(x)=g(f(x))−x` that points toward manifold (denoising = projection). Jacobian `J_f` learns tangent plane: large derivatives along manifold, `≈0` orthogonal (contractive). This is why sampling by iterative `x_{t+1}=g(f(x_t))+noise` (or Langevin using estimated score) walks the manifold.
>
> **Worked example — linear undercomplete = PCA.** Data in `ℝ²`: `x₁=(1,1)`, `x₂=(−1,−1)`, `x₃=(2,2)` lie on line `y=x`. Choose `k=1`, encoder `f(x)=wᵀx`, decoder `g(h)=w h` with `‖w‖=1`. Loss `Σ‖x_i − w wᵀ x_i‖²` minimized when `w=(1/√2,1/√2)` — projects onto line, reconstruction error 0. Any orthogonal `w⊥` gives error `Σ‖x_i‖²=12`. Gradient: `∂J/∂w = −2 Σ (x_i − w wᵀx_i)(wᵀx_i)` → zero at PCA direction. This single-neuron AE solves PCA exactly.
>
> **Worked example — sparse KL.** Target `ρ=0.1`, batch of 10 examples where `h₁` active (=1) only on 3 examples → `\hat ρ₁=0.3`. Penalty `KL(0.1‖0.3)=0.1 log(0.1/0.3)+0.9 log(0.9/0.7)=−0.109+0.226=0.117`. For 50 hidden units, sum ≈5.85 extra loss → optimizer suppresses `W₁`, raises bias negative, making unit rarer until `\hat ρ→0.1`.
>
> **Worked example — DAE vs CAE numerically.** `x=0.5`, clean, `f(x)=sigmoid(2x−1)`, `g(h)=h` (identity decoder). `z=0`, `f=0.5`, `f'=σ'(0)=0.25`, `W=2` → `J_f=0.5`.CAE penalty `λ·0.25`. Gaussian `σ=0.1`: DAE loss on `\tilde x=0.6` (noisy) is `(g(f(0.6))−0.5)²=(σ(0.2)−0.5)²≈(0.55−0.5)²=0.0025`; on clean it would be 0. The extra 0.0025 ≈ `σ²J_f²=0.01·0.25=0.0025` — matches Alain & Bengio first-order equivalence.
>
> **Worked example — score estimate.** 1D mixture: `p(x)=0.5 N(−1,0.1²)+0.5 N(1,0.1²)`. At `x=0` (valley), true score `∇\log p(0)` points left or right? Compute via Parzen estimate: `p(0)≈small`, `p(0.1)>p(0)` → score positive  → DAE `r(0)−0 >0` predicts 0.1 is more likely than 0, correctly moving toward nearest mode at ±1. Iterating `x←r(x)` converges to mode, exactly score ascent.
>
> **Common pitfalls:**
> - Overcomplete without `Ω` → exact copy, zero training loss, useless `h` — always check reconstruction vs downstream probe accuracy.
> - Confusing DAE with standard augmentation — DAE target is clean `x`, not label `y`.
> - Applying `L₂` weight decay and expecting contractive effect — decay shrinks `W` globally; CAE shrinks `‖J_f‖` adaptively (saturation-aware).
> - Score estimate holds only for small `σ`; large corruption → biased score, over-smoothed manifold.

> [!context] AI Context
> This file implements Goodfellow Ch.14 (pp.499–522): recirculation→undercomplete (§14.1)→regularized (§14.2)→sparse (§14.2.1)→denoising (§14.2.2/14.5)→contractive Jacobian penalty (§14.7)→representational power/depth (§14.3)→stochastic encoders (§14.4)→score estimation & manifold view (§14.5.1/14.6). It completes the unsupervised representation learning track before [[Machine-Learning/10-Boosting]] ensembles and feeds forward to variational/generative models. Prereqs: [[03-Forward-Pass-and-Backpropagation]], [[07-Regularization-in-DL]], [[Computer Science/Machine-Learning/06-Regularization]].

---
If you remember one thing: an autoencoder is only as good as its constraint — bottleneck, sparsity, noise, or Jacobian penalty turns identity mapping into a manifold-aware code whose reconstruction error estimates the data score.

**Up:** [[Deep-Learning_Index]]
