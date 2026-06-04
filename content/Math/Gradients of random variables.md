---
tags:
  - "#clblogs"
aliases:
  - reparameterization trick
  - reinforce
---
# Summary

We will consider taking the gradients of three different expressions w.r.t $\theta$:

$$
\begin{align}
L_1(\theta) &= \mathbb{E}_{x \sim P}[F_\theta(x)] \\
L_2(\theta) &= \mathbb{E}_{x \sim P_\theta}[F(x)] \\
L_3(\theta) &= \mathbb{E}_{x \sim P_\theta}[F_\theta(x)] \\
\end{align}
$$

All three of these expressions are [Expected values](Expected%20value%20and%20variance.md) over the same two terms $F$ and $P$ (representing the integrand and probability distribution respectively), but the difference between these expressions is which of these terms depend on $\theta$.

This note derives the gradients for each of these objectives, with final forms shown below:

$$
\begin{align}
\nabla_\theta L_1(\theta)&= \mathbb{E}_{x \sim P}[\nabla_\theta F_\theta(x)] \\
\nabla_\theta L_2(\theta)&= \mathbb{E}_{x \sim P_\theta(x)}[F(x) \nabla_\theta \log P_\theta(x)]  && \text{(REINFORCE)}\\
&= \mathbb{E}_{z \sim N(0,I)}[\nabla_\theta F(g_\theta(z))] &&\text{(Reparameterization trick)}\\
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
- In $L_3$, both the probability $P$ and the integrand $F_\theta$ depend on $\theta$:
	- Example: In maximum entropy reinforcement learning, in addition to maximizing returns $G(\tau)$, we also want to maximize the entropy of the policy, $H(\pi_\theta)$. So now both the probability distribution $P_\theta(\tau)$ and the integrand $F_\theta(\tau) = G(\tau) + H(\pi_\theta)$ depend on the policy parameters $\theta$. 
	- Similar to the $L_2$ setting, calculating $\nabla_\theta L_3(\theta)$ is the policy gradient for a "fancier" objective than the standard RL one, and lets us iteratively update policy parameters $\theta$. 


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

But if we can't calculate $\nabla_\theta P_\theta(x)$, then we have two options:
1. The *REINFORCE Estimator* uses the log-derivate trick on the derivation above to actually derive an expectation, which we can then estimate numerically using law of large numbers, just like in $\nabla_\theta L_1$.
	1. We can ALWAYS do the REINFORCE estimator and get an unbiased estimate of the gradient $\nabla_\theta L_2$, but it can be high variance.
2. The *Reparamaterization trick* rewrites $L_2 = \mathbb{E}_{x \sim P_{\theta}}[F(x)]$  into an equivalent expectation that only has the model parameters $\theta$ in the integrand (instead of in the probability distribution), which means we can just apply the same techniques we did for $L_1$. This is done by reparametrizing the probability distribution and the integrand. This isn't always possible (i.e., requires $P_\theta$ to belong to a simple reparameterizable family, like Gaussians), but when possible, can provide a low-variance estimator for the gradient.

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

Let's **assume** that the probability distribution $P_\theta(x)$ belongs to a simple parameterized family of probability distributions, like the family of gaussian: $P_\theta(x) = N(x|\mu_\theta, \sigma_\theta^2)$, meaning the parameters $\theta$ are the mean and standard deviation of $P_\theta$ and is a gaussian distribution.  

The  [change of variables](Change%20of%20Variables%20for%20PDFs%20(Probability).md)  formula tells us that we can rewrite the probability density function $P_\theta(x)$  using a different probability density function that doesn't depend on the parameters, $P_Z(z) = N(0,1)$ , where $x=g_\theta(z)$, as long as we account for volume expansion of the pdf due to $g$ (i.e., the determinant of the [Jacobian](Jacobian.md)). For example, we know that (see [Example: Gaussians](Change%20of%20Variables%20for%20PDFs%20(Probability).md#Example%20Gaussians) for derivation):


$$
\begin{align}
P_\theta(x) &= N(x  \mid \mu_\theta, \sigma_\theta^2) \\
&= \frac{N(\frac{(x - \mu_\theta)}{\sigma_\theta} \mid 0, 1)}{\mid \sigma_\theta \mid} \\
&= \frac{N(g_\theta^{-1}(x) \mid 0, 1)}{\mid \sigma_\theta \mid} \\
&= \frac{N(z \mid 0, 1)}{\mid \sigma_\theta \mid} \\
&= \frac{P_Z(z)}{\mid \sigma_\theta \mid} \\
\end{align}
$$

