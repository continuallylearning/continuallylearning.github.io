---
tags:
url: https://en.wikipedia.org/wiki/Green%27s_theorem#Relationship_to_the_divergence_theorem
---
# Prerequisites 
- [Gradient, Divergence, and Curl](Gradient,%20Divergence,%20and%20Curl.md)
- [Green's Theorem](Green's%20Theorem.md)
- [Divergence Theorem](Divergence%20Theorem.md)

# Summary
>[!summary] We will show that [Green's Theorem](Green's%20Theorem.md) is the two-dimensional version of [Divergence Theorem](Divergence%20Theorem.md). 

This is not immediately obvious, since (loosely) Green's theorem relates integrals involving tangent vectors to integrals involving curls, while Divergence theorem relates integrals involving normal vector to integrals involving divergence. 

Another way of saying this is via the following table:

|                                               | (Over 2D area) Curl or Divergence? | (Over 1D boundary) tangent or normal vector? | Equation                                                                               |
| --------------------------------------------- | ---------------------------------- | -------------------------------------------- | -------------------------------------------------------------------------------------- |
| [Green's Theorem](Green's%20Theorem.md)       | Curl $\nabla \times \vec{F}$       | Tangent vector $d\vec{l}$                    | $\oint_{B}\vec{F} \cdot d\vec{l} = \int \int_C (\nabla \times \vec{F}) \cdot d\vec{A}$ |
| [Divergence Theorem](Divergence%20Theorem.md) | Divergence $\nabla \cdot \vec{F}$  | Normal vector $d\vec{n}$                     | $\oint_{B} \vec{F} \cdot d\vec{n} = \int \int_C (\nabla \cdot \vec{F}) dA$             |

We will start with Green's theorem, and show how it is equivalent to stating the Divergence Theorem. 

The rest of this note gives a step-by-step derivation, but we present the condensed derivation right here, which also clearly shows the relationship between the curl of a vector field $\nabla \times \vec{F}$ and the divergence of a rotated version of that vector field, $\nabla \cdot \vec{\dot{F}}$. 

Feel free to skip the condensed derivation and revisit it after you've read the entire note:
## Condensed derivation

$$
\begin{align}
\oint_{B}\vec{F} \cdot d\vec{l} &= \int \int_C (\nabla \times \vec{F}) \cdot d\vec{A} && \text{(Green's Theorem)} \\
\oint_{B}F_1dx_1 + F_2 dx_2 &= \\
\oint_{B} \begin{bmatrix}
F_2 \\
-F_1
\end{bmatrix}
\cdot
\begin{bmatrix}
d x_2 \\
-dx_1
\end{bmatrix} &= \\
\oint_B \vec{\dot{F}}
\cdot d\vec{n} &= \int \int_C (\nabla \cdot \vec{\dot{F}}) dA && \text{(Divergence Theorem)} \\
\implies  \nabla \times \vec{F} &= \nabla \cdot \vec{\dot{F}} && \text{(For all points in space)}
\end{align}
$$

# Derivation
## Two ways to write Green's Theorem

Let $\vec{F}$ be a 2D vector field defined as:

$$
\vec{F}(x_1, x_2) = \begin{bmatrix} 
F_1(x_1,x_2) \\
F_2(x_1,x_2)
\end{bmatrix}
$$

Then we know [Green's Theorem](Green's%20Theorem.md) can be written in two ways:

$$
\begin{align}
\oint_{B}\vec{F} \cdot d\vec{l} &=   \int \int_C (\nabla \times \vec{F}) \cdot d\vec{A} \\
\oint_{B}F_1dx_1 + F_2 dx_2 &=  \int \int_C (\frac{\partial F_2}{\partial x_1} - \frac{\partial F_1}{\partial x_2})dA
\end{align}
$$

$B$ is a 1D boundary that encloses a 2D area $C$. To define the enclosed area, we parameterize a closed loop around the boundary counter-clockwise (same way curl is calculated), so that the tangent vector to the boundary has the inside area $C$ on the left hand-side. 

The left side of both equations is the path integral of the dot product between $\vec{F}$ and the tangent vector along the boundary, and the right side is taking the integral of the curl of $\vec{F}$ over the closed area. 

To see this, we can write the tangent vector to the boundary as

$$
d\vec{l} = \begin{bmatrix}
dx_1 \\
dx_2
\end{bmatrix}
$$

So when we take the dot product, we see that the left hand sides of the two equations above are equal:

$$
\begin{align}
\oint_{B}\vec{F} \cdot d\vec{l} &= \oint_{B}  F_1dx_1 +  F_2 dx_2
\end{align}
$$

## Rewrite Green's theorem to use normal vector instead of tangent vector

Now, consider the vector that is normal to the tangent vector to the boundary. We are specifically referring to the normal vector that points "outwards" away from the enclosed area $C$. Therefore, a differential normal vector $d\vec{n}$ is $90\degree$ (i.e., perpendicular) clockwise rotation to the tangent vector $d\vec{l}$:

$$
\begin{align}
d\vec{n} = \begin{bmatrix}
dx_2 \\
-dx_1
\end{bmatrix}
\end{align}
$$

Green's theorem can be rewritten in terms of $d\vec{n}$, the normal vector to the boundary, instead of the $d\vec{l}$ the tangent vector to the boundary:

$$
\begin{align}
\oint_{B}  F_1dx_1 +  F_2 dx_2 &= \oint_{B} \begin{bmatrix}
F_2 \\
-F_1
\end{bmatrix}
\cdot
\begin{bmatrix}
d x_2 \\
-dx_1
\end{bmatrix} 
\\
&= \oint_B \vec{\dot{F}} \cdot d\vec{n} && \text{(Define }\vec{\dot{F}} := \begin{bmatrix} F_2 \\ -F_1 \end{bmatrix} \text{)}
\end{align}
$$

$\vec{\dot{F}} = \begin{bmatrix} F_2 \\ -F_1 \end{bmatrix}$  is just a $90 \degree$ clockwise rotation of the vector field $\vec{F}$, similar to the normal vector relative to the tangent vector. We are now taking the integral of the dot product of $\vec{\dot{F}}$ with the normal to the boundary $d\vec{n}$. 

 We can now apply the [Divergence Theorem](Divergence%20Theorem.md) to relate this path integral of the dot product of $\vec{\dot{F}}$ with the normal vector $d\vec{n}$ over the boundary, to the integral over the closed area of the divergence of $\vec{\dot{F}}$. 


$$
\begin{align}
\oint_{B} \vec{\dot{F}} \cdot d\vec{n} = \int \int_C (\nabla \cdot \vec{\dot{F}}) dA
\end{align}
$$

We can therefore deduce that for all points in space:

$$
\begin{align}
\nabla \times \vec{F} &= \nabla \cdot \vec{\dot{F}}
\end{align}
$$

We can now accumulate these steps into the [Condensed derivation](#Condensed%20derivation).