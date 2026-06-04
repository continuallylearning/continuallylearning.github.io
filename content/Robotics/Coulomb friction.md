---
aliases:
  - Friction cone
  - static friction
---
# Summary

## Notation
- $\vec{F}$ denotes a vector, where $F$ is the magnitude of vector $\vec{F}$.
- $\hat{x}$ denotes a unit vector (i.e., a direction)
- $\vec{F_x} = (\vec{F} \cdot \vec{x}) \hat{x}$ is a vector that denotes the part of $\vec{F}$ that points in the direction of $\hat{x}$. 

## Definition of Coulomb Friction

(Static) Coulomb friction is a model for friction that says: given a force $\vec{F_t}$ that is applied to a mass tangentially to a surface (whose orientation is determined by the surface normal $\hat{n}$), there will be force with equal magnitude that will counter the tangent force so long as the magnitude of $\vec{F_t}$ is equal to or less than the magnitude of the normal force on the mass to the contact surface $\vec{F_n}$, multiplied by a coefficient of static friction $\mu \geq 0$: 

$$
\begin{align}
\lVert \vec{F_t} \rVert \leq \mu \lVert \vec{F_n} \rVert \\
\end{align}
$$

# Applied force and tangent/normal components

Consider a 3D force $\vec{F}$ applied at a point on a surface whose normal direction is $\hat{n}$. Define the the $\hat{z}$ direction of the applied force to be pointing in the direction of the surface normal, $\vec{F_z} = (\vec{F} \cdot \hat{n}) \hat{n}$. The other two directions $\hat{x}, \hat{y}$ point tangentially to the surface.

We assume that some amount of the applied force is in the normal direction (i.e.,: $\vec{F} \cdot \hat{n} > 0$), otherwise $F_z$ is 0 and the inequality does not apply. 

We can then decompose the applied force vector $\vec{F} = \vec{F}_n + \vec{F}_t$ into a the normal component $\vec{F}_n = \vec{F}_z$ and the tangent component $\vec{F_t} = \vec{F}_x + \vec{F}_y$.

We can now substitute these definitions into the Coulomb friction equation above to derive a relationship between $\vec{F_z}$ and $\vec{F_x}, \vec{F_y}$:

$$
\begin{align}
\lVert \vec{F_t} \rVert \leq \mu \lVert \vec{F_n} \rVert \\
\lVert \vec{F_x} + \vec{F_y} \rVert \leq \mu \lVert \vec{F_z} \rVert \\
\sqrt{(F_x)^2 + (F_y^2)}\leq \mu F_z \\
\end{align}
$$


This new equation makes it clear that when applying a force $\vec{F}$, it will cause slippage when the tangential force from it is greater than the normal force from the applied force (multiplied by the coefficient of friction). 

This implies if you apply a force to the surface and want the object to not slip, you must apply sufficient normal force to overcome whatever tangent force you’re also applying (multiplied by $\mu$). Interestingly, whether slipping will happen will be solely determined by the angle of applied force, and not its magnitude.

# Angle of applied force determines sliding, not magnitude

Whether an applied force $\vec{F}$ will cause the mass to start sliding depends only on the angle of the applied force, and not it's magnitude. This is because if we scale up the magnitude of the applied force by a scalar $\beta$, we will be scaling up the tangent and normal components by the same scalar amount and get effectively the same inequality above. To show this, let us define a new applied force $\vec{G}$ that is a scaled up version of another applied force $\vec{F}$: 

$$
\begin{align}
\vec{G} &= \beta \vec{F} \\
&= \beta \vec{F_t} + \beta \vec{F_n} \\
&= \vec{G_t} + \vec{G_n}
\end{align}
$$

Then when we use the inequality above with $\vec{G}$, we get our original inequality with $\vec{F}$. 

$$
\begin{align}
\lVert \vec{G_t} \rVert \leq \mu \lVert \vec{G_n} \rVert \\
\beta \lVert \vec{F_t} \rVert \leq \mu \beta  \lVert \vec{F_n} \rVert \\
 \lVert \vec{F_t} \rVert \leq \mu  \lVert \vec{F_n} \rVert \\
\end{align}
$$

Therefore, only the angle of the applied force relative to the surface normal determines whether coulomb friction will hold (i.e., object will stay stable) or not.

# Friction cone

If we define $\alpha$ to be the angle between $\vec{F}$ and $\hat{n}$, then if $\alpha=0$, there is no tangential component (the applied force is entirely along surface normal) and the mass will stay stable. If $\alpha = \frac{\pi}{2}$ then the applied force is completely along the surface tangent and there will be no coulomb friction. The *friction cone* is the space of all applied forces that keep the object stable due to Coulomb friction. Friction cones form the basis of [force closure](force%20closure), and are determined by the coefficient of friction $\mu$, which determines the angle of the friction cone.

## Angle of friction cone depends only on coefficient of friction

Consider the figure below (lifted from Modern Robotics, Chapter 12.2) showing the full friction cone in (a), and a projected view from one side in (b)

![test](https://raw.githubusercontent.com/continuallylearning/continuallylearning.github.io/v4/media/friction_cone.png)

Any vector inside the friction cone is stable, and anything outside is not. We know that the vectors that are on the outer edge of the cone in (b) has vertical magnitude $F_z$ and horizontal component $\mu F_z$, and there is a right triangle formed between the two components. Therefore, since these legs are opposite and adjacent to the angle $\alpha$, we can use the definition of tan to derive the angle based on the coefficient of friction:

$$
\begin{align}
\tan(\alpha) = \frac{\mu F_z}{F_z} \\
\tan(\alpha) = \mu \\
\tan^{-1}(\mu) = \alpha \\
\end{align}
$$

$\tan^{-1}$ is a monotonically increasing function, which matches our intuition: as the coefficient of friction gets higher, the maximum angle of the friction cone increases (i.e., applied forces that point more in the tangent direction can be resisted at higher coefficients of friction)
# References
- [Really good reference](https://scaron.info/robotics/friction-cones.html)
- Chapter 12.2 on contact forces goes into nice detail (https://hades.mech.northwestern.edu/images/7/7f/MR.pdf)