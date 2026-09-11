Everything before this note supplies components; this note assembles the workflow — how practitioners actually get a deep network from "compiles" to "works": debugging when loss won't fall, tuning learning rates systematically, choosing batch sizes, reading training curves like an experienced engineer, and scaling up without wasting GPU-weeks. Deep learning practice is 20% architecture, 80% disciplined experimentation.

> [!intuition] The Intuition
> Training a network is like tuning a high-performance engine with your hands in leather gloves and the hood closed — you only see gauges (loss curves) and hear sounds (failure modes). Professionals aren't luckier; they run *controlled single-variable experiments*, know each gauge's normal range by heart, and recognize failure signatures instantly: the whine of too-high LR, the silence of a dead pipeline, the sputter of vanishing gradients. This note is your gauges-and-signatures manual.

> [!math] The Math
> **The canonical workflow (each stage gates the next):**
> 1. **Overfit 10 samples.** Before any real training, verify your model+loss can drive loss ≈ 0 on ten examples. Fails → bug in model/loss/labels/pipeline (guaranteed not an optimization problem). This 5-minute test saves days.
> 2. **Baseline beats chance.** Loss near $\ln K$? Predicting uniform. Compare against trivial baselines (majority class, linear probe).
> 3. **LR range scan.** Sweep LR exponentially ($10^{-5} \dots 10^{1}$), few steps each; plot loss. Falling steadily = viable region; exploding = too big; frozen = too small. Pick just under the divergence edge.
> 4. **Scale up gradually** — small model/small crops first, verify learning scales, then grow.
>
> **Batch size — the three-way tradeoff.**
>
> $$\text{step noise} \propto \frac{1}{\sqrt{B}}, \qquad \text{steps per epoch} = \frac{N}{B}, \qquad \text{memory} \propto B$$
>
> Small B: noisy gradients (regularization + exploration, but unstable), many updates/epoch. Large B: precise gradients, GPU-efficient, but *fewer* updates per epoch and a generalization gap at extremes ("sharp minima" tendency). The **linear scaling rule**: when multiplying B by k, multiply LR by k (gradient-averaging means each large-batch step should travel k× further to make equal progress per epoch) — valid within limits; warmup compensates early instability.
>
> **Learning-rate schedules** ([[05-Optimizers]] introduced them): step decay (simple, needs milestones), cosine annealing (smooth, strong default), one-cycle policy (warmup → peak → cosine down; famously enables much higher peak LRs), warmup (transformer-mandatory — Adam's second-moment estimates are unreliable for first ~1000 steps).
>
> **Reading failure signatures:**
>
> | Symptom | Likely cause | First fix |
> |---------|-------------|-----------|
> | Loss = NaN | LR too high / FP16 overflow / bad data | ÷10 LR, grad clipping, check inputs |
> | Train loss stuck at $\ln K$ | No gradient flow | Overfit-test, check labels, init |
> | Loss oscillates wildly | LR too high or tiny batch | Lower LR / raise B / warmup |
> | Train ↓ val ↑ diverging | Overfitting | [[07-Regularization-in-DL]] toolkit |
> | Loss plateaus mid-training | LR schedule flat / local trap | Decay LR, restart from checkpoint lower |
> | Slow steady crawl | LR too low / no normalization | Raise LR, add [[06-Initialization-and-Normalization]] |
>
> **Gradient clipping** (standard for RNNs/transformers): if $\|g\| > c$, rescale $g \leftarrow \frac{c}{\|g\|} g$ — bounds any single catastrophic step while leaving normal steps untouched.
>
> **Mixed precision (FP16/BF16):** activations/gradients in half precision (~2× speed, ~½ memory); master weights in FP32; loss scaling prevents FP16 gradient underflow (BF16's wider exponent largely eliminates the need). Standard on modern GPUs.
>
> **Worked example — complete diagnostic session.** CIFAR-10 CNN: epoch 1 loss 2.31 (= ln 10 — uniform!), accuracy 10%. Diagnosis path: overfit-test on 64 images → loss reaches 0.001 ✓ model/loss fine. Check labels: discovered all labels were shifted by one index (pipeline bug). Fix → epoch 1 loss 1.9. Now LR scan shows 0.03 explodes, 0.01 falls fastest. Batch 128, cosine schedule from 0.01. Epoch 30: train 0.25, val 0.38 → overfitting signature → add augmentation + weight decay $5{\times}10^{-4}$ → val 0.31. Total: three controlled interventions, each validated by its own metric — that's the discipline.
>
> **Worked example — batch/LR co-tuning, numerically.** B=64 with LR 0.001 works (stable, improving). Want B=512 for speed. Linear rule: LR → 0.008. Try directly: NaNs at step 200. Apply warmup over 1500 steps ramping 0→0.008: stable, and wall-clock time per epoch drops 6× while final val matches the B=64 run. The failed direct attempt wasn't proof the rule is wrong — it was missing warmup, because large-LR-from-step-one breaks un-warmed Adam statistics ([[05-Optimizers]]).
>
> **Experiment hygiene:** change one variable at a time; log everything (config + git hash + curves); seed control for reproducibility comparisons; hold out a truly untouched test set touched once ([[Computer Science/Machine-Learning/08-Cross-Validation-and-Data-Splitting]] discipline applies fully); prefer fewer clean experiments over many confounded ones.

> [!context] AI Context
> This file provides the staged workflow (overfit-test → baseline → LR scan → scale), quantifies batch-size tradeoffs and the linear scaling rule, tabulates failure signatures with fixes, covers clipping/mixed-precision/warmup mechanics, and enforces experiment hygiene. It assumes all prior DL notes and completes the core track before architectures.

---
If you remember one thing: debug in stages (can it overfit 10 samples?), tune LR first and batch size jointly with it, read loss curves against known reference values, and never change two variables between runs.
