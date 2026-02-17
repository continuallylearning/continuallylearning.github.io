---
tags:
---
# Green's Theorem
Green's theorem states that given a 2D dimensional vector field $\vec{F}(x,y) = [P(x,y), Q(x,y)]$ , there is a relationship between the path integral of $\vec{F}$ around a closed loop $B$ and the enclosed area $C$:

$$
\oint_{B}\vec{F} \cdot d\vec{l} = \int \int_C (\nabla \times \vec{F}) \cdot \vec{\hat{z}} dA
$$

Another, more popular form of Green's Theorem is:

$$
\oint_B Pdx + Qdy = \int \int_{C} (\frac{\partial Q}{\partial x} - \frac{\partial P}{\partial y}) dA
$$



We can derive the second form from the first by noting that $d\vec{l} = [dx, dy]$, and that the dot product inside the integral on the left hand side can be rewritten:

$$
\begin{align}

\vec{F} \cdot d\vec{l} &=
\begin{bmatrix}
P \\
Q
\end{bmatrix} \cdot
\begin{bmatrix}
dx \\
dy
\end{bmatrix} 
\\

&=  P dx + Q  dy

\end{align}
$$

And on the right hand side, the $z$ component of $\nabla \times \vec{F}$ (which is the only non zero component when taking the dot product with $\hat{z}$) is given by the [Curl](Gradient,%20Divergence,%20and%20Curl.md):

$$
(\nabla \times \vec{F}) \cdot \hat{z} = \frac{\partial Q}{\partial x} - \frac{\partial P}{\partial y}
$$

# External links
- [Intuition into Green’s theorem](https://mathinsight.org/greens_theorem_idea)