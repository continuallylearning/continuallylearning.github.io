---
tags:
  - "#clblogs"
aliases:
  - scalar field
  - vector field
---
# Vector Fields and Scalar Fields
>[!summary]
>> Vector fields are functions that map vectors to other vectors. 
>> Scalar fields are functions that map vectors to scalars.
>> A scalar field is a vector field with only one output dimension.

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