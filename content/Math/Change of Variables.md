---
tags:
  - "#clblogs"
aliases:
  - Change of Variables for Probability Distributions
  - change of variables
---
# Summary
Consider a random variable $x \in \mathbb{R}^n$ with a probability density function of $p_X(x)$. Let $g : \mathbb{R}^n \rightarrow \mathbb{R}^n$  be a function, and  $y=g(x)$. 

The *change of variables formula* defines the probability density function of the newly mapped random variable, $p_Y(y)$ as:

$$
p_Y(y) = \frac{p_X(g^{-1}(y))}{ \mid \det J_g(g^{-1}(y)) \mid }
\\
$$

Where on the right side, we have the two terms: 
1. In the numerator, original probability density function $p_X$  evaluated as the preimage of the input $g^{-1}(y)=x$.
2. In the denominator, the absolute value of the determinant of the Jacobian of the map, evaluated at the pre image of the input.

The intuition is that if $y=g(x)$, then it makes sense that the induced probability density of $y$ is related to the probability density of the corresponding random variable $x$, and these are equal for discrete distributions. However, for continuous random variables, we need to account for the fact that the function $g$ may be stretching the space, so that the probability density functions still integrate to 1 over their respective domains. The determinant of Jacobian $\det J_g$ represents how much the area is getting stretched by $g$ when going from $y = g(x)$. Since in the above equation, we're calculating $x = g^{-1}(y)$,  so we need to divide by the determinant of the jacobian of $g$ to correct for the volume expansion of $g$ (and therefore ensuring densities integrate to 1).

# Example: Gaussians
Consider:

$$
\begin{align}
p_X(x) &= N(x \mid 0, 1) && \text{(Normally distributed x)} \\
g(x) &= \sigma x + \mu \\
\end{align}
$$

Note that for a Gaussian distribution, the probability density function can be written as:

$$
\begin{align}
N(x|\mu,\sigma^2) = \frac{1}{\sqrt{2\pi \sigma^2}}\exp (- \frac{(x - \mu)^2}{2 \sigma^2})
\end{align}
$$


Then:

$$
\begin{align}
g^{-1}(y) &= \frac{(y - \mu)}{\sigma} \\
J_g(x) &= \frac{dg}{dx}(x)  \\
&= \sigma \\
p_Y(y) &= \frac{p_X(g^{-1}(y))}{ \mid \det J_g(g^{-1}(y)) \mid } \\
&= \frac{N(g^{-1}(y) \mid 0, 1)}{\mid \sigma \mid} \\
&= \frac{N(\frac{(y - \mu)}{\sigma} \mid 0, 1)}{\mid \sigma \mid}
\\
&= \frac{1}{\mid \sigma \mid \sqrt{2\pi}}\exp (- \frac{(\frac{(y - \mu)}{\sigma})^2}{2}) \\
&= \frac{1}{\sqrt{2\pi \sigma^2}}\exp (- \frac{(y - \mu)^2}{2\sigma^2}) \\
p_Y(y) &= N(y  \mid \mu, \sigma^2)

\end{align}
$$

The importance of this result is that we see two ways to calculate the probability density function $p_Y(y)$:

$$
\begin{align}
p_Y(y)&= \frac{N(\frac{(y - \mu)}{\sigma} \mid 0, 1)}{\mid \sigma \mid} \\
&= N(y  \mid \mu, \sigma^2)
\end{align}
$$

The first approach first calculates $x = g^{-1}(y) = \frac{y - \mu}{\sigma}$, then gets the probability density value of that transformed value according to the original pdf $N(x|0,1)$, and then performs an additional correction (divide by $\mid \sigma \mid$).  This is used in the [reparameterization trick](Gradients%20of%20random%20variables.md).

The second approach simply evaluates the probability density value of the input $y$ for a Gaussian parameterized by mean $\mu$ and standard deviation $\sigma$. 

As we have shown, these are equivalent ways of calculating the pdf.

# References
- https://www.cs.ubc.ca/~murphyk/Teaching/Stat406-Spring08/homework/changeOfVariablesHandout.pdf
- https://tutorial.math.lamar.edu/classes/calciii/changeofvariables.aspx