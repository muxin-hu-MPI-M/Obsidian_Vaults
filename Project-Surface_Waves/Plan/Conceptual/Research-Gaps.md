---
Type:
tags:
  - project/surfwaves
  - wave/surface_wave
  - proposal/surfwaves
---


# [[2026-06-30]] Update: 3 pathways declare
three-pathway structure can be:
1. **Wave-mediated air-sea fluxes**  
    Sea state modifies the exchange of momentum, heat, moisture, gas, and possibly mechanical energy across the air-sea interface.
2. **Stokes-induced resolved dynamics**  
    Stokes drift enters the wave-averaged momentum balance and modifies resolved Eulerian currents, sea-surface height, convergence/divergence, and tracer transport.
3. **Wave-enhanced turbulence and mixing**  
    Waves enhance unresolved vertical mixing through Langmuir turbulence, non-breaking-wave-induced mixing, breaking-wave TKE input, and related parameterisations.
# [[2026-02-25]] Update: PhD Research Outline
Coastal upwelling systems are fundamental components of the climate system, linking atmospheric forcing to ocean circulation, air–sea exchanges, and marine productivity. Upwelling is primarily driven by alongshore winds: wind stress generates offshore Ekman transport and surface divergence, allowing cold, nutrient-rich subsurface waters to reach the surface (Belmadani et al., 2014; Tarazona and Arntz, 2001). While wind remains the first-order driver of upwelling, air–sea momentum and energy exchange occur over a wave-covered ocean surface, where surface gravity waves can modulate how atmospheric forcing is transmitted into the upper ocean. ==**These wave-mediated processes introduce additional complexity, potentially influencing the efficiency of Ekman transport, vertical mixing, and upper-ocean structure beyond classical wind-driven dynamics.**==

Surface waves influence upper-ocean processes through three physically pathways. First, the momentum pathway, where waves modify surface stress and roughness, altering how wind energy is transmitted into the ocean and thereby affecting Ekman transport. Second, the Lagrangian pathway, in which Stokes drift and wave–current interaction reorganize momentum and tracer transport in the upper ocean. Third, the turbulent kinetic energy (TKE) pathway, in which wave-induced TKE through wave breaking and Langmuir turbulence modifies vertical mixing and mixed-layer structure. Previous studies have examined these mechanisms individually or in limited regional contexts, often using uncoupled or one-way coupled models (Wu et al., 2019a,b, 2022). **As a result, the combined influence of these wave-mediated pathways on coastal upwelling and upper-ocean structure has not been systematically quantified.**

~={red}**This PhD project aims to determine and quantify how surface gravity waves mediate wind-driven upwelling and introduce additional complexity in the upper ocean**=~. We **hypothesise** that ==wave-induced modifications of surface stress, Lagrangian transport, and TKE systematically alter upper-ocean structure through modified air–sea exchanges, producing measurable changes in upwelling intensity and vertical structure that cannot be captured in insufficiently coupled modelling frameworks.==

To test this hypothesis, we will use the fully coupled ICON modelling system, which integrates atmosphere (ICON-a), wave (ICON-wave), and ocean (ICON-o) components through energetically consistent exchanges of momentum and energy, allowing wind, wave, and ocean states to evolve interactively. The Southeast Pacific (Peru–Chile) upwelling system provides an ideal test region, characterised by persistent alongshore winds and a complex wave climate. The coexistence of locally generated wind waves and energetic remotely generated swells creates favourable conditions for separating distinct wave influences of different wave types.

A hierarchy of controlled numerical experiments will isolate causal mechanisms. A baseline atmosphere–ocean coupled simulation will establish the reference wind-driven upwelling state, while a fully coupled atmosphere–wave–ocean simulation will quantify the integrated impact of wave feedbacks. Additional experiments selectively activating individual wave pathways and separating wind-wave and swell forcing will allow attribution of changes in upper-ocean structure and upwelling variability to specific mechanisms.

By quantifying how waves mediate the transfer and redistribution of momentum and energy across the air–sea interface, this project aims to provide a process-based refinement of coastal upwelling dynamics. The results are expected to clarify how wave-mediated pathways influence classical wind-driven upwelling, improve representation of air–sea exchanges in coupled models, and offer insights into the physical mechanisms controlling upper-ocean structure in eastern boundary upwelling systems.

