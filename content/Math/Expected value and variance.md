---
tags:
  - clwip
feed: show
date: 2026-01-13
---
# Expected value
The expected value of a random variable $X$ is the sum of all the values it can take on, weighted by their probability:

$$
\mathbb{E}_{x \sim \text{Pr}(X)}[x] := \sum_{x \in X} x \text{Pr}(x)  
$$

 If we have a set of $n$ numbers $X = \{x_i\}_{i=1}^{n}$, and assign them equal probability $\text{Pr}(x_i) = \frac{1}{n}$, then the expected value is equal to the average $\bar{x}$:
 
 $$
 \mathbb{E}_{x \sim \text{Pr}(X)}[x] = \sum_{x \in X} x \frac{1}{n} = \frac{\sum_{i} x_i}{n} := \bar{x}
$$

# Expected value and minimizing squared error
Consider a set of $n$ numbers $X = \{x_i\}_{i=1}^{n}$. What number $\hat{x}$ minimizes the mean squared error among this set of numbers?

$$
\arg \min_{\hat{x}} \sum_{i=1}^n ( x_i - \hat{x})^2
$$

It turns out that the solution is the average, $\hat{x} = \bar{x}$, which is related to expected values as shown above. To prove this, we can find a local minimum by taking the derivative of the above equation w.r.t $\hat{x}$ and set it equal to 0, and then solve for $\hat{x}$:

$$
\begin{aligned}
0 &= \frac{d}{d\hat{x}}\sum_{i=1}^n ( x_i - \hat{x})^2  \quad \text{(Equation for local minimum)} \\
0&= \sum_{i=1}^n \frac{d}{d\hat{x}}( x_i - \hat{x})^2 \quad \text{(Linearity of derivative)} \\
0&= \sum_{i=1}^n 2(x_i - \hat{x})(-1) \quad \text{(Chain rule)} \\
0&= \sum_{i=1}^n (x_i - \hat{x}) \quad \text{(Divide by 2 and -1 )} \\
0&= \sum_{i=1}^n x_i - \sum_{i=1}^n\hat{x} \quad \text{(Separate out summation)} \\
0&= \sum_{i=1}^n x_i - n\hat{x} \quad \text{(xhat is a constant)} \\
\hat{x}&= \sum_{i=1}^n x_i \frac{1}{n} \quad \text{(move to other side and divide by n)} \\
\hat{x} &= \bar{x} \quad \text{(Substitute in defintion of average)}
\end{aligned}
$$

>[!todo]
>- Do similar derivation for conditional expectations.
>- write down standard definition of variance 
>- Prove it equals `E[x^2] - E[x]^2`
>- Covariance
>- Covariance matrix
>- Pearson correlation coefficient