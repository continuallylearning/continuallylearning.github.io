---
tags:
  - "#clblogs"
aliases:
  - reparameterization trick
  - reinforce
---
# Summary

We will consider taking the gradients of three different expression w.r.t $\theta$:

$$
\begin{align}
L_1(\theta) &= \mathbb{E}_{x \sim P}[F_\theta(x)] \\
L_2(\theta) &= \mathbb{E}_{x \sim P_\theta}[F(x)] \\
L_3(\theta) &= \mathbb{E}_{x \sim P_\theta}[F_\theta(x)] \\
\end{align}
$$

All three of these expressions are [Expected values](Expected%20value%20and%20variance.md) over the same two terms $F$ and $P$ (representing the integrand and probability distribution respectively), but the difference between these expressions is which of these terms depend on $\theta$.

This note derives the gradients for each of these objectives, which we show now for clarity:

$$
\begin{align}
\nabla_\theta L_1(\theta)&= \mathbb{E}_{x \sim P}[\nabla_\theta F_\theta(x)] \\
\nabla_\theta L_2(\theta)&= \mathbb{E}_{x \sim P_\theta(x)}[F(x) \nabla_\theta \log P_\theta(x)]  && \text{(REINFORCE)}\\
&= \mathbb{E}_{x' \sim N(0,I)}[\nabla_\theta F(G_\theta(x'))] &&\text{(Reparameterization trick)}\\
\nabla_\theta L_3(\theta) &= \nabla_\theta L_1(\theta) + \nabla_\theta L_2(\theta)
\end{align}
$$

## Motivation
- In $L_1$, only the argument to the expected value $F$ depends on $\theta$. 
	- Example: In [[supervised learning]], $\theta$ represents the parameters to a model, $P$ is a fixed dataset (and therefore does not change as we change our model parameters), and $F_{\theta}(x)$ is a loss function on $\theta$ for a sample from the dataset.
	- Calculating $\nabla_\theta L_1(\theta)$ lets us use gradient descent to iteratively update the model weights $\theta$ to better fit the dataset. 
- In $L_2$, only the probability to the expected value $P$ depends on $\theta$.
	- Example: In [Reinforcement Learning](Reinforcement%20Learning.md), the [Value function](Value%20function.md) for a policy $\pi_\theta$  in a given state $s$ is an expected value. The probability of a trajectory $P_\theta(\tau)$ depends on the policy parameters, and the integrand $G(\tau) = F$ is the cumulative discounted rewards of a trajectory and therefore does not depend on the policy parameters $\theta$.  
	- Calculating $\nabla_\theta L_2(\theta)$ is called the [policy gradients](RL4Robots%20-%20Policy%20Gradients.md) and lets us use gradient descent to iteratively update the policy parameters $\theta$ to maximize cumulative discounted expected rewards.
- In $L_3$, both the probability $P$ and the integrand $\theta$ depend on $\theta$:
	- Example: In maximum entropy reinforcement learning, in addition to maximizing returns $G(\tau)$, we also want to maximize the entropy of the policy, $H(\pi_\theta)$. So now both the probability distribution $P_\theta(\tau)$ and the integrand $F_\theta(\tau) = G(\tau) + H(\pi_\theta)$ depend on the policy parameters $\theta$. 
	- Similar to the $L_2$ setting, calculating $\nabla_\theta L_3\theta)$ is the policy gradient for a "fancier" objective than the standard RL one, and lets us iteratively update policy parameters $\theta$. 


We will now address in turn how to calculate the gradients $\nabla_\theta L_1(\theta), \nabla_\theta L_2(\theta), \nabla_\theta L_3(\theta)$
# 1) $\nabla_\theta L_1$: Integrand F depends on parameters

