---
tags:
  - project/surfwaves
  - stokes_drift
Last Eddited: 2026-04-07
---
# Summary of the Idea

## [[2026-04-07]] 2nd version: Stokes advection & Stokes-Coriolis 
### Isolating the Large-Scale dynamical impact of Stokes drift
Surface gravity waves generate a Stokes drift that contributes to the Lagrangian motion of water parcels in the upper ocean. $$\mathbf u_{L}=\mathbf u_E + \mathbf u_s $$
Where $\mathbf u_E$ is the Eulerian velocity and $\mathbf u_s$ is the Stokes drift induced by surface waves.

Most ocean and climate models do not explicitly represent this contribution and instead assume that large-scale ocean transport is governed primarily by the Eulerian circulation. As a consequence, the potential influence of wave-induced Lagrangian transport on the redistribution of ocean properties is often neglected in large-scale climate simulations.

**One reason for this omission is that Stokes transport is expected to induce Eulerian compensations**. When a wave-driven Lagrangian transport is introduced, the ocean system would adjust through pressure gradients, sea-surface height changes, and return flows that partially offset the imposed transport. Because such adjustments can ideally compensate a large fraction of the Stokes transport, the net large-scale impact of surface waves is often assumed to be small.

**However, compensation of transport perturbations in the ocean does not occur instantaneously.** The adjustment of the circulation involves processes operating across a wide range of spatial and temporal scales and can exhibit substantial regional variability. As a result, wave-driven Lagrangian transport may perturb tracer pathways before the circulation fully compensates the imposed transport, and may lead to ~={red}residual transport that persists after large-scale adjustment has developed=~. This possibility motivates the need to quantify the dynamical response of the ocean circulation to wave-driven transport and to assess whether a measurable large-scale effect can emerge.

**To investigate these processes, this study focuses on the large-scale dynamical effects of Stokes drift.** While surface waves influence the ocean through multiple mechanisms, including wave-induced turbulence and Langmuir circulation, these processes primarily affect small-scale mixing in the upper ocean. In contrast, the present study isolates the large-scale dynamical pathway through which surface waves can influence the circulation: the modification of Lagrangian transport and the associated rotational response of the ocean. In a rotating system, the Coriolis force acts on the full Lagrangian velocity, producing an additional Coriolis–Stokes forcing that modifies the momentum balance of the ocean circulation. Capturing both the Lagrangian transport and this rotational effect therefore provides a minimal dynamically consistent representation of the large-scale dynamical influence of Stokes drift.

To isolate this mechanism, diagnosed Stokes drift fields derived from wave reanalysis products are incorporated into the ocean model. The effective Lagrangian velocity is used in the advection of mass, momentum, and tracers, while the corresponding Coriolis–Stokes forcing is included in the momentum equation. The Stokes drift field is prescribed and does not interact dynamically with the wave field, allowing the experiment to isolate the oceanic response to wave-driven transport without representing the full wave–current coupling.

Because the Coriolis force acts on the full Lagrangian velocity, the inclusion of Coriolis–Stokes forcing allows part of the dynamically consistent circulation adjustment to be represented within the model. The ocean circulation is therefore able to develop Eulerian compensating flows in response to the imposed Stokes transport, enabling a physically meaningful adjustment of the large-scale circulation.

It should be emphasised that the present configuration focuses specifically on the **large-scale dynamical effects of Stokes drift**. In particular, the experiment isolates the impact of wave-driven Lagrangian transport and its associated rotational response through the Coriolis–Stokes forcing. Other wave–current interaction processes, such as Stokes–shear forcing that generates Langmuir turbulence, are not included. These mechanisms primarily influence **small-scale turbulence and vertical mixing in the upper ocean**, whereas the present study is concerned with the **large-scale circulation adjustment and tracer redistribution** associated with wave-induced transport.

**The present idealised configuration therefore provides a controlled framework to estimate how the ocean circulation dynamically adjusts to wave-driven transport at large scales.** By comparing simulations with and without the imposed Stokes drift, the experiment directly quantifies:

- the circulation response induced by the Stokes drift,
- the degree to which the imposed transport is dynamically compensated by circulation adjustments, and
- the resulting modification of tracer pathways and large-scale property redistribution.

In this framework, differences between the perturbed simulation and the control simulation represent the dynamical response of the coupled system to the imposed wave-driven transport. The resulting tracer redistribution therefore provides a measure of the **residual large-scale effect of Stokes drift after the ocean circulation has dynamically adjusted to compensate the imposed transport**.

The central research question addressed in this study is therefore:

**To what extent can wave-driven Stokes drift generate persistent large-scale tracer redistribution after the ocean circulation dynamically compensates the imposed transport perturbation?**

Answering this question helps determine whether wave-induced Lagrangian transport can produce a measurable **large-scale dynamical impact** on ocean circulation and tracer pathways, and therefore whether this mechanism may have previously overlooked relevance for the climate system.

