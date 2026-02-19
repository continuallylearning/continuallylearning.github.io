---
tags:
  - clwip
feed: show
date: 2026-01-13
---
# Expected value
The expected value of a random variable $X$ is the sum of all the values it can take on, weighted by their probability (i.e., a weighted average):

$$
\mathbb{E}_{x \sim \text{Pr}(X)}[x] := \sum_{x \in X} x \text{Pr}(x)  
$$

 If we have a set of $n$ numbers $X = \{x_i\}_{i=1}^{n}$, and assign them equal probability $\text{Pr}(x_i) = \frac{1}{n}$, then the expected value is equal to the standard average $\bar{x}$ which sums all the numbers in $X$ and divides by its size:
 
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
0 &= \frac{d}{d\hat{x}}\sum_{i=1}^n ( x_i - \hat{x})^2  && \text{(Equation for local minimum)} \\
0&= \sum_{i=1}^n \frac{d}{d\hat{x}}( x_i - \hat{x})^2 && \text{(Linearity of derivative)} \\
0&= \sum_{i=1}^n 2(x_i - \hat{x})(-1) && \text{(Chain rule)} \\
0&= \sum_{i=1}^n (x_i - \hat{x}) && \text{(Divide by 2 and -1 )} \\
0&= \sum_{i=1}^n x_i - \sum_{i=1}^n\hat{x} && \text{(Separate out summation)} \\
0&= \sum_{i=1}^n x_i - n\hat{x} && \text{(}\hat{x} \text{ is a constant)} \\
\hat{x}&= \sum_{i=1}^n x_i \frac{1}{n} && \text{(move to other side and divide by n)} \\
\hat{x} &= \bar{x} && \text{(Substitute in definition of average)}
\end{aligned}
$$

# Variance
Variance of a random variable $X$ is average squared distance from samples of the random variable and its expected value:

$$
\begin{aligned}
Var(X) &:= \mathbb{E}_{X \sim P}[(X - \mathbb{E}_{X \sim P}[X])^2] && \text{(Definition of variance)}\\
&= \mathbb{E}_{X \sim P}[X^2 - 2X\mathbb{E}[X] +  \mathbb{E}[X]^2] \\
&= \mathbb{E}_{X \sim P}[X^2] - 2\mathbb{E}_{X \sim P}[X\mathbb{E}[X]] +  \mathbb{E}[X]^2 && \text{(Linearity of Expectation)} \\
&= \mathbb{E}_{X \sim P}[X^2] - 2(\mathbb{E}_{X \sim P}[X])\mathbb{E}_{X \sim P}[X] +  \mathbb{E}[X]^2 && \text{(}\mathbb{E}_{X \sim P}[X] \text{ is a constant)} \\
&= \mathbb{E}_{X \sim P}[X^2] - 2\mathbb{E}_{X \sim P}[X]^2 +  \mathbb{E}[X]^2  \\
&= \mathbb{E}_{X \sim P}[X^2] - \mathbb{E}_{X \sim P}[X]^2 && \text{(Another definition of variance)}\\
\end{aligned}
$$

## Standard deviation 
Standard deviation is the square root of variance:

$$
std(X) = \sqrt{Var(X)}
$$

>[!todo]
>- Do similar derivation for conditional expectations.
>- Covariance
>- Covariance matrix
>- Pearson correlation coefficient
