---
title: Centripetal acceleration
date: 2025-11-09
feed: show
---
Centripetal acceleration is the inward acceleration a body undergoes during uniform 2D circular motion. We can derive it kinematically by defining the constraints of the motion and taking the derivative of $p(t)$ twice with respect to time, which yields the acceleration. By multiplying this acceleration by mass, we obtain the centripetal force.

## Position in uniform 2D circular motion

Consider a body moving in uniform, 2D circular motion that has radius $r$. The position $p(t): \mathbb{R} \rightarrow \mathbb{R}^2$ at time $t$ can be expressed in polar coordinates:

$$
p(t) = \begin{bmatrix} x(t) \\ y(t) \end{bmatrix} = \begin{bmatrix} r \cos(\theta(t)) \\ r \sin(\theta(t)) \end{bmatrix} = \begin{bmatrix} r \cos(\omega t) \\ r \sin(\omega t) \end{bmatrix}
$$

where

$$
\theta(t) = \omega t, \quad \omega = 2 \pi f
$$

Here, $\theta(t)$ represents the angular position, and $\omega$ is the angular velocity of the body. Since the motion is uniform, $\omega$ is constant. If the body completes revolutions at frequency $f$ (i.e., it takes $\frac{1}{f}$ seconds to complete one revolution around the circle), then $\omega = 2 \pi f$.

## Linear velocity in uniform 2D circular motion points tangentially

Now that we've defined position, let's compute the first derivative of $p(t)$ with respect to $t$, which is the linear velocity $v(t)$. Using the chain rule:

$$
v(t) = \frac{dp}{dt}(t) = \begin{bmatrix} -r \omega \sin(\omega t) \\ r \omega \cos(\omega t) \end{bmatrix} = \omega \begin{bmatrix} -y(t) \\ x(t) \end{bmatrix}
$$

Since the magnitude of $p(t)$ is always $r$, we see that the magnitude of $v(t)$ is $\omega r$, and that the velocity vector is perpendicular to the radius since $\begin{bmatrix} -y(t) \\ x(t) \end{bmatrix}$ is a $90^\circ$ counterclockwise rotation of $\begin{bmatrix} x(t) \\ y(t) \end{bmatrix} = p(t)$ (and therefore tangent to the circle).

Exercise question: Prove that the position and velocity vectors are perpendicular. (hint: construct the rotation matrix that relates the two vectors).

## Centripetal acceleration in uniform 2D circular motion points inward

Taking the derivative of $v(t)$ with respect to time:

$$
a(t) = \begin{bmatrix} -r \omega^2 \cos(\omega t) \\ -r \omega^2 \sin(\omega t) \end{bmatrix} = \omega^2 \begin{bmatrix} -x(t) \\ -y(t) \end{bmatrix} = -\omega^2 p(t) = -\omega^2 r \hat{p}(t)
$$

where $\hat{p}(t)$ is the unit vector in the direction of $p(t)$ (i.e., $p(t) = r \hat{p}(t)$). We have shown that the centripetal acceleration points exactly opposite the position vector (i.e., toward the center, due to the negative sign), and its magnitude is $\omega^2 r$. Let's observe what happened: we started with a position vector, and the derivative rotated it $90^\circ$ counterclockwise (making a vector tangent to it). Applying the derivative again rotated it an additional $90^\circ$ counterclockwise, making the vector point in the opposite direction as the original.

Since $\omega = \frac{v}{r}$, we have

$$
a(t) = -\omega^2 r \hat{p}(t) = -\left(\frac{v}{r}\right)^2 r \hat{p}(t) = -\frac{v^2}{r} \hat{p}(t)
$$

We have now shown both the direction of centripetal acceleration (inward and perpendicular to the linear velocity) and its magnitude $\frac{v^2}{r}$. The direction makes sense: if the acceleration were not perpendicular to the velocity, the speed would change, contradicting the assumption of uniform circular motion. If the acceleration instead pointed outward (which is also perpendicular to the linear velocity), it would violate the kinematic constraint of circular motion, since the point would no longer remain a distance $r$ from the center.

