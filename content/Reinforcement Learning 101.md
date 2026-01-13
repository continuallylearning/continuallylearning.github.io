---
title: Reinforcement Learning 101
date: 2025-09-26
feed: show
---
Reinforcement Learning (RL) is a framework and collection of approaches for creating agents that can interact with environments to achieve goals. Some natural questions to ask are:
- What is an environment?
- What is an agent?
- What does it mean for an agent to interact with an environment?
- What is a goal?
- What ways can we make an agent achieve a goal by interacting with an environment?
We will answer all these questions, starting with defining what an environment is.

## Environment is modeled as a Markov Decision Process (MDP)

We model an environment as a Markov Decision Process (MDP), which is defined as a 5-tuple $MDP=\{S,A,T,R,\gamma\}$, where
- $S$ is the set of states
- $A$ is the set of actions
- $T(s' \mid s,a)=Pr(s' \mid s,a):S \times A  \times S \rightarrow [0,1]$  is the transition dynamics, and defines the (conditional) probability of transitioning to a state $s' \in S$ conditioned on executing action $a \in A$ from state $s \in S$.
- $R(r \mid s,a)=Pr(r \mid s,a):S \times A \times R \rightarrow [0,1]$ is the reward function, and defines the (conditional) probability of receiving a scalar reward $r$ for taking action $a$ from state $s$.
	- When the reward is a deterministic function of the state and action, you may see the reward function instead written not as a conditional probability, but as a function $R(r \mid s,a):S \times A \rightarrow R$ that maps state and actions directly to a scalar. Our probabilistic formulation generalizes this setting, and makes certain future definitions clearer as will be seen shortly.
- $\gamma \in [0,1]$ is the discount factor, and it determines how much less valuable rewards become when they are received in the future (This will be explained in more detail shortly)
To understand why an MDP is a useful model of an environment, let's define what an agent is (which is the entity that interacts with the environment).

## An agent is modeled as a policy

A policy $\pi(a \mid s)=Pr(a \mid s):S \times A \rightarrow [0,1]$ is the (conditional) probability of an action $a$ from a state $s$.
- Defining the policy to be deterministic (i.e: agent's behavior isn't stochastic as a function of state) is just a special case of our probabilistic definition.
Intuitively, an agent's behavior is determined by how it acts in different situations. A policy $\pi(a \mid s)$ determines how an agent acts in different situations, because we can sample actions from the policy given a state $a \sim \pi(* \mid s)$.

## Policies generate trajectories by interacting with environment

When an agent (policy) interacts with an environment (MDP) for $T$ timesteps, it generates a trajectory $\tau_T$. More formally, we define a trajectory as a sequence of $T$ steps, where each step contains a state, action, and reward:

$$
\tau_T = \{s_0, a_0, r_0, s_1, a_1, r_1, ..., s_{T-1}, a_{T-1}, r_{T-1}, s_T\}
$$

- This definition of trajectory (where it ends with state) is somewhat arbitrary, this is just consistent with our following definitions.
Take note our definition of a trajectory $\tau_T$ has $T+1$ states in it, but only $T$ actions and rewards. Here is why: we can think of the environment at time $t=0$ as being in some start state $s_0$. The agent will then sample an action $a_0 \sim \pi(* \mid s_0)$, and the environment will produce a reward $r_0$ and next state $s_1$, and we call this a step. So each step generates an action, reward, and (next) state, and so if we take $T$ steps, and we started with an initial state, we get $T+1$ states and $T$ actions and rewards.

## Trajectories have a cumulative discounted reward

To formalize the idea that some trajectories are "better", we want to be able to associate a scalar number to each trajectory, and "better" trajectories should be assigned higher numbers. The best trajectory has the bigger number! We formalize this by defining a function $G: \mathcal{T} \rightarrow \mathbb{R}$ that maps a trajectory to a single number representing the cumulative discounted rewards:

$$
G(\tau_T) = \sum_{t=0}^{T-1} \gamma^t \cdot r_t
$$

- When the discount factor $\gamma=1$, then this is just the cumulative rewards of the trajectory. Since $\gamma$ is between $0$ and $1$ and is exponential in time, we are (exponentially) discounting future rewards, which is why we call $G(\tau)$ the cumulative discounted rewards.
Cumulative discounted rewards $G(\tau)$ are crucial for defining what we mean by goals. Although trajectories $\tau$ that make $G(\tau)$ go up high are "good", we also have to account for how likely the trajectory is.

## Probability of trajectory depends on MDP and policy

We can formalize our aforementioned intuition for how an agent interacts with an environment by defining the probability of a trajectory in a highly factorized way. We won't go into details here (it is best described using graphical models), but our intuition is equivalent to making a bunch of conditional independence assumptions that are implied by our definitions of the transition dynamics, reward function, and policy all being function of the current state (and action). This is what the word *Markov* in MDP refers to.

For a specific environment $MDP$ and policy $\pi$, we can define the probability of a trajectory $\tau_T$ as:

$$
Pr_{\pi,T,R}(\tau_T) = Pr(s_0) \cdot \prod_{t=1}^{T} \pi(a_{t-1} \mid s_{t-1}) \cdot R(r_{t-1} \mid s_{t-1},a_{t-1}) \cdot T(s_t \mid s_{t-1},a_{t-1})
$$

- We add $\pi$, $T$, and $R$ to the subscript of $Pr(\tau)$ to emphasize that this probability depends on the distributions of the policy, transition dynamics, and reward function, but when that is not relevant, we just refer to it as $Pr(\tau)$.
- $Pr(s_0)=d(s):S \rightarrow [0,1]$ is called the starting state distribution, and is a distribution over starting states (i.e: how likely a state $s$ is to be the state the agent starts in). This (marginal) distribution can be dependent (or independent) of the policy and transition dynamics.
- Homework Problem: Getting the time indices to all line up takes care! Confirm that our definition of $Pr(\tau_T)$ doesn't have an "index out of bounds" error.
Now that we have defined the (cumulative discounted) value of a trajectory, as well as the probability of a trajectory, we can define a goal via the *reward hypothesis*

## Goals are maximizing Cumulative Discounted Expected (CDE) rewards

[To quote Rich Sutton, the reward hypothesis is defined as](http://incompleteideas.net/rlai.cs.ualberta.ca/RLAI/rewardhypothesis.html):

"That all of what we mean by goals and purposes can be well thought of as maximization of the expected value of the cumulative sum of a received scalar signal (reward)."

We can formalize this statement as the following optimization problem: Find the policy $\pi$ from a space of policies $\Pi$ that maximizes the Cumulative Discounted Expected rewards (CDE):

$$
\pi^* = \arg\max_{\pi \in \Pi} E_{\tau \sim Pr(*)}[G(\tau)]
$$

- We've done all the work to define all the terms in this optimization problem. Note that $Pr(\tau)$ depends on $\pi$, $T$ and $R$, while $G(\tau)$ just depends on $\gamma$ (Confirm this by looking at the definition of $G(\tau)$ and $Pr(\tau)$ and see what is required to compute them given you have $\tau$ already). To emphasize this, we may write $Pr_{\pi,T,R}(\tau)$ to emphasize the probability depends on the policy, which we leverage shortly.
- We call $\pi^*$ the optimal policy, and "solving the RL problem" amounts to finding the optimal policy (or at least getting as "close" to optimal as possible). The optimal policy does not need to be unique (i.e: there may be multiple optimal policies for a single MDP).
In order to find optimal policies, it helps to understand the CDE rewards of a policy from different states. We formalize this with a value function $V^\pi(s)$

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



