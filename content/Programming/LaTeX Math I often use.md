---
tags:
feed: show
date: 2026-01-14
---

# Vectors
>[!info] 
>> use `\vec{}` to denote vectors, as opposed to scalars. Useful for vector fields vs. scalar fields too.
>

$$
\begin{aligned}
\vec{F} &= k \vec{x} && \text{(Hooke's law)} \\

\end{aligned} 
$$
# Logical operators 
>[!info] 
> Logical operators in LaTeX typically are `l[name]`, like `\lnot`, `\land`, etc. 

```
$$
\lnot (A \land B) \iff \lnot B \lor \lnot A \quad \text{(De Morgan’s law)}
$$
```
$$
\lnot (A \land B) \iff \lnot B \lor \lnot A \quad \text{(De Morgan’s law)}
$$
# Mid for probabilities and sets
```
$$
Pr(x \mid y) = \frac{Pr(y \mid x) Pr(x)}{Pr(y)} \quad \text{(Bayes theorem)}
$$
```

$$
Pr(x \mid y) = \frac{Pr(y \mid x) Pr(x)}{Pr(y)} \quad \text{(Bayes theorem)}
$$

```
$$
X = \{(y,z) \mid y \in Y, z \in Z \} \quad \text{(Cartesian Product)}
$$
```

$$
X = \{(y,z) \mid y \in Y, z \in Z \} \quad \text{(Cartesian Product)}
$$
# Norm of matrix or vector 


```
$$
\| x \| = ()
$$
```

$$
\begin{aligned}
\| x \| := \sqrt{(x \cdot x)} && \text{(Euclidean norm)}
\end{aligned}
$$
# Multi-line equations

>[!info]
>> use the `\begin{aligned}` and `\end{aligned}` to make multi-line equations, where each line is separated by `\\`. 
>> use `&` before operator (e.g., `=`) to make equations line up at them, and `\\` for new lines. You can then use `&&` to make `\text{}` or other math notes line up on the right side.

```
$$
\begin{aligned}
\vec{E}(\vec{x}) &= \int_{V} \frac{\rho(\vec{r})\widehat{(\vec{x} - \vec{r})}}{4 \pi \epsilon_0\mid \vec{x} - \vec{r} \mid ^2} d\tau && \text{(Electric field from charge distribution)} \\
\vec{E} &= - \nabla V && \text{(Electric field from electric potential )} \\
\end{aligned} 
$$
```

$$
\begin{aligned}
\vec{E}(\vec{x}) &= \int_{V} \frac{\rho(\vec{r})\widehat{(\vec{x} - \vec{r})}}{4 \pi \epsilon_0\mid \vec{x} - \vec{r} \mid ^2} d\tau && \text{(Electric field from charge distribution)} \\
\vec{E} &= - \nabla V && \text{(Electric field from electric potential )} \\
\end{aligned} 
$$

# Making column vectors and row vectors

>[!info] 
>> Use `vdots` and `ldots` for 3 dots to be rendered vertically and horizontally, respectively. 

```
$$
\begin{bmatrix}
x_1 \\
\vdots \\
x_n \\
\end{bmatrix}
$$
```

$$
\begin{bmatrix}
x_1 \\
\vdots \\
x_n \\
\end{bmatrix}
$$

```
$$
[x_1, \ldots, x_n]
$$
```
$$
[x_1, \ldots, x_n]
$$