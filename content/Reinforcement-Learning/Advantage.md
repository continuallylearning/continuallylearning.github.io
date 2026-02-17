---
title: Advantage
date: 2025-09-27
feed: show
tags:
---
In Reinforcement Learning (RL), we define the advantage $A^\pi(s,a)$ of a policy $\pi$ as:

$$
A^\pi(s,a) = Q^\pi(s,a) - V^\pi(s)
$$

One way to interpret the advantage is as the relative value of taking a certain action $a$ from a state $s$, and then following the policy (this is exactly what $Q^\pi(s,a)$ represents), as compared to the expected value of following the policy from state $s$ (this is exactly what the [Value function](Value%20function.md) $V^\pi(s)$ represents).

## Advantage is equal to expected TD error

Another way of interpreting the advantage is as the expected TD error:

$$
\begin{aligned}
A^\pi(s,a) &= Q^\pi(s,a) - V^\pi(s) && \text{(definition of advantage)} \\
&= E_{s',r \sim T,R}[r + \gamma \cdot V^\pi(s')] - V^\pi(s) && \text{(substitute Q)} \\
&= E_{s',r \sim T,R}[r + \gamma \cdot V^\pi(s') - V^\pi(s)] && \text{(linearity of expectation)}
\end{aligned}
$$

Where $T$, $R$, and $\gamma$ are the transition dynamics, reward function, and discount factor for the MDP. $r + \gamma V^\pi(s') - V^\pi(s)$ is the (one-step) TD-Error.

## Expected Advantage is 0

If we take the expectation of the advantage over the distribution of actions from the policy (in a given state $s$), we get the following:

$$
\begin{aligned}
E_{a \sim \pi(* \mid s)}[A^\pi(s,a)] &= E_{a \sim \pi(* \mid s)}[Q^\pi(s,a) - V^\pi(s)] && \text{(expectation applied to advantage definition)} \\
&= E_{a \sim \pi(* \mid s)}[Q^\pi(s,a)] - E_{a \sim \pi(* \mid s)}[V^\pi(s)] && \text{(linearity of expectation)} \\
&= V^\pi(s) - E_{a \sim \pi(* \mid s)}[V^\pi(s)] && \text{(substitute } E_{a \sim \pi(* \mid s)}[Q^\pi(s,a)] \text{ for } V^\pi(s) \text{)} \\
&= V^\pi(s) - V^\pi(s) && \text{(} V^\pi(s) \text{ doesn't depend on } a \text{ so expectation is ignored)} \\
&= 0
\end{aligned}
$$

This means that the expected advantage (under the policy) is 0, which makes sense when we consider the first interpretation of advantage (relative value of an action and the value of following the policy).

