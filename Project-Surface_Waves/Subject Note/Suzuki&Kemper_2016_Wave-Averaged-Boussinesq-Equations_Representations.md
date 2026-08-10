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
> \boxed{\partial_t \mathbf{u}^L+ \left(\nabla \times \mathbf{u}^L+\mathbf{f}\right)\times \mathbf{u}^L = \mathbf{b}+\mathbf{D}^{u} -(\nabla p + \frac{1}{2}|\mathbf{u}^L|^2) + \underbrace{(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L}_{\text{Stokes vortex force}} + \partial_t \mathbf{u}^s} \tag{3}
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
![[Screenshot 2026-07-20 at 11.17.09.png | center]]

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
**This section only discuss the scales of full Stokes vortex force:**
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

Then let’s compare the $\omega^s \vec{z} \times \mathbf{v}^L$ to the $w^L \partial_z \mathbf{v}^s$:
$$\frac{\omega^s \vec{z} \times \mathbf{v}^L} {w^L\partial_z\mathbf{v}^s} \sim \frac{UU_s/L_s}{WU_s/H_s} \sim \frac{UH_s}{\epsilon U L_s}  =  \frac{\epsilon_s}{\epsilon} = \frac{H_s/L_s}{H/L}$$
depends on the relative difference between 2 ratios.

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
> Thus, $w^L \partial_z \mathbf{v}^s$ should be considered, while the $(- w^L \partial_x w^s, - w^L \partial_y w^s)$ term is $\epsilon_s^2$ times smaller than the $w^L \partial_z \mathbf{v}^s$, which can be neglect when $\epsilon_s=H_s / L_s$ is small; the $\omega^s \vec{z} \times \mathbf{v}^L$ can also be neglected if $\epsilon_s \ll \epsilon$, where $\epsilon=H/L$.
> 
> Therefore, one can find two ways to represent the wave-influenced Stokes vortex force in the horizontal momentum equation.
> - **Origin**:
> $$ \boxed{[(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L]_{\text{h}} = w^L(\partial_z \mathbf{v}^s - \nabla_h w^s) + \omega^s \vec{z} \times \mathbf{v}^L}$$
> - **Neglect** $- w^L \partial_x w^s$ **and** $- w^L \partial_y w^s$ if $\epsilon_s \ll 1$:
> $$ \boxed{[(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L]_{\text{h}} \approx w^L\partial_z \mathbf{v}^s  + \omega^s \vec{z} \times \mathbf{v}^L} $$
> The difference is only the product of vertical Lagrangian velocity and horizontal gradient of vertical Stokes drift velocity $w^L \nabla_h w^s$.
> - **Neglect** $\omega^s \vec{z} \times \mathbf{v}^L$ if $\epsilon_s \ll \epsilon$:
> $$ \boxed{[(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L]_{\text{h}} \approx w^L\partial_z \mathbf{v}^s} $$
> 


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
the dynamically relevant hydrostatic balance is (see why using dynamically relevant part in [[Suzuki&Kemper_2016_Wave-Averaged-Boussinesq-Equations_Representations#Why comparing scaling using dynamically relevant pressure?]])
$$\partial_z P'=b,\qquad b=-\frac{g\rho'}{\rho_0}.$$
In the hydrostatic primitive equations, after Boussinesq scaling, the horizontal pressure-gradient term is
$$\frac{1}{\rho_0}\nabla_h p'=\nabla_h P'.$$
For large-scale ocean circulation, the leading horizontal balance is usually geostrophic:
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
> [!Important] **Wavy-hydrostatic Approximation**
> So the vertical-gradient Stokes terms can be $O(0.1)$ or even $O(1)$ relative to the leading hydrostatic pressure-buoyancy balance. Therefore, in a wavy-hydrostatic approximation, it is reasonable to retain
> $$-u^L\partial_z u^s - v^L\partial_z v^s$$
> while neglecting
> $$u^L\partial_x w^s,\qquad v^L\partial_y w^s$$
> under the assumption $\epsilon_s\ll 1.$ The resulting wavy-hydrostatic balance is
> $$\partial_z P'=b-u^L\partial_z u^s-v^L\partial_z v^s$$
> or equivalent: $$\partial_z p=-\rho g-\rho_0(u^L\partial_z u^s+v^L\partial_z v^s)$$, in vectorised form: $$\boxed{\partial_z p=-\rho g-\rho_0 \mathbf{v}^L \cdot \partial_z \mathbf{v}^s}$$
> up to smaller vertical-inertia, diffusion, and horizontal $w^s$-gradient terms.

#### Why comparing scaling using dynamically relevant pressure?
> [!Quote] **Why using dynamically relevant hydrostatic balance?**
> Write $$A_s=u^L\partial_z u^s+v^L\partial_z v^s.$$
> The full dimensional wavy-hydrostatic equation is $$\partial_z p=-\rho g-\rho_0 A_s. $$
> Now split $$ p=p_0(z)+p',\qquad \rho=\rho_0+\rho', $$
> with the **resting background** $$ \partial_zp_0=-\rho_0g. $$
> Subtract this from the full equation: $$ \partial_zp'=-\rho'g-\rho_0A_s. $$
> Divide by $\rho_0$, using $P'=p'/\rho_0$ and $b=-g\rho'/\rho_0$: $$ \boxed{\partial_zP'=b-A_s.} $$
> So the relevant comparison is **not** $A_s\sim g$, which is very unlikely to be comparable, but rather $A_s\sim b\sim \partial_zP'$

### 🌟 Does neglecting terms in Stokes vortex force causing inconsistency?
The above sections individually discussed the scaling of Stokes vortex force in both horizontal and vertical momentum equations. However, in the end, the proposed ignorances in both cases:
- $-w^L\nabla_h w^s = (- w^L \partial_x w^s, - w^L \partial_y w^s)$ in horizontal momentum equation
- $\mathbf{v}^L \cdot \nabla_h w^s = (u^L\partial_x w^s,\ v^L\partial_y w^s)$ in vertical momentum equation
can be summed up into a general form.

> [!Attention] Write the full Stokes vortex force as $$ \mathbf{F}^s=(\nabla\times\mathbf{u}^s)\times\mathbf{u}^L=\underbrace{\begin{pmatrix}w^L\partial_z\mathbf{v}^s\\-\mathbf{v}^L\cdot\partial_z\mathbf{v}^s\end{pmatrix}}_{\mathbf{F}^s_{\mathbf{v}^s}}+\underbrace{\begin{pmatrix} \zeta^s\hat{\mathbf{z}} \times\mathbf{v}^L\\0\end{pmatrix}}_{\mathbf{F}^s_{\zeta^s}}+\underbrace{\begin{pmatrix}-w^L\nabla_h w^s\\\mathbf{v}^L\cdot\nabla_h w^s\end{pmatrix}}_{\mathbf{F}^s_w},\qquad \zeta^s=\partial_xv^s-\partial_yu^s.$$

The complete $\nabla_h w^s$-dependent force pair is
$$ \mathbf F_w^s=\begin{pmatrix}-w^L\nabla_h w^s\\\mathbf v^L\cdot\nabla_h w^s\end{pmatrix}. $$
#### Energy
The three blocks are independently perpendicular to $\mathbf u^L=(u^L, v^L, w^L)=(\mathbf{v}^L,w)$:
$$ \begin{align} \mathbf{u}^L\cdot(\mathbf{F}^s_{\mathbf{v}^s}+\mathbf{F}^s_{\zeta^s})&=\mathbf{v}^L\cdot\left(w^L\partial_z\mathbf{v}^s+\zeta^s\hat{\mathbf z}\times\mathbf v^L\right)-w^L\mathbf v^L\cdot\partial_z\mathbf v^s=0, \\ \mathbf{u}^L\cdot\mathbf{F}^s_w&=-w^L\mathbf v^L\cdot\nabla_hw^s+w^L\mathbf v^L\cdot\nabla_hw^s=0. \end{align}$$
However, dropping the vertical component of $\mathbf F_w^s$ due to wavy-hydrostatic but retain the horizontal one would leave the energy residual. The horizontal and vertical contributions of $\nabla_h w^s$-dependent force pair to the Lagrangian kinetic-energy budget are
$$ \begin{align}\mathcal P_h&=\rho_0\mathbf v^L\cdot\left(-w^L\nabla_hw^s\right)=-\rho_0w^L\mathbf v^L\cdot\nabla_hw^s, \\\mathcal P_z&=\rho_0w^L\left(\mathbf v^L\cdot\nabla_hw^s\right)=+\rho_0w^L\mathbf v^L\cdot\nabla_hw^s.\end{align}$$
Therefore,
$$ \mathcal P_h+\mathcal P_z=0. $$
If you retain the horizontal term but omit the vertical term, the uncanceled work is
$$ \mathcal P_{\mathrm{error}}=-\rho_0w^L\mathbf v^L\cdot\nabla_hw^s.$$
The sign is flow-dependent because $\mathbf v^L\cdot\nabla_hw^s$ can be positive or negative. Therefore, the hybrid approximation may act as either an artificial energy source or sink.

> [!Attention] **Conclusion** 
> - **dropping the complete $\mathbf F_w^s$ block does not introduce direct spurious work by the vortex force**. 
> - **But if drop incompletely it will leave energy residuals act act as artificial energy source/sink**

#### Potential vorticity
the paired reduced force is equivalent to replacing the Stokes velocity inside the vortex force by
$$ \widetilde{\mathbf{u}}^s=(u^s,v^s,0). $$
Then
$$ \mathbf{F}_0^s=[\nabla\times\widetilde{\mathbf{u}}^s]\times\mathbf{u}^L. $$
So the reduced model conserves the reduced PV, with the Eulerian velocity:
$$\widetilde{\mathbf u}=\mathbf u^L-\widetilde{\mathbf u}^s$$
So the ==**reduced Boussinesq WAB PV**== is:
$$ \boxed{\widetilde q_B=\left[\nabla\times(\mathbf{u}^L-\widetilde{\mathbf{u}}^s)+f\hat{\mathbf z}\right]\cdot\nabla b} $$
Under the hydrostatic primitive-equation scaling, which ignores the horizontal gradient of the vertical velocity, the reduced Boussinesq WAB PV becomes the ==**reduced Boussinesq-hydrostatic WAB PV**==:
$$ \widetilde q_H=(f+\zeta^L-\zeta^s)\partial_z b+\partial_z(u^L-u^s)\partial_y b-\partial_z(v^L-v^s)\partial_x b $$
where $\zeta$ stands for the vertical component of the relative vorticity. This can be summed more compactly to:
$$ \boxed{\widetilde q_H=(f+\zeta^L-\zeta^s)\partial_z b+\hat{\mathbf z}\cdot\left[\partial_z(\mathbf v^L-\mathbf v^s)\times\nabla_h b\right].} $$
This is the **same structure as ordinary hydrostatic Boussinesq PV** (see details in [[Terminologies_in_Meteorology_Oceanography#Hydrostatic Boussinesq Ertel PV]],
$$ q_H=(f+\zeta)\partial_zb+\partial_z u\,\partial_yb-\partial_zv\,\partial_xb, $$
but with $\mathbf v\rightarrow \mathbf v^L-\mathbf v^s$. 
The wavy-hydrostatic relation does not mean the PC should use $b-\mathbf{v}^L\cdot \partial_z \mathbf{v}^s$ as the stratifying variable. The PV still uses the real buoyancy $b$. The extra Stokes term is the vertical component of the reduced vortex force, needed to keep the hydrostatic pressure balance paired with the horizontal Stokes force. 

For conservation, in the ideal continuous limit, and buoyancy $b$ is the material invariant of the flow that transports the vorticity:
$$ \frac{D_L}{Dt}=\partial_t + (\mathbf{u}^L\cdot \nabla),\qquad D_h=D_v=0,\qquad \frac{D_L b}{Dt}=0, \qquad \nabla\cdot\mathbf{u}^L=0. $$
both reduced PV satisfy: 
$$\boxed{\frac{D_L \widetilde{q}_B}{Dt}=0,\qquad \frac{D_L \widetilde{q}_H}{Dt}=0}$$
> [!Attention] **Under ideal condition, both reduced Boussinesq WAB PV and its hydrostatic scaling version is materially conserved by the Lagrangian flow**

It does **not** exactly conserve the full-WAB PV
$$ q=\left[\nabla\times(\mathbf{u}^L-\mathbf{u}^s)+f\hat{\mathbf z}\right]\cdot\nabla b, $$
because the reduced model has removed the curl contribution from $w^s$. The difference is
$$ \widetilde q-q=\partial_yw^s\,\partial_xb-\partial_xw^s\,\partial_yb. $$

> [!Attention] **So the clean conclusion is: Paired reduction preserves zero-work structure and conserves a reduced PV, but not the exact full-WAB PV**

## 🌟 WAB in ICON structure
> [!Quote] Recall the notation from (Korn, 2017):
> - State vector $\mathbf{v}, \eta, T , S$ consisting of a horizontal velocity field $\mathbf{v}$, the surface elevation $\eta$ and the oceanic tracers temperature and salinity $C = \{T , S\}$.
> - $f$ the Coriolis parameter, $\rho$ and $\rho_0$ the sea water density and its reference value, $p$ the hydrostatic pressure, $g$ the gravitational constant, $\vec{z}$ the local vertical upward unit vector and $B$ describes the bottom topography 
> - Vertical velocity $w$; horizontal vector operator e.g.,$\nabla_h$; $D_h$ describes the horizontal velocity diffusion
> - $\mathrm{A}^{\mathrm{v}}$ the coefficient of vertical velocity diffusion, horizontal and vertical diffusion coefficients for a tracer $C$ are denoted by $\mathrm{K}^C$ and $\mathrm{A}^C$.

> [!Quote] Recall the Stokes vortex force term, which is the key terms in the WAB momentum equation:$$ \mathbf{F}^s=(\nabla\times\mathbf{u}^s)\times\mathbf{u}^L=\underbrace{\begin{pmatrix}w^L\partial_z\mathbf{v}^s\\-\mathbf{v}^L\cdot\partial_z\mathbf{v}^s\end{pmatrix}}_{\mathbf{F}^s_{\mathbf{v}^s}}+\underbrace{\begin{pmatrix} \zeta^s\hat{\mathbf{z}} \times\mathbf{v}^L\\0\end{pmatrix}}_{\mathbf{F}^s_{\zeta^s}}+\underbrace{\begin{pmatrix}-w^L\nabla_h w^s\\\mathbf{v}^L\cdot\nabla_h w^s\end{pmatrix}}_{\mathbf{F}^s_w},\qquad \zeta^s=\partial_xv^s-\partial_yu^s.$$

The below shows a hierarchy of WAB momentum equations that with different complexities.

### Hierarchy 0: Model velocity & Stokes transport tracer independently
Don’t touch the momentum part, let’s the Lagrangian velocity only transport the tracers
- a very idiot but quick idea.
- This is identical to the case of: Tracers are advected with $u^E + u^S$ from independently run ocean and wave models. 
- Here the implicit assumption is that, at the ocean top layers, the impact of wave-driven Eulerian response is negligible or at least less important than Stokes drift
### Hierarchy 1: Preserve strict hydrostatic balance
#### Has only one component left in Stokes vortex force
This is the case that we neglect both $\mathbf{F}^s_{\mathbf{v}^s}$ and $\mathbf{F}^s_w$ blocks in the Stokes vortex force term. The only remains is the vertical Stokes vorticity correction $\mathbf{F}^s_{\zeta^s}$. The motivation of doing such reduction is to preserve a strict hydrostatic balance in the vertical momentum equation (i.e., no wavy-hydrostatic).

This reduction is energy consistent, as work of the $\mathbf{F}^s_{\mathbf{v}^s}$ is exactly zero as a pair:
$$\mathbf u^L\cdot\mathbf F_{\partial_z\mathbf v^s}^s=\mathbf v^L\cdot(w^L\partial_z\mathbf v^s)+w^L(-\mathbf v^L\cdot\partial_z\mathbf v^s)=0.$$
similar to the $\mathbf{F}^s_w$. The term $\zeta^s\hat{\mathbf{z}} \times\mathbf{v}^L$ is individually perpendicular to $\mathbf{v}^L$ so it does not need a hydrostatic pressure partner for energy.

However, this reduction do not maintain a divergent-free hydrostatic vorticity. Previously we discussed, for the reduced wave velocity $\widetilde{\mathbf u} =\mathbf u^L-\widetilde{\mathbf u}^s=(u^L-u^s,\ v^L-v^s,\ w^L)$, the hydrostatic reduced vorticity should be
$$ \boldsymbol\Omega_H=(-\partial_zv^L+\partial_zv^s,\ \partial_zu^L-\partial_zu^s,\ f+\zeta^L-\zeta^s). $$
Now take divergence:
$$ \nabla\cdot\boldsymbol\Omega_H=\partial_x(-\partial_zv^L+\partial_zv^s)+\partial_y(\partial_zu^L-\partial_zu^s)+\partial_z(f+\zeta^L-\zeta^s). $$
Using $\zeta^L=\partial_xv^L-\partial_yu^L, \zeta^s=\partial_xv^s-\partial_yu^s$, all terms cancel:
$$ \nabla\cdot\boldsymbol\Omega_H=0. $$
But if you keep only the vertical Stokes vorticity correction $-\zeta^s$ and drop the horizontal Stokes-vorticity pieces $\partial_zv^s,\partial_zu^s$, then you are using
$$ \boldsymbol\Omega_\zeta^H=(-\partial_zv^L,\ \partial_zu^L,\ f+\zeta^L-\zeta^s). $$
Its divergence is
$$ \nabla\cdot\boldsymbol\Omega_\zeta^H=\partial_x(-\partial_zv^L)+\partial_y(\partial_zu^L)+\partial_z(f+\zeta^L-\zeta^s). $$
The Lagrangian parts cancel:
$$ -\partial_x\partial_zv^L+\partial_y\partial_zu^L+\partial_z(\partial_xv^L-\partial_yu^L)=0. $$
So only the unpaired Stokes term remains:
$$\nabla\cdot\boldsymbol\Omega_\zeta^H=-\partial_z\zeta^s$$
> [!Important] **So unless we have $-\partial_z\zeta^s \approx 0$, this is not a curl-consistent 3D vorticity vector, and exact Ertel-PV conservation is not guaranteed.**

> [!Important] **Consideration: Preserve hydrostatic balance, has one component left in Stokes vortex force**
> The ICON-o ocean primitive equation framework in WAB form is:
> $$
> \begin{align}
> &\frac{\partial \mathbf{v}^L}{\partial t} + (f+ \omega^L)\vec{z} \times \mathbf{v}^L + \frac{\nabla_h |\mathbf{v}^L|^2}{2}+ w^L \frac{\partial \mathbf{v}^L}{\partial z} + \frac{1}{\rho_0}\nabla_h p  - D_h \mathbf{v}^L - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{v}} \frac{\partial}{\partial z} \mathbf{v}^L - \zeta^s \vec{z} \times \mathbf{v}^L - \frac{\partial \mathbf{v}^s}{\partial t} = 0
> \\
> &\frac{\partial p}{\partial z} = -\rho g
> \\
> &\text{div}_h \mathbf{v}^L + \frac{\partial w^L}{\partial z} = 0 \tag{5}
> \\
> &\frac{\partial \eta}{\partial t} + \text{div}_h \int_{-B}^{\eta}\mathbf{v}^L\;dz = 0
> \\
> & \frac{\partial C}{\partial t} + \text{div}(C\mathbf{v}^L)-\text{div}_h(\mathrm{K}^{\mathrm{C}} \nabla C) - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{C}} \frac{\partial}{\partial z} C = 0
> \\
> &\rho = F_{eos}(p, T, S),
> \end{align}
> $$
> 
> **noted**: 
> - here the diffusion term is using the Lagrangian velocity, but in C-L WAB format, it should use the Eulerian velocity instead
> - Similar issue maybe in the other coefficients in the tracer equation. 
> - In practice, we will keep the current setting, but keep in mind that there’s a difference

#### No Stokes vortex force
This is the cleanest “no vortex-force” approximation. Energy-wise, this avoids the artificial unpaired work from keeping only part of the vortex force. PV-wise, the conserved ideal PV is then the ordinary hydrostatic PV built from $\mathbf u^L$.

But be careful with interpretation. This no-vortex-force model is **not** the full WAB dynamics. It treats the Lagrangian velocity as the model velocity and omits the Stokes-vorticity correction to the quasi-Eulerian momentum. So it conserves a clean model PV, but not the full wave-averaged PV. if drop the vortex force entirely, your equation no longer represents $\mathbf u^L=\mathbf u+\mathbf u^s$ inside the CL/WAB momentum structure. Instead it behaves like $\mathbf u^L$ is just the ordinary Eulerian model velocity.

> [!Important] **Consideration: No Stokes vortex force**
> The ICON-o ocean primitive equation framework in WAB form is:
> $$
> \begin{align}
> &\frac{\partial \mathbf{v}^L}{\partial t} + (f+ \omega^L)\vec{z} \times \mathbf{v}^L + \frac{\nabla_h |\mathbf{v}^L|^2}{2}+ w^L \frac{\partial \mathbf{v}^L}{\partial z} + \frac{1}{\rho_0}\nabla_h p  - D_h \mathbf{v}^L - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{v}} \frac{\partial}{\partial z} \mathbf{v}^L - \frac{\partial \mathbf{v}^s}{\partial t} = 0
> \\
> &\frac{\partial p}{\partial z} = -\rho g
> \\
> &\text{div}_h \mathbf{v}^L + \frac{\partial w^L}{\partial z} = 0 \tag{5}
> \\
> &\frac{\partial \eta}{\partial t} + \text{div}_h \int_{-B}^{\eta}\mathbf{v}^L\;dz = 0
> \\
> & \frac{\partial C}{\partial t} + \text{div}(C\mathbf{v}^L)-\text{div}_h(\mathrm{K}^{\mathrm{C}} \nabla C) - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{C}} \frac{\partial}{\partial z} C = 0
> \\
> &\rho = F_{eos}(p, T, S),
> \end{align}
> $$
> 
> **noted**: 
> - here the diffusion term is using the Lagrangian velocity, but in C-L WAB format, it should use the Eulerian velocity instead
> - Similar issue maybe in the other coefficients in the tracer equation. 
> - In practice, we will keep the current setting, but keep in mind that there’s a difference

### Hierarchy 2: Negligible vert. Stokes (Partial Stokes vortex force)
This is by far the ~={red}best and most practical version=~, which consider the significant/important part of the Stokes vortex force while neglect the negligible vertical stokes to reduce complexity. 

> [!Attention] **Eventually, it is the version that neglect the $\mathbf{F}^s_w$ block in the Stokes vortex force term, but still keep the energy/PV consistency.**

> [!Attention] **Consideration: negligible vertical Stokes drift velocity**
> The ICON-o ocean primitive equation framework in WAB form is:
> $$
> \begin{align}
> &\frac{\partial \mathbf{v}^L}{\partial t} + (f+ \omega^L)\vec{z} \times \mathbf{v}^L + \frac{\nabla_h |\mathbf{v}^L|^2}{2}+ w^L \frac{\partial \mathbf{v}^L}{\partial z} + \frac{1}{\rho_0}\nabla_h p  - D_h \mathbf{v}^L - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{v}} \frac{\partial}{\partial z} \mathbf{v}^L - (w^L \frac{\partial \mathbf{v}^s}{\partial z} + \zeta^s \vec{z} \times \mathbf{v}^L) - \frac{\partial \mathbf{v}^s}{\partial t} = 0
> \\
> &\frac{\partial p}{\partial z} = -\rho g - \rho_0 \mathbf{v}^L \cdot \partial_z \mathbf{v}^s
> \\
> &\text{div}_h \mathbf{v}^L + \frac{\partial w^L}{\partial z} = 0 \tag{5}
> \\
> &\frac{\partial \eta}{\partial t} + \text{div}_h \int_{-B}^{\eta}\mathbf{v}^L\;dz = 0
> \\
> & \frac{\partial C}{\partial t} + \text{div}(C\mathbf{v}^L)-\text{div}_h(\mathrm{K}^{\mathrm{C}} \nabla C) - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{C}} \frac{\partial}{\partial z} C = 0
> \\
> &\rho = F_{eos}(p, T, S),
> \end{align}
> $$
> 
> **noted**: 
> - here the diffusion term is using the Lagrangian velocity, but in C-L WAB format, it should use the Eulerian velocity instead
> - Similar issue maybe in the other coefficients in the tracer equation. 
> - In practice, we will keep the current setting, but keep in mind that there’s a difference
### Hierarchy 3: Non-hydrostatic case
However, in this case, we haven’t conclude on the vertical scaling. As the below version keep the horizontal gradient of vertical Stokes term, one should compare the scaling to the ordinary vertical acceleration term. The resulting ratio depends heavily on the horizontal scale of wave fields:
$$ \boxed{\frac{u^L \partial_x w^s}{u^L \partial_x w^L}\sim\frac{U\epsilon_sU_s/L_s}{\epsilon^2U^2/H}=\frac{\epsilon_s}{\epsilon}\frac{U_s}{U}\frac{L}{L_s}.} $$
The horizontal gradient of vertical Stokes can be similar or smaller scales than the ordinary vertical terms, so when preserving the $v^L \partial_y w^s - u^L \partial_x w^s$ in the vertical acceleration equation due to the consideration of full Stokes vortex force term, we might also need to preserve the ordinary vertical acceleration term, **which is basically non-hydrostatic cases!**

>[!Important] **Consideration: Non-hydrostatic**
> The ICON-o ocean primitive equation framework in WAB form is:
> $$
> \begin{align}
> &\frac{\partial \mathbf{v}^L}{\partial t} + (f+ \omega^L)\vec{z} \times \mathbf{v}^L + \frac{\nabla_h |\mathbf{v}^L|^2}{2}+ w^L \frac{\partial \mathbf{v}^L}{\partial z} + \frac{1}{\rho_0}\nabla_h p  - D_h \mathbf{v}^L - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{v}} \frac{\partial}{\partial z} \mathbf{v}^L - (w^L (\frac{\partial \mathbf{v}^s}{\partial z} - \nabla_h w^s)+ \zeta^s \vec{z} \times \mathbf{v}^L) - \frac{\partial \mathbf{v}^s}{\partial t} = 0
> \\
> &\partial_t w^L + v^L\partial_y w^L + u^L \partial_x w^L + \partial_z p = -\rho g + D^w - w^L \partial_z w^L  + (v^L\partial_y w^s - v^L \partial_z v^s - u^L \partial_z u^s + u^L \partial_x w^s) + \partial_t w^s
> \\
> &\text{div}_h \mathbf{v}^L + \frac{\partial w^L}{\partial z} = 0
> \\
> &\text{div}_h \mathbf{v}^s + \frac{\partial w^s}{\partial z} = 0 \tag{5}
> \\
> &\frac{\partial \eta}{\partial t} + \text{div}_h \int_{-B}^{\eta}\mathbf{v}^L\;dz = 0
> \\
> & \frac{\partial C}{\partial t} + \text{div}(C\mathbf{v}^L)-\text{div}_h(\mathrm{K}^{\mathrm{C}} \nabla C) - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{C}} \frac{\partial}{\partial z} C = 0
> \\
> &\rho = F_{eos}(p, T, S),
> \end{align}
> $$
> 
> **noted**: 
> - here the diffusion term is using the Lagrangian velocity, but in C-L WAB format, it should use the Eulerian velocity instead
> - Similar issue maybe in the other coefficients in the tracer equation. 
> - In practice, we will keep the current setting, but keep in mind that there’s a difference