## [[2026-04-01]] 1st version: Isolating Stokes advection only
Also recorded in: [[Regular-Meeting_Note#Isolating the Lagrangian transport effect of surface wave]]
The later discussion can be found at: [[Regular-Meeting_Note#Discussion with Nobu]]
### Isolating the Lagrangian transport effect of surface wave
Surface gravity waves generate a Stokes drift that contributes to the Lagrangian motion of water parcels in the upper ocean. $$\mathbf u_{L}=\mathbf u_E + \mathbf u_s $$
Where $\mathbf u_E$ is the Eulerian velocity and $\mathbf u_s$ is the Stokes drift induced by surface waves.

However, most ocean and climate models neglect the Stokes transport component and assume that large-scale tracer transport is governed solely by the Eulerian circulation. As a result, the potential contribution of wave-induced Lagrangian transport to upper ocean tracer redistribution is not explicitly represented.

To investigate the possible climate relevance of this missing transport pathway, this study will incorporate the diagnosed Stokes drift fields derived from wave reanalysis products into the advection operators of the ocean model: $$\frac{D \phi}{D t}+(\mathbf u_E + \mathbf u_s)\cdot \nabla \phi = ...  $$
where $\phi$ represents variables such as mass, momentum, and tracers that are transported by the effective Lagrangian velocity. In this configuration, Stokes drift contributes directly to the transport of ocean properties while the model continues to solve the full momentum and mass conservation equations.

This experiment design isolates the transport pathway associated with surface waves, while allowing the coupled ocean-atmosphere system to dynamically adjust through pressure gradients, sea-surface height changes, and Eulerian return flows. Any perturbation introduced by the imposed Stokes drift can therefore induce circulation adjustments within the resolved ocean dynamics.

The experiment does not attempt to represent the full wave-current interaction dynamics described by the wave-averaged momentum equations. In particular, wave-induced momentum forcing terms such as Coriolis-Stokes forcing are not explicitly included. Instead, the objective is to isolate the role of wave-induced Lagrangian transport as an additional transport pathway.

A key physical motivation for this design is that compensation of transport perturbations in the ocean does not occur instantaneously. Adjustment of the ocean typically involved processes operate across various spatial and temporal scales and exhibit regional variability. Consequently, the introduction of wave-driven Lagrangian transport may alter surface tracer pathways and upper-ocean heat redistribution before the ocean circulation fully compensates the imposed perturbation, and may lead to a residual tracer transport that persists after large-scale Eulerian compensation has developed, particularly in regions where dynamical adjustment is slow.

The present configuration therefore provides a controlled framework to test how the ocean circulation dynamically adjusts to wave-driven Lagrangian transport. The imposed Stokes drift introduces an additional transport pathway, to which the ocean circulation dynamically adjusts through pressure gradients, sea-surface height changes, and Eulerian return flows. Because the full wave momentum dynamics are not represented, the resulting circulation changes should be interpreted as the system’s response to the imposed transport perturbation rather than the true dynamical pathways of wave-current interaction. Nevertheless, this framework allows a direct assessment of **how much of the imposed Stokes transport is effectively compensated by the circulation adjustments and how much contributes to residual tracer redistribution.** Quantifying the resulting tracer transport therefore provides ~={red}**a measure of the potential climate relevance of wave-induced Lagrangian transport in an idealised setting**=~. In this way, the experiment evaluates whether this additional transport has the potential to influence the coupled climate system, even when allowing the ocean circulations to dynamically adjust to the imposed perturbation. The key question addressed in this study is whether wave-driven Lagrangian transport can generate a persistent residual tracer redistribution after the large-scale ocean circulation dynamically compensates the imposed transport perturbation.

#### Summary
- **Experimental design:**  
    The idealised experiment is designed to isolate the transport pathway associated with Stokes drift. Specifically, the diagnosed Stokes drift is added to the Eulerian velocity to form an effective Lagrangian velocity that advects mass, momentum, and tracers with water parcels.
- **Dynamical adjustment allowed:**  
    The coupled ocean–atmosphere system is allowed to dynamically adjust to the imposed transport perturbation through pressure gradients, sea-surface height changes, and Eulerian circulation responses, ensuring that the overall mass balance of the system is maintained.
- **Idealised representation of wave effects:**  
    The experiment does not attempt to represent the full wave–current interaction dynamics described by the wave-averaged momentum equations. Consequently, several wave-induced dynamical processes are not included, such as Coriolis–Stokes forcing, Langmuir turbulence, and wave-induced pressure corrections. In addition, the imposed transport may introduce inconsistencies in the full dynamical framework (e.g., potential vorticity balance), since the momentum pathways associated with the wave field are not explicitly represented.
- **Interpretation of the response:**  
    As a result, the diagnosed circulation changes should be interpreted as the ocean’s dynamical response to an imposed transport perturbation rather than the true physical pathways through which waves interact with the ocean.
- **Scientific value of the experiment:**  
    Despite these idealisations, the configuration provides a controlled framework to assess the potential climate relevance of wave-driven Lagrangian transport. By quantifying how much of the imposed Stokes transport is dynamically compensated and how much contributes to residual tracer redistribution, the experiment evaluates whether this additional transport pathway can produce a measurable impact on the coupled climate system.