$$
\begin{align}
L_1(\theta) &= \mathbb{E}_{x \sim P}[F_\theta(x)] \\
&= \sum_{x}F_\theta(x)P(x) \\
\nabla_\theta L_1(\theta) &= \nabla_\theta  \mathbb{E}_{x \sim P}[F_\theta(x)] \\
&= \nabla_\theta \sum_{x}F_\theta(x)P(x) \\
&= \sum_{x} \nabla_\theta (F_\theta(x)P(x)) && \text{(Linearity of gradient)} \\
&= \sum_{x} \nabla_\theta F_\theta(x)P(x) +  F_\theta(x)\nabla_\theta P(x)  && \text{(Product rule)} \\
&= \sum_{x} \nabla_\theta F_\theta(x)P(x) && ( F_\theta(x)\nabla_\theta P(x)  = 0 \text{ since } \nabla_\theta P(x)  = 0 )  \\
&= \mathbb{E}_{x \sim P}[\nabla_\theta F_\theta(x)] \\
\end{align}
$$

## Numerical calculation
Because $\nabla_\theta L_1(\theta) =  \mathbb{E}_{x \sim P}[\nabla_\theta F_\theta(x)]$, we can approximate the gradient of $L_1$ w.r.t $\theta$ by using the [[law of large numbers]] and use the empirical average to estimate the expectation by taking an average of the gradients over the dataset:

$$
\begin{align}
\nabla_\theta L_1(\theta)
&= \mathbb{E}_{x \sim P}[\nabla_\theta F_\theta(x)] \\
&\approx \frac{1}{n} \sum_{i=1}^{n} \nabla_\theta F_\theta(x_i) \\
\end{align}
$$

# 2) $\nabla_\theta L_2$: Probability P depends on parameters

For $L_1(\theta)$, we just calculated the gradient directly, and ended up with an expression that was an expectation, which made estimating $\nabla_\theta L_1$ straight-forward by using monte-carlo estimates.

Let's try to take the gradient of $L_2$ in a way similar to how we did $L_1$:

$$
\begin{align}
L_2(\theta) &= \mathbb{E}_{x \sim P_{\theta}}[F(x)] \\
&= \sum_{x}F(x)P_\theta(x) \\
\nabla_\theta L_2(\theta) &= \nabla_\theta  \mathbb{E}_{x \sim P_\theta}[F(x)] \\
&= \nabla_\theta \sum_{x}F(x)P_\theta(x) \\
&= \sum_{x} \nabla_\theta (F(x)P_\theta(x)) && \text{(Linearity of gradient)} \\
&= \sum_{x} \nabla_\theta F(x)P_\theta(x) +  F(x)\nabla_\theta P_\theta(x)  && \text{(Product rule)} \\
&= \sum_{x}   F(x)\nabla_\theta P_\theta(x)  && (\nabla_\theta F(x)P_\theta(x)  = 0 \text{ since } \nabla_\theta F(x)  = 0 )  \\
\end{align}
$$

Because $F(x)$ isn't necessarily a probability distribution, we can't do the same step as we did in $\nabla_\theta L_1$ where we got an expectation immediately. If we can analytically calculate $\nabla_\theta P_\theta(x)$ and summation, then in theory we can calculate $\nabla_\theta L_2$ directly. 

But if we can't calculate $\nabla_\theta P_\theta(x)$, then we have two options: The 
1. The *REINFORCE Estimator* uses the log-derivate trick on the derivation above to actually derive an expectation, which we can then estimate numerically using law of large numbers, just like in $\nabla_\theta L_1$.
	1. We can ALWAYS do the REINFORCE estimator and get an unbiased estimated of the gradient $\nabla_\theta L_2$, but it can be high variance.
2. The *Reparamaterization trick* rewrites $L_2 = \mathbb{E}_{x \sim P_{\theta}}[F(x)]$  into an equivalent expectation that only has the model parameters $\theta$ in the integrand (instead of in the probability distribution), which means we can just apply the same techniques we did for $L_1$. This is done by reparametrizing the probability distribution and the integrand. This isn't always possible (i.e., useful to use a simple parameterized probability distribution family for $P_\theta$), but when possible, can provide a low-variance estimator for the gradient.

## REINFORCE Estimator

Let us continue the derivation for $\nabla_\theta L_2$ from above by using the [[log-derivative trick]]: 

