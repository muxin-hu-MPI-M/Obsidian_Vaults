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
	- <span style="background:#affad1">comment</span>: the $u_{Ew}$ is defined as the residual of Eulerian velocity between with and without wave forcing. the paper called it as “wave-driven Eulerian currents”, which I think should rephrase as “Eulerian velocity response to imposed wave forcing”, which is more accurate.
	- <span style="background:#affad1">for my study</span>, I isolate the Eulerian ocean response to an imposed, ERA5-derived Stokes drift profile through the conservative Stokes-force terms of the wave-averaged momentum equation. The diagnosed velocity residual, is therefore interpreted as the ***Stokes-force-induced Eulerian velocity anomaly*** instead.  
		- It tells you how the model’s prognostic velocity changes when Stokes forcing is included.
		- “*We define the Stokes-forcing-induced Eulerian velocity anomaly as the difference between the Stokes-forced and control simulations. This residual is a pragmatic sensitivity diagnostic: it includes both the direct response to the imposed Stokes terms and the subsequent nonlinear adjustment of the ocean state. It should therefore not be interpreted as a unique decomposition of the real ocean circulation into Stokes-forces-driven and non-Stokes-forces-driven components.*”

# Theoretical background and state of the art
## Impact of waves on Lagrangian surface drift velocities
- the overall magnitude and vertical shear of Stokes drift depend on the sea-state (Breivik & Christensen, 2020)
- “Wave-driven Eulerian current velocities arise from a combination of different processes related to interactions between Eulerian currents and Stokes drift, acting on a fluid particle through so-called ~={red}**Stokes forces**=~ (see, e.g., van den Bremer and Breivik, 2018, for a review), **as well as ~={red}wave-induced changes in air–sea momentum=~ and ~={red}turbulent energy fluxes=~**” (Rühs et al., 2025, p. 218)
	- ~={red}**Stokes force**=~:
		- “As all of the interactions with waves in the WAB equations involve only one wave statistic—the Stokes drift—the interaction terms will be collectively referred to as Stokes forces.” (Suzuki and Fox-Kemper, 2016)
		- The different effects of (non-breaking) surface waves on the Eulerian mean flow in the form of Stokes forces are described by wave-averaged momentum equations (e.g., Craik and Leibovich, 1976; Suzuki and Fox-Kemper, 2016).
		- “They always include the Stokes–Coriolis force (Hasselmann, 1970, 1971), which, however, appears either together with the vortex force and a wave-induced modification of the pressure (e.g., Craik and Leibovich, 1976) or together with Stokes advection and Stokes shear forces (e.g., Suzuki and Fox-Kemper, 2016).” (Rühs et al., 2025, p. 219)
	- ~={red}**wave-induced change in air-sea momentum flux**=~:
		- “The momentum flux from the atmosphere to the ocean is impacted by surface waves in two ways. Firstly, waves modify the sea surface roughness and, consequently, the regional atmospheric momentum flux (e.g., Charnock, 1955; Li et al., 2020). 
		- Secondly, waves alter when, where, and how much of this momentum flux is available to drive ocean currents
			- as waves grow, they absorb momentum from the wind (also referred to as wave-supported stress) that otherwise would have contributed to driving ocean currents, 
			- whereas as waves dissipate, they transfer momentum to ocean currents (also referred to as wave-to-ocean stress) (see Breivik et al., 2015; Couvelard et al., 2020).”
	- ~={red}**wave-induced change in turbulent energy flux**=~:
		- “As waves break, they inject turbulent kinetic energy into the surface layer, and vertical mixing is enhanced over a depth on the order of the significant wave height (Craig and Banner, 1994; Drennan et al., 1992).”
		- “Moreover, the waveaveraged flow generates Langmuir turbulence, resulting in vertical mixing over even greater depths (McWilliams et al., 1997), and significantly deepens the mixed layer in large areas of the world ocean (Couvelard et al., 2020).”
		- The related hydrographic changes can, in turn, introduce changes in horizontal Eulerian currents.
		- ==These changes in horizontal Eulerian surface currents due to wave-induced mixing could be as important as the impact of Stokes drift (Rascle et al., 2006; Rascle and Ardhuin, 2009).”==
	- <span style="background:#affad1">Comment</span>: my study isolates the Eulerian ocean response to an imposed, ERA5-derived Stokes drift profile through the conservative Stokes-force terms of the wave-averaged momentum equation. The diagnosed velocity residual, is therefore interpreted as the **Stokes-force-induced Eulerian velocity anomaly** , rather than the full wave-induced Eulerian velocity anomaly. The “full” wave effect would also include wave-modified wind stress, wave-supported stress, wave breaking energy input, Langmuir turbulence, and wave effects on turbulent mixing.
	- <span style="background:#affad1">for my study</span>, “Because the other processes (e.g., surface momentum and turbulent kinetic energy fluxes) are kept identical to the control experiment, the diagnosed residuals should be interpreted as the oceanic adjustment to Stokes-drift momentum forcing alone. It does not include wave-induced modifications of air-sea fluxes, wave breaking, or Langmuir-enhanced turbulence.”
- 
	