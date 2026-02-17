---
title: Reflected Inertia
date: 2025-12-28
feed: show
---
## Overview of a Transmission System

A transmission system couples the motions of an input shaft (here called the motor shaft) and an output shaft (here called the load shaft). The motor and load shafts are connected via the transmission mechanism (e.g., a gearbox), which has a mechanical advantage/gear ratio that determines how angular velocities, torques and perceived inertias relate between both sides of the transmissions. Good resources for further reading are:

- [Russ Tedrake's course notes](https://manipulation.csail.mit.edu/robot.html)
- [The basics of motion control by John Mazurkiewicz](https://fab.cba.mit.edu/classes/961.04/topics/motion_control2.pdf)

## Basic Electric Motor Model

Consider an electric motor, which turns electric energy into kinetic energy. The motor has a motor shaft (i.e: a rotating component), and when a current $I$ is supplied to the motor, a torque $\tau_{motor}$ is produced and applied to the motor shaft. The relationship between current and torque is linear, and the constant of proportionality $k$ is called the torque constant:

$$
\tau_{motor} = k \times I
$$

## Angular Kinematics of the Motor Shaft

The motor shaft has an angular position $q_{motor}$, angular velocity $\omega_{motor}$, and angular acceleration $\alpha_{motor}$. Although these functions depend on time (and are related via derivatives w.r.t time), we only include the time input in equations when relevant:

$$
\begin{aligned}
\omega_{motor}(t) &= \frac{dq_{motor}(t)}{dt} \\
\alpha_{motor}(t) &= \frac{d\omega_{motor}(t)}{dt}
\end{aligned}
$$

## Linear Kinematics of the Motor Gear

Let the motor shaft be the input to a transmission system, which connects to an output shaft called the load. Specifically, let the input motor shaft be connected to the center of a gear with radius $R_{motor}$, and define the tangential position, velocity, and acceleration of a point on the rim of the motor gear as $s_{motor}$, $v_{motor}$, $a_{motor}$. Since the gear is a circle, we know the relationship between linear and angular kinematics:

$$
\begin{aligned}
s_{motor} &= R_{motor} \times q_{motor} \\
v_{motor} &= R_{motor} \times \omega_{motor} \\
a_{motor} &= R_{motor} \times \alpha_{motor}
\end{aligned}
$$

## Linear Kinematics of the Load Gear

Similarly, the load shaft is attached to a gear of radius $R_{load}$, and the linear and angular kinematics of the load are linearly related via the radius $R_{load}$:

$$
\begin{aligned}
s_{load} &= R_{load} \times q_{load} \\
v_{load} &= R_{load} \times \omega_{load} \\
a_{load} &= R_{load} \times \alpha_{load}
\end{aligned}
$$

## Gear Contact Constraint

Additionally, we assume the input and output gears are making contact at some point where the rims of the gears come into contact, and if there is no slippage, the linear velocities of the rims for the gear must equal each other.

$$
v_{motor} = v_{load}
$$

## Gear Ratio and Mechanical Advantage

We can substitute in the angular velocities for the linear velocities, and see that there is a linear relationship between the angular velocities of the motor and load based on the ratio of radii of the gears, called the gear ratio $n = \frac{R_{load}}{R_{motor}}$, which determines the mechanical advantage of the transmission system. Notice that the angular velocities of the motor and shaft differ based on the gear ratio.

$$
\begin{aligned}
v_{motor} &= v_{load} \\
R_{motor} \times \omega_{motor} &= R_{load} \times \omega_{load} \\
\omega_{motor} &= \frac{R_{load}}{R_{motor}} \times \omega_{load} \\
\omega_{motor} &= n \times \omega_{load}, \quad n = \frac{R_{load}}{R_{motor}}
\end{aligned}
$$

Since angular accelerations are time derivatives of angular velocity, and the gear ratio is independent of time, they have a similar relationship:

$$
\alpha_{motor} = n \times \alpha_{load}
$$

## Torque-Speed Tradeoff

Assuming power supplied to the motor shaft via a torque is perfectly conserved when transferred to the load shaft, then the input power $P_{in}$ (applied to motor shaft) must equal the output power $P_{out}$ (applied to load). We can leverage the fact that power is equal to the product of torque and angular velocity $P = \tau \cdot \omega$ , and substitute in the relationship between angular velocities to deduce a relationship between the torques of the motor and load.

$$
\begin{aligned}
P_{in} &= P_{out} \\
\tau_{motor} \cdot \omega_{motor} &= \tau_{load} \cdot \omega_{load} \\
\tau_{motor} \cdot \omega_{load} \times n &= \tau_{load} \cdot \omega_{load} \\
\tau_{motor} &= \frac{\tau_{load}}{n} \\
\tau_{motor} &= \frac{R_{motor}}{R_{load}} \times \tau_{load}
\end{aligned}
$$

Notice how the motor's torque $\tau_{motor}$ goes down as the gear ratio $n$ goes up (assuming $\tau_{load}$ is constant), whereas the motor's angular velocity $\omega_{motor}$ instead goes up as the gear ratio $n$ goes up (assuming $\omega_{load}$ is constant): we refer to this as the torque-speed tradeoff.

## Inertia of the Motor and Load

The motor and load both consist of distributed mass, which is modeled by their moment of inertia $I_{motor}$ and $I_{load}$ respectively. The moment of inertia (for 1D rotation in this case, a scalar), determines the relationship between torques and angular accelerations of the shafts about their axes:

$$
\begin{aligned}
\tau_{motor} &= I_{motor} \times \alpha_{motor} \\
\tau_{load} &= I_{load} \times \alpha_{load}
\end{aligned}
$$

## Reflected Inertia

Consider a torque being applied at the one end of the transmission, like the motor. If the motor isn't connected to a transmission yet, then the amount of inertia resisting the torque will be the motor's inertia $I_{motor}$, which is the first equation above. However, once the motor is connected to the transmission, the motor's torque will additionally be resisted by the inertia at the other end of the transmission, in this case the load's inertia $I_{load}$. This extra inertia experienced at one end of a transmission due to the inertia at the other end is called the reflected inertia, and it can be defined for both ends.

How much extra inertia will the motor experience due to the inertia of the load? Since we know that the torques and angular accelerations of the two ends of the transmission are coupled via the gear ratio, we can plug these relationships into the second equation above that defines the load's torque, and see how the motor torque and angular acceleration relate to the load's inertia:

$$
\begin{aligned}
\tau_{load} &= I_{load} \times \alpha_{load} \\
n \times \tau_{motor} &= I_{load} \times \frac{\alpha_{motor}}{n} \\
\tau_{motor} &= \frac{I_{load}}{n^2} \times \alpha_{motor}
\end{aligned}
$$

Notice how the inertia due to the load that the motor shaft experiences in order to attain a certain angular acceleration at the motor shaft is $\frac{I_{load}}{n^2}$. This means that the experienced inertia of the load at the motor is cut quadratically by the gear ratio (which makes sense since there is mechanical advantage that amplifies the torque). We can do a similar derivation to see that $\tau_{load} = I_{motor} \times n^2 \times \alpha_{load}$, which means that the inertia of the motor experienced at the load is quadratic in the gear ratio. We now conclude that the total inertia as seen from the motor $I_{motor}^{total}$ and at the load $I_{load}^{total}$ is the sum of the inertia at the one end of the transmission and reflected inertia from the other end:

$$
\begin{aligned}
I_{motor}^{total} &= I_{motor} + \frac{I_{load}}{n^2} \\
I_{load}^{total} &= I_{load} + I_{motor} \times n^2
\end{aligned}
$$

This is important in robotics for safety and backwards drivability, because if the gear ratio is high, then even motors with small inertias make it challenging to apply torques from the load onto the motor shaft.

## Acknowledgement

Thank you to Michael Fishman for reviewing this article, catching typos, and making suggestions for improving clarity.

