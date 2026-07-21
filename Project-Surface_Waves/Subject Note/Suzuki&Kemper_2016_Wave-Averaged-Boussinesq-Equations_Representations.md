---
tags:
  - stokes_drift
  - "#project/surfwaves"
  - wave/surface_wave
  - presenter/Nobushiro_Suzuki
  - important_paper
  - Theory
Last Eddited: 2026-04-24
---
# Finalising WAB momentum equation in ICON structure
## Vectors form
Wave-averaged Boussinesq (WAB) Momentum equations in different full 3D vector forms (Suzuki & Fox-Kemper, 2016):
$$
\begin{align}
\partial_t \mathbf{u}+ \left(\nabla \times \mathbf{u}+\mathbf{f}\right)\times \mathbf{u}^L  &= \mathbf{b}+\mathbf{D}^{u} -(\nabla p + \frac{1}{2}|\mathbf{u}^L|^2),
\tag{1}
\\
\partial_t \mathbf{u}^L+ \left(\mathbf{u}^{L}\cdot\nabla\right)\mathbf{u}^L + \mathbf{f}\times\mathbf{u}^{L}  &= \mathbf{b}+\mathbf{D}^{u} -\nabla p - \mathbf{u}^L \times \left(\nabla \times \mathbf{u}^s \right) + \partial_t \mathbf{u}^s
\tag{2}
\end{align}
$$
Eq.1 is the momentum equation structure that implemented in the ICON source code, with all oepraters.

