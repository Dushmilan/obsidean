Deep Learning is machine learning with neural networks many layers deep — models that learn their own features from raw data rather than relying on hand-crafted ones. This sub-vault builds the theory from a single neuron up to modern architectures: how networks compute, how they learn via backpropagation, why training is delicate, and how CNNs and Transformers became the dominant designs.

**The Intuition:** If classical ML is cooking from recipes, deep learning is building the kitchen itself. The core track (perceptrons → backprop → optimizers → regularization) is plumbing and wiring: invisible when done right, catastrophic when done wrong. Architectures are floor plans — CNNs arrange neurons to exploit spatial structure in images, Transformers wire every token directly to every other so context flows freely. Everything here feeds into your HuggingFace LLM Course notes, which pick up exactly where [[10-Attention-and-Transformers]] ends.

**The Math:**

| Track | Notes | Focus |
|-------|-------|-------|
| Neural Network Core | [[01-Perceptrons-to-Neural-Networks]] → [[02-Activation-Functions]] → [[03-Forward-Pass-and-Backpropagation]] → [[04-Loss-Functions]] → [[05-Optimizers]] → [[06-Initialization-and-Normalization]] → [[07-Regularization-in-DL]] → [[08-Training-Deep-Nets-in-Practice]] | The complete mechanics: forward computation, gradient flow, optimization dynamics, stabilization |
| Architectures | [[09-CNNs]] · [[10-Attention-and-Transformers]] | Exploiting spatial structure; sequence modeling with attention |
| Representation Learning | [[11-Autoencoders]] | Unsupervised codes: undercomplete, sparse, denoising, contractive, stochastic & score estimation (Goodfellow Ch.14) |

**Learning Path:** 01–08 must be read roughly in order — backprop assumes activations, optimizers assume gradients, normalization assumes optimization. 09 and 10 assume all of the above but not each other. Read 10 before starting HuggingFace Chapter 3 ([[Computer Science/HuggingFace-LLM-Course/03-How-Transformers-Work]]). [[11-Autoencoders]] assumes 02–07 and is independent of 09–10; read after [[07-Regularization-in-DL]] for the regularized/sparse/contractive view.

**Prerequisites:** The whole Machine Learning foundations track — [[Computer Science/Machine-Learning/Machine-Learning_Index]], especially [[Computer Science/Machine-Learning/03-Gradient-Descent]] and [[Computer Science/Machine-Learning/04-Logistic-Regression]]. Comfortable chain-rule calculus and matrix derivatives.

**AI Context:** This file is the entry point for the Deep Learning notes. It maps the two tracks (network core, architectures), enforces reading order, and links forward into the HuggingFace LLM Course. Use it as the navigation hub for anything neural-network related in this vault.

---
**Key References:** [Deep Learning](https://www.deeplearningbook.org/) — Goodfellow, Bengio, Courville · [Dive into Deep Learning](https://d2l.ai/) — Zhang, Lipton, Li, Smola · [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/) — Nielsen

---

## 🌐 Main Vault Network

Every subject hub links every other — jump anywhere from here.

| Subject | Hub |
|----------|------|
| 📐 Mathematics | [[Maths]] |
| ⚛️ Physics | [[Physics_Index]] |
| 📊 Statistics | [[Stats_Index]] |
| 💻 Computer Science | [[Computer-Science_Index]] |
| 🤖 CS 188 — AI | [[CS-188_Index]] |
| 🤗 HuggingFace LLM Course | [[Computer Science/HuggingFace-LLM-Course/HuggingFace-LLM-Course_Index|HuggingFace-LLM-Course_Index]] |
| 🧠 Machine Learning | [[Computer Science/Machine-Learning/Machine-Learning_Index|Machine-Learning_Index]] |
| 🎓 University — Semester 1 | [[Physics/Cross/GP1-Syllabus-Map|GP1 Syllabus Map]] |
| 🛠️ Projects & Ops (OpenCode) | [[OpenCode/INDEX|INDEX]] |

**Master index:** [[Vault-Index]]

---

**Up:** [[Vault-Index]]