Reference: 
- Belmadani, A., Echevin, V., Codron, F., Takahashi, K., and Junquas, C. (2014). What dynamics drive future wind scenarios for coastal upwelling off Peru and Chile? Climate Dynamics, 43(7):1893–1914.
- Tarazona, J. and Arntz, W. (2001). The Peruvian Coastal Upwelling System. In Caldwell, M. M., Heldmaier, G., Lange, O. L., Mooney, H. A., Schulze, E.-D., Sommer, U., Seeliger, U., and Kjerfve, B., editors, Coastal Marine Ecosystems of Latin America, volume 144, pages 229–244. Springer Berlin Heidelberg, Berlin, Heidelberg. Series Title: Ecological Studies.
- Wu, L., Breivik, , and Rutgersson, A. (2019a). Ocean-Wave-Atmosphere Interaction Processes in a Fully Coupled Modeling System. Journal of Advances in Modeling Earth Systems, 11(11):3852– 3874. 
- Wu, L., Staneva, J., Breivik, , Rutgersson, A., Nurser, A. J. G., Clementi, E., and Madec, G. (2019b). Wave effects on coastal upwelling and water level. Ocean Modelling, 140:101405.
- Wu, L., Breivik, , and Qiao, F. (2022). The Redistribution of Air–Sea Momentum and Turbulent Kinetic Energy Fluxes by Ocean Surface Gravity Waves. Journal of Physical Oceanography, 52(7):1483–1496. Publisher: American Meteorological Society Section: Journal of Physical Oceanography.

# [[2026-02-24]] Update: PhD Research Outline
## Detailed version
### Physical problem
Coastal upwelling systems are key components of the climate system, sustaining high biological productivity and regulating regional air–sea exchanges of momentum, heat, and tracers. Upwelling is primarily driven by alongshore winds that induce offshore Ekman transport and divergence of surface waters, allowing cold, nutrient-rich subsurface water to rise toward the surface. The efficiency of this process depends critically on how atmospheric momentum and energy are transferred across the air–sea interface and redistributed within the upper ocean.
Surface gravity waves play a fundamental role in mediating these exchanges. By modifying surface stress, generating Stokes drift, and injecting turbulence through wave breaking, waves influence how wind forcing is transmitted into the ocean mixed layer. These processes may alter upper-ocean stratification, vertical mixing, and transport pathways, thereby affecting the intensity and structure of coastal upwelling. Despite their potential importance, the dynamical role of surface waves in regulating upwelling systems remains incompletely understood.
### Missing Mechanisms in Current Understanding
Previous studies have demonstrated that wave-induced processes can significantly influence air–sea interactions in coastal regions. However, most investigations rely on uncoupled or one-way coupled modelling frameworks, in which the wave field does not evolve interactively with atmospheric and oceanic states. Such configurations prevent feedbacks among wind forcing, wave evolution, and ocean response, limiting the dynamical consistency of simulated air–sea exchanges.
Furthermore, wave effects are often investigated individually and represented inconsistently across studies. Key processes, including wave-modified momentum fluxes, Stokes-drift-related dynamics, and wave-breaking-induced turbulence, are rarely evaluated within a unified and energetically consistent framework. Consequently, the combined and interacting impacts of surface waves on coastal upwelling dynamics remain poorly quantified.
### Central Hypothesis
This project is guided by the following hypothesis: **Wave-induced modifications of surface stress and upper-ocean turbulence systematically alter upper-ocean structure through modified air–sea exchanges, leading to measurable changes in coastal upwelling intensity that cannot be captured in insufficiently coupled modelling frameworks.**
Testing this hypothesis requires a dynamically consistent framework in which atmosphere, waves, and ocean evolve interactively.
### Conceptual Framework: Wave-Induced Pathways
Wave impacts on air–sea interaction will be examined through three physically distinct pathways:
1. **Wave-Mediated Momentum Pathway**:
   Surface waves regulate how atmospheric momentum is transferred into the ocean by modifying surface roughness and stress partitioning. These processes influence Ekman transport and therefore directly affect upwelling-favourable divergence.
2. **Lagrangian pathway**
   Waves generate Lagrangian Stokes drift that modifies momentum balances through the Coriolis–Stokes force and reorganises tracer and mass transport within the upper ocean, potentially altering vertical structure and transport efficiency.