> [!Attention] **Vectorised WAB momentum equation**
> Thus, to efficiently use the operators of the ICON, and avoid too much modification, the final WAB momentum equation will be introduced as replacing all Eulerian velocity to the Lagrangian velocity $\mathbf{u}^L=\mathbf{u}+ \mathbf{u}^s$:
> $$
> \boxed{\partial_t \mathbf{u}^L+ \left(\nabla \times \mathbf{u}^L+\mathbf{f}\right)\times \mathbf{u}^L = \mathbf{b}+\mathbf{D}^{u} -(\nabla p + \frac{1}{2}|\mathbf{u}^L|^2) + (\nabla \times \mathbf{u}^s)\times \mathbf{u}^L + \partial_t \mathbf{u}^s} \tag{3}
> $$
> This replacement will lead to two additional terms at the right-hand-side of Eq.(3), they are:
> - wave-influenced Stokes vortex force: $(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L$
> - prognostic Stokes velocity: $\partial_t \mathbf{u}^s$

Comparing Eq.(3) to Eq.(1), one can find:
$$(\nabla \times \mathbf{u}^L)\times \mathbf{u}^L + \nabla(p + \frac{1}{2}|\mathbf{u}^L|^2)-(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L = (\mathbf{u}^L \cdot \nabla)\mathbf{u}^L + \nabla p + \mathbf{u}^L \times (\nabla \times \mathbf{u}^s)$$
Because:
- Distributivity of cross product: $(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L = - \mathbf{u}^L \times (\nabla \times \mathbf{u}^s)$
- Convective acceleration identity: $(\mathbf{u}\cdot\nabla)\mathbf{u} = (\nabla \times \mathbf{u}) \times \mathbf{u} + \nabla(\frac{1}{2}|\mathbf{u}|^2)$
See detailed conversion rules in [[Mathematical_Understanding#Conversions in WAB momentum framework]]

## 3D expansions
The **3D version** without the consideration of wavy-hydrostatic approximation are summarised below:
$$
\begin{align}
\partial_t u^L - (f + \omega_z^L)v^L + w^L\omega_y^L &= b_x + D^u -\partial_x p - (u^L \partial_x u^L+ v^L \partial_x v^L + w^L \partial_x w^L)  + (w^L\omega_y^s - v^L \omega_z^s) + \partial_t u^s 
\\
\partial_t v^L + (f + \omega_z^L)u^L - w^L\omega_x^L &= b_y + D^v -\partial_y p - (u^L \partial_y u^L+ v^L \partial_y v^L + w^L \partial_y w^L)  + (u^L\omega_z^s - w^L \omega_x^s) + \partial_t v^s
\\
\partial_t w^L + v^L\omega_x^L - u^L\omega_y^L &= b_z + D^z -\partial_z p - (u^L \partial_z u^L+ v^L \partial_z v^L + w^L \partial_z w^L)  + (v^L\omega_x^s - u^L \omega_y^s) + \partial_t w^s
\end{align}
$$
Where:
$$
\nabla \times \mathbf{u}^L =
\begin{pmatrix}
\omega_x^L \\
\omega_y^L \\
\omega_z^L
\end{pmatrix}
=
\begin{pmatrix}
-\partial_z v^L+\partial_y w^L \\
\partial_z u^L-\partial_x w^L \\
\partial_x v^L-\partial_y u^L
\end{pmatrix}, \quad\quad
\nabla \times \mathbf{u}^s =
\begin{pmatrix}
\omega_x^s \\
\omega_y^s \\
\omega_z^s
\end{pmatrix}
=
\begin{pmatrix}
-\partial_z v^s+\partial_y w^s \\
\partial_z u^s-\partial_x w^s \\
\partial_x v^s-\partial_y u^s
\end{pmatrix}.
$$
Thus,
$$
(\nabla \times \mathbf{u}^L) \times \mathbf{u}^L =
\begin{pmatrix}
	w^L\omega_y^L- v^L\omega_z^L \\
	u^L\omega_z^L- w^L\omega_x^L \\
	v^L\omega_x^L- u^L\omega_y^L
\end{pmatrix}, \quad\quad
(\nabla \times \mathbf{u}^s) \times \mathbf{u}^L =
\begin{pmatrix}
	w^L\omega_y^s- v^L\omega_z^s \\
	u^L\omega_z^s- w^L\omega_x^s \\
	v^L\omega_x^s- u^L\omega_y^s
\end{pmatrix}
$$
And the Kinetic energy of the Lagrangian velocity, or conservative force, introduces a wave influence in the pressure energy transport when combining with the pressure:
$$ -\nabla(p+\frac{1}{2}|\mathbf{u}^L|^2) = -\nabla p - u_j^L\nabla u_j^L$$
Where the $- u_j^L\nabla u_j^L$ is in the form of Einstein summation. See details in [[Mathematical_Understanding#Conversions in WAB momentum framework]]

> [!Important] **3D WAB momentum Equations**:
> Expanding all the vorticity terms, cancelling the $-w^L \partial_x w^L$ and $-w^L \partial_y w^L$ at both sides on the horizontal momentum equation, one can get the full 3D WAB momentum equations like:
> $$
> \begin{align}
> \partial_t u^L - (f + \partial_x v^L - \partial_y u^l)v^L + w^L\partial_z u^L &= b_x + D^u -\partial_x p - (u^L \partial_x u^L+ v^L \partial_x v^L)  + (w^L \partial_z u^s - w^L \partial_x w^s- v^L \partial_x v^s + v^L \partial_y u^s) + \partial_t u^s 
> \\
> \partial_t v^L + (f + \partial_x v^l - \partial_y u^L)u^L - w^L\partial_z v^L &= b_y + D^v -\partial_y p - (u^L \partial_y u^L+ v^L \partial_y v^L)  + (u^L\partial_x v^s - u^L \partial_y u^s + w^L \partial_z v^s- w^L\partial_y w^s ) + \partial_t v^s \tag{4}
> \\
> \partial_t w^L + v^L\partial_y w^L + u^L \partial_x w^L &= b_z + D^w -\partial_z p - w^L \partial_z w^L  + (v^L\partial_y w^s - v^L \partial_z v^s - u^L \partial_z u^s + u^L \partial_x w^s) + \partial_t w^s
> \end{align}
> $$

## 2D horizontal vector + vertical
Now to match with the ICON-o ~={red}**ocean primitive equations** (Equation (1) in Korn, 2017)=~:
![[Screenshot 2026-07-20 at 11.17.09.png | centre]]

For consistency, we use the same notation as above:
- $f$ the Coriolis parameter, $\rho$ and $rho_0$ the sea water density and its reference value, $p$ the hydrostatic pressure, $g$ the gravitational constant, $\vec{z}$ the local vertical upward unit vector and $B$ describes the bottom topography 
- horizontal velocity field: $\mathbf{v}=(u,v)$; vertical velocity $w$; horizontal vector operator e.g.,$\nabla_h$; $D_h$ describes the horizontal velocity diffusion
- $\mathrm{A}_v$ the coefficient of vertical velocity diffusion, horizontal and vettical diffusion coefficients for a tracer $C$ are denoted by $\mathrm{K}^C$ and $\mathrm{A}^C$.

> [!Important] **WAB momentum equation in 2D horizontal vector + Vertical velocity**
> The WAB form of momentum equation becomes:
> $$
> \begin{align}
> &\partial_t \mathbf{v}^L + (f+ \omega^L)\vec{z} \times \mathbf{v}^L + \frac{\nabla_h |\mathbf{v}^L|^2}{2}+ w^L \partial_z \mathbf{v}^L + \frac{1}{\rho_0}\nabla_h p - D_h \mathbf{v}^L - \frac{\partial}{\partial z} A_v \frac{\partial}{\partial z} \mathbf{v}^L - (w^L (\partial_z \mathbf{v}^s - \nabla_h w^s) + \omega^s \vec{z} \times \mathbf{v}^L) - \partial_t \mathbf{v}^s = 0
> \\
> &\partial_t w^L + v^L\partial_y w^L + u^L \partial_x w^L + \partial_z p = -\rho g + D^w - w^L \partial_z w^L  + (v^L\partial_y w^s - v^L \partial_z v^s - u^L \partial_z u^s + u^L \partial_x w^s) + \partial_t w^s
> \\
> &\text{div}_h \mathbf{v}^L + \frac{\partial w^L}{\partial z} = 0
> \end{align}
> $$

vertical velocity is diagnostic for both Eulerian and Stokes, diagnose from the horizontal velocities using continuity

## Scaling Analysis
> [!Quote] **Scaling**
> First, introducing the scales
> For the Lagrangian velocity, let:
> $$ x,y\sim L,\qquad z\sim H,\qquad u^L,v^L\sim U,\qquad w^L\sim W.$$> for large-scale circulation, vertical velocity is diagnosed from the continuity:
> $$ \text{div}_h \mathbf{v}^L + \frac{\partial w^L}{\partial z} = 0 $$
> thus,
> $$ \epsilon = \frac{H}{L} \ll 1, \qquad w^L \sim W \sim \epsilon U$$
> While for the Stokes velocity, it has its own length and velocity scales:
> $$ x,y \sim L_s, \qquad z\sim H_s, \qquad u^s, v^s\sim U_s, $$
> and the vertical Stokes velocity also satisfy continuity:
> $$\epsilon_s = \frac{H_s}{L_s}, \qquad w^s\sim W_s\sim\epsilon_s U_s $$
### Horizontal momentum
**This section only discuss the scales of additional terms we implement**, they are:
$$
(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L = 
 \begin{pmatrix}
	w^L\partial_z u^s - w^L \partial_x w^s - v^L \partial_x v^s + v^L \partial_y u^s \\
	u^L\partial_x v^s - u^L \partial_y u^s + w^L \partial_z v^s - w^L \partial_y w^s \\
	-v^L\partial_z v^s + v^L \partial_y w^s - u^L \partial_z u^s + u^L \partial_x w^s
 \end{pmatrix},
$$
The horizontal component:
$$
 [(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L]_{\text{h}} 
 =
 \begin{pmatrix}
 	w^L\partial_z u^s - w^L \partial_x w^s - v^L \partial_x v^s + v^L \partial_y u^s \\
 	u^L\partial_x v^s - u^L \partial_y u^s + w^L \partial_z v^s - w^L \partial_y w^s 
 \end{pmatrix} 
$$
The horizontal gradient of vertical Stokes drift velocity terms: $- w^L \partial_x w^s$ and $- w^L \partial_y w^s$ are significantly smaller than the vertical gradient of horizontal Stokes drift velocity terms. The ratio between (1) horizontal gradient of vertical Stokes drift velocity and (2) vertical gradient of horizontal Stokes drift velocity:
$$
\frac{w^L\nabla_h w^s} {w^L\partial_z\mathbf{v}^s} \sim \frac{W\epsilon_s U_s/L_s}{WU_s/H_s} =  \epsilon_s^2.
$$
therefore, if the vertical length scale of Stokes profile is significantly smaller than the horizontal length scale,
$$
\frac{w^L\nabla_h w^s} {w^L\partial_z\mathbf{v}^s} \sim \frac{W\epsilon_s U_s/L_s}{WU_s/H_s} =  \epsilon_s^2 = (\frac{H_s}{L_s})^2 \ll 1
$$
Thus the horizontal gradient of vertical Stokes drift velocity terms can be safely neglected if we consider the vertical gradient of horizontal Stokes drift.

Then let’s compare the $w^L \partial_z \mathbf{v}^s$ terms with the other terms in the horizontal momentum equation (Eq. (4)). In large-scale ocean circulation, the leading horizontal balance is often the geostrophic balance between the pressure gradient and Coriolis:
$$ f\vec{z}\times \mathbf{v}^L+\frac{1}{\rho_0}\nabla_hp \sim 0$$
Thus the leading scaling is: 
$$ fU$$
So the Stokes-to-Coriolis ratio is:
$$ \frac{w^L \partial_z \mathbf{v}^L}{fU} \sim \frac{\epsilon U U_s/H_s}{fU}=\frac{\epsilon U_s}{f H_s}$$
using typical values:
$$ f\sim 10^{-4}\ \mathrm{s^{-1}}, \qquad H_s\sim 10\text{--}50\ \mathrm{m}, \qquad U_s\sim 0.01\text{--}0.1\ \mathrm{m\,s^{-1}}, $$
and
$$ \epsilon=\frac{H}{L}\sim 10^{-3}\text{-}10^{-2}, $$
we get
$$ \frac{\epsilon U_s}{fH_s} \sim 0.002\text{-}1. $$
For example,
$$ \epsilon=10^{-2},\quad U_s=0.05\ \mathrm{m\,s^{-1}},\quad H_s=20\ \mathrm{m} $$
gives
$$ \frac{\epsilon U_s}{fH_s} = \frac{10^{-2}\times 0.05}{10^{-4}\times 20} = 0.25. $$
So this term can be smaller than Coriolis, but not always negligibly small. It becomes more important when:
$$ U_s \text{ is large}, \qquad H_s \text{ is small}, \qquad f \text{ is small}, \qquad \epsilon=H/L \text{ is not extremely small}. $$
> [!Important] 
> Thus, $w^L \partial_z \mathbf{v}^s$ should be considered, while the $(- w^L \partial_x w^s, - w^L \partial_y w^s)$ term is $\epsilon_s^2$ times smaller than the $w^L \partial_z \mathbf{v}^s$, which can be neglect when $\epsilon_s=H_s / L_s$ is small
> 
> Therefore, one can find two ways to represent the wave-influenced Stokes vortex force in the horizontal momentum equation.
> - **Origin**:
> $$ \boxed{[(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L]_{\text{h}} = w^L(\partial_z \mathbf{v}^s - \nabla_h w^s) + \omega^s \vec{z} \times \mathbf{v}^L}$$
> - **Neglect** $- w^L \partial_x w^s$ **and** $- w^L \partial_y w^s$:
> $$ \boxed{[(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L]_{\text{h}} \approx w^L\partial_z \mathbf{v}^s  + \omega^s \vec{z} \times \mathbf{v}^L} $$
> 
> The difference is only the product of vertical Lagrangian velocity and horizontal gradient of vertical Stokes drift velocity $w^L \nabla_h w^s$.

> [!Warning] However, although in the momentum framework, the terms can be safely neglected, one still need to test if neglect terms will introduce significant artificial sink/source terms in the potential vorticity or not.

### Vertical Momentum Scaling
The vertical momentum equation after expansion is
$$\partial_t w^L + v^L\partial_y w^L + u^L\partial_x w^L + \partial_z p = -\rho g + D^w - w^L\partial_z w^L + \left(v^L\partial_y w^s - v^L\partial_z v^s - u^L\partial_z u^s + u^L\partial_x w^s\right) + \partial_t w^s.$$
To do scaling consistently, divide by $\rho_0$ and use the kinematic pressure anomaly
$$P'=\frac{p'}{\rho_0}.$$
After removing the resting hydrostatic background,
$$p=p_0(z)+p',\qquad \rho=\rho_0+\rho',$$
with
$$\partial_z p_0=-\rho_0 g,$$
the dynamically relevant hydrostatic balance is
$$\partial_z P'=b,\qquad b=-\frac{g\rho'}{\rho_0}.$$
From horizontal geostrophic scaling,
$$\nabla_h P'\sim fU,$$
so
$$P'\sim fUL.$$
Therefore,
$$\partial_z P'\sim \frac{P'}{H}\sim \frac{fUL}{H}=\frac{fU}{\epsilon},\qquad \epsilon=\frac{H}{L}.$$
Thus the leading pressure-buoyancy scale is
$$\boxed{\partial_z P' \sim b \sim \frac{fU}{\epsilon}.}$$

Thus, the ordinary vertical acceleration terms scale as
$$\partial_t w^L,\quad u^L\partial_x w^L,\quad v^L\partial_y w^L,\quad w^L\partial_z w^L \sim \epsilon^2\frac{U^2}{H}.$$
Relative to the leading hydrostatic scale,
$$\frac{\epsilon^2 U^2/H}{fU/\epsilon}=\epsilon^2 Ro,\qquad Ro=\frac{U}{fL}.$$
Therefore,
$$\boxed{\partial_t w^L,\ u^L\partial_x w^L,\ v^L\partial_y w^L,\ w^L\partial_z w^L \ll \partial_z P',\ b}$$
when
$$\epsilon^2 Ro\ll 1.$$
Now consider the Stokes terms. The vertical-gradient terms scale as
$$u^L\partial_z u^s,\quad v^L\partial_z v^s \sim \frac{UU_s}{H_s}.$$
The horizontal-gradient terms involving $w^s$ scale as
$$u^L\partial_x w^s,\quad v^L\partial_y w^s \sim \frac{UW_s}{L_s}\sim \frac{U\epsilon_s U_s}{L_s}.$$
Comparing them,
$$\frac{u^L\partial_x w^s}{u^L\partial_z u^s}\sim \frac{U\epsilon_s U_s/L_s}{UU_s/H_s}=\epsilon_s^2.$$
Therefore,
$$\boxed{u^L\partial_x w^s,\ v^L\partial_y w^s \text{ are smaller than } u^L\partial_z u^s,\ v^L\partial_z v^s \text{ by } \epsilon_s^2.}$$
Thus the horizontal-gradient terms of $w^s$ can be neglected if $\epsilon_s\ll 1.$
Now compare the vertical-gradient Stokes terms with the leading hydrostatic scale:
$$\frac{UU_s/H_s}{fU/\epsilon}=\frac{\epsilon U_s}{fH_s}.$$
This ratio is not automatically small. With typical values
$$\epsilon\sim 10^{-3}\text{--}10^{-2},\qquad U_s\sim 0.01\text{--}0.1\ \mathrm{m\,s^{-1}},\qquad H_s\sim 10\text{--}50\ \mathrm{m},\qquad f\sim 10^{-4}\ \mathrm{s^{-1}},$$
we obtain
$$\frac{\epsilon U_s}{fH_s}\sim 10^{-3}\text{--}1.$$
For example,
$$\epsilon=10^{-2},\qquad U_s=0.05\ \mathrm{m\,s^{-1}},\qquad H_s=20\ \mathrm{m},$$
gives
$$\frac{\epsilon U_s}{fH_s}=\frac{10^{-2}\times 0.05}{10^{-4}\times 20}=0.25.$$
> [!Important]
> So the vertical-gradient Stokes terms can be $O(0.1)$ or even $O(1)$ relative to the leading hydrostatic pressure-buoyancy balance. Therefore, in a wavy-hydrostatic approximation, it is reasonable to retain
> $$-u^L\partial_z u^s - v^L\partial_z v^s$$
> while neglecting
> $$u^L\partial_x w^s,\qquad v^L\partial_y w^s$$
> under the assumption $\epsilon_s\ll 1.$ The resulting wavy-hydrostatic balance is
> $$\boxed{\partial_z P'=b-u^L\partial_z u^s-v^L\partial_z v^s}$$
> or equivalent: $$\boxed{\partial_z p=-\rho g-\rho_0(u^L\partial_z u^s+v^L\partial_z v^s)}$$
> up to smaller vertical-inertia, diffusion, and horizontal-\(w^s\)-gradient terms.

## WAB in ICON structure
Recall the notation from (Korn, 2017):
- State vector $\mathbf{v}, \eta, T , S$ consisting of a horizontal velocity field $\mathbf{v}$, the surface elevation $\eta$ and the oceanic tracers temperature and salinity $C = \{T , S\}$.
- $f$ the Coriolis parameter, $\rho$ and $\rho_0$ the sea water density and its reference value, $p$ the hydrostatic pressure, $g$ the gravitational constant, $\vec{z}$ the local vertical upward unit vector and $B$ describes the bottom topography 
- Vertical velocity $w$; horizontal vector operator e.g.,$\nabla_h$; $D_h$ describes the horizontal velocity diffusion
- $\mathrm{A}^{\mathrm{v}}$ the coefficient of vertical velocity diffusion, horizontal and vertical diffusion coefficients for a tracer $C$ are denoted by $\mathrm{K}^C$ and $\mathrm{A}^C$.

> [!Attention] **WAB Equations in ICON-o structure**
> The ICON-o ocean primitive equation framework in WAB form is:
> $$
> \begin{align}
> &\frac{\partial \mathbf{v}^L}{\partial t} + (f+ \omega^L)\vec{z} \times \mathbf{v}^L + \frac{\nabla_h |\mathbf{v}^L|^2}{2}+ w^L \frac{\partial \mathbf{v}^L}{\partial z} + \frac{1}{\rho_0}\nabla_h p  - D_h \mathbf{v}^L - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{v}} \frac{\partial}{\partial z} \mathbf{v}^L - (w^L (\frac{\partial \mathbf{v}^s}{\partial z} - \nabla_h w^s) + \omega^s \vec{z} \times \mathbf{v}^L) - \frac{\partial \mathbf{v}^s}{\partial t} = 0
> \\
> &\frac{\partial p}{\partial z} = -\rho g - \rho_0 (v^L \partial_z v^s + u^L \partial_z u^s)
> \\
> &\text{div}_h \mathbf{v}^L + \frac{\partial w^L}{\partial z} = 0
> \\
> &\text{div}_h \mathbf{v}^s + \frac{\partial w^s}{\partial z} = 0 \tag{5}
> \\
> &\frac{\partial \eta}{\partial t} + \text{div}_h \int_{-B}^{\eta}\mathbf{v}^L\;dz = 0
> \\
> & \frac{\partial C}{\partial t} + \text{div}(C\mathbf{v})-\text{div}_h(\mathrm{K}^{\mathrm{C}} \nabla C) - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{C}} \frac{\partial}{\partial z} C = 0
> \\
> &\rho = F_{eos}(p, T, S),
> \end{align}
> $$

If decompose the pressure as a sum of surface pressure and internal hydrostatic pressure: $$p(x,y,z,t)=p_{hy}(x,y,z,t)+ p_{s}(x,y,t) $$where the hydrostatic pressure is given by the weight of a water column above a vertical level $z$: $$p_{hy}(x,y,z,t) = g\int_{z}^{0}\rho(x,y,z',t)\;dz' $$
The surface pressure $p_s$ depends only on the horizontal coordinates, assuming a well mixed surface with a uniform density given by the reference density, surface pressure is modelled in terms of the surface elevation $\eta$: $p_s(x, y, t) = g\rho_0\eta(x, y, t)$. Inserting the pressure decomposition into Eq.(5) yields for the velocity equation:
$$\frac{\partial \mathbf{v}^L}{\partial t} + (f+ \omega^L)\vec{z} \times \mathbf{v}^L + \frac{\nabla_h |\mathbf{v}^L|^2}{2}+ w^L \frac{\partial \mathbf{v}^L}{\partial z} + \frac{1}{\rho_0}\nabla_h p_{hy} + g\nabla_h \eta - D_h \mathbf{v}^L - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{v}} \frac{\partial}{\partial z} \mathbf{v}^L - (w^L (\frac{\partial \mathbf{v}^s}{\partial z} - \nabla_h w^s) + \omega^s \vec{z} \times \mathbf{v}^L) - \frac{\partial \mathbf{v}^s}{\partial t} = 0 $$

## Numerical consideration



# Craik–Leibovich (CL) Vortex Force
## CL vortex force form and identity
The vortex force can be rewritten using vector identities (as $A\times B=-B\times A$):
$$\mathbf u^s \times \boldsymbol{\omega}^E = -(\nabla \times \mathbf u^E)\times \mathbf u^s $$
and decomposed as (see Appendix in ~={blue}(Suzuki & Fox-Kemper, 2016))=~:
$$-(\nabla \times \mathbf u^E)\times \mathbf u_s = u^s_j \nabla u^E_j - (\mathbf u^s \cdot \nabla)\mathbf u^E$$
where:
- $u^s_j \nabla u^E_j$: Eulerian-gradient term
- $(\mathbf u^s \cdot \nabla)\mathbf u^E$: Stokes advection term

Rearrange the above two terms into:
- Combine the **Stokes advection** with the Eulerian advection to get **Lagrangian advection term**: $$(\mathbf u^s \cdot \nabla)\mathbf u^E+(\mathbf u^E \cdot \nabla)\mathbf u^E=(\mathbf u^L \cdot \nabla)\mathbf u^E$$
- After the GLM reorganisation, combining the **shear-like term** with the gradient of kinetic energy for both Eulerian velocity and Lagrangian velocity, one can get the **Stokes shear term**: $$u^s_j \nabla u^E_j+\frac{1}{2}(\nabla|\mathbf u^E|^2)-\frac{1}{2}(\nabla|\mathbf u^L|^2)=-u^L_j \nabla u^s_j$$
> [!Attention] Important Separation:
> Stokes shear force ($-u^L_j \nabla u^s_j$) is the part that transfers wave energy to Eulerian velocities, and Stokes advection ($(\mathbf u^s \cdot \nabla)\mathbf u^E$) is the part that does not transfer wave energy ~={blue}(Suzuki & Fox-Kemper, 2016)=~

## Vortex force affects relative vorticity
### Curl of the CL vortex force
Take the curl of the momentum equation, the vortex force contributes to vorticity evolution through: $$\nabla \times (\mathbf u^s \times \boldsymbol{\omega}^E)=(\omega^E \cdot \nabla)\mathbf u^s-(\mathbf u^s \cdot \nabla)\omega^E + \mathbf u^s(\nabla\cdot\omega^E)-\omega^E(\nabla \cdot \mathbf u^s)$$
Where:
-  $(\omega^E \cdot \nabla)\mathbf u^s$: Vorticity-Stokes shear coupling
- $-(\mathbf u^s \cdot \nabla)\omega^E$: Stokes advection of vorticity
- $\mathbf u^s(\nabla\cdot\omega^E)=0$, as $\nabla\cdot\omega^E=0$
- $-\omega^E(\nabla \cdot \mathbf u^s)\approx0$, as $\nabla\cdot u^s \approx 0$

# Experiment: Neglecting the Stokes shear term
## Assumption
If the Stokes shear term is neglected, the model assumes:
- Stokes drift acts solely as an **additional advecting velocity**
- wave effects enter momentum evolution only through:
    - Lagrangian advection
    - Lagrangian Coriolis force
but:
- the Stokes shear term is explicitly neglected
- Therefore, gradient-mediated wave-mean interactions are not represented
- the formulation departs from energetically consistent GLM dynamics

## What is retained/lost
==Retained: **transport role of Stokes drift**==
-  advection of velocity and vorticity by Stokes drift
-  kinematic redistribution of relative vorticity
-  Coriolis modification via Lagrangian velocity
-  Mass transport effects associated with Stokes drift (via continuity)
==Lost: **shear-interaction role**==
-  wave-to-Eulerian kinetic energy transfer (which is via Stokes shear force)
-  Wave-induced deformation of the flow via Stokes drift gradients
	- strain, tilting and shear production
-  Vortex-force-related contributions tied to Stokes shear
	- part of wave-driven vorticity restructuring
-  Full GLM energetic and dynamical consistency

> [!Attention] In short, waves can redistribute momentum, but cannot energise the Eulerian flow
### Consequences for vorticity dynamics
The resulting system remains correct for:
- kinematic transport of vorticity
but becomes incomplete for:
- Shear-driven vorticity generation and deformation
- Wave-modified vortex stretching/tilting linked to Stokes gradients
- Energetically consistent coupling between waves and rotational flow

## Summary
### Experiment Description
A kinematic wave-modified flow model where Stokes drift enters ONLY through the material advection velocity $u^L=u^E+u^s$, while all gradient-mediated wave-mean coupling is neglected. This forms a controlled closure choice.
### Mechanism Isolation
Retained pathway
- Lagrangian transport pathway: how waves move fluid parcels
	- redistribution of tracers, vorticity, mass
Removed pathway
- dynamical coupling pathway: how waves reshape the flow
	- Wave-to-Eulerian energy transfer
	- wave-mean shear interaction involving gradients of Stokes drift
	- Shear-mediated flow deformation
### Scientific Question
“How much of the large-scale climate response can be explained purely by wave-modified transport, without wave–mean dynamical coupling?”

> [!Attention] Paragraph:
> - This framework isolates the role of wave-induced Lagrangian transport from wave–mean shear interactions in controlling large-scale ocean variability. 
> - It does not represent wave-driven momentum transfer or energetically consistent wave-mean flow interaction. 
> - The aim is to isolate the pure kinematic role of wave-induced transport and assess whether it alone can modify large-scale and seasonal ocean circulation patterns relative to a baseline state with no wave effects. 
> - This design allows a clean mechanistic separation between a non-wave reference state and a wave-transport-only state, providing a controlled test of whether Lagrangian transport by waves is sufficient to produce climatically relevant modifications in circulation and stratification patterns.


# Reconstruct the Stokes drift profile from ERA5

## [[2026-05-20]] Assess Stokes profile reconstruction method
We reconstruct the Stokes drift profile using the Phillips-spectrum approximation of Breivik et al. (2016), which requires the surface Stokes drift and the Stokes transport. ERA5 provides the surface Stokes drift vector directly, while the Stokes transport is estimated from bulk wave parameters using the first-moment approximation. Because surface Stokes drift is more strongly weighted toward short wind waves whereas Stokes transport is more sensitive to longer waves and swell, we further test a two-component reconstruction separating wind sea and total swell.

For grid points where the ERA5 surface Stokes vector lies inside the positive cone spanned by the wind-sea and total-swell mean propagation directions, we solve a non-negative two-component vector decomposition and reconstruct separate wind-sea and swell Stokes profiles. Where the two directions are nearly collinear, or where the non-negative decomposition is not admissible, we revert to a bulk total-sea reconstruction and flag these points as low-directional-information cases.

This procedure should be interpreted as a directionally constrained bulk approximation, not as a full spectral partition of Stokes drift. Its main purpose is to reduce the physically unrealistic assumption that the full Stokes transport always follows the surface Stokes drift direction. We therefore evaluate the sensitivity of ICON forcing to the decomposed and fallback reconstructions separately.