Attention is the mechanism that lets a network dynamically decide which parts of its input matter for each computation — and the Transformer, built almost entirely from self-attention, is the architecture behind every modern large language model. This note derives scaled dot-product attention from first principles, assembles the full transformer block, and connects directly to your HuggingFace LLM Course notes, which pick up exactly where this ends.

> [!intuition] The Intuition
> You are reading this sentence: to understand "it" in "The cat knocked the glass off the table because *it* was clumsy," your focus darts back to "cat." Attention formalizes exactly that darting: each word emits a query ("what am I looking for?"), every other word offers a key ("what do I contain?"), and query–key matches determine how much of each word's content (value) flows into the current word's new representation. Unlike RNNs ([[Computer Science/Deep-Learning/09-CNNs]]' sequential predecessors), every token reaches every other token in a single step — no information bottleneck through a hidden state.

> [!math] The Math
> **Scaled dot-product attention.** Given queries $Q$, keys $K$, values $V$ (each $n \times d_k$ for $n$ tokens):
>
> $$\mathrm{Attention}(Q, K, V) = \mathrm{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right)V$$
>
> Unpacking the three matrices' roles:
> - $QK^\top$ — every pairwise similarity score ($n \times n$: token $i$'s query dotted with token $j$'s key). Row $i$ = how relevant every token is to position $i$.
> - $\sqrt{d_k}$ scaling — with independent $N(0,1)$ components, dot products have variance $d_k$; without division, scores reach ±large magnitudes, softmax saturates, gradients die ([[02-Activation-Functions]] saturation logic in a new costume).
> - softmax — turns each row of scores into a probability distribution over "where to look."
> - multiply by $V$ — output for token $i$ = attention-weighted average of all tokens' values.
>
> **Multi-head attention.** One attention head computes one relevance pattern; heads run in parallel learned subspaces then concatenate:
>
> $$\mathrm{head}_h = \mathrm{Attention}(QW_h^Q,\ KW_h^K,\ VW_h^V), \qquad MHA(X) = \mathrm{Concat}(\text{head}_1..\text{head}_H)\,W^O$$
>
> Heads specialize — syntactic tracking, coreference, positional patterns — because each head's projections can carve different relation types from the same input. Cost per head drops by $1/H$, keeping compute ≈ single big head.
>
> **The transformer block** (pre-norm variant, GPT-style):
>
> $$X \leftarrow X + \mathrm{MHA}\big(\mathrm{LN}(X)\big) \qquad\quad X \leftarrow X + \mathrm{FFN}\big(\mathrm{LN}(X)\big)$$
>
> with FFN = two dense layers + nonlinearity ([[02-Activation-Functions]]' GELU/SwiGLU): $\mathrm{FFN}(x) = W_2\,\phi(W_1 x)$, typically expanding 4×. Division of labor: **attention moves information between positions; FFN transforms it within positions** (empirically FFN holds most parameters & factual memory). Residual streams ([[09-CNNs]] ResNet heritage) carry identity paths; LayerNorm stabilizes ([[06-Initialization-and-Normalization]]).
>
> **Positional encoding.** Self-attention is permutation-*blind* — shuffle input tokens, outputs shuffle identically; order must be injected. Original sinusoids:
>
> $$PE_{(pos, 2i)} = \sin\!\Big(\frac{pos}{10000^{2i/d}}\Big), \qquad PE_{(pos, 2i+1)} = \cos\!\Big(\frac{pos}{10000^{2i/d}}\Big)$$
>
> Each dimension oscillates at a different frequency — like binary counters or clock hands — so any relative offset corresponds to a fixed rotation learnable via linear map. Modern variants: learned absolute embeddings, relative encodings (T5 bias), RoPE rotating Q/K by position-dependent angles (LLaMA-family standard).
>
> **Causal masking** (decoder-only LMs): token $i$ must not peek at future tokens $j > i$. Implement by setting masked scores to $-\infty$ pre-softmax:
>
> $$\mathrm{score}_{ij} = \begin{cases} \dfrac{q_i k_j^\top}{\sqrt{d_k}} & j \le i \\ -\infty & j > i \end{cases}$$
>
> Row $i$'s softmax then distributes only over positions ≤ $i$. Training does this for *all* positions simultaneously in one pass (parallel teacher-forcing); inference autoregressively reuses past via KV-caching.
>
> **Complexity — the transformer's tax:** self-attention costs $O(n^2 d)$ time/memory in sequence length n (the $n \times n$ matrix). CNN-style locality costs $O(nkd)$ but only local mixing; RNNs $O(nd^2)$ but sequential (unparallelizable). The quadratic term drives long-context research: sparse/linear attention, FlashAttention's IO-aware exact computation, sliding windows.
>
> **Worked example — hand-computing attention.** Three one-dimensional tokens after projections: $q_1 = 1$, keys $k = (1, 0, -1)$, values $v = (10, 0, -5)$, $d_k = 1$. Scores for token 1: $(1{\cdot}1, 1{\cdot}0, 1{\cdot}{-}1)/\sqrt{1} = (1, 0, -1)$. Softmax: $e^1 : e^0 : e^{-1} = 2.718 : 1 : 0.368$ → normalized $(0.665, 0.245, 0.090)$. Output: $0.665(10) + 0.245(0) + 0.090(-5) = 6.65 - 0.45 = 6.20$ — dominated by token 2's value because its key matched best. Now scale check: same setup with sharper logits $(3, 0, -3)$ → softmax $(0.948, 0.047, 0.002)$ → output $9.47$: temperature controls focus sharpness. Every LLM's contextual understanding reduces to these three lines at massive scale.
>
> **Worked example — causal mask in action.** Four tokens, unmasked row 2 would attend everywhere; masked scores row 2: $(s_{21}, s_{22}, -\infty, -\infty)$ → softmax over just two entries: attends only to tokens 1–2 ✓. Verify training parallelism: the full $4\times4$ masked matrix processes all four prediction tasks (predict tokens 2,3,4 given prefixes 1, 1–2, 1–3) in one forward pass — the efficiency that makes LLM pretraining affordable.
>
> **From here to LLMs:** decoder-only stack = N × [causal MHA → FFN] blocks with residuals/norms; pretrained on next-token prediction over trillions of tokens; emergent in-context abilities appear at scale. Your HuggingFace notes continue directly: [[Computer Science/HuggingFace-LLM-Course/03-How-Transformers-Work]] implements this block library-first; [[Computer Science/HuggingFace-LLM-Course/05-Transformer-Architectures]] contrasts encoder-only (BERT), decoder-only (GPT), encoder-decoder (T5).

> [!context] AI Context
> This file derives scaled dot-product attention including the √dk correction, multi-head structure, the complete pre-norm transformer block, positional encodings and RoPE motivation, causal masking mechanics, and complexity comparisons vs CNN/RNN. It is the bridge note: DL track terminus, HuggingFace LLM Course on-ramp.

---
If you remember one thing: attention = differentiable dictionary lookup — softmax(QKᵀ/√dk)V routes information between positions by learned similarity; stacked with FFNs, residuals, and norms, it becomes the transformer powering every modern LLM.
