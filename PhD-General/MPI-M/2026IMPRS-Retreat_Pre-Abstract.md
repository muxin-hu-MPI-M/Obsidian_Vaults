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
Wind-generated ocean surface waves influence the coupled ocean-atmosphere system through several physically distinct pathways. They redistribute the momentum and energy flux at the air-sea interface, enhance upper-ocean mixing through Langmuir turbulence and wave-related turbulent kinetic energy input, and modify the ocean circulation through Stokes-drift-induced momentum effects. While the first two pathways have been widely studied and shown to affect mixed-layer structure and air-sea exchange at climate-relevant scales, the dynamic role of Stokes drift in climate system remains less clearly quantified.

The Stokes-drift-induced momentum pathway is often neglected because Stokes drift is expected to be offset by Eulerian compensating currents, implying a weak net large-scale effect after time-averaging. However, this assumption remains insufficiently tested under unsteady wave forcing. Synoptic and seasonal winds, together with remotely generated swell, produce Stokes drift field that varies strongly in space and time. At the same time, Eulerian compensation is a dynamical ocean adjustment depends strongly on sea state such as stratification and rotation (i.e., Coriolis force). The interaction between variable Stokes forcing and regionally dependent ocean adjustment therefore cannot be assumed to be local, shallow, complete, or steady. These features collectively support the necessity to quantify the Stokes-drift-induced momentum pathway in a global ocean model forced with unsteady wave field.

The central question is whether spatiotemporally varying Stokes forcing can reorganise ocean circulation and leave non-negligible residual tracer transport after Eulerian compensation. Heat transport is used as a climate-relevant diagnostic of this residual response. The conceptual picture is that Stokes drift may move near-surface warm water, while the compensating return flow may occur at different depths and carry water with different temperature properties. Residual heat or tracer redistribution may therefore remain even if volume transport is partly compensated. Within the global analysis, the tropical Indo-Pacific will be used as a regional testbed because it is strongly exposed to remotely generated swell and exhibits complex, seasonally varying wave regimes.

To address this gap, I have implemented ERA5-derived Stokes drift forcing in ICON-o using a reduced wave-averaged Boussinesq framework. The experiment compares a reference simulation forced by ERA5 atmospheric fields with a Stokes-forced simulation using the same atmospheric forcing plus reconstructed hourly Stokes drift profiles. Preliminary diagnostics suggest that the oceanic response is not confined to the shallow Stokes layer. The next step is to quantify the depth structure, regional and seasonal organisation, and heat-transport signature of this residual response in the paired simulations.


> !The central scientific question is therefore to what extent spatiotemporally varying Stokes forcing dynamically reorganises ocean circulation and produces non-negligible residual tracer transport after Eulerian compensation. 

### Comment from Noel
- the first paragraph is okay-ish, providing general background and a little bit more background
- the second paragraph is trying to “asking permission to do research”
- you can be more specific about the Stokes-dirft-induced momentum pathway, mentioning the key question in the second paragraph
- The IMPRS retreat will be about the current focus, no need to mention something else that won’t be the current focus (like the few other pathways)
- Your version is more like a PhD thesis introduction that consists all the wave stuff you want to investigate 
- can follow the “Nature structure” of abstract:
![[Pasted image 20260824215631.png]]

## Draft 3
### Text
> The large scale circulation of the subtropical ocean and atmosphere act to redistribute heat from the tropics to higher latitudes. Mid-latitude storms play a crucial role in this poleward heat transport  (Barry et al., 2002). However, tempestuous winds that develop under these storms generate energetic ocean surface waves that can induce equatorward transports may counteract large scale energy flows through a mechanisms known as Stokes drift. It is unclear whether the relatively small scale Stokes drift can have a net impact on large scale heat transports because its influence may be compensated by the ocean Eulerian adjustments. Here, we modify the momentum equations of a global ocean circulation model to account for the tracer transport and ocean dynamical adjustments that result from Stokes drift. Using a hierarchy of simulations, we obtain the different effects of Stokes without adjustments and with increasing complexity.

The large-scale ocean-atmosphere circulation redistributes excess tropical heat toward higher latitudes. Mid-latitude storms play a crucial role in this poleward heat transport, but the tempestuous winds associated with these storms also generate energetic surface gravity waves. Through Stokes drift, these waves induce a surface-intensified Lagrangian transport that can move near-surface water and tracers along the direction of wave propagation. In regions exposed to remotely generated swell, the depth-integrated Stokes transport can include an equatorward component and may therefore oppose part of the heat redistribution carried by the large-scale circulation.

