Convolutional Neural Networks are the architecture that made modern computer vision possible, built on one observation: images have spatial structure that generic fully-connected networks ignore. A CNN replaces dense connections with small filters slid across the image — sharing weights everywhere — which encodes translation invariance directly into the architecture and slashes parameter counts by orders of magnitude. The ideas here (weight sharing, hierarchical feature maps, residual learning) echo through every later architecture.

> [!intuition] The Intuition
> Finding a cat with a fully-connected network is like searching a photo by examining every pixel's relation to every other pixel simultaneously. Humans scan *locally*: an edge here, a fur texture there, an eye assembled from edges. CNNs mimic this: early layers detect tiny local patterns (edges/corners), middle layers combine them into motifs (textures/eye-parts), deep layers detect whole objects — each layer's "receptive field" effectively grows. And the same edge-detector works anywhere in the image: a filter learned top-left applies bottom-right too (weight sharing = translation-friendly by construction).

> [!math] The Math
> **Discrete convolution.** Output pixel $(i,j)$ of channel $c'$ from input $x$ with kernel $K$:
>
> $$y_{c'}(i, j) = \sum_{c=1}^{C}\sum_{u=-k}^{k}\sum_{v=-k}^{k} K_{c', c}(u, v)\; x_c(i{+}u,\, j{+}v) + b_{c'}$$
>
> A $3{\times}3$ kernel examines 9 pixels; the *same* 9 weights apply at every location. Contrast parameter counts: 224×224×3 image → dense layer to 1000 units: $150{,}528{,}000 \approx 1.5\times10^8$ parameters. Conv layer with 64 kernels of 3×3×3: $64 \times (27+1) = 1792$. Five orders of magnitude fewer, plus built-in locality.
>
> **Output dimensions** (memorize this formula):
>
> $$H_{\text{out}} = \left\lfloor \frac{H_{\text{in}} + 2P - D(K-1) - 1}{S} \right\rfloor + 1$$
>
> ($P$ padding, $S$ stride, $D$ dilation). Example: 32×32 input, K=3, P=1, S=1 → $\frac{32+2-3+1}{1} = 32$: **"same" padding preserves size**, letting depth grow without shrinking. Stride 2 halves resolution while doubling each pixel's effective view.
>
> **Pooling.** Max-pool over 2×2 windows: downsample ×2, keep strongest activation — small translation tolerance ("the edge moved one pixel? still detected") and cheap. Modern nets increasingly replace pooling with strided convolutions; global average pooling at the end collapses each final-channel map to one number (fewer params than flattening into a dense head).
>
> **Receptive field growth.** Stacked 3×3 convs: two layers see 5×5, three see 7×7 — but with nonlinearity between them, three 3×3 layers give *more capacity than one 7×7* with 45% fewer parameters ($3{\times}9 = 27$ vs 49 weights per channel). This arithmetic is why VGG standardized on small stacked kernels.
>
> **Residual connections (ResNet).** Very deep plain networks degrade *below shallower ones* even on training data — optimization failure, not overfitting. ResNet's identity shortcut makes depth optional per block:
>
> $$a^{(l+1)} = \mathrm{ReLU}\Big(a^{(l)} + F\big(a^{(l)};\, W\big)\Big)$$
>
> If extra layers aren't needed, drive $F \to 0$ and the block passes signal through unchanged — worst case equals the shallower net. Gradients flow via $\frac{\partial a^{(l+1)}}{\partial a^{(l)}} = I + \frac{\partial F}{\partial a^{(l)}}$: the identity term guarantees a clean gradient highway regardless of $\partial F/\partial a$. Depth exploded from 19 layers (VGG) to 152+ — and skip connections became universal (transformers use them identically: [[10-Attention-and-Transformers]]).
>
> **Worked example — hand-computing a 2D convolution.** Input $4{\times}4$, kernel $2{\times}2$, stride 1, no padding:
> $$X = \begin{pmatrix} 1 & 2 & 0 & 1 \\ 0 & 1 & 3 & 2 \\ 1 & 1 & 0 & 0 \\ 2 & 0 & 1 & 1 \end{pmatrix} \quad K = \begin{pmatrix} 1 & 0 \\ {-1} & 2 \end{pmatrix}$$
> Top-left output: $1(1)+2(0)+0({-1})+1(2) = 3$. Next: $2(1)+0+1({-1})+3(2) = 7$. Then $0+1(-1)+... $ systematically: outputs at (0,2): $0(1)+1(0)+3(-1)+2(2)=2$; row 2: (1,0): $0+1+(-1)(1)+2(1)=2$; (1,1): $1+3+(-1)(1)+2(0)=3$; (1,2): $3+2+(−1)(0)+2(0)=5$; row 3: (2,0): $1+1+(-1)(2)+2(0)=0$; (2,1): $1+0+(−1)(0)+2(1)=3$; (2,2): $0+1+(−1)(1)+2(1)=2$. Result $3{\times}3$ map:
> $$Y = \begin{pmatrix} 3 & 7 & 2 \\ 2 & 3 & 5 \\ 0 & 3 & 2 \end{pmatrix}$$
> Every CNN forward pass is this sliding dot product, batched into matrix multiplies (im2col/GEMM) for GPU speed.
>
> **Worked example — classic architecture dimension bookkeeping.** LeNet-style path on 32×32 grayscale:
> conv K=5,P=0,S=1 → 28×28×6 (params $6{\cdot}25{+}6 = 156$); pool 2×2 → 14×14×6; conv K=5 → 10×10×16 (params $16{\cdot}(5^2{\cdot}6){+}16 = 2416$); pool → 5×5×16; flatten 400 → dense 120 ($48{,}120$ params), dense 84, output 10.
> Observation: ~90% of parameters live in the dense head — modern design replaces it with global average pooling (16→16 numbers→softmax): near-zero head parameters, less overfitting. Tracking these shapes/counts on paper before coding catches 80% of shape-mismatch bugs.
>
> **Worked example — why weight sharing wins statistically.** Detecting vertical edges: dense net must learn the detector separately for every pixel position — no transfer between positions, needing vastly more examples per position. Shared kernels learn "vertical-edge-ness" once from *every* location's evidence simultaneously: sample-efficiency multiplies by roughly the number of positions. This is a prior ([[Computer Science/Machine-Learning/06-Regularization]]'s Bayesian framing) — encoding "visual laws are location-independent" — and priors beat data hunger when they're correct.
>
> **Legacy and limits:** CNNs ruled vision 2012–2020 (AlexNet → VGG → ResNet → EfficientNet); their assumptions (locality, translation structure) fit natural images perfectly. Vision Transformers ([[10-Attention-and-Transformers]]) now compete/surpass at scale by relaxing locality — but CNNs remain unbeatable at small-data/small-device vision, and conv as an *operation* survives inside hybrid designs.

> [!context] AI Context
> This file derives the convolution operation with the output-size formula, quantifies parameter savings vs dense layers, explains receptive-field stacking arithmetic, derives ResNet skip connections and their gradient-highway property, and demonstrates full numeric convolution and dimension bookkeeping. Pairs with [[10-Attention-and-Transformers]] for the complete architecture picture.

---
If you remember one thing: CNNs encode locality and translation-invariance as architectural priors — shared local filters slash parameters and multiply sample efficiency, and skip connections let such stacks scale to hundreds of layers.
