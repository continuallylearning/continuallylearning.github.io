---
title: Continuity equation
feed: show
date: 2025-09-20
---
The continuity equation describes a type of conservation law, specifically that a quantity can either be (locally) transported, generated, or destroyed. For the continuity equation to hold, a specific relationship between the density and flow of a quantity must be true, which we describe now:

According to [Wikipedia](https://en.wikipedia.org/wiki/Continuity_equation#Differential_form), the continuity equation can be written in differential form as follows:

$$
\frac{\partial \rho}{\partial t}+\nabla \cdot j=\sigma
$$

To understand this equation, we will define all the terms and operators, in addition to defining their units/types to make it clear why the equation makes sense.

- $\rho(x,t)$ is a scalar field representing the density of a quantity $q$ at a position $x$ at time $t$ .
	- $x:R^d$ represents position in space. Say the units are meters $m$ .
	- $t:R$ represents point in time. Say the units are seconds $s$ .
	- $\rho:R^d \times R \rightarrow R$ represents density, which is the amount of quantity per unit volume. Say the units are $\frac{q}{m^3}$ .
	- $\frac{\partial \rho}{\partial t}(x,t):R^d \times R \rightarrow R$ represents the derivative of density w.r.t time, or the rate of change of density (at a given point in space and time). Since the units of $\rho$ are $\frac{q}{m^3}$, and we're taking the derivative of it w.r.t time, then the units of $\frac{\partial \rho}{\partial t}$ is $\frac{\frac{q}{m^3}}{s}=\frac{q}{sm^3}$ (since derivative has units of the ratio between output and input variable types, see definition of derivative for more information.)
	- Since $\frac{\partial \rho}{\partial t}$ has units $\frac{q}{sm^3}$ , and in the continuity equation we add it to $\nabla \cdot j$ , and we can only add things together that have the same units, we better make sure that $\nabla \cdot j$ has the same units. Let's check that out.
- $j=\rho v$ is the flux density of $q$ , where flux density represents the total amount of $q$ per unit time ( $s$ ) per unit surface area. It is a vector field. Say the units are $\frac{q}{sm^2}$
	- $j(x,t):R^d \times R \rightarrow R^d$ is a vector field that is a product of the density and flow of a quantity.
	- $v(x,t):R^d \times R \rightarrow R^d$ is the flow (or velocity field), a vector field that represents the velocity of the quantity at some point in space ( $x$ ) and time ( $t$ ). Velocity is a ratio of change in position over change in time, so say the units are $\frac{m}{s}$
	- Let's confirm the units of flux density ( $\frac{q}{sm^2}$ ) agrees with the units we get when we multiply the density of q $\rho$ with the flow $v$ . Since $\rho$ has units $\frac{q}{m^3}$ and $v$ has units $\frac{m}{s}$ then: 
		$$
		\rho v:(\frac{q}{m^3})(\frac{m}{s})=\frac{q}{sm^2}
		$$
	- It's acceptable to multiply $\rho$ and $v$ because $v$ is a vector and $\rho$ is a scalar, so the resulting flux density vector $j$ points in the same direction as the flow, but its magnitude is scaled by the density $\rho$ . Notice that both flux density and flow maps a spatial vector (size $d$ ) and a point in time (size $1$ ) to a vector with same shape as the spatial vector (size $d$ )
- $\nabla \cdot$ is the divergence operator. It maps a vector field $f(x_1,...,x_n):R^d \rightarrow R^d=[y_1(x_1,..,x_n)...y_n(x_1,...,x_n)]$ to a scalar field $(\nabla \cdot f)(x_1,...x_n):R^d \rightarrow R$ via 
	$$
	(\nabla \cdot f)(x_1,...x_n)=\sum_{i=1}^n \frac{\partial y_i}{\partial x_i}(x_1,...,x_n)
	$$
	- Since the divergence operator is summing up partial derivatives $\frac{\partial y_i}{\partial x_i}$ , the units of the resulting scalar field are the same as the units of the partial derivatives. Since the (partial) derivative is a ratio between the change in the function $y_i$ and the change in the input variable $x_i$ , then if the units of $y_i$ are $a$ and the units of $x_i$ are $b$ , then the units of $\nabla \cdot f$ are $\frac{a}{b}$
- $\nabla \cdot j$ is the divergence of the flux. It is a scalar field that represents the amount of $q$ flows out locally per unit volume per second. It has units $\frac{q}{sm^3}$ (compared to flux density, which has units $\frac{q}{sm^2}$ ).
	- ⚠️ We have to be careful when thinking about $\nabla \cdot j$ , because $j:R^d \times R \rightarrow R^d$ has $d+1$ input dimensions ( $d$ for space, $1$ for time), and has $d$ output dimensions ( $d$ for space), but our definition of divergence wants the number of input and output dimensions to be the same. We "ignore" the time dimension because we are interested in calculating, at a fixed point in time, the total flux over a spatial surface of an infinitely small volume centered at a fixed point in space. We could say $j_t(x_1,...,x_n)=j(x_1,...,x_n,t)$ and work with $j_t$ to make things more clear (but we won't). This may also be clearer when examining the relationship between divergences and surface integrals via the Fundamental Theorem for Divergences, but that is not explored now.
	- The flux (a scalar quantity) is the surface integral of the flux density over a closed surface, and has units $\frac{q}{s}$ . It represents the net amount of quantity (flow through a surface) per unit time. While (scalar) flux isn't mentioned explicitly in the differential form of the continuity equation above, it is related to the integral of $\nabla \cdot j$ over an enclosed volume via the divergence theorem, which we will not discuss further.
	- Let's compute the units of $\nabla \cdot j$ . We know that the (relevant) input variables for $j$ are spatial ( $m$ , see note above), and that the output variable units are $\frac{q}{sm^2}$ . Since $\nabla \cdot f$ takes a vector field $f$ and produces a scalar field whose output variables are of units $\frac{a}{b}$ where $a$ and $b$ are the output and input units of $f$ , then $\nabla \cdot j$ has units $\frac{\frac{q}{sm^2}}{m}=\frac{q}{sm^3}$ .
	- We just confirmed that $\frac{\partial \rho}{\partial t}$ and $\nabla \cdot j$ have the same units of $\frac{q}{sm^3}$ , which means we can add them together like is done in the continuity equation. That's great! The last thing is we need to define $\sigma$ , and make sure it has the same units.
- $\sigma(x,t)$ represents the amount of $q$ generated per unit volume per unit time, and it's a scalar field. The units are therefore $\frac{q}{sm^3}$ , which matches the left side of the equation, which means everything "types out" correctly.
	- When $\sigma$ positive, it represents a source from which $q$ is generated, and when it's negative, it represents a sink for where $q$ is removed.
	- When $q$ represents a conserved quantity (a quantity that can't be generated or destroyed, only transferred), then $\sigma=0$ , which would mean $\frac{\partial \rho}{\partial t} = - \nabla \cdot j$ .
