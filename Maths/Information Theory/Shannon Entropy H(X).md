### 2. Shannon Entropy $H(X)$

Self-information measures the surprise of a _single_ outcome. But what if we want to know the **average surprise** of a random variable $X$ before we even observe it?

**The Intuition:** Think of a biased coin that lands on Heads 99% of the time. If I ask you to guess the next flip, you'll just guess Heads every time, and you'll rarely be surprised. The "average surprise" (uncertainty) is very low. Now think of a fair coin (50/50). You have no idea what's coming. The "average surprise" is at its maximum.

**The Math:** Shannon Entropy, denoted as $H(X)$, is simply the **expected value** (the average) of the self-information across all possible outcomes.

$$H(X) = \mathbb{E}[I(X)] = \sum_{x \in X} p(x) \cdot I(x)$$

Substitute our definition of $I(x)$:

$$H(X) = -\sum_{x \in X} p(x) \log_2(p(x))$$

**What does this mean for AI?** Entropy $H(X)$ represents the **absolute minimum number of bits (or nats) required, on average, to encode a message** from this source. It is the fundamental limit of lossless data compression. In machine learning, if your target variable $Y$ has high entropy, the problem is inherently harder to learn because there is more uncertainty to resolve.

---
