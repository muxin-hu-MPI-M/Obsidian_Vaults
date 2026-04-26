---
tags:
  - stokes_drift
  - "#project/surfwaves"
  - wave/surface_wave
  - presenter/Nobushiro_Suzuki
Last Eddited: 2026-04-24
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
the leading-order contribution are: $$\nabla \times (\mathbf u^s \times \boldsymbol{\omega}^E)\approx(\omega^E \cdot \nabla)\mathbf u^s-(\mathbf u^s \cdot \nabla)\omega^E$$
which are:
- $-(\mathbf u^s \cdot \nabla)\omega^E$: Stokes advection of vorticity
- $(\omega^E \cdot \nabla)\mathbf u^s$: Vorticity-Stokes shear coupling

If we taking the decomposition of CL vortex: $-(\nabla \times \mathbf u^E)\times \mathbf u_s = u^s_j \nabla u^E_j - (\mathbf u^s \cdot \nabla)\mathbf u^E=\mathbf F$, we now compute $\nabla \times \mathbf F$: $$\nabla \times \mathbf F=-[(\mathbf u^s \cdot \nabla)\omega^E-(\omega^E\cdot \nabla)\mathbf u^s]+\epsilon_{ikl}(\partial_ku_j^s)(\partial_lu_j^E)$$
The two components have contributions of:
- Stokes advection term ($- (\mathbf u^s \cdot \nabla)\mathbf u^E$):
	- Pure vorticity advection explicitly $-(\mathbf u^s \cdot \nabla)\omega^E$
	- Vorticity-shear coupling $(\omega^E\cdot \nabla)\mathbf u^s$
- Eulerian-gradient term ($u^s_j \nabla u^E_j$):
	- non-linear interaction term
### Interpretation
The decomposition of CL vortex into: (1) Stokes advection term and (2) gradient-Stokes term contributes differently to the evolution of relative vorticity $\omega^E$.
- The Stokes advection term:
	- Advect vorticity through pure Stokes-advection
	- local change of vorticity through coupling between vorticity and shear of Stokes
- The Eulerian-gradient:
	- Affect evolution of vorticity through non-linear interactions 

# Experiment: Neglecting the Stokes shear term
## Assumption
If the Stokes shear term is neglected, the model assumes:
- Stokes drift acts solely as an **additional advecting velocity**
- wave effects enter momentum evolution only through:
    - Lagrangian advection
    - Lagrangian Coriolis force
but:
- wave–mean energy exchange through shear is excluded 
- wave–induced deformation of the mean flow by Stokes drift gradients is not represented in a dynamically consistent way

## What is retained/lost
Retained:
-  advection of velocity and vorticity by Stokes drift
-  redistribution of relative vorticity (kinematic transport)
-  Coriolis modification via Lagrangian velocity
Lost
-  wave–mean energy transfer mechanism (non-divergent work)
-  Stokes drift gradient–induced deformation of mean flow
-  part of wave-modified vorticity restructuring associated with shear interactions
-  full GLM energetic consistency

## Consequences for vorticity dynamics
The resulting system:
✔ remains correct for:

- kinematic transport of vorticity
- advection of enstrophy by Stokes drift

⚠ becomes incomplete for:

- wave-modified vorticity deformation mechanisms
- energetically consistent wave–mean coupling
- shear-driven modification of flow structures


## Revised interpretation

> The model represents a wave-modified flow in which Stokes drift acts only as an advecting velocity within the Lagrangian framework, while the shear-mediated pathway through which wave gradients perform work on the Eulerian mean flow is intentionally removed. This preserves kinematic transport but removes the energetic wave–mean coupling mechanism.