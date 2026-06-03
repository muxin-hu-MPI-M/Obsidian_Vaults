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
- ==**We find that the representation of surface waves tends to increase the simulated mean Lagrangian surface drift speed in winter through the dominant impact of Stokes drift and tends to decrease the mean Lagrangian surface drift speed in summer through the dominant impact of wave-driven Eulerian currents.**== 
- Furthermore, ==**simulations that approximate the surface wave impact by including Stokes drift (but ignoring wave-driven Eulerian currents) do not necessarily yield better estimates**== of surface particle dispersal patterns with explicit wave impact representation than simulations that do not include any surface wave impact. 
- Our results imply that – whenever possible velocity fields from a coupled ocean–wave model should be used for surface particle dispersal simulations.

# Introduction


# Theoretical background and state of the art
## Impact of waves on Lagrangian surface drift velocities
- the overall magnitude and vertical shear of Stokes drift depend on the sea-state (Breivik & Christensen, 2020)
- “Wave-driven Eulerian current velocities arise from a combination of different processes related to interactions between Eulerian currents and Stokes drift, acting on a fluid particle through so-called Stokes forces (see, e.g., van den Bremer and Breivik, 2018, for a review), **as well as wave-induced changes in air–sea momentum and turbulent energy fluxes**” (Rühs et al., 2025, p. 218)
	- <span style="background:#affad1">Comment</span>: In my intended experiment, we consider only the processes in the wave-averaged momentum equations that induce the wave-driven Eulerian current, including Coriolis-Stokes force, Stokes shear force and Stokes advection. The other processes that are not present in the momentum equation of the ocean velocities are neglected, including the wave-induced changes in air-sea momentum and turbulent energy fluxes
	- <span style="background:#affad1">which means</span>, we are not representing the full processes that is related to induce wave-driven Eulerian current velocities.