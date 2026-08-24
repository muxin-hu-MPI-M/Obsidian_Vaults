---
tags:
  - project/surfwaves
  - project/PhD_general
Last Eddited: 2026-08-23
---
# Abstract
## Requirement
- Identify recent results or new information you want to present, then create a title and outline for your abstract
- 400 words
- Ensure that the abstract and title fit on one page; Formatting is carried out by the IMPRS office; List of reference is not required

## Outline
1. **Background**  
    Surface waves affect the ocean through fluxes, mixing, and Stokes-drift-induced momentum effects.
2. **Scientific gap**  
    The Stokes-induced momentum pathway is often neglected because Eulerian compensation is expected to offset Stokes transport. However, compensation is itself a dynamical adjustment involving currents, pressure gradients, vertical shear, and tracer pathways. Its depth structure, regional dependence, and residual impact remain poorly quantified.
3. **Research objective**  
    Isolate the Stokes-induced momentum pathway in ICON-o and assess how the ocean circulation adjusts to it. Important scope is the heat transport
4. **Recent progress**  
    ERA5-derived Stokes profiles have been introduced into a reduced wave-averaged Boussinesq framework in ICON-o, and control/Stokes-forced simulations have begun.
5. **Preliminary indication**  
    Early diagnostics suggest the response is not confined to the shallow Stokes layer, motivating analysis of deeper adjustment and tracer implications.
6. **Next step**  
    Quantify compensation, residual circulation, and possible heat/tracer redistribution.

## Draft 1
### Text
Wind-generated ocean surface waves influence both the ocean and atmosphere through several physically distinct pathways. They can redistribute the momentum and energy flux at the air-sea interface, enhance upper-ocean mixing through Langmuir turbulence and wave-related turbulent kinetic energy (TKE) input, and modify the ocean circulation through Stokes-drift-induced momentum effects. While the first two pathways have been widely studied and shown to affect the climate sensitivity of the entire earth, the dynamic role of Stokes drift remains less clearly quantified, even though diagnostic estimates suggest Stokes-induced tracer transport may be large enough to matter for basin-scale redistribution of tracer fields like sea surface temperature (SST).

This pathway is often neglected because Stokes transport is expected to be offset by Eulerian compensating currents, implying a weak net large-scale effect. However, this assumption remains insufficiently tested under realistic wave forcing. Compensation is a dynamical ocean adjustment involving currents, pressure gradients, vertical velocity, and tracer pathways, not simply a local cancellation of mass transport in the upper ocean. Nevertheless, growing evidence suggests that compensation can be incomplete and can vary with region, depth and time. In addition, the global wind and wave fields are strongly inhomogeneous and seasonally varying, with swell connecting remote generation regions to distance ocean basins. These features nevertheless support the necessity to quantify the Stokes-drift-induced momentum pathway in a global ocean model with realistic wave field.

The central scientific question is therefore whether realistic Stokes forcing can produce non-negligible residual changes in climate scale in ocean circulation and tracer transport after ocean dynamical Eulerian compensation. One specific scope of this study is the Stokes-induced heat transport as it provides a particularly climate-relevant focus: Stokes drift may move near-surface warm water, while the compensating return flow may occur at different depths and carry water with different temperature properties. Even if volume transport is partly compensated, residual heat or tracer redistribution may remain.

To address this gap, I have implemented ERA5-derived Stokes drift forcing in ICON-o using a reduced wave-averaged Boussinesq framework. The experiment compares a reference simulation forced by ERA5 atmospheric fields with a Stokes-forced simulation using the same atmospheric forcing plus reconstructed hourly Stokes drift profiles. Recent progress includes successful implementation of the Stokes forcing terms and the start of paired control and Stokes-forced simulations. Preliminary diagnostics suggest that the oceanic response is not confined to the shallow Stokes layer, motivating further analysis of the depth structure, seasonality, and regional organisation of the residual circulation and heat-transport response.

