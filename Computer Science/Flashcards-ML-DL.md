---
date: 2026-08-24
type: flashcards
tags: [flashcards, ml, dl]
---

# ML/DL Flashcards

> Requires the **Spaced Repetition** community plugin. Cards use `::` (Q::A).

## Machine Learning

#flashcards
What is the normal-equation solution for linear regression? :: $w = (X^\top X)^{-1}X^\top\mathbf{y}$
Gradient descent update rule? :: $w \leftarrow w - \alpha\,\nabla_w J(w)$
Bias–variance decomposition of expected error? :: $\text{Bias}^2 + \text{Variance} + \sigma^2_{\text{noise}}$
Ridge closed form? :: $(X^\top X + \lambda I)^{-1}X^\top\mathbf{y}$ — L2 penalty guarantees invertibility
Why does Lasso produce sparsity? :: L1 diamond constraint region has corners on axes → some coefficients hit exactly zero
Precision vs recall formulas? :: $P=TP/(TP+FP)$ · $R=TP/(TP+FN)$
When does accuracy mislead? :: Class imbalance — always-majority baseline scores high while catching nothing
F1 vs arithmetic mean of P and R? :: Harmonic mean punishes imbalance between precision and recall
AUC threshold-free meaning? :: Probability model ranks a random positive above a random negative

## Deep Learning

#flashcards
Perceptron's fatal flaw? :: Cannot represent XOR — linear boundary only
Universal Approximation Theorem caveat? :: Existence ≠ learnability; one wide layer may need exponentially many units
ReLU derivative / failure mode? :: 1 for z>0 else 0 → dying ReLUs when pre-activation always negative
Vanishing gradient cause? :: Saturating activations multiply derivatives <1 across depth (~$0.25^n$ for sigmoids)
Four backprop equations? :: $\delta^{(l)}=(W^{(l+1)\top}\delta^{(l+1)})\odot\phi'(z^{(l)})$; $\partial J/\partial W=\delta a^{\top}$
He init variance? :: $\mathrm{Var}(w)=2/n_{\text{in}}$ — compensates ReLU halving activation variance
Softmax + cross-entropy gradient w.r.t. logits? :: $\hat p_k - y_k$
Adam bias correction purpose? :: Unbiases $m_t,v_t$ EMA estimates that start at 0 ($\div(1-\beta^t)$)
Dropout scaling rule (inverted)? :: Multiply kept activations by $1/p$ so inference needs no change
Scaled dot-product attention? :: $\mathrm{softmax}(QK^\top/\sqrt{d_k})V$ — √dk keeps scores unit-variance so softmax doesn't saturate
Causal mask implementation? :: Set future scores $j>i$ to $-\infty$ before softmax
Residual connection gradient benefit? :: Identity highway $I+\partial F/\partial a$ prevents vanishing gradients in deep stacks
