---
tags:
  - "#Ocean_Dynamics"
  - "#Theory"
  - "#Subject-Note"
  - project/surfwaves
  - "#Gravity_waves"
Last Eddited: 2025-11-30
---
# Introduction

Gravity waves play an important role in the adjustment of ocean currents towards geostrophic equilibrium.
- **Internal gravity waves** contribute by breaking to mixing processes in the ocean interior, especially to diapycnal mixing.
- **Surface gravity waves** are effective in the exchange of momentum, heat, water, and gases between the ocean and the atmosphere, in addition to their practical significance for shipping.

**The force of gravity** is fundamental for the existence of gravity waves and indeed for nearly all oceanic motions with the exception of sound waves. **Earth’s rotation** is also important and leads to significant modifications.

For internal gravity waves, the existence of a density stratification caused by temperature and salinity variations with depth is crucial. The density stratification is described by a background density $\rho_0$ or equivalently the _Brunt–Väisälä_ frequency $N(z)$defined in:
$$ N^2(z)=g(\alpha \frac{\partial \theta_b}{\partial z}-\gamma \frac{\partial S_b}{\partial z} ) $$

The variable $N$ has a dimension of an inverse time, and is called _Brunt–Väisälä_ frequency (or stability or buoyancy frequency)

The role of $N$ for the stability of the water column and frequency of particle oscillations about a reference level was discussed in the chapter of _Static Stability_ in Chapter 2.9.2. **In general, the stratification is stable if $N^2>0$ and unstable if $N^2<0$ and neutrally stable if $N^2=0$.** In the ocean, the period of stability oscillations normally is between 10 and 20 min (upper ocean) and a few hours at great depths. Close to the surface very large values of N may occur in the seasonal thermocline. A typical profile of $N(z)$ in midlatitudes is shown in Figure below:

![[Screenshot 2025-10-24 at 14.16.53.png | center]]

# 7.1 Governing Equations

Equation of motion describes the adjustment of the pressure field as a consequence of a divergent velocity field. The adjustment occurs with the time-scales of sound waves.

Hence, a much longer time scale of gravity waves compared to sound waves it is possible to approximate by **assuming that in effect the adjustment has already been accomplished**. Formally, this approximation eliminating sound waves from the system.

One can use:
- the Boussinesq system.
- formulate density variable $\rho'=\rho-\rho_b(z)$ which is the deviation of the in-situ density from the stationary background state.
- The background state is characterised by the Brunt-Väisälä frequency $N(z)$ as in the quasi-geostrophic theory

