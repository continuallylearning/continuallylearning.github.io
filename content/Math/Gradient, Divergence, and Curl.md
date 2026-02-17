---
tags:
feed: show
date: 2026-01-14
---
# Prerequisites
- [Fundamental Theorem of Calculus](Fundamental%20Theorem%20of%20Calculus.md)
-  [[Divergence Theorem]]
- [Green's Theorem](Green's%20Theorem.md)

# Vector Fields and Scalar Fields
>[!summary]
>> Vector fields are functions that map vectors to other vectors. 

A vector field $\vec{F} : \mathbb{R}^n \rightarrow \mathbb{R}^m$ is a function that takes as input an $n$ dimensional vector and maps it to a $m$ dimensional vector:

$$
\vec{F}(x_1,\ldots,x_n) = 
\begin{bmatrix}
F_1(x_1,\ldots,x_n) \\
\vdots \\
F_m(x_1,\ldots,x_n)
\end{bmatrix}
$$

The $i$th output of $\vec{F}(x_1,\ldots,x_n)$ is $F_i(x_1,…,x_n)$, where each $F_i : \mathbb{R}^n \rightarrow \mathbb{R}$ is a scalar field that depends on the $n$ inputs $x_1,\ldots,x_n$ and returns a scalar. In other words, a scalar field is a vector field whose output dimension $m=1$.

For the rest of the text, we will use $\vec{F}$ to denote a vector field, and $f$ to denote a scalar field. 

# Del / Nabla Operator
>[!summary]
>> $\nabla$, called *nabla* or *del*, is an operator that acts like a function that operates on scalar and vector fields, and is fundamental to vector calculus. 