3. **Turbulence Pathway**
   Surface waves enhance upper-ocean turbulence through two mechanisms: (i) direct TKE injection associated with wave breaking and (ii) Langmuir turbulence arising from wave–current interaction between Stokes drift shear and Eulerian currents (Craik–Leibovich mechanism). Together, these processes modify mixed-layer depth, stratification, and air–sea exchange efficiency.
This classification separates wave effects according to their dominant influence on momentum transfer, Lagrangian transport, and turbulent energy budgets while acknowledging the coupled nature of wave–current interactions.
### Method: Fully coupled Modelling Framework
The project will employ the fully coupled ICON modelling system, in which atmosphere (ICON-A), wave (ICON-Wave), and ocean (ICON-O) components interact through energetically consistent exchanges of momentum and energy. This framework enables two-way feedbacks between atmospheric forcing, wave evolution, and ocean response, allowing wave-induced processes to emerge dynamically rather than being prescribed.
The Southeast Pacific (Peru–Chile) upwelling system is selected as the study region due to its persistent alongshore winds and complex wave climate characterised by the coexistence of locally generated wind waves and remotely generated swells. These characteristics provide an ideal environment for separating different wave influences on air–sea exchanges.
### Experimental Design
A hierarchy of numerical experiments will be conducted to isolate and quantify wave effects:
- **CTRL:** Atmosphere–ocean coupled simulation without explicit wave feedback.
- **WITHWAVE:** Fully coupled atmosphere–wave–ocean simulation enabling wave feedbacks.
- **PROCESS experiments:** Simulations selectively activating individual wave-induced mechanisms to isolate contributions from momentum, Stokes-drift, and turbulence pathways.
- **Sensitivity experiments:** Simulations separating the roles of locally generated wind waves and remotely generated swell forcing.
This experimental strategy allows causal attribution of wave impacts on upper-ocean structure and upwelling variability.
### Research Questions
The project addresses the following quantifiable questions:
1. What are the relative contributions of wave-mediated momentum transfer, Stokes-drift processes, and wave-induced turbulence to air–sea exchanges in the Southeast Pacific upwelling system?    
2. To what extent do wave-induced processes modify upwelling intensity, vertical structure, and temporal variability compared with atmosphere–ocean coupled simulations without wave feedback?
3. How do locally generated wind waves and remotely generated swells differ in their influence on air–sea momentum and energy exchanges?
### Expected Scientific Advances
This research will provide a dynamically consistent and process-based assessment of how surface waves influence coastal upwelling systems. By quantifying the relative importance of distinct wave-induced pathways within a fully coupled framework, the project aims to:
- clarify mechanisms linking surface waves and coastal upwelling dynamics,
- identify biases arising from insufficiently coupled modelling approaches,
- improve physical understanding of air–sea exchange processes in eastern boundary systems, and
- inform future development of coupled Earth system models and parameterisations.

