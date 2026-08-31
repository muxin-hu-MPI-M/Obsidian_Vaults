---
tags:
  - project/surfwaves
  - project/PhD_general
Last Eddited: 2026-08-23
---

# Presentation
## Requirement
- Use the prescribed IMPRS-ESM corporate design templates available on the MPI-M Intranet.
- Customise the content of the slides, keeping the title page format unchanged (color, font type, and size).
- Title Page Requirements:
	- Display the title of your talk, your name, and the names and roles of your Advisory Panel members.
	- Include your affiliation details and the dates marking the start and expected completion of your PhD.
	- Mention, if applicable, the publication status of your work (e.g. in preparation, submitted, under review).
- Presentation Structure
	- **10-minute presentation** followed by 5-minute discussion
		- 1/3: The first third of your talk should be general enough to be understood by an interdisciplinary audience.
		- 2/3: The next third should cater to those familiar with your discipline.
		- 3/3: The final third should highlight how your research has advanced knowledge in your field.
	- Begin the presentation by clearly stating your research question, Highlight what is new and why it matters
	- Explain how you addressed the research problem and discuss the expected or achieved results
	- Include only figures and data that are crucial to your message
	- Clearly label all axes and legends on your graphs to ensure they contribute effectively to the understanding of your results
	- End your presentation with a summary or conclusions slide that serves as the take-home message for the audience
	- Number the slides

