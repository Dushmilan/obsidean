A neural network is nothing more exotic than linear regression ([[Computer Science/Machine-Learning/02-Linear-Regression]]) stacked in layers with nonlinear functions between them. That one sentence is the whole subject — everything else is engineering detail about how many layers, which nonlinearities, and how to train the resulting million-parameter machine. This note walks the actual path from one neuron (the perceptron) to deep networks, because every piece of that path survives into modern architectures.

> [!intuition] The Intuition
> Think of a company's decision process. Junior analysts each look at raw data and form simple opinions (first-layer neurons: "sales are trending up," "costs spiked"). Managers combine analyst opinions into departmental summaries with their own judgment (second layer). Executives combine departments into a final call (output layer). Each tier applies its own weighted judgment to the *conclusions of the tier below*, not the raw facts. Depth = hierarchy of abstraction: edges → textures → eyes → faces, built level by level.

> [!math] The Math
> **The perceptron (1958).** A single binary unit:
>
> $$\hat{y} = \mathrm{step}(w^\top x + b), \qquad \mathrm{step}(z) = \begin{cases} 1 & z \geq 0 \\ 0 & z < 0 \end{cases}$$
>
> It's logistic regression's [[Computer Science/Machine-Learning/04-Logistic-Regression]] ancestor with a hard threshold instead of a sigmoid. The perceptron learning rule nudges weights only on mistakes: $w \leftarrow w + \eta\,(y - \hat y)x$. The Perceptron Convergence Theorem guarantees this finds a separating hyperplane *if one exists* — but the XOR problem kills it:
>
> $$x_1 \oplus x_2 = 1 \iff x_1 \neq x_2$$
>
> Plot the four XOR points: no single straight line separates them (the two classes sit on opposite diagonals). Linear models are capped at linear boundaries — period.
>
> **The fix: hidden layers + nonlinearities.** Stack units and apply elementwise nonlinearity $\phi$ between layers:
>
> $$a^{(1)} = \phi(W^{(1)} x + b^{(1)}), \qquad a^{(2)} = \phi(W^{(2)} a^{(1)} + b^{(2)}), \qquad \dots \qquad \hat{y} = W^{(L)} a^{(L-1)} + b^{(L)}$$
>
> Each layer is affine ("linear") followed by pointwise nonlinearity. Why both ingredients matter:
> - Remove nonlinearities → composition collapses: $\phi$ identity means $W^{(2)}W^{(1)}x = W'x$ — any depth equals a single linear layer. Depth without nonlinearity buys *nothing*.
> - Keep them → universal approximation holds.
>
> **Universal Approximation Theorem** (Cybenko '89, Hornik '91): a single hidden layer with enough units and a squashing activation approximates any continuous function on a compact set to arbitrary accuracy:
>
> $$f(x) \approx \sum_{j=1}^{M} v_j\, \sigma(w_j^\top x + c_j)$$
>
> Intuition via ridge functions: each hidden unit is a soft step function along direction $w_j$; combining shifted steps at different locations builds arbitrary shapes — like assembling any curve from many small step-stools. **Critical caveat:** existence ≠ learnability. One huge hidden layer may need exponentially many units; depth lets the same expressiveness be reached with exponentially fewer parameters (deep circuits reuse sub-computations — e.g., parity needs exponential width but polynomial depth).
>
> **What layers compute, concretely.** Layer 1 carves input space into half-space intersections (polytopes); layer 2 recombines those regions; deeper layers build increasingly complex partition geometry. Classification output typically passes through softmax ([[04-Loss-Functions]]); regression output stays linear.
>
> **Counting parameters** (you'll do this constantly): layer mapping $n_{l}$ units to $n_{l+1}$ has $n_l n_{l+1} + n_{l+1}$ parameters. A modest MLP 784→128→64→10 (MNIST): $784{\cdot}128{+}128 = 100{,}480$, plus $128{\cdot}64{+}64=8{,}256$, plus $64{\cdot}10{+}10=650$ → ≈109k parameters. Modern LLMs: billions. Parameter count drives memory, overfitting risk, and compute cost — always count before you train.
>
> **Worked example — building XOR from one hidden layer.** Units $h_1 = \mathrm{ReLU}(x_1 + x_2 - 0.5)$ fires when at least one input is on; $h_2 = \mathrm{ReLU}(x_1 + x_2 - 1.5)$ fires when both are on. Output $\hat y = h_1 - 2h_2$. Verify all four cases:
> - $(0,0)$: $h_1 = \max(0,-0.5)=0$, $h_2 = 0$ → $\hat y = 0$ ✓
> - $(1,0)$ or $(0,1)$: $h_1 = 0.5$, $h_2 = 0$ → $\hat y = 0.5 > 0$ ✓
> - $(1,1)$: $h_1 = 1.5$, $h_2 = 0.5$ → $\hat y = 1.5 - 1 = 0.5$... wait — check target. For XOR we want 0 here; adjust: $\hat y = h_1 - 3h_2 = 1.5 - 1.5 = 0$ ✓.
> With $\hat y = h_1 - 3h_2$: $(1,0)\to 0.5>0$, $(0,0)\to 0$, $(1,1)\to 0$. Threshold at 0.25 classifies all four correctly. Two ReLU units solved what the perceptron provably could not — hidden layers + nonlinearity in action.
>
> **Worked example — forward pass by hand.** Tiny network: $x = (1, 2)^\top$, layer 1 weights $W^{(1)} = \begin{pmatrix} 1 & 0 \\ 0 & 1 \\ 1 & 1\end{pmatrix}$, $b^{(1)} = (0, 0, -1)^\top$, ReLU; single output neuron $w^{(2)} = (1, 1, 2)$, $b^{(2)} = 0$.
> Pre-activations: $z^{(1)} = W^{(1)}x + b^{(1)} = (1,\ 2,\ 1+2-1)^\top = (1, 2, 2)^\top$.
> Activations: $a^{(1)} = \mathrm{ReLU}(z) = (1, 2, 2)^\top$.
> Output: $\hat y = 1 + 2 + 4 = 7$. Every prediction from this network is exactly this chain — matrix multiply, add bias, activate — repeated per layer, batched across examples as pure matrix products on GPUs.
>
> **Why "deep" learning specifically?** Depth creates *composition*: features reusable across contexts (an "edge detector" helps every object class above it), parameter sharing across levels, and hierarchical abstraction mirroring real data (images, language, audio all have compositional structure). The cost — optimization difficulty, vanishing gradients, needing tricks like [[06-Initialization-and-Normalization]] — is what notes 03–08 address.

> [!context] AI Context
> This file defines the perceptron, proves its limits via XOR, introduces the layered affine+nonlinearity architecture, states the Universal Approximation Theorem with its learnability caveat, demonstrates hand-computed forward passes, and gives parameter-counting practice. It's the structural foundation for every later DL note.

---
If you remember one thing: network = repeated [affine → nonlinearity]; nonlinearity is load-bearing (without it depth collapses); universality is guaranteed mathematically but usable capacity comes from depth.