## Concise version
Coastal upwelling systems are key components of the climate system, linking atmospheric forcing to ocean circulation, ecosystem productivity, and regional air–sea exchanges. Upwelling is primarily driven by alongshore winds that induce offshore Ekman transport and surface divergence, allowing cold, nutrient-rich subsurface waters to reach the surface (Belmadani et al., 2014; Tarazona and Arntz, 2001). The efficiency and variability of this process depend critically on how momentum and energy are transferred across the air–sea interface and redistributed within the upper ocean. While surface gravity waves are known to mediate these exchanges, their dynamical role in shaping coastal upwelling remains insufficiently quantified (Wu et al., 2019b, 2022).
Surface waves modify air–sea interaction through multiple mechanisms. They alter surface stress and roughness, generate Lagrangian Stokes drift, and enhance upper-ocean turbulence through multiple processes. Despite growing evidence that these mechanisms affect upper-ocean structure, most previous studies rely on uncoupled or one-way coupled modelling frameworks in which the wave field does not evolve interactively with both atmospheric and oceanic states (Wu et al., 2019a,b). Such approaches suppress feedbacks among winds, waves, and ocean circulation and typically examine wave effects in isolation, preventing a dynamically consistent assessment of their combined influence. Consequently, it remains unclear whether neglecting interactive wave coupling leads to systematic biases in simulated coastal upwelling systems.
This PhD project hypothesis that wave-induced modifications of surface stress and upper-ocean turbulence systematically alter upper-ocean structure through modified air-sea exchanges, producing measurable changes in coastal upwelling intensity that cannot be captured in insufficiently coupled modelling frameworks. Wave impacts are interpreted through 3 pathways: (1) momentum pathway, in which waves modify the transmission of wind momentum and thus Ekman transport; (2) Lagrangian pathway, where Stokes drift alters momentum balances and tracer transport; and (3) turbulent kinetic energy (TKE) pathway, in which wave-induced TKE change influence vertical mixing and modify mixed-layer structure. Together, these pathways provide a unified framework for attributing wave influences on upwelling dynamics.
This hypothesis will be tested using the fully coupled ICON modelling system, which interactively coupled atmosphere, wave, and ocean components through energetically consistent exchanges of momentum and energy. The Southeast Pacific (Peru–Chile) upwelling system is selected as the study region because persistent alongshore winds and a complex wave climate, shaped by both locally generated wind waves and remotely generated swells, provide an ideal environment for isolating wave effects.
A hierarchy of controlled simulations will be conducted: a baseline atmosphere–ocean coupled experiment (CTRL), a fully coupled atmosphere–wave–ocean simulation (WITHWAVE), targeted process experiments isolating individual wave pathways, and sensitivity experiments separating wind-wave and swell influences. This design enables causal attribution of how waves modify upper-ocean structure and upwelling variability.
This PhD project aims to address three questions: (i) what are the relative contributions of momentum, Lagrangian, and turbulence pathways to wave-modulated air–sea exchanges; (ii) how strongly do interactive wave processes alter upwelling intensity and vertical structure compared with simulations without wave feedback; and (iii) how differently do wind waves and swells influence upper-ocean dynamics. The expected outcome is a process-based and quantitatively constrained understanding of how surface waves regulate coastal upwelling and an assessment of biases arising from insufficient coupling in current models, contributing to improved representation of air–sea exchanges in coupled Earth system models.

Reference: 

Belmadani, A., Echevin, V., Codron, F., Takahashi, K., and Junquas, C. (2014). What dynamics drive future wind scenarios for coastal upwelling off Peru and Chile? Climate Dynamics, 43(7):1893–1914.

Wu, L., Breivik, , and Rutgersson, A. (2019a). Ocean-Wave-Atmosphere Interaction Processes in a Fully Coupled Modeling System. Journal of Advances in Modeling Earth Systems, 11(11):3852– 3874. 

Wu, L., Staneva, J., Breivik, , Rutgersson, A., Nurser, A. J. G., Clementi, E., and Madec, G. (2019b). Wave effects on coastal upwelling and water level. Ocean Modelling, 140:101405.

Wu, L., Breivik, , and Qiao, F. (2022). The Redistribution of Air–Sea Momentum and Turbulent Kinetic Energy Fluxes by Ocean Surface Gravity Waves. Journal of Physical Oceanography, 52(7):1483–1496. Publisher: American Meteorological Society Section: Journal of Physical Oceanography.


