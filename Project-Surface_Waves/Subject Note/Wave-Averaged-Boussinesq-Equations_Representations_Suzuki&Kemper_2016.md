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
- wave–mean energy exchange through shear is excluded 
- wave–induced deformation of the mean flow by Stokes drift gradients is not represented in a dynamically consistent way

## What is retained/lost
==Retained: **transport role of Stokes drift**==
-  advection of velocity and vorticity by Stokes drift
-  redistribution of relative vorticity (kinematic transport)
-  Coriolis modification via Lagrangian velocity
==Lost: **shear-interaction role**==
-  wave–mean energy transfer mechanism (non-divergent work)
-  Stokes drift gradient–induced deformation of mean flow
-  part of wave-modified vorticity restructuring associated with shear interactions
-  full GLM energetic consistency
### Consequences for vorticity dynamics
The resulting system remains correct for:
- kinematic transport of vorticity
but becomes incomplete for:
- wave-modified vorticity deformation mechanisms
- energetically consistent wave–mean coupling
- shear-driven modification of flow structures

## Summary
### Experiment Description
A kinematic wave-modified flow model where Stokes drift enters ONLY through the material advection velocity $u^L=u^E+u^s$, while all gradient-mediated wave-mean coupling is neglected. This forms a controlled closure choice.
### Mechanism Isolation
Keep
- Lagrangian transport pathway: how waves move fluid parcels
	- redistribution of tracers, vorticity, mass
Neglect
- dynamical coupling pathway: how waves reshape the flow
	- wave-mean shear interaction involving gradients of Stokes drift
	- energy transfer between waves and mean flow
### Scientific Question
“How much of the large-scale climate response can be explained purely by wave-modified transport, without wave–mean dynamical coupling?”

> [!Attention] Paragraph:
> - This framework isolates the role of wave-induced Lagrangian transport from wave–mean shear interactions in controlling large-scale ocean variability. 
> - It does not represent wave-driven momentum transfer or energetically consistent wave-mean flow interaction. 
> - The aim is to isolate the pure kinematic role of wave-induced transport and assess whether it alone can modify large-scale and seasonal ocean circulation patterns relative to a baseline state with no wave effects. 
> - This design allows a clean mechanistic separation between a non-wave reference state and a wave-transport-only state, providing a controlled test of whether Lagrangian transport by waves is sufficient to produce climatically relevant modifications in circulation and stratification patterns.
