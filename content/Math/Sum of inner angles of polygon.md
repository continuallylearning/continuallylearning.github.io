---
tags:
  - "#clblogs"
  - clwip
date: 2026-06-03
---
# Summary

For an $n$-sided polygon, the sum of the inner angles is:

$$
\sum_{i=1}^{n}  \theta_{i}^\text{inner} = 180(n-2)
$$

If $n=3$, then we see that the sum of inner angles if $180(3-2) = 180$, which is a fact I memorized as a student. We first prove this using a triangle, and then discuss how our proof generalizes to all $n$-sided polygons.
# Proof

First, consider a triangle described by the points $A,B,C$, with respective inner angles $\theta_1^\text{inner}, \theta_2^\text{inner}, \theta_3^\text{inner}$.  Note that these angles do not necessarily equal each other.

![triangle_inner_angles](media/triangle_inner_angles.jpg)

We have also written out the turn angles, $\theta_1^\text{turn}, \theta_2^\text{turn}, \theta_3^\text{turn}$, by extending out the lines of the triangle. The turn and inner angles are supplementary, meaning each pair of inner/turn angles sums to $180$:

$$
\begin{align}
\theta_{i}^{\text{inner}} + \theta_{i}^{\text{turn}} &= 180 && \forall i \in \{1,2,3\}  &&& \text{(Def. of supplementary angles)} \\
\implies \theta_{i}^{\text{turn}} &= 180 - \theta_{i}^{\text{inner}} && \text{(Rearrange equation)}
\end{align}
$$

Additionally, we know the sum of the turn angles equals $360 \degree$. We will prove this by considering a person walking the entire perimeter of the shape counter-clockwise (i.e., always heading in the direction of the arrows on the triangle edges), and turning $\theta_i^\text{turn}$ degrees at each point:
- First, the person standing at point $A$, and facing towards point $B$, and the total degrees they have turned is $0$. 
- They will walk towards $B$, and then turn towards point $C$ by turning counter-clockwise $\theta_2^\text{turn}$ degrees. Therefore, the total degrees turned is now $\theta_2^\text{turn}$. 
- They will next walk towards $C$, and then turn towards point $A$ by turning counter-clockwise $\theta_3^\text{turn}$ degrees. Therefore, the total degrees turned is now $\theta_2^\text{turn} +\theta_3^\text{turn}$.
- They will last walk towards $A$, and then turn towards point $B$ by turning counter-clockwise $\theta_1^\text{turn}$ degrees. The person has now walked the entire perimeter and turned  a total degrees of $\theta_2^\text{turn} +\theta_3^\text{turn} + \theta_1^\text{turn}$.
Not only is the person back at the starting point after all three turn turns, but they are also facing the same direction they started. Because each turn was counter-clockwise (so each turn positively contributes to the total turn) and that the shape is a polygon (so no lines cross-over), ending up facing the same orientation as we started means the person spun a total of $360 \degree$. Therefore, we conclude that:

$$
\begin{align}
\theta_{1}^\text{turn} + \theta_{2}^\text{turn} + \theta_{3}^\text{turn} &= 360 && \text{(Constraint on turn angles)}
\end{align}
$$

We can now substitute in our rearranged version of the definition of supplementary angles into the constraint on turn angles equation. Here, I will use $n=3$ to represent the number of sides/inner angles of a triangle:

$$
\begin{align}
\sum_{i=1}^{n} \theta_{i}^\text{turn} &= 360 && \text{turn angle constraint equation)}\\
\sum_{i=1}^{n} (180-  \theta_{i}^\text{inner}) &= 360 && \text{(Substitution)}\\
180n + \sum_{i=1}^{n} (- \theta_{i}^\text{inner}) &= 360 && \text{(Factor out 180)}\\
\sum_{i=1}^{n}  \theta_{i}^\text{inner} &= -360 +180n && \text{(Rearrange)}\\
\sum_{i=1}^{n}  \theta_{i}^\text{inner} &= 180(n-2) && \text{(Factor out 180)}\\
\end{align}
$$

This is exactly the formula we know and love for the sum of inner angles of a $n$-sided polygon, which gives us the answer for a triangle: $180 \degree$.  

You'll notice that our proof above did not rely on anything about the fact that it was a triangle: the triangle just meant that the polygon had $3$ sides/angles. Therefore, we can apply our proof to all $n$-sided polygon, which is why we wrote the above derivation with $n$. 

# Credit

Thank you to [Michael Fishman](https://fishman.ai) for providing the proof and inspiration to write this note, [Zoheb Anjum](Zoheb%20Anjum.md) for additional clarity, and [Thao Nguyen](https://thao-nguyen-ai.github.io/) for reviewing/make suggestions to this note.