# [[2026-02-12]] Update: Ideas
some potential ideas that are discussed in the [brainstorming session](Regular-Meeting_Note#2026-02-12)
1. **Force the ICON-O with ERA5 atmospheric/wave forcing to study the Stokes transport**
	- use the ERA5 wave outputs (e.g., ocean stress $\tau_{oc}$) to force the ocean
2. **Would ~={red}extreme swell events=~ modulate the upper ocean tracer transport and affect:**
	1. spreads of the pollutants in the coastal upwelling region?
		- study if in extreme cases, the onshore Stokes transport (near the surface) could affect the offshore Ekman transport, to what extent? how’s the spread of the pollutants?
		- need high resolution regional model
		- need high frequency data (hourly, daily)
	2. production of phytoplankton in the coastal upwelling region?
		- If it is, to what extent? 
		- Dynamical reason? → change in upwelling intensity? structure?
		- need to run HAMOCC (additional coast; should be the later stage of my PhD!!)
3. **~={red}Stokes transport of heat that crosses the Equator Pacific=~**
	- Context: the mean Stokes transport, which is largely modulated by the swells, can travel and propagate across the equator, which might induce a significant amount of mean heat transport that the upper ocean mean currents cannot
	- It might incorporate large seasonality. As the storms-rich season in two hemispheres are different. The swell generation (mainly from storms) might also show great seasonality, and so does the seasonal mean Stokes transport of heat.


# [[2026-02-09]] Update: Refine research questions
1. Which wave-induced processes, including 
	1. **Wave-mediated momentum pathways**
		- Waves mediate how much and through which pathways does wind momentum reach to ocean
	2. **Stokes-drift-driven processes**
		- Waves generate Lagrangian drift that reorganises momentum, tracers and momentum
	3. **Wave breaking and associated irreversible processes**
		- Waves breaking transfers energy and momentum into turbulence, bubbles (i.e., dissipation) that change air-sea exchange efficiency
	play the dominant role in the modulation of ~={red}air-sea exchanges=~ in the Peruvian coastal region.
	
2. How do these wave-induced modifications affect the Peruvian coastal upwelling system in terms of its ~={red}structure and variability=~
	1. ==**Wave-mediated momentum -> change the stress that ocean "feels"**==
	   - Without wave: $\tau_{oc}=\tau_a$ ($\tau_{oc}=\tau_{oc}(\text{wind})$)
	   - With wave: $\tau_{oc} = \tau_{a} - \tau_{in} -\tau_{ds}$ ($\tau_{oc}=\tau_{oc}(\text{wind, wave})$)
	   - Directly Affect Ekman flow $v_{ek}$ estimation (to the 1st-order)
	2. **Stokes transport -> modify the offshore mass transport (i.e., divergence of surface flow)**
	   - Without wave (to the 1st-order): $w_{up}\sim \int_{-h}^{0} \nabla \cdot v_{ek}\;dz$, $w_{up}\sim V_{ek}$
	   - With wave (to the 1st-order): $w_{up}\sim \int_{-h}^{0} (v_{ek}+v_{st})\;dz$, $w_{up}\sim (V_{ek}+V_{st})$
	1. ==**Wave-induced $\Delta TKE$ → affect stratification → affect ocean state → ...?**==
	   - need to find the a "target" metric to analyse

3. What are the relative contributions of locally generated wind waves and remotely generated swells to air-sea exchanges in the Peruvian upwelling system?

> [!Question] 
> - Why are these questions important? 
> - What can a better understanding bring to the community?

# [[2025-12-11]] Update: PhD Proposal 1st version

## Main Gap
**“Poorly quantifying surface wave effects and their relative importance in the modification of air-sea interactions in Southeast Pacific Upwelling system”**
### Sub Gaps:
- What’s the relative contribution from local wind pattern (local wave), comparing to the contributions from Swells (remote wave)
	- Southern Ocean, characterised by strong winds and significant wave-climate, will be the major source of remote waves
- Which wave-effect dominate the air-sea interactions? What’s its role in upwelling system?
- Are there any wave-upwelling feedback mechanisms in such fully coupled simulation?
### Considered Wave Effects
- momentum flux (wind/wave stress)
- Stokes drift related effects (e.g., Langmuir Turbulence, Stokes-Coriolis force, advection by Stokes drift)
- surface roughness
	- might change heat fluxes, momentum flux, in the atmosphere

## Background
### Why Southeast Pacific?
- The **Southeast Pacific** presents a compelling region for investigating the effects of surface waves on air-sea interactions due to its complex and dynamically changing wave climate.
	- The **equatorial tropical regions** are characterised by a **complex multimodal wave climate**, driven by the interaction of **remotely generated swells**—primarily the dominant **Southern Ocean generated swell**—and **higher-frequency waves generated by prevailing trade winds**. This complexity results in a high annual mean wave period that exceeds 10 s in this area.
	- Historically, the broader **South Pacific** region has shown evidence of **significant positive trends** in high-frequency significant wave height ($H_s$) extremes (90th percentile), with increases ranging from **$0.5$ to $1 \text{ cm } \text{yr}^{-1}$**.
	- Furthermore, this region is projected to experience one of the most significant climate changes in its wave field, with the **mean wave height** ($H_s$) projected to **rise by 5–10% by 2100**. This makes the Southeast Pacific a critical and sensitive area to quantify the present and future relative importance of surface wave effects in the modification of local air-sea heat, momentum, and gas fluxes.
### Why upwelling systems?
- The Foundation: Upwelling is largely Driven by Wind Stress.
	- Since the rate and intensity of upwelling are directly proportional to the magnitude of the initial **wind stress ($\tau$)} and the subsequent **Ekman currents**, any process that modifies either of these is critical to model accurately.
	- Surface waves modify momentum/tracer fluxes in the air-sea interface. Its corresponding wave effect is therefore has large potential to influence the simulation of upwelling systems
		- “In addition, waves can significantly affect momentum flux in the coastal areas affected by coastal upwelling” (Wu et al., 2022, p. 1492)
	- The Flaw in traditional models
		- the impact of surface waves on air-sea interaction processes is often ignored
		- limited coupling between atmosphere, and ocean oversimplify the important role of surface waves, which act as intermediate reservoir for momentum and energy
