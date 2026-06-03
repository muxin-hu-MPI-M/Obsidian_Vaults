---
tags:
  - project/surfwaves
  - wave/surface_wave
  - wave
  - important_paper
Last Eddited: 2025-12-04
---
# Abstract
- Numerical simulations of marine surface particle dispersal are a crucial tool for addressing many outstanding issues in physical oceanography of societal relevance, such as marine plastic pollution. However, the quality of these Lagrangian simulations depends on the ability of the underlying numerical model to represent prevailing ocean circulation features. 
- Here, we investigate how simulated marine surface particle dispersal changes if the – often omitted or only approximated – impact of wind-generated surface waves on upper-ocean circulation is considered. We use velocity fields from a high-resolution coupled ocean–wave model simulation and a complementary stand-alone ocean model simulation for the Mediterranean Sea to answer the following questions: 
	- (1) how does the explicit representation of waves impact simulated surface particle dispersal, and what is the relative impact of Stokes drift and wave-driven Eulerian currents? 
	- (2) How accurately can the wave impact be approximated by the commonly applied approach of advecting particles with non-wave-driven Eulerian currents and Stokes drift from stand-alone ocean and wave models? 
- We find that the representation of surface waves tends to increase the simulated mean Lagrangian surface drift speed in winter through the dominant impact of Stokes drift and tends to decrease the mean Lagrangian surface drift speed in summer through the dominant impact of wave-driven Eulerian currents. 
- Furthermore, ~={red}==**simulations that approximate the surface wave impact by including Stokes drift (but ignoring wave-driven Eulerian currents) do not necessarily yield better estimates**===~ of surface particle dispersal patterns with explicit wave impact representation than simulations that do not include any surface wave impact. 
- Our results imply that – whenever possible velocity fields from a coupled ocean–wave model should be used for surface particle dispersal simulations.

# Introduction
- Moreover, the presence of surface waves alters the Eulerian current field itself via various (partially nonlinear and interacting) processes. 
- **By pragmatically defining wave-driven Eulerian currents as the residual of the circulation with and without wave forcing, the Eulerian velocity can be decomposed into a wave-driven component ($u^{Ew}$) and a non-wave-driven component ($u^{Enw}$)** **(e.g., Cunningham et al., 2022). 
- Notably, at least part of the wave-driven Eulerian currents tend to act in the opposing direction of Stokes drift (see Higgins et al., 2020, and references therein for a review of this “anti-Stokes” effect). Combining these individual terms, the Lagrangian surface drift velocity of the particle (uL) can be expressed as: $$u^L = u^{Ew} + u^{Enw} + u^s$$

# Theoretical background and state of the art
## Impact of waves on Lagrangian surface drift velocities
- the overall magnitude and vertical shear of Stokes drift depend on the sea-state (Breivik & Christensen, 2020)
- “Wave-driven Eulerian current velocities arise from a combination of different processes related to interactions between Eulerian currents and Stokes drift, acting on a fluid particle through so-called ~={green}Stokes forces=~ (see, e.g., van den Bremer and Breivik, 2018, for a review), **as well as wave-induced changes in air–sea momentum and turbulent energy fluxes**” (Rühs et al., 2025, p. 218)
	- Stokes force: “As all of the interactions with waves in the WAB equations involve only one wave statistic—the Stokes drift—the interaction terms will be collectively referred to as Stokes forces.” (Suzuki and Fox-Kemper, 2016)
	- <span style="background:#affad1">Comment</span>: my study isolates the Eulerian ocean response to an imposed, ERA5-derived Stokes drift profile through the conservative Stokes-force terms of the wave-averaged momentum equation. The diagnosed velocity residual, is therefore interpreted as the **Stokes-force-induced Eulerian velocity**, rather than the full wave-induced Eulerian velocity. The “full” wave effect would also include wave-modified wind stress, wave-supported stress, wave breaking energy input, Langmuir turbulence, and wave effects on turbulent mixing.
	- “Because the other processes (e.g., surface momentum and turbulent kinetic energy fluxes) are kept identical to the control experiment, the diagnosed residuals should be interpreted as the oceanic adjustment to Stokes-drift momentum forcing alone. It does not include wave-induced modifications of air-sea fluxes, wave breaking, or Langmuir-enhanced turbulence.”
	