It is unclear whether Stokes-induced transport leaves a non-negligible imprint on large-scale heat transport because its influence my be compensated by the ocean adjustments. When Stokes drift is introduced into the momentum balance, the ocean can adjust through compensating Eulerian currents, pressure-gradient changes, vertical velocity changes, and modified tracer pathways. Nevertheless, this adjustment depends on regional conditions such as rotation, stratification, mixing, and basin geometry. Even if volume transport is partly compensated, the return flow may occur at different depths and carry water with different temperature properties, allowing residual heat or tracer redistribution to remain.

To quantify the potential residual effect of Stokes drift, we modified a global ocean circulation model so that both Stokes-induced tracer transport and the associated dynamical ocean adjustment can be represented. Specifically, Stokes-drift-induced momentum effects were implemented in the ICON-o primitive equations using the wave-averaged Boussinesq framework. By comparing reference and Stokes-forced simulations, we can assess how the imposed Stokes transport modifies the circulation, how much of this transport is compensated by model-generated Eulerian currents, and what residual imprint remains in the circulation and tracer distributions.

Preliminary diagnostics suggest that the oceanic response is indeed not confined to the shallow Stokes layer, indicating that compensation may involve deeper circulation adjustments. The next step is to quantify the depth structure, regional and seasonal organisation, and heat-transport signature of this residual response, with particular attention to regions such as the tropical Indo-Pacific where remotely generated swell contributes strongly to seasonal wave variability. 

(maybe few sentences for broader perspective?)

### Comment from Huayu
- the first paragraph is too long and is not direct, it would be nice to have a straightforward background-gap in the first paragraph.
- Occam’s razor theory: the simplest explanation is usually the best one. Like the storms I mentioned, if we do not investigate the storms we might don’t want to mention them (如无必要，勿增实体)

## Draft 4
### Text
> original: 
> The large-scale ocean-atmosphere circulation redistributes excess tropical heat toward higher latitudes. Mid-latitude storms play a crucial role in this poleward heat transport, but the tempestuous winds associated with these storms also generate energetic surface gravity waves. Through Stokes drift, these waves induce a surface-intensified Lagrangian transport that can move near-surface water and tracers along the direction of wave propagation. In regions exposed to remotely generated swell, the depth-integrated Stokes transport can include an equatorward component and may therefore oppose part of the heat redistribution carried by the large-scale circulation.

Storm-generated ocean surface waves may provide a previously under-quantified pathway of heat transport. Strong winds generate energetic waves and swell, which induce the Stokes drift, a surface-intensified Lagrangian transport in the wave propagating direction. In regions exposed to equatorward-propagating swell, this Stokes transport can move near-surface water and tracers toward lower latitudes. Because the large-scale ocean-atmosphere circulation generally redistributes excess tropical heat poleward, this wave-induced transport may oppose part of the climatological heat redistribution. However, whether Stokes-induced transport leaves a non-negligible imprint on large-scale heat transport remains unclear. 

This uncertainty arises because Stokes transport cannot be interpreted as an isolated advective flux. When Stokes drift is introduced into the momentum balance, the ocean can adjust through compensating Eulerian currents, pressure-gradient changes, vertical velocity changes, and modified tracer pathways. These adjustments are often expected to offset the imposed Stokes transport, implying a weak net effect. Nevertheless, this adjustment depends on regional conditions such as rotation, stratification， and basin geometry. Even if volume transport is partly compensated, the return flow may occur at different depths and carry water with different temperature properties, allowing residual heat or tracer redistribution to remain. 

To quantify the potential residual effect of Stokes drift, we modified a global ocean circulation model so that both Stokes-induced tracer transport and the associated dynamical ocean adjustment can be represented. Specifically, Stokes-drift-induced momentum effects were implemented in the ICON-o primitive equations using the wave-averaged Boussinesq framework. By comparing reference and Stokes-forced simulations, we can assess how the imposed Stokes transport modifies the circulation, how much of this transport is compensated by model-generated Eulerian currents, and what residual imprint remains in the circulation and tracer distributions. 

Preliminary diagnostics suggest that the oceanic response is indeed not confined to the shallow Stokes layer, indicating that compensation may involve deeper circulation adjustments. The next step is to quantify the depth structure, regional and seasonal organisation, and heat-transport signature of this residual response, with particular attention to regions such as the tropical Indo-Pacific where remotely generated swell contributes strongly to seasonal wave variability. 