### 1. Joint & Conditional Entropy

Imagine you are predicting the weather.

- X is the season (Summer, Winter).
- Y is the temperature (Hot, Cold).


**Joint Entropy H(X,Y)** is the total uncertainty of the _pair_ of events happening together. How surprised are you to find out it's both "Summer" AND "Hot"? Mathematically, it's just the entropy of the combined distribution:
$$H(X,Y)=−∑x,y​p(x,y)log2​p(x,y)$$
**Conditional Entropy H(Y∣X)** is the uncertainty left in Y _after_ you already know X. If I tell you it is Winter (X), how much uncertainty is left about the temperature (Y)? It should be much lower than if I told you nothing! Mathematically, it's the weighted average of the entropy of Y for every possible value of X:

$$H(Y∣X)=∑x​p(x)H(Y∣X=x)$$

**The Chain Rule (Crucial for AI):** 
These two concepts are linked by a beautiful, simple equation:

$$H(X,Y)=H(X)+H(Y∣X)$$

_(The total uncertainty of the system = the uncertainty of XXX + the remaining uncertainty of YYY once XXX is known)._

### 2. Mutual Information $I(X;Y)$

This is the star of the show for Machine Learning.

**The Intuition:** Mutual Information measures **how much knowing one variable reduces the uncertainty of the other**. It is the amount of "information" that **X** and **Y** share.

- If **X** and **Y** are completely independent (like the color of my shoes and the temperature outside), knowing **X** tells you _nothing_ about **Y**. Mutual Information is **0**.
- If **X**  perfectly determines **Y** (like the radius of a circle and its area), knowing **X** completely destroys the uncertainty of **Y**. Mutual Information is at its **maximum**.


**The Math:** Because $H(Y)$ is the total uncertainty of **Y** , and $H(Y|X)$ is the uncertainty of **Y** _left over_ after knowing **X**, the difference between them is exactly the information **X** gave us!

$$I(X;Y)=H(Y)−H(Y∣X)$$

Using the Chain Rule from above, we can also write this in a highly symmetric, beautiful way:

$$I(X;Y)=H(X)+H(Y)−H(X,Y)$$

**AI Context:** In representation learning, we want our neural network's hidden layers **(Z)** to capture as much Mutual Information as possible with the target label **(Y)**, so $I(Z;Y)$ is high. At the same time, we want **Z** to ignore irrelevant noise in the input **(X)**, so we want to minimize $I(Z;X)$ if X contains useless background pixels. This trade-off is called the **Information Bottleneck**!