### Comment from Codex Skill: ARS/academic-paper-reviewer mode
- The gap is almost there, but it is currently split across three ideas: Stokes drift is understudied, compensation may be incomplete, and heat/tracer transport may remain. These are all correct, but the abstract would be stronger if you state one central gap more explicitly: **We do not yet know how a realistic, spatially and seasonally varying Stokes forcing dynamically reorganizes ocean circulation, and whether the adjusted response leaves non-negligible residual heat/tracer transport.** That sentence is the spine. Everything else should support it.
- **Main Issues**
	1. **The first paragraph slightly overclaims.**  
	    “affect the climate sensitivity of the entire earth” is too broad for an abstract. Better: “affect SST, mixed-layer structure, and air--sea exchange at climate-relevant scales.”
	2. **The gap needs a cleaner hierarchy.**  
	    Right now, “dynamic role,” “tracer transport,” “SST,” “compensation,” and “realistic wave forcing” appear close together. I would order them as:  
	    Stokes transport exists → often assumed compensated → compensation is a dynamical adjustment → realistic wave forcing makes this adjustment spatially/seasonally structured → heat/tracer residual is the climate-relevant consequence.
	3. **The phrase “Nevertheless” is used against your own logic.**  
	    In paragraph 2, “Nevertheless, growing evidence...” should be “However” or “Recent studies, however,”. Later, “These features nevertheless support...” should be “These features support...”. 
	4. **The heat transport scope is good, but should be framed as a diagnostic, not the whole research question.**  
	    Say: “Heat transport is used as a climate-relevant diagnostic of this residual response.” That avoids narrowing the whole study too much.
	5. **Some wording is non-native or imprecise.**  
	    Use:
	    - “air--sea” not “air-sea” if LaTeX style allows.
	    - “distant ocean basins” not “distance ocean basins”.
	    - “climate-scale residual changes” not “residual changes in climate scale”.
	    - “dynamical Eulerian compensation” or simply “Eulerian compensation”, not “ocean dynamical Eulerian compensation”.
- **Tighter abstract logic**
	- Waves matter for climate; Stokes is the less quantified pathway
	- Why Stokes was neglected; why that assumption is insufficient
	- Scientific question, with heat transport as the climate-relevant diagnostic
	- What I did and what preliminary diagnostic suggest


## Draft 2
### Text
Wind-generated ocean surface waves influence both the ocean and atmosphere through several physically distinct pathways. They can redistribute the momentum and energy flux at the air-sea interface, enhance upper-ocean mixing through Langmuir turbulence and wave-related turbulent kinetic energy (TKE) input, and modify the ocean circulation through Stokes-drift-induced momentum effects. While the first two pathways have been widely studied and shown to affect mixed-layer structure and air-sea exchange at climate-relevant scales, the dynamic role of Stokes drift in climate system remains less clearly quantified.

This pathway is often neglected because Stokes drift is expected to be offset by Eulerian compensating currents, implying a weak net large-scale effect after time-averaging. However, this assumption remains insufficiently tested under realistic wave forcing. Eulerian compensation is a dynamical ocean adjustment involving currents, pressure gradients, vertical velocity, and tracer pathways, not simply a local cancellation of mass transport in the upper ocean. In addition, the global wind-induced wave fields are strongly inhomogeneous and temporally varying, with energetic swell connecting remote generation regions to distant basins, affecting the local Stokes drift field. These features collectively support the necessity to quantify the Stokes-drift-induced momentum pathway in a global ocean model with realistic wave field.

The central scientific question is therefore whether realistic, spatially and temporally varying Stokes forcing can dynamically reorganise ocean circulation and produce non-negligible residual tracer transport after Eulerian compensation. 

! The central scientific question is therefore whether realistic Stokes forcing can produce non-negligible residual changes in climate scale in ocean circulation and tracer transport after Eulerian compensation. One specific scope of this study is the Stokes-induced heat transport as it provides a particularly climate-relevant focus: Stokes drift may move near-surface warm water, while the compensating return flow may occur at different depths and carry water with different temperature properties. Even if volume transport is partly compensated, residual heat or tracer redistribution may remain.

To address this gap, I have implemented ERA5-derived Stokes drift forcing in ICON-o using a reduced wave-averaged Boussinesq framework. The experiment compares a reference simulation forced by ERA5 atmospheric fields with a Stokes-forced simulation using the same atmospheric forcing plus reconstructed hourly Stokes drift profiles. Recent progress includes successful implementation of the Stokes forcing terms and the start of paired control and Stokes-forced simulations. Preliminary diagnostics suggest that the oceanic response is not confined to the shallow Stokes layer, motivating further analysis of the depth structure, seasonality, and regional organisation of the residual circulation and heat-transport response.