Hence:
$$ \begin{align} \rho_0 (\frac{D\mathbf u}{Dt}+2\Omega\times \mathbf u)&=-\nabla p'-\rho'\nabla\Phi + \mathbf F \\ \nabla \cdot \mathbf u &=0 \\ \frac{D\rho'}{Dt}-\frac{\rho_0N^2}{g}\omega&=G_\rho \end{align} $$

Where the $\mathbf F$ and $G_\rho$ is the expression of combination of nonlinear advection terms and the frictional/diffusive terms. Will be specified to 0 in this chapter.

In the traditional approximation for the Coriolis force, the vector $\Omega$ is approximated by its locally vertical component: $2\Omega \approx \mathbf f$ with $\mathbf f=(0,0,2\Omega \sin{\phi})=(0,0,f)$. This approximation holds if the aspect ratio (ratio of vertical to horizontal length scales of motion) is small. However, for short gravity waves where the aspect ratio is not necessarily small, rotation is altogether insignificant. Therefore, in the present chapter the Coriolis frequency $f$ will be treated as constant.

Consider small deviations from the background state. One obtains the linear system:

$$ \begin{align} \frac{\partial \mathbf u_h}{\partial t}+\mathbf f\times \mathbf u_h &= -\frac{1}{\rho_0}\nabla_hp \\

\frac{\partial w}{\partial t} &= -\frac{1}{\rho_0}\frac{\partial p}{\partial z}-g\frac{\rho'}{\rho_0} \\

\frac{\partial \rho'}{\partial t}-\frac{\rho_0 N^2}{g}w &= 0 \\

\nabla_h \cdot \mathbf u_h + \frac{\partial w}{\partial z} &=0 \end{align} $$

Which explicitly distinguished between horizontal and vertical vector components, and simplified the notation for the density variable

### Boundary Condition
No mass flux crosses the boundary (kinematic boundary condition). At the bottom $z=-h$ it takes the form $w=-\mathbf u_h \cdot\nabla_h h$. For the free surface located at $z=\xi(\mathbf x_h,t)$, and ignoring the $\mathbf u_h \cdot \nabla_h \xi$ which is small in accordance with linear wave theory, that condition is expressed as $w=\partial \xi/\partial t$ at $z=\xi$. In the absence of atmospheric pressure changes, the surface elevation $\xi$ is related to pressure by $\xi=p/g\rho_0$ at $z=0$.

Therefore the boundary condition:
- At bottom:

$$ w=-\mathbf u_h \cdot\nabla_h h \quad at\quad z=-h $$

- At surface:

$$ \begin{equation} w=\frac{1}{g\rho_0}\frac{\partial p}{\partial t}\quad at\quad z=0 \end{equation} $$

In many situations, (8) is approximated by the simpler condition:

$$ \begin{equation} w=0 \quad at \quad z=0 \end{equation} $$

which is the rigid-lid condition

# 7.2 Plane Gravity Waves

Since all coefficients are independent of $x_h$ and $t$, a solution describing plane waves in the horizontal directions can be found by a separation ansatz of the form:

$$ \begin{equation} \begin{bmatrix} \mathbf u \\ p \\ w \\ \rho \end{bmatrix}

\begin{bmatrix} U_0dW/dz \\P_0dW/dz \\ W(z) \\ R_0N^2(z)W(z) \end{bmatrix} e^{i(\mathbf k_h \cdot\mathbf u_h-\omega t)} \end{equation} $$

Here:

- $e^{i(\mathbf k_h \cdot\mathbf u_h-\omega t)}$ represents the oscillatory wave part, identical to $e^{i(kx+ly-\omega t)}$
    - the $\mathbf k_h$ is the horizontal wavenumber vector in $m^{-1}$;
    - $\omega$ is the wave frequency in $s^{-1}$

Here $U_0,P_0,R_0$ are constants, and $W(z)$ is the vertical structure function which will be determined below. Inserting (10) to (4,6,7) will get:

$$ \begin{align} U_0&=i(\omega\mathbf k_h-i\mathbf f \times \mathbf k_h)/\omega k_h^2 \\

P_0&=i\rho_0(\omega^2-f^2)/\omega k_h^2 \\

R_0&=i\rho_0/g\omega \end{align} $$

The dimensions of the factors $U_0,P_0,R_0$ differ from those of the field $\mathbf u,p,\rho$. Inserting (10) in (5), using (12) and (13) gets:

$$ \begin{equation} \frac{d^2W}{dz^2}+k_h^2\frac{N^2(z)-\omega^2}{\omega^2-f^2}W=0 \end{equation} $$

as a differential equation for the function $W(z)$ representing the vertical dependence

The wave dispersion relation is in the form of $\omega^2=...$

# 7.4 The Influence of Boundaries

## 7.4.4 Accuracy of the Rigid-lid Condition

The boundary condition at the sea surface has been approximated by the rigid-lid condition (9), The accuracy is investigated below. With the plan wave solution (10), the exact surface condition 98) takes the form:

$$ \begin{equation} W=\frac{\omega^2-f^2}{gk_h^2}\frac{dW}{dz}\quad at\quad z=0 \end{equation} $$

It is sufficient to consider only the case $N=const$, so we insert the solution of $W(z) = A_+(e^{ik_3z}-e^{-ik_3z}e^{-2ik_3H}=A\sin{(z+H)})$ ($A=2iA_+e^{-ik_3H}$ and $k_3$ is the wavenumber at vertical direction) which already satisfies the boundary condition at $z=-H$ into the above equation. It follows that the upper boundary condition can be satisfied only if:

$$ \begin{equation} \omega^2=f^2+\frac{k_h^2}{k_3^2}gk_3\tan k_3H \end{equation} $$

> [!Attention]
> Using the above equation to replace the $\omega$ in $k_3(z)=\pm(\frac{N^2(z)-\omega^2}{w^2-f^2})^{1/2}k_h$ leads to:
> 
> $$ \begin{equation} \frac{H(N^2-f^2)}{g}=[(k_h^2+k_3^2)H^2]\frac{\tan k_3H}{k_3H} \end{equation} $$
> 
> Which can be used to determine the vertical wave number $k_3$ for any given $k_h$.

the left side has the magnitude:

$$

\epsilon=H\frac{N^2-f^2}{g}\approx\frac{N^2H}{g}\sim\frac{H}{g}\frac{g}{\rho_0}\frac{\Delta \rho}{\rho_0}\approx 2\times 10^{-3} \ll 1 $$

Therefore the right hand side of the equation above also must be $\ll1$, and thus lead to one of the two dimensionless factors $[(k_h^2+k_3^2)H^2]$ and $\frac{\tan k_3H}{k_3H}$ must be $\ll 1$. The later gives:

$$ \begin{equation} k_3H=n\pi+O(\epsilon), \quad n=1,2,... \end{equation} $$

Up to terms of order $\epsilon$. Therefore, the error caused by the rigid-lid approximation is small, approximately 0.2% for internal gravity waves

# 7.5 Surface Waves

==**The restoring force of gravity can be effective only in the presence of vertical density gradients.**== The largest possible density gradient occurs at the air-sea interface where the density changes by three orders of magnitude. Therefore, one can expect a wave mode associated with excursions of the free surface.

We start from the relation relates vertical to horizontal wave number based on the exact surface boundary condition (8). We already found one possibility to satisfy this relationship is to set one of the dimensionless factor $\frac{\tan k_3H}{k_3H} \ll1$. The only alternative possibility is that the other dimensionless factor is small, i.e., $(k_h^2+k_3^2)H^2\ll1$. For arbitary values of $k_h$ this cannot be satisfied with a real $k_3$. Allowing, however, imaginary values for $k_3$, one has:

$$ \begin{equation} k_3=ik_h+O(\epsilon) \end{equation} $$

> [!Attention]
> The vertical structure becomes now exponentially decreasing from the surface. With the equation and the identity $ix\tan ix=-x\tanh x$, from the $k_h$ and $k_3$ relation, we can get:
> 
> $$ \begin{equation} \omega^2=f^2+gk_h\tanh k_hH+O(\epsilon) \end{equation} $$
> 
> Which is **dispersion relation of surface gravity waves**, **tells how fast oscillations in time $\omega$ are related to variations in space $k$.**

With the $k_3$ expression and the solution to $W(z)$, the vertical profiles of all field variables are given as hyperbolic functions. With (11), the solution for the velocity field is given as:
$$ \begin{align}w &= a\omega \frac{\sinh\!k_h(z+H)}{\sinh k_hH} \sin\theta=W(z)\sin{\theta}, \\[6pt]\mathbf{u} &= a\omega \frac{\cosh\!k_h(z+H)}{\sinh k_hH}\left( \frac{\mathbf{k}_h}{k_h}\cos\theta - \frac{\mathbf{f} \times \mathbf{k}_h}{\omega k_h}\sin\theta\right) \nonumber\\&= \mathbf{U}_c(z)\cos\theta + \mathbf{U}_s(z)\sin\theta\end{align} $$

With $\theta=\mathbf k_h \cdot \mathbf x_h-\omega t$. Here the amplitude $a$ is chosen such that the surface elevation is $\xi=a\cos{\theta}$. With (12) (13), solutions for the other field variables can be obtained. The solutions correspond to those of internal gravity waves, except for the imaginary vertical wave number. Note that to order $\epsilon$, both solutions and the dispersion relation are independent of stratification.

Two limiting cases can be distinguished.

- For long waves with $k_hH \ll 1$, one has $\tanh k_hH\approx k_hH$, and pressure as well as horizontal velocity do no longer depend on depth while $w$ decreases linearly from surface to bottom. The dispersion relation becomes $\omega^2=f^2+gHk_h^2$, and rotation is only significant on scales of order $\sqrt{gH}/f\sim2,000\:m$, such as for tides. The phase speed $c=\sqrt{f^2/k_h^2+gH}\approx \sqrt{gH}$, and exceeds this value are vertically constant, and the vertical velocity decreases linearly to the bottom
- For short waves with $k_hH \gg 1$, $\tanh k_hH \;\rightarrow1$ applies. Since $gk_h \gg g/H \gg f^2$, the dispersion relation is $\omega^2=gk_h$ (so called _**deep water limit**_)so that in the dispersion diagram all curves fall together. From (21) it further follows that pressure and thus all other fields decrease exponentially in the vertical direction.

### Particle motion
With assumptions and simplification:
- In the surface with $k_hH \ll 1$,the particle paths (along the wave group) are nearly horizontal
- In the deep water with $k_hH \gg 1$, particle paths are circles with the radius which vanishes exponentially with depth

In the general case when rotation cannot be neglected, the orientation of the elliptical path is no longer vertical but tilted in the direction orthogonal to $k_h$

### Stokes drift
Consider the average of particle paths over phase, a non-vanishing part is called _**Stokes drift**_ and causes a transport into the direction of $k_h$. Although its amplitude is proportional to the square of the wave amplitude and thus is very small, it is important for momentum and mass transport of waves.

From papers:
- “the sea surface velocity could be divided into two parts, the sea surface current velocity and surface wave induced Stokes drift velocity.” (Zhao et al., 2022)
    - “In-situ observations show that during a hurricane, the Stokes drift could account for up to 20% of the sea surface current on average (Curcic et al., 2016).” (Zhao et al., 2022)
    - “both sea surface current and Stokes drift are un-negligible when calculating air-sea flux”