### General Role of Surface Waves in Air-sea Interaction (Wu et al., 2022)
- **Momentum Flux Buffering:** Waves significantly alter the wind stress, which is the momentum flux, across the interface2222. Waves act as a **buffering role** in the transfer of energy and momentum and **redistribute** these fluxes both in time and space3.
- **Decoupling of Stress:** The presence of waves means the **ocean-side stress** ($\tau_{oc}$), which drives ocean circulation and dynamics, is often **not identical** to the **air-side stress** ($\tau_{a}$), which is the momentum flux lost from the atmosphere44. Waves can alter both the **magnitude and direction** of the stress transferred to the ocean555.
- **TKE Generation:** **Wave breaking** is the primary mechanism that enhances the **Turbulent Kinetic Energy (TKE) dissipation rate** in the near-surface layer6. The resulting breaking-induced TKE flux can significantly affect key ocean variables, including **sea surface temperature (SST)** and **surface currents**.
#### Relevance to the Southeast Pacific Upwelling System
- **Coastal Dynamics:** The redistributive role of waves in stress becomes **more significant closer to coasts** due to strong wind gradients13131313.
- **Upwelling Sensitivity:** Coastal **upwelling** is determined by the **ocean-side stress ($\tau_{oc}$), not the air-side stress ($\tau_{a}$)**14. Therefore, waves can **significantly alter** the momentum flux and, consequently, **coastal upwelling**15.
- **Need for Further Study:** The redistributive role of the sea state on **coastal upwelling** and climate simulations **needs to be investigated further**16.
### Traditional Model Limitations (Wu et al., 2019)
- **Traditional atmosphere, ocean, wave models run independently**
	- the impact of surface waves on air-sea interaction processes is often ignored since the temporal and spatial scales of surface waves are much smaller than the atmospheric and oceanic dynamic scales (Hasselmann, 1991)
	- Energy and momentum fluxes do not fully account for the impact of the oceanic wave field at the air-sea interface
- **Invalid Core Assumption:** In traditional atmosphere, ocean, and coupled models, the momentum flux to the ocean interior ($\tau_{oc}$) is commonly **assumed to be identical** to the momentum flux lost from the atmosphere ($\tau_{a}$). This assumption is **invalid** under non-equilibrium conditions, such as **growing and decaying waves** and in **fetch-limited conditions**.
- **Neglected Wave Dynamics:** The wave-induced alteration of stress magnitude and, particularly, the **directional difference** between $\tau_{oc}$ and $\tau_{a}$ are **rarely considered** in coupled climate and forecast models.
- **Poorly Quantified TKE:** TKE flux is often parameterised as a function of surface friction velocity when a wave model is unavailable. This traditional parameterisation is generally **not adequate** under the non-equilibrium sea states (growing and decaying waves) that are common over the open ocean12.
- **Inclusion of wave effects have been proved to improves the model performance** compare to the stand alone circulation model
	- (Wu et al., 2019) concludes that the inclusion of waves improves the model performance in terms of sea level height, temperature and circulation
	- But in Wu et al., 2019, the ocean model is forced by wave model. there’s no coupling between atmosphere, wave and ocean, no feedback from the wave field to the atmosphere, no feedback from the ocean interior to the wave field
### Both Wu’s studies ignore the Langmuir turbulence and bottom stress terms
- The Stokes production of TKE occurs when the **turbulent Reynolds stresses** in the oceanic mixed layer interact with the **vertical shear of the Stokes drift velocity** (the wave-induced Lagrangian current). This interaction extracts energy directly from the surface waves and injects it into the turbulence of the upper ocean
- Studies have demonstrated that the magnitude of Stokes production of TKE can be **of the same order as the conventional shear production** (i.e., TKE generated by the shear of the mean Eulerian current).
### Why studies different impact from waves?
- There’re complex interactions between these wave effects.
	- For example, (Wu et al., 2019) founded that the stokes drift advection could largely counters the effect of the Coriolis-Stokes Force on ocean circulation. But two effects do not always cancel entirely, as seen for the SST and associated coastal upwelling in Baltic Sea.