where $g_\theta(z) = \sigma_\theta z + \mu_\theta$. 

What's important about this second way of writing $P_{\theta}(x) = \frac{N(\frac{(x - \mu_\theta)}{\sigma_\theta} \mid 0, 1)}{\mid \sigma_\theta \mid}$  is that the probability density function we are using no longer depends on $\theta$ (since the mean and std are 0 and 1 respectively. However, the value we evaluate at $z = g^{-1}_\theta(x) = \frac{x - \mu_\theta}{\sigma_\theta}$ and how we transform it (by dividing by $\sigma_\theta$) now do depend on $\theta$, where we did not have that in the first way of writing it $N(x  \mid \mu_\theta, \sigma_\theta^2)$: we just plugged in $x$ and returned the pdf for gaussian parameterized by $\mu_\theta, \sigma_\theta$. 

Note that we can relate the infinitesimal $dx$ and $dz$ through the Jacobian:

$$
\begin{align}
\frac{dx}{dz} &= J_{g_\theta} \\
dx &= \mid \det J_{g_\theta} \mid dz
\end{align}
$$

Because of this relationship, we can now rewrite $L_2$ using the probability density function that doesn't depend on the parameters $\theta$ to make it easy to take the gradient: 

$$
\begin{align}
L_2(\theta) &= \mathbb{E}_{x \sim P_{\theta}}[F(x)] \\
&= \int_X F(x)P_\theta(x)dx \\
&= \int_Z F(g_\theta(z))  P_{\theta}(g_\theta(z)) \mid \det J_{g_\theta} \mid dz && \text{(Change variables)} \\
&= \int_Z F(g_\theta(z)) \frac{N(z|0,1)}{\mid \det J_{g_\theta} \mid} \mid \det J_{g_\theta} \mid dz && \text{(Reparameterize pdf)}\\
&= \int_Z F(g_\theta(z)) N(z|0,1)dz  && \text{(Cancel det J)}\\
L_2(\theta) &= \mathbb{E}_{z \sim N(0,1)}[F(g_\theta(z))] \\
\end{align}
$$

By reparameterizing the expectation to be using the random variable $x' \sim N(0,I)$, we moved the $\theta$ from the probability distribution into the integrand. We could use other simple distributions instead of a gaussian as long as we can sample from it and transform the likelihoods between the simple distribution and original distribution (see [Change of Variables for PDFs (Probability)](Change%20of%20Variables%20for%20PDFs%20(Probability).md)).

### Numerical calculation

We can now numerically estimate this gradient the same way we did for $\nabla_\theta L_1$, which shows that we can move the gradient into the expectation (since only the integrand depends on $\theta$) and then estimate it numerically with law of large numbers:

$$
\begin{align}
L_2(\theta) &= \mathbb{E}_{z \sim N(0,I)}[F(g_\theta(z))] \\
\nabla_\theta L_2(\theta) &= \nabla_\theta \mathbb{E}_{z \sim N(0,I)}[F(g_\theta(z))] \\
&= \mathbb{E}_{z \sim N(0,I)}[\nabla_\theta F(g_\theta(z))] \\
&\approx \frac{1}{n} \sum_{i=1}^n\nabla_\theta F(g_\theta(z_i)) \\
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

&\approx \frac{1}{n} \sum_{i=1}^{n} \nabla_\theta F_\theta(x_i) + \frac{1}{n} \sum_{i=1}^n\nabla_\theta F(g_\theta(x_i')) && \text{(Reparameterization trick)} \\
&\text{where } x_i \sim P_\theta, x'_i \sim N(0,I)
\end{align}
$$

# Acknowledgements 
I’d like to thank [[Zoheb Anjum]] for providing useful feedback on the note (fixing math and typos).

# References
- [Gregory blog](https://gregorygundersen.com/blog/2018/04/29/reparameterization/)
	- shows derivation for derivative of expected value with parameter in distribution by using product rule, which is awesome / best way to describe how you can just “calculate gradient for a probability distribution” 
- [REINFORCE vs Reparameterization Trick](https://stillbreeze.github.io/REINFORCE-vs-Reparameterization-trick/)