---
tags:
  - Theory
Last Eddited: 2025-12-01
---
# Acceleration in a rotating frame

let $\mathbf v_{R}$ be the velocity in the rotating frame

The relation between the inertial and rotating frame derivatives is:

$$ \begin{equation} \frac{dA}{dt}\bigg|_{I}=\frac{dA}{dt}\bigg|_{R}+\mathbf\Omega \times A \end{equation} $$

for any vector $A$

Hence, apply this to $\mathbf v_{R}$, we get the inertial acceleration $\mathbf a_{I}$:

$$ \begin{equation} \begin{split} \mathbf a_{I} &=\frac{d\mathbf v_{I}}{dt} \\ &=\frac{d}{dt}\bigg|_{I}(\mathbf v_{R}+\mathbf\Omega \times r) \\ &=\frac{d\mathbf v_{R}}{dt}\bigg|_{R}+\mathbf\Omega \times \mathbf v_{R} + \frac{d}{dt}\bigg|_{R}(\mathbf\Omega \times r)+\mathbf\Omega \times (\mathbf\Omega \times r)\\ &=\frac{d\mathbf v_{R}}{dt}\bigg|_{R} + 2\mathbf\Omega \times \mathbf v_{R}+\mathbf\Omega \times (\mathbf\Omega \times r) \end{split} \end{equation} $$

Thus, there’re two $\Omega \times r$ terms, which are the Coriolis acceleration term, comes from:

- Differentiating the rotating coordinate basis
- Differentiating the position vector in rotating frame

While get back to the rotating framework (target frame) and times the mass to get the Force ($F=ma$):

$$ \begin{equation} m\frac{d\mathbf v_{R}}{dt}\bigg|_{R} =m\frac{d\mathbf v_{I}}{dt}- 2m\mathbf\Omega \times \mathbf v_{R}-m\mathbf\Omega \times (\mathbf\Omega \times r) \end{equation} $$

The Coriolis force in the rotating frame is therefore:

$$ \begin{equation} F_{C}=-2m\mathbf\Omega \times \mathbf v_{R} \end{equation} $$

and the Coriolis acceleration in the rotating frame is:

$$ \mathbf a_{C}=-2\mathbf\Omega \times \mathbf v_{R} $$

If focused on the local coordinate system:

- x: eastward → i
- y: northward → j
- z: upward → k

In the local frame, the Earth’s rotation vector decompose to horizontal and vertical components:

$$ \mathbf \Omega=\Omega \cos\phi \vec{j}+\Omega \sin\phi \vec{k} $$

Then, the Coriolis acceleration becomes:

$$ -2\mathbf \Omega \times \mathbf v=2\Omega \cos\phi u\vec{k}-2\Omega \sin\phi (u\vec{j}-v\vec{i}) $$

> Keep the horizontal component of the acceleration since the horizontal velocity dominate in geophysical flow, then we have the horizontal Coriolis acceleration:
> 
> $$ \begin{equation} \begin{split} \mathbf a_{C,h} &=-2\Omega \sin\phi (u\vec{j}-v\vec{i}) \\ &=f\vec{k}\times\mathbf v_{h} \end{split} \end{equation} $$
> 
> where:
> 
> $$ \begin{equation} f=2\Omega \sin\phi \end{equation} $$
> 
> The Coriolis parameter $f$ is a scalar, but it represents the vertical component of the Earth’s rotation, hence the vector form is:
> 
> $$ \begin{equation} \mathbf f=2\mathbf \Omega_{vertical}=2\Omega \sin\phi \vec{k} \end{equation} $$
> 
> Meaning:
> 
> - $f>0$ → vector points upward → North Hemisphere → deflect motion to right
> - $f<0$ → vector points downward → South Hemisphere → deflect motion to left
> - $f\approx0$ → near equator, no horizontal Coriolis effect

Thus:

- The Coriolis parameter vector form points in vertical direction, but the effect is in the horizontal direction through the cross product with the flow velocity
- The direction of the effect (that deflect the flow velocity) can be determined use right-hand rule