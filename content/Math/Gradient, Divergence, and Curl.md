---
tags:
  - clwip
feed: show
date: 2026-01-14
---
# Vector Fields and Scalar Fields
>[!summary]
>> Vector fields are functions that map vectors to other vectors. 

A vector field $F : \mathbb{R}^n \rightarrow \mathbb{R}^m$ is a function that takes as input an $n$ dimensional vector and maps it to a $m$ dimensional vector:
$$
F(x_1,\ldots,x_n) = 
\begin{bmatrix}
F_1(x_1,\ldots,x_n) \\
\vdots \\
F_m(x_1,\ldots,x_n)
\end{bmatrix}
$$
The $i$th output of $F(x_1,\ldots,x_n)$ is $F_i(x_1,…,x_n)$, where each $F_i : \mathbb{R}^n \rightarrow \mathbb{R}$ is a scalar field that depends on the $n$ inputs $x_1,…,x_n$ and returns a scalar. In other words, a scalar field is a vector field whose output dimension $m=1$.

For the rest of the text, we will use $F$ to denote a vector field, and $f$ to denote a scalar field. 

# Del / Nabla Operator
>[!summary]
>> $\nabla$, called *nabla* or *del*, is an operator that acts like a function that operates on scalar and vector fields, and is fundamental to vector calculus. 

There are three ways to apply $\nabla$:
- [Gradient](#Gradient): $\nabla f$ 
	- scalar field $\rightarrow$ vector field
- [Divergence](#Divergence): $\nabla \cdot F$
	- vector field $\rightarrow$ scalar field
- [Curl](#Curl): $\nabla \times F$
	- vector field $\rightarrow$ vector field

These are fundamental operators for vector calculus. 
# Gradient
>[!summary]
>> a scalar field f maps an n dimensional input x to a single number. The gradient of that scalar field maps that input x to another n dimensional vector that (locally) maximizes f


Given a scalar field $f : \mathbb{R}^n \rightarrow \mathbb{R}$, the gradient of the scalar field $\nabla f : \mathbb{R}^n \rightarrow \mathbb{R}^n$  results is a vector field that maps an $n$ dimensional input to an $n$ dimensional output:

$$
(\nabla f)(x_1,…,x_n) = 
\begin{bmatrix}
\frac{\partial f}{\partial x_1}(x_1,…,x_n)\\
… \\

\frac{\partial f}{\partial x_n}(x_1,…,x_n)
\end{bmatrix}
$$

$(\nabla f)$ denotes taking the scalar field $f$ and calculating the gradient $\nabla f$, which is a vector field.
$(\nabla f)(x_1,…,x_n)$ represents evaluating the input $x$ for the resulting vector field, where the $i$th output is the partial derivative of the scalar field $f$ with respect to the $i$th input $x_i$: $\frac{\partial f}{\partial x_i}(x_1,…,x_n)$
# Divergence
Given a vector field $F : \mathbb{R}^n \rightarrow \mathbb{R}^n$, the divergence of the vector $\nabla \cdot F : \mathbb{R}^n \rightarrow \mathbb{R}$ field results in a scalar field that takes in an $n$ dimensional input:

$$
(\nabla \cdot F)(x_1,...,x_n) = \sum_{i=1}^n \frac{dF_i}{dx_i}(x_1,...,x_n)
$$
$(\nabla \cdot F)(x_1,...,x_n)$ denotes taking the vector field $F$ and calculating the divergence $\nabla \cdot F$, which is a scalar field.

If we think of the output vector of a vector field F as the flow of some quantity (the magnitude represents the amount of quantity, and the direction is the way it is flowing), then the divergence of the vector field tells us whether the amount of the quantity is being created, destroyed, or unchanged at each point in space. 

## Divergence-free vector field
If a vector field $F$ is divergence-free, then the following are all true:
1. $\nabla \cdot F = 0$ everywhere
2. 
# Curl

## Curlless Vector field
If a vector field $F$ is curlless, then the following are all true:
1. $\nabla \times F = 0$ : The curl of the vector field is 0 everywhere
2.  $\int_{a}^{b} F dl$ is path independent: The path integral of the vector field is independent of the path (and only depends on the starting points $a$ and $b$).
3. :$\oint F dl = 0$ :The closed path integral of the vector field is always 0.
4. There exists a scalar field $U$ such that $F = \nabla U$.

Let's first show that 1 implies 3:
Recall that Stokes Theorem tells us that the following is always true:
$$
\oint F dl = 
$$

# Green’s theorem
Greens theorem states that given a 2D dimensional vector field $F(x,y) = [P(x,y), Q(x,y)]$, there is a relationship between the path of $F$ around a closed loop $C$ and the enclosed area $D$:

$$
\oint_{C}F \cdot dl = \int \int_D (\nabla \times F) \cdot \hat{z} dA
$$
Note that since $dl = [dx, dy]$, then the dot product inside the integral on the left hand side can rewritten:
$$
F dl = P \partial x + Q \partial y
$$
And on the right hand side, the $z$ component of $\nabla \times F$ (which is the only non zero complement when dot producted with $\hat{z}$) is given by:

$$
(\nabla \times F) \cdot \hat{z} = \frac{\partial Q}{\partial x} - \frac{\partial P}{\partial y}
$$


# Divergence Theorem

# Stokes Theorem

# Flux

# Internal References
- vector calculus is relevant in [Electromagnetism](Electromagnetism.md)
# External References
- [Wikipedia on Del](https://en.wikipedia.org/wiki/Del)
- [Cool visualizations of divergence and curl](https://x.com/alec_helbling/status/2010714168421224784?s=46&t=-nw8YJi_X37DswYJuiAlDA)
- Griffiths E&M textbook 
- [Intuition into Green’s theorem](https://mathinsight.org/greens_theorem_idea)