- Thus, consistently introduce these wave effects together and investigate their relative contributions is important.
### The use of ICON-wave
- The **ICON-Wave** is a fully coupled wave model with the new energetically consistent coupling wave approach that can be applied in both intermediate (e.g., CMIP-type models) and high-resolution (5 km ocean, 5km or 10 km atmosphere coupled) configuration.
- It provides the opportunity to investiagte the wave-effects in an atmosphere-ocean-wave energetically consistent fully coupling.
## What’s New in our (target) Study compare to literature in the field
- We aim to conduct a full atmosphere-wave-ocean coupling simulation with the new energetically consistent coupling approach.
	- the ICON model is capable to investigate the wave effects and their feedback to the atmosphere/ocean field.
	- provide a full picture of the wave influence in air-sea interactions in key climate systems, like the upwelling
- With the ICON, different wave-effects can be studied, including: (1) momentum flux (wind/wave stress); (2) Stokes drift related effects (e.g., Langmuir Turbulence, Stokes-Coriolis force, advection by Stokes drift); (3) surface roughness.
	- Their relative role in air-sea interactions over the upwelling system can be specified
	- The dominant role can be identified
	- Some previously ignored wave effects in climate models(namely, the Langmuir Turbulence, an important source for TKE production), can be quantified and studied.