## Abstract
Taken directly from [[2026IMPRS-Retreat_Pre-Abstract#Text]]
**Title**: Assessing the Climate-Scale Imprint of Wave-Induced Stokes Transport

Wind-induced ocean surface waves may provide a previously under-quantified pathway of heat distribution. As waves propagate away from their generation regions, they transport near-surface water and tracers through a mechanism known as Stokes drift. Reanalysis products show that subtropical Stokes transport often has an equatorward component, away from major storm-track regions. This transport opposes the climatological poleward heat redistribution by the large-scale oceanic and atmospheric circulations. However, because Stokes drift is not represented explicitly in most global ocean models, its net climate-scale imprint remains unclear.

To constrain this potential impact, we modified the momentum equations in ICON-O to include Stokes-drift-induced effects. Once introduced, the ocean adjusts through compensating Eulerian currents, pressure-gradient changes and altered vertical motion. By allowing this adjustment to develop dynamically within the ocean system, we can assess how the imposed Stokes transport modifies ocean circulation, how much is compensated by the Eulerian currents, and what residual imprint remains in circulation and tracer distributions.

Preliminary diagnostics suggest that the oceanic response to the imposed Stokes drift extends well below the relatively shallow Stokes layer, indicating deeper circulation adjustments. The next step is to quantify the depth structure, regional and seasonal organisation, and heat transport signature of this residual response. The analysis will remain global in scope while emphasising timescales and regions that provide clear dynamical interpretation. More broadly, this study examines one pathway by which large-scale ocean circulation responds to wind-wave-induced effect.

## Outline
Since we only have 10 minutes, need to be straightforward for almost everything, and no need to add details that will not be discussed.
### Draft 1
1. **General intro**: 
	1. What is Stokes drift? (==Figure 1==: net drift of asymmetric wave orbital motion) 
	2. What is its role? (moves water, and associated tracers, heat)
	3. Close connections to the atmosphere through winds (wind-driven waves)
2. **Why is it matter?**
	1. The most energetic waves are generated through the fast winds associated with weather systems like mid-latitude storms, which plays a key role in the atmospheric poleward heat transport.
	2. Reanalysis products (that implement a wave model) shows that the waves and the associated Stokes drift often has an equatorward component, away from the major storm-track regions. 
	3. As the Stokes drift moves the heat, this Stokes-induced heat transport opposes the poleward heat redistribution by those storms and in general those large-scale circulations in the atmosphere and ocean
	4. The upper-bound estimate shows great magnitudes in meridional heat transport (==Figure 2==: Unadjusted Stokes-only heat transport)
3. **To quantify the net climate-scale imprint on heat transport, we implemented the Stokes drift into the ocean model in a dynamical consistent way**
	1. Once the Stokes drift is introduced, the ocean adjusts through compensating Eulerian currents, pressure-gradient changes and altered vertical motion. These effect combined may oppose the impact induced from Stokes, and has long supporting the hypothesis of negligible Stokes-induced influence on the climate scale.
	2. However, we argues this hypothesis, as these adjustments may offset the imposed Stokes transport, but their strength and vertical structure depend on regional conditions such as rotation and stratification. If the compensating return currents occur at different depth and carries water with different properties (e.g., temperature), residual heat or tracer redistribution may remain even when volume transport is partly compensated. (==Figure 3==: Schematic of the redistribution)
	3. Thus, to constrain the net imprint, we modified the ICON-O primitive equations to include Stokes-drift-induced effects under the Wave-Averaged Boussinesq framework (functions retain the leading order wave couplings with other scales, found using multiscale asymptotic analysis). By allowing ocean adjustments to develop dynamically, one can assess the net imprint
4. **Current status and Preliminary diagnose**
	1. The developing phase is finished, currently
	2. Preliminary analysis suggest that the oceanic response to the imposed Stokes drift extends below the relative shallow Stokes layer, as expected
	3. The ocean heat transport shows anomalous heat convergences in the Southern Ocean
5. **Next step**:
	1. Extending the simulation to longer durations
	2. quantify the depth structure of the oceanic response, detecting regional organisation and heat transport signature
6. **Conclusion (takeaway message)**

### Draft 2
**Title Slide: Assessing the Climate-Scale Imprint of Wave-Induced Stokes Transport**  
Takeaway: wind-induced waves may provide an under-quantified pathway of heat redistribution through three-dimensional ocean adjustment.

1. **Opening: What Is Stokes Drift? (~1 min)**  
	1. Use Figure 1. 
	2. Introduce the physical idea: water parcels experience a small net drift in the direction of wave propagation. This Stokes drift is surface-intensified and can move near-surface water and tracers.
2. **Why Could This Matter for Climate? (~2 min)**  
	1. Use Figure 2.  
	2. Show that reanalysis-based Stokes transport has strong spatial and seasonal structure. In the subtropics, it often has an equatorward component, away from major storm-track regions. 
	3. The Stokes-only upper-bound estimate suggests that the imposed wave-driven transport is not trivially small, and may be relevant for heat redistribution.
3. **Dynamical Test: Introduce Stokes Drift Into ICON-o (~3 min)**  
	1. Use Figure 3.  
	2. To move from potential transport to net climate-scale imprint, we introduce Stokes-drift-induced effects into ICON-o using the wave-averaged Boussinesq framework. Once Stokes drift is included, the ocean responds through compensating Eulerian currents, pressure-gradient changes, and altered vertical motion. The strength and depth of this adjustment depend on regional conditions such as rotation and stratification. If the response extends below the shallow Stokes layer, it may reorganise circulation and heat transport in a way that is not captured by the Stokes-only estimate.
4. **Current Status and Preliminary Diagnostics (~2 min)**  
	1. Use Figure 4
	2. The Stokes-drift implementation in ICON-o is finished, and paired reference/Stokes-forced simulations have started. Preliminary diagnostics suggest that the oceanic response extends below the shallow Stokes layer, consistent with a three-dimensional adjustment. Early heat-transport diagnostics show regional signals, including anomalous heat convergence in the Southern Ocean, which need to be tested with longer simulations.
5. **Next Steps and Takeaway (~2 min)**  
	1. Next, quantify the depth structure, seasonal variability, regional organisation, and heat-transport signature of the residual response.  
	2. Final message: Stokes drift is not only a small surface transport; when introduced dynamically, it may trigger a three-dimensional ocean adjustment with a detectable climate-scale imprint.

## Frame the outline into key sentences
Frame the presentation with core sentences for ideas, transitions, and conclusions:
- **Opening**:
	- When we look at the ocean surface, the most visible motion is often the waves. These waves are generated by winds blowing over the ocean surface. 
		- ~={green}give a length scale map?=~
	- As waves propagate, the asymmetric orbital motion of water parcels produces a small net drift in the direction of wave propagation to move mass, heat and other tracers. This is Stokes drift.
- **Why could this matter for climate?**
	- Its depth-integrated transport is spatially organised and not trivially small. 
		- ==Figure 2(a) volume transport== shows the global climatological Stokes volume transport estimated from ERA5 wave Reanalysis. 
		- It shows coherent Stokes transport patterns, including equatorward components in the subtropics, away from major storm-track regions. 
		- the imposed wave-driven transport is organised enough to motivate a climate-scale question.
	- As Stokes drift also transport tracers like heat, it may contribute to heat redistribution. 
		- ==Figure 2(b) heat transport== shows the estimated Stokes-only upper bound heat transport by combining the depth-integrated Stokes transport with near surface temperature. 
		- Its zonal integration shows clear divergence from the storm-track regions and convergence towards the tropics, suggesting that the wave-induced pathway may partly oppose the usual poleward redistribution of heat by large-scale circulations 
		- This upper-bound estimate does not quantify the net ocean response, but it shows that Stokes transport is spatially organised and potentially climate-relevant.
	- ~={red}General gap: The question emerges: whether this underrepresented wave-induced pathway can leave a large-scale imprint on heat redistribution.=~
		- ~={green}Comment from Huayu & Jingzhi: Sounds weird of listing this general gap here. I should sharpen it to just one. The only problem is how fast I can mention the gap in the first few minutes=~
- **Introduce Stokes drift into ICON-o**
	- To move from potential transport to net climate-scale imprint, we introduce Stokes drift into the ICON-O in a dynamically consistent way. 
		- ==Figure 3 Mathematical expressions== shows the mathematical framework. Specifically, Stokes-induced momentum effects are included in the ICON-O primitive equations under the Wave Averaged Boussinesq framework
		- Once Stokes drift is included, the ocean responds through compensating Eulerian currents, pressure-gradient changes, and altered vertical motion. These responses can oppose, reshape, or redistribute the impact of the imposed Stokes forcing.
		- The strength and vertical structure depend on regional conditions such as Coriolis strength and stratification. Therefore, the heat-transport imprint cannot be inferred from the Stokes transport alone. 
		- We need a global ocean model that allows the imposed Stokes forcing and the ocean response to be diagnosed together
	- ~={red}Sharpen gap: The sharper question is whether wave-induced Stokes drift reorganises ocean circulation in a way that leaves a net, detectable, and organised imprint on large-scale heat distribution.=~
- **Current Status and Preliminary Diagnostics**
	- The Stokes-drift implementation in ICON-o is finished and verified
	- ==Show the details about the paired simulations==: paired reference/Stokes-forced simulations have started
		- prescribed Stokes drift velocity profile estimated from ERA5 wave reanalysis to force the ICON-O as the Stokes-forced simulation; 
		- The reference simulation is the ERA5 atmosphere forced ICON-O
	- Preliminary diagnostics suggest that the oceanic response extends below the shallow Stokes layer, consistent with a three-dimensional adjustment. Early heat-transport diagnostics show regional signals, including anomalous heat convergence in the Southern Ocean, which need to be tested with longer simulations.
- **Next step & Takeaway message**
	- Next, quantify the depth structure, seasonal variability, regional organisation, and heat-transport signature of the residual response.  
	- Takeaway message: 
		- Wind-induced waves may provide a previously under-quantified pathway of heat redistribution, through their dynamical interactions with the ocean circulation

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

Wind-induced ocean surface waves may provide a previously under-quantified pathway of heat and tracer distribution. Strong winds generate energetic waves and swell, which induce the Stokes drift that results a Lagrangian transport in the wave propagating direction. In regions exposed to equatorward-propagating swell, this Stokes transport can move near-surface water and tracers toward lower latitudes. Against the background of predominantly poleward heat redistribution by the climate system, such wave-induced transport could partly oppose the climatological heat redistribution. Whether it leaves a non-negligible large-scale imprint, however, remains unclear.

This uncertainty arises because Stokes transport is not an isolated advective flux. When Stokes drift is introduced into the momentum balance, the ocean can adjust through compensating Eulerian currents, pressure-gradient changes, vertical velocity changes, and altered tracer pathways. These adjustments are often expected to offset the imposed Stokes transport, implying a weak net effect. However, their strength and vertical structure depend on regional conditions such as rotation and stratification. Even if volume transport is partly compensated, the return flow may occur at different depths and carry water with different temperature properties, allowing residual heat or tracer redistribution to remain. 

To quantify the potential residual effect of Stokes drift, we modified a global ocean circulation model so that both Stokes transport and the associated dynamical ocean adjustment can be represented. Specifically, Stokes-drift-induced momentum effects were implemented in the ICON-o primitive equations using the wave-averaged Boussinesq framework. By comparing reference and Stokes-forced simulations, we can assess how the imposed Stokes transport modifies the circulation, how much of this transport is compensated by model-generated Eulerian currents, and what residual imprint remains in circulation and tracer distributions. 

Preliminary diagnostics suggest that the oceanic response is indeed not confined to the shallow Stokes layer, indicating that compensation may involve deeper circulation adjustments. The next step is to quantify the depth structure, regional and seasonal organisation, and heat-transport signature of this residual response, with particular attention to regions such as the tropical Indo-Pacific where remotely generated swell contributes strongly to seasonal wave variability. 

> [!Important] Short version
> Wind-induced ocean surface waves may provide a previously under-quantified pathway of heat distribution. Strong winds generate energetic waves and swell, which induce the Stokes drift that contributes to the Lagrangian transport in the wave propagating direction. In regions exposed to equatorward-propagating swell, this Stokes transport can move near-surface water and tracers toward lower latitudes, potentially opposing part of the climatological poleward heat redistribution. Whether this process leaves a non-negligible large-scale imprint remains unclear.
> 
> This uncertainty arises because Stokes transport is not an isolated advective flux. Once introduced into the ocean momentum balance, the ocean adjusts through compensating Eulerian currents, pressure-gradient changes, modified vertical motion, and altered tracer pathways. These adjustments may offset the imposed Stokes transport, but their strength and vertical structure depend on regional conditions such as rotation and stratification. If the compensating return currents occur at different depth and carries water with different properties (e.g., temperature), residual heat or tracer redistribution may remain even when volume transport is partly compensated.
> 
> To quantify this potential residual effect, we modified the ICON-o so that both Stokes drift and associated dynamical ocean adjustment can be represented. Specifically, we implemented Stokes-drift-induced momentum effects in ICON-o primitive equations using the wave-averaged Boussinesq framework. By comparing reference and Stokes-forced simulations, we can assess how Stokes transport modifies ocean circulation, how much of this transport is compensated by model-generated Eulerian currents, and what residual imprint remains in circulation and tracer distributions.
> 
> Preliminary diagnostics suggest that the oceanic response to the imposed Stokes drift is not confined to the shallow Stokes layer, indicating deeper circulation adjustments. The next step is to quantify the depth structure, regional and seasonal organisation, and heat transport signature of this residual response, with particular attention to the tropical Indo-Pacific, where remotely generated swell contributes strongly to seasonal wave variability.



## Draft 5
### Text
Wind-induced ocean surface waves may provide a previously under-quantified pathway of heat distribution. As waves propagate away from their generation regions, they transport near-surface water and tracers through a mechanism known as Stokes drift. Reanalysis products show that subtropical Stokes transport often has an equatorward component, away from major storm-track regions. This transport opposes the climatological poleward heat redistribution by the large-scale oceanic and atmospheric circulations. However, because Stokes drift is not represented explicitly in most global ocean models, its net climate-scale imprint remains unclear.

To constrain this potential impact, we modified the momentum equations in ICON-O to include Stokes-drift-induced effects. Once introduced, the ocean adjusts through compensating Eulerian currents, pressure-gradient changes and altered vertical motion. By allowing this adjustment to develop dynamically within the ocean system, we can assess how the imposed Stokes transport modifies ocean circulation, how much is compensated by the Eulerian currents, and what residual imprint remains in circulation and tracer distributions.

Preliminary diagnostics suggest that the oceanic response to the imposed Stokes drift extends well below the relatively shallow Stokes layer, indicating deeper circulation adjustments. The next step is to quantify the depth structure, regional and seasonal organisation, and heat transport signature of this residual response. A specific focus will be whether hemispheric asymmetries in wave forcing, linked to more frequent Southern Ocean storm activity and remotely propagated swell, contribute to changes in oceanic cross-equatorial heat transport. More broadly, this study examines one pathway by which large-scale ocean circulation responds to wind-wave-induced effect.


## Draft 6
### Text

**Title**: Assessing the Climate-Scale Imprint of Wave-Induced Stokes Transport

Wind-induced ocean surface waves may provide a previously under-quantified pathway of heat distribution. As waves propagate away from their generation regions, they transport near-surface water and tracers through a mechanism known as Stokes drift. Reanalysis products show that subtropical Stokes transport often has an equatorward component, away from major storm-track regions. This transport opposes the climatological poleward heat redistribution by the large-scale oceanic and atmospheric circulations. However, because Stokes drift is not represented explicitly in most global ocean models, its net climate-scale imprint remains unclear.

To constrain this potential impact, we modified the momentum equations in ICON-O to include Stokes-drift-induced effects. Once introduced, the ocean adjusts through compensating Eulerian currents, pressure-gradient changes and altered vertical motion. By allowing this adjustment to develop dynamically within the ocean system, we can assess how the imposed Stokes transport modifies ocean circulation, how much is compensated by the Eulerian currents, and what residual imprint remains in circulation and tracer distributions.

Preliminary diagnostics suggest that the oceanic response to the imposed Stokes drift extends well below the relatively shallow Stokes layer, indicating deeper circulation adjustments. The next step is to quantify the depth structure, regional and seasonal organisation, and heat transport signature of this residual response. The analysis will remain global in scope while emphasising timescales and regions that provide clear dynamical interpretation. More broadly, this study examines one pathway by which large-scale ocean circulation responds to wind-wave-induced effect.