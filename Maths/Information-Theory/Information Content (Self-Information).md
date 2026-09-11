### 1. Information Content (Self-Information)

Before we can measure the information of a whole dataset, we need to measure the information of a **single event**.

**The Intuition:** Imagine I tell you, "The sun rose in the East today." You aren't surprised — the probability was basically 1, so the information gained was 0. Now imagine I tell you, "It snowed in the Sahara Desert today." You are highly surprised! The probability was near 0, so the information gained is huge.

We need a function, let's call it $I(x)$ (Information of event $x$), that maps probability $p(x)$ to "surprise". It must satisfy three logical rules:

- **Certain events have zero surprise:** If $p(x) = 1$, then $I(x) = 0$.
- **Rare events have high surprise:** As $p(x) \to 0$, $I(x) \to \infty$.
- **Independent events add up:** If I flip two independent coins, the surprise of getting (Heads, Heads) should be the surprise of the first Heads _plus_ the surprise of the second Heads. Mathematically: $I(x,y) = I(x) + I(y)$.

**The Math:** Because probabilities of independent events _multiply_ ($p(x,y) = p(x) \cdot p(y)$), but we want their information to _add_, we need a function where $f(a \cdot b) = f(a) + f(b)$.

In mathematics, there is only one continuous function that turns multiplication into addition: **The Logarithm!**

Therefore, we define Information Content (or Self-Information) as:

$$I(x) = -\log_2(p(x))$$

We use a negative sign because probabilities are between 0 and 1, and the log of a fraction is negative. We want information to be a positive number.

**AI Context:** If we use $\log_2$, the unit of information is a **bit** (binary digit). In AI and Deep Learning (PyTorch, TensorFlow), we almost always use the natural logarithm $\ln$ (or $\log_e$). The unit is then a **nat**. You'll see nats everywhere in ML loss functions!

---
