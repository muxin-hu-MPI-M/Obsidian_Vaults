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
# Setup for ICON
## Finalising WAB momentum equation
### Vectors form
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

Thus, to efficiently use the operators of the ICON, and avoid too much modification, the final WAB momentum equation will be introduced as replacing all Eulerian velocity to the Lagrangian velocity $\mathbf{u}^L=\mathbf{u}+ \mathbf{u}^s$:
$$
\boxed{\partial_t \mathbf{u}^L+ \left(\nabla \times \mathbf{u}^L+\mathbf{f}\right)\times \mathbf{u}^L = \mathbf{b}+\mathbf{D}^{u} -(\nabla p + \frac{1}{2}|\mathbf{u}^L|^2) + (\nabla \times \mathbf{u}^s)\times \mathbf{u}^L + \partial_t \mathbf{u}^s} \tag{3}
$$
This replacement will lead to two additional terms at the right-hand-side of Eq.(3), they are:
- wave-influenced Stokes vortex force: $(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L$
- prognostic Stokes velocity: $\partial_t \mathbf{u}^s$

> [!Tips] 
Comparing Eq.(3) to Eq.(1), one can find:
> $$(\nabla \times \mathbf{u}^L)\times \mathbf{u}^L + \nabla(p + \frac{1}{2}|\mathbf{u}^L|^2)-(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L = (\mathbf{u}^L \cdot \nabla)\mathbf{u}^L + \nabla p + \mathbf{u}^L \times (\nabla \times \mathbf{u}^s)$$
> Because:
> - Distributivity of cross product: $(\nabla \times \mathbf{u}^s)\times \mathbf{u}^L = - \mathbf{u}^L \times (\nabla \times \mathbf{u}^s)$
> - Convective acceleration identity: $(\mathbf{u}\cdot\nabla)\mathbf{u} = (\nabla \times \mathbf{u}) \times \mathbf{u} + \nabla(\frac{1}{2}|\mathbf{u}|^2)$
> 
> See detailed conversion rules in [[Mathematical_Understanding#Conversions in WAB momentum framework]]

### 3D expansions
The Full **3D version** without the consideration of wavy-hydrostatic approximation are summarised below:
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



### 2D horizontal + vertical (ICON-form)
- horizontal vector operator e.g.,$\nabla_h$ 
- vertical velocity is diagnostic for both Eulerian and Stokes, diagnose from the horizontal velocities (continuity, already in the source code)

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