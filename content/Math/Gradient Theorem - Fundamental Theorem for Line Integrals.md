---
aliases:
---
# Prerequisites
- [Fundamental Theorem of Calculus](Fundamental%20Theorem%20of%20Calculus.md)
- [[Line integral]]

# Gradient Theorem
The gradient theorem, also known as the fundamental theorem of calculus for line integrals, says that the line integral of the gradient of a scalar field, $\nabla f$ over a curve $\gamma$ with start and end points $a$ and $b$, is equal to evaluating the difference of the original scalar field $f$ at the endpoints $b$ and $a$:

$$
\int_a^b (\nabla f) \cdot d\vec{r} = f(b) - f(a)
$$

Where $d\vec{r}$ is the tangent vector to the curve $\gamma$. 

## Proof
Let $f : \mathbb{R}^n \rightarrow \mathbb{R}$ be a scalar field, and $\gamma$ be a differentiable curve in $\mathbb{R}^n$ starts at $a \in \mathbb{R}^n$ and ends at $b \in \mathbb{R}^n$. We can parameterize the curve $\gamma$ with a differentiable function $r(t) : [0,1] \rightarrow \mathbb{R}^n$, so that $r(0) = a$ and $r(1) = b$. 

Note that $f(r(t))$ represents the scalar field $f$ evaluated along the curve $\gamma$ at time $t$. $f(r(t))$ maps $\mathbb{R} \rightarrow \mathbb{R}^n \rightarrow \mathbb{R}$ by mapping time to a configuration and then to a scalar.  If we take the derivative with respect to time, then using the chain rule, we get the [[Directional derivative]] for $f$ in the direction of $r'(t) = \frac{d}{dt} r(t)$, which is the tangent vector to the curve $\gamma$: 

$$
\frac{d}{dt}(f(r(t))) = \nabla f(r(t)) \cdot r'(t)
$$

We can now prove the Gradient Theorem by applying the definition of a line integral, using the chain rule from above, and also applying the [Fundamental Theorem of Calculus](Fundamental%20Theorem%20of%20Calculus.md):

$$
\begin{align}
\int_a^b (\nabla f) \cdot d\vec{r} &=  \int_0^1 \nabla f(r(t)) \cdot r'(t) dt && \text{(Def. of line integral)} \\
&=  \int_0^1 \frac{d}{dt}(f(r(t)))dt && \text{(Substitute via chain rule)} \\
&=  f(r(1)) - f(r(0)) && \text{(Fundamental Theorem of Calculus)} \\
&=  f(b) - f(a) && \text{(Substitute in } r(0)=a, r(1) = b \text{)} \\
\end{align}
$$
# References
- [Wikipedia on Gradient Theorem](https://en.wikipedia.org/wiki/Gradient_theorem)