There are three ways to apply $\nabla$:
- [Gradient](#Gradient): $\nabla f$ 
	- scalar field $\rightarrow$ vector field
- [Divergence](#Divergence): $\nabla \cdot \vec{F}$
	- vector field $\rightarrow$ scalar field
- [Curl](#Curl): $\nabla \times \vec{F}$
	- vector field $\rightarrow$ vector field

These are fundamental operators for vector calculus. 
# Gradient
>[!summary]
>> a scalar field f maps an n dimensional input x to a single number. The gradient of that scalar field maps that input x to another n dimensional vector that (locally) maximizes f


Given a scalar field $f : \mathbb{R}^n \rightarrow \mathbb{R}$, the gradient of the scalar field $\nabla f : \mathbb{R}^n \rightarrow \mathbb{R}^n$  results in a vector field that maps an $n$ dimensional input to an $n$ dimensional output:

$$
(\nabla f)(x_1,\ldots,x_n) = 
\begin{bmatrix}
\frac{\partial f}{\partial x_1}(x_1,\ldots,x_n)\\
\vdots \\
\frac{\partial f}{\partial x_n}(x_1,\ldots,x_n)
\end{bmatrix}
$$

$(\nabla f)$ denotes taking the scalar field $f$ and calculating the gradient $\nabla f$, which is a vector field.
$(\nabla f)(x_1,\ldots,x_n)$ represents evaluating the input $x$ for the resulting vector field, where the $i$th output is the partial derivative of the scalar field $f$ with respect to the $i$th input $x_i$: $\frac{\partial f}{\partial x_i}(x_1,\ldots,x_n)$.

Note that we could write $\vec{(\nabla f)}$ to emphasize applying the gradient results in a vector field, but it is implied by the fact that gradient always returns a vector field. 
# Divergence
Given a vector field $\vec{F} : \mathbb{R}^n \rightarrow \mathbb{R}^n$, the divergence of the vector field, $\nabla \cdot \vec{F} : \mathbb{R}^n \rightarrow \mathbb{R}$ , is a scalar field that takes in an $n$ dimensional input:

$$
(\nabla \cdot \vec{F})(x_1,\ldots,x_n) = \sum_{i=1}^n \frac{\partial F_i}{\partial x_i}(x_1,\ldots,x_n)
$$

$(\nabla \cdot \vec{F})(x_1,\ldots,x_n)$ denotes taking the vector field $\vec{F}$ and calculating the divergence $\nabla \cdot \vec{F}$, which is a scalar field.

If we think of the output vector of a vector field $\vec{F}$ as the flow of some quantity (the magnitude represents the amount of quantity, and the direction is the way it is flowing), then the divergence of the vector field tells us whether the amount of the quantity is being created, destroyed, or unchanged at each point in space. 

## Divergence-free vector field
If a vector field $\vec{F}$ is divergence-free, then the following are all true:
1. $\nabla \cdot \vec{F} = 0$ everywhere.
2. $\int_{S} \vec{F} \cdot d\vec{a}$ is independent of the open surface: the flux of an open surface only depends on its boundary.
3. $\oint_{S} \vec{F} \cdot d\vec{a} = 0$ : the surface integral over any closed surface is 0. 
4. $\vec{F} = \nabla \times \vec{U}$: The vector field is the curl of another vector field $\vec{U}$ .

>[!todo] 
> Proof
# Curl

Curl of vector field $\vec{F}$ is defined as the cross product of $\nabla$ and $\vec{F}$:

$$
\begin{align}
\nabla \times \vec{F} &= 
\begin{vmatrix}
\hat{x_1} & \hat{x_2} & \hat{x_3} \\
\frac{\partial}{\partial x_1} & \frac{\partial}{\partial x_2} & \frac{\partial}{\partial x_3} \\
F_{1} & F_{2} & F_{3}
\end{vmatrix}
\end{align}
$$

It is a measure of how much the vector field curls around the point we evaluate at. 

>[!todo] 
> Write the 2D version of curl, where only third component matters. Reference [Green's Theorem](Green's%20Theorem.md)

## Curl-free vector field
If a vector field $\vec{F}$ is curl-free, then the following are all true:
1. $\nabla \times \vec{F} = 0$ : The curl of the vector field is 0 everywhere
2. $\int_{a}^{b} \vec{F} \cdot d \vec{l}$ is path independent: The path integral of the vector field is independent of the path (and only depends on the starting points $a$ and $b$).
3. $\oint \vec{F} \cdot d\vec{l} = 0$ :The closed path integral of the vector field is always 0.
4. There exists a scalar field $f$ such that $\vec{F} = \nabla f$.

>[!summary] Proof summary
>> We will first assume (4), then prove (4) -> (2) -> (3) -> (1)

Let's start by assuming (4) is true, meaning:

$$
\vec{F} = \nabla{f}
$$
We can prove (2) $\int_{a}^{b} \vec{F} \cdot d\vec{l}$ is path independent, because of the [[Gradient Theorem - Fundamental Theorem for Line Integrals]]:


$$
\begin{align}
\int_{a}^b \vec{F} \cdot d\vec{l} &= \int_a^b (\nabla f) \cdot d \vec{l} && \text{(Substitution of} \vec{F} = \nabla{f} \text{)}\\
&= f(b) - f(a) && \text{(Gradient Theorem)}
\end{align}
$$

This implies that the path integral $\int_{a}^{b} \vec{F} \cdot d\vec{l}$  is independent of the path, and only depends on the endpoints $a$ and $b$. 

From (2) we can prove (3) $\oint \vec{F} \cdot d\vec{l} = 0$ :

$$
\begin{align}
\oint \vec{F} \cdot d\vec{l} &= \int_{a}^a \vec{F} \cdot d\vec{l} && \text{(Def. of closed-loop path integral)}\\
&= f(a) - f(a) && \text{(Proposition (2))} \\
&= 0 
\end{align}
$$

From (3), we can prove (1) $\nabla \times \vec{F} = 0$  using [Green's Theorem](Green's%20Theorem.md):

$$
\oint_C \vec{F} \cdot d\vec{l} = \int \int_D (\nabla \times \vec{F}) \cdot \hat{z} dA 
$$

Where $C$ is the boundary enclosing $D$. Since we can make $D$ as small as we want, this implies that the curl integrated over any infinitesimally small area is always 0, and therefore the curl must be 0 at every point, or in other words (1):

$$
\nabla \times \vec{F} = 0
$$

# Related ideas
- [[Divergence Theorem]]
- [Green's Theorem](Green's%20Theorem.md)
- [Fundamental Theorem for Divergences](Divergence%20Theorem.md)
- vector calculus is relevant in [Electromagnetism](Electromagnetism.md)
# External References
- [Wikipedia on Del](https://en.wikipedia.org/wiki/Del)
- [Cool visualizations of divergence and curl](https://x.com/alec_helbling/status/2010714168421224784?s=46&t=-nw8YJi_X37DswYJuiAlDA)
- Griffiths E&M textbook 