$$
\begin{align}
\nabla_\theta L_2(\theta) &= \sum_{x}   F(x)\nabla_\theta P_\theta(x)    \\
&= \sum_{x}   F(x) P_\theta(x)\nabla_\theta \log P_\theta(x) && \text{(Log-derivative trick)}   \\
&= \mathbb{E}_{x \sim P_\theta(x)}[F(x) \nabla_\theta \log P_\theta(x)]   \\
\end{align}
$$

By using the log-derivative trick, we got back a $P_\theta(x)$, which we could then use to rewrite $\nabla_\theta L_2$ as an expectation. Notice that our new expectation still has the probability distribution depend on $\theta$, but that is fine because it is exactly equal to what we want: $\nabla_\theta L_2$. 

### Numerical calculation

This expectation is different from $\nabla_\theta L_1$, but we can estimate it numerically the same way via law of large numbers:

$$
\begin{align}
\nabla_\theta L_2(\theta)
&= \mathbb{E}_{x \sim P_\theta(x)}[F(x) \nabla_\theta \log P_\theta(x)] \\
&\approx \frac{1}{n} \sum_{i=1}^{n} F(x_i) \nabla_\theta \log P_\theta(x_i) \\
\end{align}
$$
## Reparameterization trick

The reparameterization also rewrites $\nabla_\theta L_2$ as an expectation, but rather than try to continue on from our original derivation, it reparametrizes the probability distribution and the integrand by making a new probability distribution that doesn't depend on the parameters $\theta$, and introduces them into the integrand by using a deterministic transformation from the simple distribution to the original one.

Let's **assume** that the probability distribution $P_\theta(x)$ belongs to a simple parameterized family of probabiltiy distributions, like the family of gaussian: $P_\theta(x) = N(x|\mu_\theta, \sigma_\theta^2)$, meaning the parameters $\theta$ are the mean and standard deviation of $P_\theta$ and is a gaussian distribution.  

Because we assumed $P_\theta(x)$ is a gaussian, we can introduce a deterministic function $G_\theta(x) = \frac{(x - \mu_\theta)}{\sigma_\theta}$, which has the following property:

$$
\begin{align}
Pr(x) &= N(x|0,I) \\
&= P_\theta(G_\theta(x))  \\
&= N(x|\mu_\theta, \sigma_\theta^2)
\end{align}
$$

- [ ] prove the above

In short, the probability of a sample $x \sim N(x|0,I)$ from the unit gaussian is equal to the same probability of the transformed sample $G_\theta(x) \sim P_\theta(x)$ from the original distribution. Because of this relationship, we can now rewrite $L_2$ in a different way that makes it easy to take the gradient: 

$$
\begin{align}
L_2(\theta) &= \mathbb{E}_{x \sim P_{\theta}}[F(x)] \\
&= \sum_{x}F(x)P_\theta(x) \\
&= \sum_{x'}F(G_\theta(x'))N(x') \\
L_2(\theta) &= \mathbb{E}_{x' \sim N(I,0)}[F(G_\theta(x'))] \\
\end{align}
$$

By reparameterizing the expectation to be using the random variable $x' \sim N(0,I)$, we moved the $\theta$ from the probability distribution into the integrand. We could use other simple distributions instead of a gaussian as long as we can sample from it and transform the likielihoods between the simple distribution and original distribution (see [change of variables for probability distributions](change%20of%20variables%20for%20probability%20distributions.md)).

### Numerical calculation

We can now numerically estimate this gradient the same way we did for $\nabla_\theta L_1$, which shows that we can move the gradient into the expectation (since only the integrand depends on $\theta$) and then estimate it numerically with law of large numbers:

$$
\begin{align}
L_2(\theta) &= \mathbb{E}_{x' \sim N(I,0)}[F(G_\theta(x'))] \\
\nabla_\theta L_2(\theta) &= \nabla_\theta \mathbb{E}_{x' \sim N(I,0)}[F(G_\theta(x'))] \\
&= \mathbb{E}_{x' \sim N(I,0)}[\nabla_\theta F(G_\theta(x'))] \\
&\approx \frac{1}{n} \sum_{i=1}^n\nabla_\theta F(G_\theta(x_i')) \\
\end{align}
$$

- [ ] todo: show the computational graph for reparameterization trick, and how it makes it so gradient doesn't backprop through stochastic sampling due to deterministic transformation.
- [ ] Prove this is lower variance than REINFORCE. Discuss importance of variance/bias (potentially another blog post).

# 3) $\nabla_\theta L_3$: Probability P and F both depend on parameters

For $\nabla_\theta L_3$, let's follow a similar derivation for directly calculating the gradient like we did for $\nabla_\theta L_1$ and in the REINFORCE estimator for $\nabla_\theta L_2$:

$$
\begin{align}
L_3(\theta) &= \mathbb{E}_{x \sim P_\theta}[F_\theta(x)] \\
&= \sum_{x}F_\theta(x)P_\theta(x) \\
\nabla_\theta L_3(\theta) &= \nabla_\theta  \mathbb{E}_{x \sim P_\theta}[F_\theta(x)] \\
&= \nabla_\theta \sum_{x}F_\theta(x)P_\theta(x) \\
&= \sum_{x} \nabla_\theta (F_\theta(x)P_\theta(x)) && \text{(Linearity of gradient)} \\
&= \sum_{x} \nabla_\theta F_\theta(x)P_\theta(x) +  F_\theta(x)\nabla_\theta P_\theta(x)  && \text{(Product rule)} \\
&= \sum_{x} \nabla_\theta F_\theta(x)P_\theta(x) + \sum_{x} F_\theta(x)\nabla_\theta P_\theta(x)  \\
&= \mathbb{E}_{x \sim P(x)}[\nabla_\theta F_\theta(x)]  +   \sum_{x} F_\theta(x)\nabla_\theta P_\theta(x)  \\
&= \nabla_\theta\mathbb{E}_{x \sim P(x)}[ F_\theta(x)]  +   \sum_{x} F_\theta(x)\nabla_\theta P_\theta(x)  \\
&= \nabla_\theta\mathbb{E}_{x \sim P(x)}[ F_\theta(x)]  +  \nabla_\theta\mathbb{E}_{x \sim P_\theta(x)}[ F(x)]   \\
&= \nabla_\theta L_1(\theta)  +  \nabla_\theta L_2(\theta)   \\
\end{align}
$$

As we can see, for calculating $\nabla_\theta L_3$, where both the probability $P_\theta$ and integrand $F_\theta$ depend on the parameters $\theta$, we just need to sum the gradients for the two cases where only either $F$ or $P$ depend on $\theta$.

### Numerical calculation

This means that when we want to numerically calculate $\nabla_\theta L_3$, we can calculate the left term using the one approach we discussed for $\nabla_\theta L_1$, and for the right term, we can either use the REINFORCE estimator or the reparameterization trick:


$$
\begin{align}
\nabla_\theta L_3(\theta) &= \nabla_\theta L_1(\theta)  +  \nabla_\theta L_2(\theta) \\
&\approx \frac{1}{n} \sum_{i=1}^{n} \nabla_\theta F_\theta(x_i)  +  \frac{1}{n} \sum_{i=1}^{n} F(x_i) \nabla_\theta \log P_\theta(x_i) && \text{(REINFORCE)}\\

&\approx \frac{1}{n} \sum_{i=1}^{n} \nabla_\theta F_\theta(x_i) + \frac{1}{n} \sum_{i=1}^n\nabla_\theta F(G_\theta(x_i')) && \text{(Reparameterization trick)} \\
&\text{where } x_i \sim P_\theta, x'_i \sim N(0,I)
\end{align}
$$



# References
- [Old - Reparameterization trick](Old%20-%20Reparameterization%20trick.md)
	- my original post, I should probably just grab the computational graph and then delete it since this post now captures everything
- [Gregory blog](https://gregorygundersen.com/blog/2018/04/29/reparameterization/)
	- shows derivation for derivative of expected value with parameter in distribution by using product rule, which is awesome / best way to describe how you can just “calculate gradient for a probability distribution” 
	- Id like to rewrite my blog using his starting point (both probability and argument for expected value), but make connection to REINFORCE (when argument doesn’t depend on parameters, just probability.)
- [REINFORCE vs Reparameterization Trick](https://stillbreeze.github.io/REINFORCE-vs-Reparameterization-trick/)