## Summary
This version is also the draft for my [first Panel Report](Panel-Meeting_Note#Progress Report)

Surface waves play a fundamental role in mediating air-sea exchanges of momentum, heat, and turbulent energy, and their influence is expected to be particularly important in wind-driven coastal upwelling systems (Wu et al., 2019, 2022). Coastal upwelling results from wind-driven horizontal divergence that causes cold, nutrient-rich subsurface water to rise to the surface. Its intensity and structure are therefore primarily controlled by the along-shore wind stress and the associated offshore Ekman transport that induces this divergence (Belmadani et al., 2014; Tarazona & Arntz, 2001). Any process modifying the magnitude or direction of stress at the air-sea interface thus has the potential to substantially affect coastal upwelling dynamics. Surface waves may contribute to this modulation through multiple wave-induced effects, including wave-modified momentum fluxes, Stokes-drift-related processes, wave-induced turbulence kinetic energy (TKE) fluxes, and changes in surface roughness. 

Previous studies have demonstrated that surface waves can significantly influence air-sea interactions in coastal regions and associated upwelling systems. For example, based on a wave hindcast generated with WAVEWATCH III forced by Climate Forecast System Reanalysis data, \cite{wu_redistribution_2022} found that the redistributive effects of waves on momentum and TKE fluxes are significant in several major coastal upwelling regions worldwide. Similarly, using a regional configuration of Nucleus for European Modelling of the Ocean (NEMO) model forced by the WAve Model (WAM), \cite{wu_ocean-wave-atmosphere_2019} showed that the wave-induced momentum and TKE fluxes exert a stronger influence on Baltic coastal upwelling than that from Stokes-Coriolis forcing and Stokes drift on the mass and tracer advection. These results thus indicate that various wave-induced processes can play distinct roles in shaping air-sea fluxes and upwelling dynamics, leading to a complex combined effect.

However, the interpretation of these results is constrained by methodological limitations. In particular, most existing studies employ uncoupled or one-way coupled modelling frameworks, which prevent feedbacks between the atmosphere, wave field, and ocean interior \citep{wu_ocean-wave-atmosphere_2019,wu_wave_2019}. As a result, the simulated wave-field is not evolving interactively with the atmospheric and ocean states, thereby limiting the dynamical consistency of the simulated coastal air-sea exchange. In addition, not all relevant wave-induced processes are represented consistently across studies. For example, the Langmuir turbulence, an important contributor to the upper-ocean TKE production \citep{kantha_preliminary_2009, kantha_note_2010}, is not always included or is treated in a simplified manner. Consequently, while previous work provides valuable insights into individual wave-induced processes within specific model configurations, it does not yet offer a dynamically consistent assessment of these processes and their combined effects of surface waves on coastal upwelling systems.

To address the limitations identified above, this PhD research aims to investigate wave-induced processes on air-sea interactions in the upwelling system using a fully coupled ocean-wave-atmosphere modelling framework. Specifically, we aim to employ the ICON model, in which ICON-a (atmosphere), ICON-Wave (Wave), and ICON-o (Ocean) are coupled using a new energetically consistent exchange of momentum and energy. This framework allows surface waves, atmospheric forcing, and oceanic responses to evolve interactively, while enabling a more realistic representation of key wave-induced processes, including the Langmuir turbulence. 
The Southeast Pacific (Peru-Chile) upwelling system is selected as the focus of this study. Persistent alongshore winds sustain upwelling throughout the year in this region \cite{caldwell_peruvian_2001}, making it a representative of the wind-driven eastern boundary upwelling system. In addition, the Southeast Pacific features a complex and highly variable wave climate, largely driven by the coexistence of locally generated higher-frequency wind waves and remotely generated low-frequency swells, both of which can influence air-sea exchanges \citep{echevarria_seasonal_2019}. Besides, this variability is strongly modulated by large-scale climate modes, including El Niño Southern Oscillation (ENSO) and Pacific-South American (PSA) pattern, which can substantially affect regional wind patterns, thereby influencing wave characteristics \citep{caldwell_peruvian_2001, echevarria_influence_2020}. Observational and hindcast studies indicate pronounced wave variability in this region, including increasing trends in high-frequency significant wave height extremes, while future projections suggest further changes in the regional wave climate \citep{casas-prat_wind-wave_2024}. These characteristics make the Southeast Pacific a sensitive and well-suited test bed for a fundamental, process-based investigation of how surface waves modulate air–sea fluxes and influence coastal upwelling dynamics.

% The Southeast Pacific (Peru-Chile) upwelling system is chosen as the focus of this study. This upwelling system is particularly noteworthy because persistent alongshore winds sustain the upwelling process throughout the year \citep{caldwell_peruvian_2001}. In addition, the Southeast Pacific features a complex wave climate, largely due to the coexistence of locally generated wind waves and remotely generated swell, which can both induce significant influences. Its variability is largely influenced by several key patterns of climate variability which influencing wind-wave characteristics, including El Nino Southern Oscillation (ENSO) and Pacific-South American (PSA). Observational and hindcast studies indicate pronounced wave variability in this region, including increasing trends in high-frequency significant wave height extremes, while future projections suggest further changes in the regional wave climate \citep{casas-prat_wind-wave_2024}. These features make the Southeast Pacific a sensitive and climatically relevant test bed for the fundamental study on how surface waves modulate air-sea fluxes and influence coastal upwelling dynamics.

> [!Attention] **Specifically, this PhD research seeks to address the following research questions:**
> - Which wave-induced processes, namely wave-modified momentum fluxes, Stokes-drift related effects (i.e., Langmuir turbulence, the Coriolis-Stokes force, and Stokes drift on mass and tracer advection), and wave-induced changes in surface roughness, play the dominant role in the modulation of air-sea exchanges in the coastal upwelling region in the Southeast Pacific?
> - How do these wave-induced modifications influence the intensity, vertical structure, and temporal variability of Southeast Pacific coastal upwelling?
> - What are the relative contributions of locally generated wind waves and remotely generated swells to air-sea exchanges in the Southeast Pacific upwelling system?

By systematically incorporating and evaluating multiple wave-induced processes within a fully coupled and energetically consistent modelling framework, this research seeks to provide a process-based and quantitative assessment of surface-wave impacts on air-sea interactions in one of the key upwelling systems. The results are expected to improve understanding of the mechanisms linking surface waves, air-sea interactions, and upwelling dynamics, and to help clarify the relative importance of different wave-induced processes in such systems.


**Reference**:
Wu, L., Staneva, J., Breivik, Ø., Rutgersson, A., Nurser, A. J. G., Clementi, E., & Madec, G. (2019). Wave effects on coastal upwelling and water level. _Ocean Modelling_, _140_, 101405. [https://doi.org/10.1016/j.ocemod.2019.101405](https://doi.org/10.1016/j.ocemod.2019.101405)

Wu, L., Breivik, Ø., & Qiao, F. (2022). The Redistribution of Air–Sea Momentum and Turbulent Kinetic Energy Fluxes by Ocean Surface Gravity Waves. _Journal of Physical Oceanography_, _52_(7), 1483–1496. [https://doi.org/10.1175/JPO-D-21-0218.1](https://doi.org/10.1175/JPO-D-21-0218.1)