If decompose the pressure as a sum of surface pressure and internal hydrostatic pressure: $$p(x,y,z,t)=p_{hy}(x,y,z,t)+ p_{s}(x,y,t) $$where the hydrostatic pressure is given by the weight of a water column above a vertical level $z$: $$p_{hy}(x,y,z,t) = g\int_{z}^{0}\rho(x,y,z',t)\;dz' $$
The surface pressure $p_s$ depends only on the horizontal coordinates, assuming a well mixed surface with a uniform density given by the reference density, surface pressure is modelled in terms of the surface elevation $\eta$: $p_s(x, y, t) = g\rho_0\eta(x, y, t)$. 

> [!Attention] 
> Inserting the pressure decomposition into Eq.(5) yields for the velocity equation: $$\frac{\partial \mathbf{v}^L}{\partial t} + (f+ \omega^L)\vec{z} \times \mathbf{v}^L + \frac{\nabla_h |\mathbf{v}^L|^2}{2}+ w^L \frac{\partial \mathbf{v}^L}{\partial z} + \frac{1}{\rho_0}\nabla_h p_{hy} + g\nabla_h \eta - D_h \mathbf{v}^L - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{v}} \frac{\partial}{\partial z} \mathbf{v}^L - (w^L (\frac{\partial \mathbf{v}^s}{\partial z} - \nabla_h w^s) + \omega^s \vec{z} \times \mathbf{v}^L) - \frac{\partial \mathbf{v}^s}{\partial t} = 0 $$





