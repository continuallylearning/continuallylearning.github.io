---
tags:
feed: show
date: 2026-01-14
---

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
\| x \|^2
$$
```

$$
\| x \|^2
$$
# Multi-line equations

>[!info]
>> use `&` before operator (e.g., `=`) to make equations line up at them, and `\\` for new lines. `quad` provides space between math on the same line, which can be useful for putting `\text{}` on the right side

```
$$
\begin{aligned}
1 + 2 + 3 &= 7 - 1 \quad \text{(This is true)} \\
10 &= 10 \quad \text{(Also true!)}
\end{aligned}
$$
```

$$
\begin{aligned}
1 + 2 + 3 &= 7 - 1 \quad \text{(This is true)} \\
10 &= 10 \quad \text{(Also true!)}
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