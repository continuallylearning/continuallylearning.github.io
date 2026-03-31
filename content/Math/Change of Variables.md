---
tags:
  - "#clblogs"
aliases:
  - Change of Variables for Probability Distributions
  - change of variables
  - probability density function
  - pdf
  - cdf
  - cumulative distribution function
---
# References
- [libretexts]( https://stats.libretexts.org/Bookshelves/Probability_Theory/Probability_Mathematical_Statistics_and_Stochastic_Processes_(Siegrist)/03%3A_Distributions/3.07%3A_Transformations_of_Random_Variables) writeup on change of variables is amazing. A lot of this is lifted from that but rewritten based on [Michael Fishman](Michael%20Fishman.md)'s more "direct" writing.

# Summary
Consider a random variable $X$ that takes on real values in $\mathbb{R}$, and let $f(x)$ be the probability density function (pdf) for the random variable $X$. Then we can define the cumulative distribution function (cdf) for the random variable as the integration of the pdf from $-\infty$ to $x$: 

$$
\begin{align}
F(x) &:= \mathbb{P}(X \leq x) \\
&= \int_{-\infty}^{x} f(t)dt \\
\rightarrow \frac{dF}{dx}(x) &= f(x) 
\end{align}
$$

Now let $r(X)=Y$ be a smooth, invertible function that induces a new random variable $Y$. We can calculate its pdf $g(y)$ by calculating its cdf $G(Y)$, in terms of $X$'s cdf, which we know, and then take the derivative to get the pdf. We present the final form that this note derives.

## Key takeaways

For single variable:

$$
\begin{align}
g(y) &= f(r^{-1}(y)) \mid \frac{d}{dy}r^{-1}(y) \mid \\
&= f(x) \mid \frac{dx}{dy}(y) \mid &&\text{where } x = r^{-1}(y)\\
&= \frac{f(x)}{\mid \frac{dy}{dx}(x)\mid}\\
\end{align}
$$

For multivariate setting:

$$
\begin{align}
g(y) &= f(x) \mid \det J_{r^{-1}} (y)\mid &&\text{where } x = r^{-1}(y)\\ \\
 &= \frac{f(x)}{\mid \det J_{r} (x)\mid }
\end{align}
$$

# Monotonically increasing derivation

$$
\begin{align}
G(y) &=  \mathbb{P}(Y \leq y) \\
&= \mathbb{P}(r(X) \in (-\infty,y]) \\
&= \mathbb{P}(X \in r^{-1}((-\infty,y])) \\
&= \int_{-\infty}^{r^{-1}(y)} f(x)dx && \text{(Monotonically increasing)}\\
&= F(r^{-1}(y)) \\
\rightarrow g(y) &= \frac{d}{dy}G(y) \\
&= \frac{d}{dy}F(r^{-1}(y)) \\
&= \frac{d}{dx}F(r^{-1}(y)) \frac{d}{dy}r^{-1}(y) && (\frac{dF}{dy} = \frac{dF}{dx}\frac{dx}{dy}) \\
&= f(r^{-1}(y)) \frac{d}{dy}r^{-1}(y) && (\frac{dF}{dx}(x) = f(x)) \\
\end{align}
$$

# Monotonically decreasing derivation

$$
\begin{align}
G(y) &=  \mathbb{P}(Y \leq y) \\
&= \mathbb{P}(r(X) \in (-\infty,y]) \\
&= \mathbb{P}(X \in r^{-1}((-\infty,y])) \\
&= \int_{r^{-1}(y)}^{\infty} f(x)dx && \text{(Monotonically decreasing)}\\
&= \mathbb{P}[X >r^{-1}(y)]\\
&= 1 - F(r^{-1}(y))  && \text{(Complement of } F(r^{-1}(y))) \\ 
\rightarrow g(y) &= \frac{d}{dy}G(y) \\
&= \frac{d}{dy}(1 - F(r^{-1}(y)) ) \\
&= - \frac{d}{dx}F(r^{-1}(y)) \frac{d}{dy}r^{-1}(y) && (\frac{dF}{dy} = \frac{dF}{dx}\frac{dx}{dy}) \\
&= - f(r^{-1}(y)) \frac{d}{dy}r^{-1}(y) && (\frac{dF}{dx}(x) = f(x)) \\
\end{align}
$$

The one-to-one assumption for $r$ was required to let us assume the inverse $r^{-1}(Y)=X$  is well define. The main difference between the monotonically increasing/decreasing setting is that $r^{-1}((-\infty, y]) = (- \infty, r^{-1}(y))$ for increasing, and $r^{-1}((-\infty, y]) = (r^{-1}(y), \infty)$ for decreasing.
# Combined change of variables formula
$g(y)$ looks similar for both monotonic increasing and decreasing, except a sign. However, since $r^{-1}(y)$ is monotonically decreasing when $r(y)$ is, then we know its derivative $\frac{d}{dy}r^{-1}(y)$ is also negative, so there is a negative component that will always cancel out. This means we can write $g(y)$ in a single form regardless if its increasing/decreasing by just taking the absolute value of the derivative:

$$
\begin{align}
g(y) &= f(r^{-1}(y)) \mid \frac{d}{dy}r^{-1}(y) \mid \\
&= f(x) \mid \frac{dx}{dy}(y) \mid && \text{where } x = r^{-1}(y)\\
&= \frac{f(x)}{\mid \frac{dy}{dx}(x)\mid}\\
\end{align}
$$

The last line is true because the derivatives of smooth, one-to-one functions are reciprocals (scalar inverses), so we can divide by the derivative of $r$ instead of multiply by its inverse. 

# Multivariable change of variables

Let $r : \mathbb{R}^n \rightarrow \mathbb{R}^n$ be a one-to-one, monotonically increasing or decreasing for each output dimensional, and differentiable function from $\mathbb{R}^n$ onto itself. The [Jacobian](Jacobian.md) of the inverse function $r^{-1}(y) = x$ is the $n \times n$ matrix of first partial derivatives:

$$
(J_{r^{-1}})_{ij} =  \frac{\partial x_i}{\partial y_j}
$$

We can use the determinant of the Jacobian to calculate the multivariate change of variables formula (more detailed geometric proof [here](https://web.williams.edu/Mathematics/sjmiller/public_html/probabilitylifesaver/supplementalchap_changeofvar.pdf)):

$$
\begin{align}
g(y) &= f(r^{-1}(y)) \mid \det J_{r^{-1}}(y)\mid \\
 &= f(x) \mid \det J_{r^{-1}}(y)\mid && \text{where } x = r^{-1}(y)\\
 &= \frac{f(x)}{\mid \det J_{r}(x)\mid}
\end{align}
$$

The determinant of Jacobian represents how much the area is getting stretched by the transformation when going from $y = r(x)$,  so we need to account for it to correct for the volume expansion of the transformation. 

# Example: Gaussians
Consider:

$$
\begin{align}
f(x) &= N(x \mid 0, 1) && \text{(Normally distributed x)} \\
r(x) &= \sigma x + \mu \\
\end{align}
$$

Note that for a Gaussian distribution, the probability density function can be written as:

$$
\begin{align}
N(x|\mu,\sigma^2) = \frac{1}{\sqrt{2\pi \sigma^2}}\exp (- \frac{(x - \mu)^2}{2 \sigma^2})
\end{align}
$$


Then if we have a new random variable $Y = r(X)$, it's pdf $g(y)$ can be calculated using change of variables formula, and we get the following:

$$
\begin{align}
r^{-1}(y) &= \frac{(y - \mu)}{\sigma} \\
J_r(x) &= \frac{dy}{dx}(x)  \\
&= \sigma \\
g(y) &= \frac{f(r^{-1}(y))}{ \mid \det J_r(r^{-1}(y)) \mid } \\
&= \frac{N(r^{-1}(y) \mid 0, 1)}{\mid \sigma \mid} \\
&= \frac{N(\frac{(y - \mu)}{\sigma} \mid 0, 1)}{\mid \sigma \mid}
\\
&= \frac{1}{\mid \sigma \mid \sqrt{2\pi}}\exp (- \frac{(\frac{(y - \mu)}{\sigma})^2}{2}) \\
&= \frac{1}{\sqrt{2\pi \sigma^2}}\exp (- \frac{(y - \mu)^2}{2\sigma^2}) \\
g(y) &= N(y  \mid \mu, \sigma^2)

\end{align}
$$

The importance of this result is that we see two ways to calculate the probability density function $g(y)$:

$$
\begin{align}
g(y)&= \frac{N(\frac{(y - \mu)}{\sigma} \mid 0, 1)}{\mid \sigma \mid} \\
&= N(y  \mid \mu, \sigma^2)
\end{align}
$$

The first approach first calculates $x = r^{-1}(y) = \frac{y - \mu}{\sigma}$, then gets the probability density value of that transformed value according to the original pdf $N(x|0,1)$, and then performs an additional correction (divide by $\mid \sigma \mid$).  This is used in the [reparameterization trick](Gradients%20of%20random%20variables.md).

The second approach simply evaluates the probability density value of the input $y$ for a Gaussian parameterized by mean $\mu$ and standard deviation $\sigma$. 

As we have shown, these are equivalent ways of calculating the pdf.

# References
- https://www.cs.ubc.ca/~murphyk/Teaching/Stat406-Spring08/homework/changeOfVariablesHandout.pdf
- https://tutorial.math.lamar.edu/classes/calciii/changeofvariables.aspx
- https://tutorial.math.lamar.edu/classes/calci/substitutionruleindefinite.aspx
- https://tutorial.math.lamar.edu/classes/calci/Differentials.aspx
- https://stats.libretexts.org/Bookshelves/Probability_Theory/Probability_Mathematical_Statistics_and_Stochastic_Processes_(Siegrist)/03%3A_Distributions/3.07%3A_Transformations_of_Random_Variables