## Numerical Discretisation of the WAB
### Original discretisation of ICON-o
Starting from the discretisation form of the original ICON-o primitive equations in weighted weak-form on admissible reconstructions (Korn., 2017):
![[Screenshot 2026-08-06 at 17.05.38.png]]
**Operator Summary**
- $P$: reconstructs horizontal velocity from edge-normal scalars to cell-center vectors.
- $P^T$: maps cell-center vectors back to edge-normal scalars.
- $M = P^T P$: maps edge-normal scalars to edge-normal scalars through a cell-center vector reconstruction; $M[v,C]=P^T(CPv)$
- $\hat{P}$: maps from edges to vertex-located vectors
- $\hat{P}^T$: maps from vertex-vectors into edge-located fluxes
- $\hat{M} = \hat{P}^T \hat{P}$: maps edge-normal scalars to edge-normal scalars through a vertex-located vector reconstruction.
- $Q$: reconstructs quantities from vertical interfaces to layer centers.
- $Q^T$: maps quantities from layer centers back to vertical interfaces.
- $D_z$: takes the vertical derivative of a layer-centered quantity and places it on vertical interfaces.

ICON grid is a triangular prism:
![[figure-17-21.png|center||190]]

Locations of each variable:

| Variable                                 | horizontal location | vertical location     | Additional info              |
| ---------------------------------------- | ------------------- | --------------------- | ---------------------------- |
| $\mathbf{v}, \mathbf{v}^L, \mathbf{v}^s$ | edge                | mid layer             | edge-normal velocity, scalar |
| $w, w^L, w^s$                            | cell center         | top layer (interface) | scalar                       |
| $\mathbf{D}_z \mathbf{v}$                | edge                | top layer (interface) |                              |
| $f, \omega$                              | vortices            | mid layer             |                              |


