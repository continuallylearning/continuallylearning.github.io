---
title: Value function
feed: show
date: 2025-09-26
tags:
---
## Prerequisites
- [Reinforcement Learning 101](Reinforcement%20Learning%20101.md)
## Value functions represent CDE rewards from a state

The value function $V^\pi(s)$ is defined as the CDE rewards conditioned on running the policy $\pi$ from a given state $s$:

$$
V^\pi(s) = E_{\tau \sim Pr(*)}[G(\tau) \mid s_0=s]
$$

Given a policy $\pi$ and an MDP, estimating $V^\pi(s)$ is called *policy evaluation*. One way to understand why the value function is useful (i.e: why evaluating a policy is useful) is to define our objective above with it:

$$
\pi^* = \arg\max_{\pi \in \Pi} E_{\tau \sim Pr(*)}[G(\tau)] = \arg\max_{\pi \in \Pi} E_{s \sim d(*)}[V^\pi(s)]
$$

Notice how in the first line, the expectation is over a distribution of trajectories, while in the second line, the expectation is over a distribution of (starting) states. This means that the cumulative discounted expected rewards of a policy (over a distribution of trajectories) is equal to the expected value function of a policy (over the distribution of starting states).