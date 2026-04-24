---
tags:
  - stokes_drift
  - "#project/surfwaves"
  - wave/surface_wave
  - presenter/Nobushiro_Suzuki
Last Eddited: 2026-04-24
---
# Craik–Leibovich (CL) vortex force and vorticity transport
## Vortex force form and identity
The vortex force can be rewritten using vector identities:
$$\mathbf u^s \times \boldsymbol{\omega}^E = -(\nabla \times \mathbf u^E)\times \mathbf u^s $$
and decomposed as (see Appendix in ~={blue}(Suzuki & Fox-Kemper, 2016))=~:
$$-(\nabla \times \mathbf u^E)\times \mathbf u_s = u^s_j \nabla u^E_j - (\mathbf u^s \cdot \nabla)\mathbf u^E$$
where:
- $u^s_j \nabla u^E_j$: shear-like term
- $(\mathbf u^s \cdot \nabla)\mathbf u^E$: Stokes advection term

After the GLM reorganisation, combining the shear-like term with the gradient of kinetic energy for both Eulerian velocity and Lagrangian velocity, one can get the **Stokes shear term**: $$u^s_j \nabla u^E_j+\frac{1}{2}(\nabla|\mathbf u^E|^2)-\frac{1}{2}(\nabla|\mathbf u^L|^2)=-u^L_j \nabla u^s_j$$
## Vortex force affects relative vorticity
Take the curl of the momentum equation:$$\boldsymbol{\omega}^E = \nabla \times \mathbf u^E$$
The vortex force contributes to vorticity evolution through the: $$\nabla \times (\mathbf u^s \times \boldsymbol{\omega}^E)$$
Using identities, the leading-order contribution is: $$(\mathbf u^s \cdot \nabla)\boldsymbol{\omega}^E$$
## Interpretation: how vorticity is transported
### Pure advection by Stokes drift
$$(\mathbf u^s \cdot \nabla)\boldsymbol{\omega}^E$$

This term represents:
- transport of Eulerian vorticity by wave-induced drift
- **no creation or destruction of vorticity**
- purely kinematic redistribution

It can serves to:
- advects vorticity
- does not change total vorticity content
- behaves like an additional velocity field

> [!Important] This is the **Stokes advection of vorticity**
### Wave-induced redistribution (Stokes shear effect)
After full decomposition and pressure/Bernoulli corrections, additional Stokes shear terms remain: $$- u^L_j \nabla u^s_j$$
These contribute to:
- deformation of the mean flow by wave shear
- indirect modification of vorticity distribution
- coupling between wave structure and mean flow gradients

It serves to:
- modifies spatial distribution of vorticity
- depends on gradients of Stokes drift
- responsible for wave–mean interaction beyond kinematics

> [!Important] This part represents **wave-modified vorticity redistribution beyond simple advection**

# Neglecting the Stokes shear term
## Assumption
If the Stokes shear term is neglected, the model assumes:
- Stokes drift is fully represented as an **additional advecting velocity**
- wave effects act only through:
  - Lagrangian advection: $(\mathbf u^L \cdot \nabla)\mathbf u^E$
  - Lagrangian Coriolis force
but:
- **no direct wave–mean energy exchange via shear**
- no explicit coupling through Stokes drift gradients
## What is retained
The system still includes:
- advection of velocity and vorticity by $\mathbf u^s$
- Coriolis modification via Lagrangian velocity
- basic transport of relative vorticity
## What is lost
Neglecting Stokes shear removes:
- wave–mean energy transfer mechanism
- interaction with spatial gradients of Stokes drift
- part of the vortex-force-induced vorticity redistribution
- consistency with full GLM energy budget

## Consequences for vorticity dynamics
The resulting model:
- remains correct for:
	- kinematic advection of vorticity
	- large-scale transport patterns
- becomes incomplete for:
	- wave-driven vorticity restructuring
	- energetically consistent wave–mean coupling
	- small-scale shear effects induced by waves

## Revised scientific interpretation
Under this approximation, the system effectively becomes:
> [!Attention] 
> ***A wave-modified flow where the Stokes drift acts only as an additional advecting velocity in the Lagrangian framework, while the energetic pathway through which wave shear transfers momentum and energy to the Eulerian mean flow is intentionally excluded. ***
> 
> Thus:
> - preserves transport physics
> - removes part that transfer wave energy
> Which maintains minimal physical consistency and scientifically interesting
