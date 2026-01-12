---
title: Differentiable kinematics, Jacobians, and Jacobian transpose
date: 2026-01-01
feed: show
---
## Overview

We cover forward kinematics and the Jacobian, relating joint velocities to end-effector velocities with the Jacobian, and forces to torques via the Jacobian transpose.

In this post:

- All vectors are assumed to be column vectors.
- Transposes are written explicitly {% raw %}$(\cdot)^T${% endraw %}.
- {% raw %}$\boxed{\text{Boxes}}${% endraw %} denote key equations.
- "task space" and "Cartesian space" are used interchangeably.

## Useful References

- [Studywolf's blog on "ROBOT CONTROL PART 2: JACOBIANS, VELOCITY, AND FORCE"](https://studywolf.wordpress.com/2013/09/02/robot-control-jacobians-velocity-and-force/)
- [Interactive Python Notebook on Differentiable Kinematics](https://github.com/ericrosenbrown/robot-hacking/blob/main/robot-arm-fun/jacobian.ipynb)

## Forward Kinematics

We model the joint configuration of the robot at time $t$ as

{% raw %}
$$
q(t): \mathbb{R} \rightarrow \mathbb{R}^n
$$
{% endraw %}

where $n$ is the number of independent joints, each contributing one degree of freedom (DoF). We denote the task space of the robot (for example, the end-effector position and orientation) at time $t$ as

{% raw %}
$$
x(t): \mathbb{R} \rightarrow \mathbb{R}^m
$$
{% endraw %}

where $m$ is the task-space / Cartesian space dimensionality.

The kinematic structure of the robot determines a forward kinematics function $F$

{% raw %}
$$
F: \mathbb{R}^n \rightarrow \mathbb{R}^m
$$
{% endraw %}

which maps joint configurations to the task space:

{% raw %}
$$
\boxed{x(t) = F(q(t))}
$$
{% endraw %}

## Differentiable Kinematics and the Jacobian

If the forward kinematics function is differentiable with respect to $q$, we can differentiate both sides of the equation with respect to time. Applying the chain rule yields a relationship between joint velocities and Cartesian velocities:

{% raw %}
$$
\begin{aligned}
x(t) &= F(q(t)) \\
\frac{dx}{dt}(t) &= \frac{dF(q(t))}{dt} && \text{(Derivative w.r.t time)} \\
\frac{dx}{dt}(t) &= \frac{dF}{dq}(q(t)) \frac{dq}{dt}(t) && \text{(Chain rule)}
\end{aligned}
$$
{% endraw %}

If we define the Cartesian and joint velocities as:

{% raw %}
$$
\dot{x}(t) := \frac{dx}{dt}(t), \quad \dot{q}(t) := \frac{dq}{dt}(t)
$$
{% endraw %}

and the Jacobian matrix as

{% raw %}
$$
J(q) := \frac{dF}{dq}(q) \in \mathbb{R}^{m \times n}
$$
{% endraw %}

We can substitute these to rewrite the above differentiable kinematics equations as

{% raw %}
$$
\boxed{\dot{x}(t) = J(q(t)) \dot{q}(t)}
$$
{% endraw %}

The Jacobian maps joint velocities to Cartesian velocities and depends explicitly on the current joint configuration.

## Jacobian Transpose relates forces and torques

Torques and forces are also related via the transpose of the Jacobian. To derive this, remember that power $P$ (measured in joules per second) is the rate of energy transfer. Power can be expressed in Cartesian space:

{% raw %}
$$
P_{Cartesian} = f^T \dot{x}
$$
{% endraw %}

where {% raw %}$f \in \mathbb{R}^m${% endraw %} is a constant force vector applied to the end effector, and {% raw %}$\dot{x}${% endraw %} is the Cartesian velocity as defined above. Notice that $f$ and {% raw %}$\dot{x}${% endraw %} are both (column) vectors, and since we are taking their dot product, we transpose the force vector $f^T$.

Power can also be expressed in the configuration space, where we replace force $f$ and Cartesian velocity {% raw %}$\dot{x}${% endraw %}, with their joint analogues torque {% raw %}$\tau \in \mathbb{R}^n${% endraw %} and joint velocity {% raw %}$\dot{q}${% endraw %} respectively (the latter defined above).

{% raw %}
$$
P_{joint} = \tau^T \dot{q}
$$
{% endraw %}

Since {% raw %}$\tau${% endraw %} and {% raw %}$\dot{q}${% endraw %} are both (column) vectors and we are taking their dot product, we also transpose the torque {% raw %}$\tau${% endraw %} as above for force.

Since total power is conserved between Cartesian and joint spaces, we can equate these, and then rearrange the equation to relate torque {% raw %}$\tau${% endraw %} to forces $f$ via the transpose of the Jacobian $J^T$.

{% raw %}
$$
\begin{aligned}
P_{Cartesian} &= P_{joint} && \text{(equal power)} \\
f^T \dot{x} &= \tau^T \dot{q} && \text{(substitute power definitions)} \\
f^T J \dot{q} &= \tau^T \dot{q} && \text{(since } \dot{x} = J\dot{q} \text{)} \\
f^T J &= \tau^T && \text{(holds for all } \dot{q} \text{)}
\end{aligned}
$$
{% endraw %}

If we transpose both sides, we get

{% raw %}
$$
\boxed{J^T f = \tau}
$$
{% endraw %}

Notice how our final relationship $J^T f = \tau$ has the force $f$ and torque {% raw %}$\tau${% endraw %} vectors both as column vectors, as expected.

## 2 DoF Revolute Arm Example

In practice, $F$ is often nonlinear and can be quite complex, depending on the robot's geometry. For example, consider a serial robot arm composed of two links, with lengths $l_0$ and $l_1$. The first link is connected to the world via a revolute joint, and the second link is connected to the end of the first link via a revolute joint.

The end-effector's 2D position {% raw %}$x = \begin{bmatrix} x_0 \\ x_1 \end{bmatrix}${% endraw %} in the plane is given by:

{% raw %}
$$
x = F(q) = F\left(\begin{bmatrix} \theta_0 \\ \theta_1 \end{bmatrix}\right) = \begin{bmatrix} l_0 \cos(\theta_0) + l_1 \cos(\theta_0 + \theta_1) \\ l_0 \sin(\theta_0) + l_1 \sin(\theta_0 + \theta_1) \end{bmatrix}
$$
{% endraw %}

The Jacobian is obtained by differentiating the forward kinematics with respect to the joint angles:

{% raw %}
$$
J(q) = \frac{\partial x}{\partial q} = \begin{bmatrix} \frac{\partial x_0}{\partial \theta_0} & \frac{\partial x_0}{\partial \theta_1} \\ \frac{\partial x_1}{\partial \theta_0} & \frac{\partial x_1}{\partial \theta_1} \end{bmatrix}
$$
{% endraw %}

As a result, we know that the Jacobian for this robot arm is:

{% raw %}
$$
J(q) = J\left(\begin{bmatrix} \theta_0 \\ \theta_1 \end{bmatrix}\right) = \begin{bmatrix} -\sin(\theta_0) l_0 - \sin(\theta_1 + \theta_0) l_1 & -\sin(\theta_1 + \theta_0) l_1 \\ \cos(\theta_0) l_0 + \cos(\theta_1 + \theta_0) l_1 & \cos(\theta_1 + \theta_0) l_1 \end{bmatrix}
$$
{% endraw %}
