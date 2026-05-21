---
tags:
  - project/surfwaves
  - stokes_drift
Last Eddited: 2026-04-07
---
# Summary of the Idea

## [[2026-04-07]] 2nd version: Stokes advection & Stokes-Coriolis 
key changes:
- Isolating the Stokes advection alone is physically inconsistent. Stokes-induced Lagrangian advection consider the Lagrangian velocity ($\mathbf u^{L}$). Thus, the Coriolis force must act on Lagrangian velocity, which explicitly introduces the Coriolis-Stokes forcing ($-\mathbf f \times \mathbf u^s$)
- The updated experimental design considers the Stokes advection and Coriolis-Stokes forcing while intentionally neglect Stokes shear term. The setup focuses on the large-scale dynamical effect of Stokes drift while maintains the minimal physical consistency in the momentum equation.
### Isolating the Large-Scale dynamical impact of Stokes drift
Surface gravity waves generate a Stokes drift that contributes to the Lagrangian motion of water parcels in the upper ocean. $$\mathbf u^{L}=\mathbf u^E + \mathbf u^s $$
Where $\mathbf u^E$ is the Eulerian velocity and $\mathbf u^s$ is the Stokes drift induced by surface waves. Most ocean and climate models neglect this contribution, assuming that large-scale ocean transport is primarily governed by the Eulerian circulation. Consequently, the potential influence of wave-induced Lagrangian transport on the redistribution of ocean properties is often overlooked in large-scale climate simulations.

One reason for this omission is that Stokes transport is expected to induce compensating Eulerian flows. When a wave-driven Lagrangian transport is introduced, the ocean circulation adjusts through pressure gradients, sea-surface height changes, and return flows that partially offset the imposed transport. Since such adjustments can compensate a significant fraction of the Stokes transport, the net large-scale impact of surface waves is commonly assumed to be small.

However, these compensating processes are not instantaneous. The adjustment involves dynamical processes operating across a wide range of spatial and temporal scales, with substantial regional variability. As a result, wave-driven Lagrangian transport may temporally perturb tracer pathways and circulation pattern before significant compensation occurs, potentially leaving residual effects that persist after system adjusts. This possibility motivates the need to quantify the dynamical response of the ocean circulation to wave-driven transport and to assess whether a measurable large-scale effect can emerge, particularly over regions with slow Eulerian compensation.

Quantifying these effects requires explicitly isolating the dynamical response of the circulation to wave-driven transport. This study focuses on the large-scale dynamical effects of Stokes drift. Surface waves influence the upper ocean through multiple mechanisms, including wave-mediated momentum transfer, wave-induced turbulence, and wave-driven Lagrangian transport. While wave effects on momentum and turbulence primarily modify vertical mixing and boundary layer processes, the mechanism considered here operates through a different pathway: the direct modification of Lagrangian transport and the associated rotational response of the large-scale circulation via the Coriolis-Stokes force. 

Isolating this mechanism has scientific value. It allows to track the potential influence of wave-driven Lagrangian transport on large-scale circulation and tracer pathways without confounding effects from other wave-driven processes. Therefore, this approach can potentially provide a clearer assessment of whether wave-driven Lagrangian transport can produce a measurable large-scale circulation response. In this framework, materially conserved quantities such as buoyancy ($b$), salinity ($s$), absolute vorticity ($\omega^a$), and potential vorticity ($q$) are advected by the Lagrangian velocity (Suzuki & Fox-Kemper, 2016):
$$ 
\begin{align}
\partial_t b + (\mathbf u^L \cdot \nabla)b &= D^b, \\
\partial_t s + (\mathbf u^L \cdot \nabla)s &= D^s, \\
\partial_t \omega^a + (\mathbf u^L \cdot \nabla)\omega^a &= (\omega^a \cdot \nabla)\mathbf u^L + \nabla \times \mathbf b+\nabla \times D^b, \quad \omega^a\equiv \mathbf f+\nabla \times \mathbf u^E\\
\partial_t q + (\mathbf u^L \cdot \nabla)q &= D^q = \nabla \cdot (\mathbf D^u\times \nabla b + \omega^a D^b), \quad q\equiv \omega^2 \cdot \nabla b\\
\end{align}
$$
where $D^a, D^b, \mathbf D^u$ represent diffusion processes acting on buoyancy, salinity and velocity, respectively. The corresponding momentum equation (Suzuki & Fox-Kemper, 2016) is:
$$ \partial_t \mathbf u^E + (\mathbf u^L \cdot \nabla)\mathbf u^E = -\mathbf f\times \mathbf u^L+ \mathbf b + \mathbf D^u -\nabla p -u_j^L\nabla u_j^s$$
Here, the Coriolis term $-\mathbf f\times \mathbf u^L=-\mathbf f\times (\mathbf u^E+\mathbf u^s)$ introduces the Coriolis-Stokes forcing. The Stokes shear term ($-u_j^L\nabla u_j^s$), which primarily drives small-scale turbulence, is intentionally neglected. By including only the Lagrangian transport and the corresponding rotational adjustment, the model captures the minimal, dynamically consistent representation of the large-scale effect of Stokes drift.

To implement this mechanism, prescribed Stokes drift fields derived from wave reanalysis products are incorporated into the ocean model. The resulting Lagrangian velocity is used in the advection of mass, momentum, and tracers, and the associated Coriolis–Stokes forcing is included in the momentum equation. Because the Stokes drift is prescribed and does not interact dynamically with the wave field, the configuration isolates the circulation response to wave-driven Lagrangian transport without representing the full spectrum of wave–current interactions.

Within this framework, the ocean circulation is free to dynamically adjust to the imposed Stokes transport, allowing compensating Eulerian flows to develop and maintaining a physically consistent large-scale balance. At the same time, the modified surface velocity can influence the air–sea momentum exchange, enabling the coupled atmosphere–ocean system to respond through its existing air–sea coupling. These adjustments occur without explicitly representing the evolution of the surface wave field or wave-induced turbulence. This idealised configuration therefore isolates the dynamical pathway associated with Stokes-modified Lagrangian transport while allowing the coupled circulation to adjust consistently. 

By focusing on a single mechanism in this idealised configuration, the study aims to quantify:
- the circulation response induced by the Stokes drift,
- the degree to which the imposed transport is dynamically compensated by circulation adjustments, and
- the resulting modification of tracer pathways and large-scale property redistribution.
- the corresponding large-scale responses in both ocean and atmosphere

Differences between the perturbed simulation and the control simulation represent the dynamical response of the coupled system to the imposed wave-driven transport. The central research question addressed in this study is therefore: ***“To what extent can wave-driven Stokes drift generate persistent large-scale tracer redistribution after the ocean circulation dynamically compensates the imposed transport perturbation?”*** Answering this question helps determine whether wave-induced Lagrangian transport can produce a measurable large-scale dynamical impact on ocean circulation and tracer pathways, and therefore whether this mechanism may have previously overlooked relevance for the climate system.


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