### Discretisation of our reduced WAB in ICON-o structure
Here we take the hierarchy 2 as the reference primitive equation set. 
> [!Quote] **Momentum equations in Hierarchy 2**
> Our core modifications are in the momentum equations:$$\boxed{\begin{align} &\frac{\partial \mathbf{v}^L}{\partial t} + (f+ \omega^L)\vec{z} \times \mathbf{v}^L + \frac{\nabla_h |\mathbf{v}^L|^2}{2}+ w^L \frac{\partial \mathbf{v}^L}{\partial z} + \frac{1}{\rho_0}\nabla_h p  - D_h \mathbf{v}^L - \frac{\partial}{\partial z} \mathrm{A}^{\mathrm{v}} \frac{\partial}{\partial z} \mathbf{v}^L - (w^L \frac{\partial \mathbf{v}^s}{\partial z} + \zeta^s \vec{z} \times \mathbf{v}^L) - \frac{\partial \mathbf{v}^s}{\partial t} = 0 \\
&\frac{\partial p}{\partial z} = -\rho g - \rho_0 \mathbf{v}^L \cdot \partial_z \mathbf{v}^s \end{align}}$$

#### Terms related to vertical Stokes gradient
In [[Suzuki&Kemper_2016_Wave-Averaged-Boussinesq-Equations_Representations#🌟 Does neglecting terms in Stokes vortex force causing inconsistency?]], we already concluded that the Stokes vortex force, vertical-Stokes-gradient block:
$$\mathbf{F}_{\mathbf{v}^s}^s=\begin{pmatrix}w^L\partial_z\mathbf{v}^s\\-\mathbf{v}^L\cdot\partial_z\mathbf{v}^s\end{pmatrix}$$
is energy conserved when we preserve or neglect both horizontal and vertical components. **We now need to discretise the block and also prove that their discretisation also conserve the energy.**

> [!Attention] **Discretisation of $\mathbf{F}_{\mathbf{v}^s}^s$:**
> For **Horizontal component**, it is discretised by first reconstructing $v^s$ to cell centers, taking its vertical derivative (from mid layer to top layer), multiplying by $w^L$, moving the result to layer centers with $Q$, then mapping it back to edges with $P^T$. $$w^L\partial_z \mathbf{v}^s \quad\longrightarrow\quad \boxed{ P^T Q\left(w^L D_z P\mathbf{v}^s\right) }$$
> For **Vertical component**, it is discretised by reconstructing $v^L$ to cell centers, mapping it to vertical interfaces with $Q^T$, dotting it with the vertical derivative of reconstructed $v^s$, and adding the minus sign. $$-\mathbf{v}^L\cdot \partial_z \mathbf{v}^s \quad\longrightarrow\quad \boxed{ -\left(Q^T P\mathbf{v}^L\right)\cdot \left(D_z P\mathbf{v}^s\right) }$$
> 

This pairing is energy-consistent because $P^T$ is the adjoint of $P$, and $Q^T$ is the adjoint of $Q$. The detailed proof is summarised below:

> [!Important] **Discretisation of $\mathbf{F}_{\mathbf{v}^s}^s$ conserves energy**
> 1. The energy is the dot product of horizontal component with horizontal velocity, written in the below bracket ($<\;,\;>$): $$<P^T Q(w^L D_z P\mathbf{v}^s), \mathbf{v}^L>_{\text{edge}}^{\text{middle}}$$ the whole product should locate at the middle layer of the cell prism and in the edges.
> 2. Use the definition of $P$, times it to the above, it becomes: $$<Q(w^L D_z P\mathbf{v}^s), P\mathbf{v}^L>_{\text{center}}^{\text{middle}}$$ the whole product is located at the middle layer but at the center of the horizontal face.
> 3. Use the definition of $Q^T$ , times it to the above, it becomes: $$<w^L D_z P\mathbf{v}^s, Q^TP\mathbf{v}^L>_{\text{center}}^{\text{top}}$$ the whole product is now located at the top layer (i.e., interface) and still at the center of the horizontal face.
> 4. Because the vertical velocity $w^L$ is a scalar right at the top layer and in the cell center, we can treat it as a *scalar* quantity. Then apply the vector identity $(wa)\cdot b = w(a \cdot b)$, the above becomes: $$<w^L , (D_z P\mathbf{v}^s)\cdot (Q^TP\mathbf{v}^L)>_{\text{center}}^{\text{top}}$$
> 5. Now the energy for the vertical discretisation: $$<-(Q^T P\mathbf{v}^L)\cdot (D_z P\mathbf{v}^s), w^L>_{\text{center}}^{\text{top}}$$, it also locates at the top layer and cell center.
> 6. Obviously, the above format plus the energy of vertical discretisation equals to zero: $$<w^L , (D_z P\mathbf{v}^s)\cdot (Q^TP\mathbf{v}^L)>_{\text{center}}^{\text{top}} + <-(Q^T P\mathbf{v}^L)\cdot (D_z P\mathbf{v}^s), w^L>_{\text{center}}^{\text{top}} = 0 $$

#### Stokes vorticity term
The term here:
$$-\zeta^s \vec{z} \times \mathbf{v}^L$$
is the **compensation of replacing the Eulerian velocity in original Nonlinear Coriolis term to the Lagrangian velocity**:
$$(f+\omega)\times \mathbf{v} \quad \rightarrow (f+\omega^L)\times \mathbf{v}^L$$
Therefore, in practice, we can:
- either calculate the additional Stokes vorticity term
- or find a way to use the original Nonlinear Coriolis term

If calculate the additional Stokes vorticity term, the discretised form should be:
$$P^T[\textbf{curl}_z\mathbf{v}^sP\mathbf{v}^L]$$
Which will make sure the final outcome is still the edge-normal scalars at mid layer. 
However, in practice, the subroutine to calculate the original Nonlinear Coriolis term is separated and is calculated without the $\textbf{curl}_z$, as we only have operator $\textbf{curl}$. The subroutine can be found in `mo_scalar_product` in the name of `nonlinear_coriolis_3d`

## Implementation in ICON-o source code
### Standalone Stokes Forcing Module Plan
**Summary**
- First build only mo_ocean_stokes_forcing.
- Do not extend t_hydro_ocean_diag, do not change AB time stepping, and do not add persistent diagnostic fields yet.
- All routines take explicit input arrays and write to caller-provided output arrays, so the module can be unit-tested in isolation.

**Key Changes**
- Add `src/ocean/dynamics/mo_ocean_stokes_forcing.f90`.
- Public routines:
    - `stokes_local_to_cartesian_cells`: convert cell-centered local u_s/v_s into t_cartesian_coordinates.
    - `stokes_cell_to_edge_normal`: map Cartesian cell Stokes vectors to edge-normal vn_s using `map_cell2edges_3d`.
    - `stokes_vertical_shear_tendency`: compute w^L d(v^s)/dz, following the `veloc_adv_vert_mimetic_rot` pattern.
	    - The shear tendency at the first mid layer uses shear at surface and shear at first interface. The shear at the surface use `(d v_s / dz)_surface ~= (v_s(surface) - v_s(level 1)) / distance(surface, level 1)`
    - `stokes_vorticity_tendency`: compute $\zeta_s \times \mathbf{v}^L$ by deriving $\zeta_s$ from ***vn_s*** (edge-normal Stokes velocity) and applying the nonlinear-Coriolis-style vertex-to-edge stencil without planetary f.
    - `stokes_time_tendency`: compute (vn_s_now - vn_s_old) / dtime, with first-step behavior controlled by the caller.
    - `wavy_hydrostatic_source`: compute cell-centered scalar v^L · ∂z v_s; pressure integration can wait for the next phase.
	    - it also incorporates the surface Stokes drift velocity
- Keep signs explicit in routine names/comments: ==**each routine returns the raw mathematical product**== first, and the caller/test decides whether it enters momentum as -term or +term.

**The intended full integration**
1. Read or reconstruct ERA5-like cell-centered u_s/v_s.
2. Convert to Cartesian cell vectors with stokes_local_to_cartesian_cells.
3. Map to edge-normal vn_s with stokes_cell_to_edge_normal.
4. Form Lagrangian edge velocity: `vn_L = vn_E + vn_s`
5. Feed vn_L through the existing ICON momentum/tracer/continuity operators wherever the primitive equations should use Lagrangian velocity.
6. Add compensation terms from mo_ocean_stokes_forcing:
    - stokes_vertical_shear_tendency: w^L d(v^s)/dz
    - stokes_vorticity_tendency: zeta_s zhat x v^L
    - stokes_time_tendency: d(v^s)/dt
    - wavy_hydrostatic_source: v^L dot d(v^s)/dz, later used in pressure integration

The main missing integration pieces are storage/lifetime choices: where vn_s_old lives for d(v_s)/dt, where temporary vn_L/w_L are built, and where the wavy-hydrostatic source enters pressure-gradient calculation.

---
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