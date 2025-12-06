---
tags:
---
# General Outline

- **First part** (about one third): Present the key results from your Master’s thesis to provide some background and context
- **Second part**: Build on this with a short overview of relevant literature on the field you are planning to explore during your PhD
- **Third part**: Highlight the research gap you aim to address and outline the direction of your project.

# Detailed Outline

## **Title:** **From Large-Scale AMOC Uncertainty to Small-Scale Wave Processes**

**1. The Big Picture: AMOC Uncertainty**

- **Key Idea:** Climate models disagree on the magnitude of AMOC weakening under CO2 forcing, creating significant challenges for future climate prediction.

**2. My Master's Finding: The WMT Link**

- **Key Idea:** This uncertainty may directly correlates with the models’ uncertainty in the background mean state of the ocean.
- **Two kinds of uncertainty**:
    - Location of primary deep mixing
        - Models that are characterised by stronger deep mixing in the Labrador–Irminger Seas, also features a greater surface density sensitivity to SST over this region, exhibits significantly greater AMOC weakening
    - Water Mass Transformation (WMT)
        - AMOC weakening shows a strong linear positive correlation with WMT decline in the densest density range.
        - Models with stronger climatological WMT in dense water formation regions (e.g., Labrador Sea, GIN Seas) tend to experience a larger WMT decline, and thereafter, a greater AMOC decline

**3. The Bridge to My PhD: A Hypothesis**

- **Key Idea:** Inter-model differences in the representation of small-scale surface processes and their modulation of air-sea fluxes are a key source of spread in the simulated oceanic mean state. Consequently, more accurate parameterisations of these processes are essential for constraining this mean-state spread and its downstream uncertainty in AMOC projections

**4. Zooming In: Surface Waves**

- **Target**: My PhD will focus on one particularly influential yet often simplified surface process: Surface waves
    - **Intro**: wind-generated water wave, occurs on the free surface of bodies of water as a result of the wind; Mainly gravity waves (gravity as the main restoring force)
    - **Why important?**: Active players in air-sea interaction through several key mechanisms
        - Langmuir Turbulence: Wave-driven Stokes drift generates vortices that greatly enhance upper-ocean mixing
        - Surface roughness: Waves alter the sea surface texture, modifying the transfer of momentum from the atmosphere to the ocean
    - **Knowledge gap?:** While known to be importance, wave effects are often crudely parameterised in current climate models → ‼️check literature

**6. My Method: The ICON-Waves configuration**

- **Key Idea: The ICON-Wave** is a fully coupled wave model with the new energetically consistent coupling wave approach that can be applied in both intermediate (e.g., CMIP-type models) and high-resolution (5 km ocean, 5km or 10 km atmosphere coupled) configuration.
- Comparison to other coupled configuration: A table for comparison
    - ECMWF
    - FESOM

**5. Objectives**

With the implemented ICON-waves, we are capable to:

- **Quantify** the impact of prognostic wave coupling (vs. a simplified parameterisation) on air-sea exchanges and upper-ocean tracer fluxes
- **Isolate** and **quantify** the relative contributions of some specific wave effects (e.g., Langmuir turbulence, surface roughness) to changes in air-sea exchange, to determine which is the dominant player
- **Diagnose** the downstream impacts of fully coupled wave model on large-scale circulation patterns (e.g., AMOC, Walker circulation) and identify the key feedback mechanisms that drive these impacts.

**7. Current Status & Next Steps**

- **Now:**
    - Setting up the ICON-a/o coupled model with a basic setup where the wave effects are directly estimated from local wind conditions (i.e., simplified local parametric wave model); This reduced configuration will also be used as the base line for later analysis
    - Literature reading:
        - How to implement surface wave models
        - Surface wave effects in ocean models
        - Mixing schemes
- **Next:** Running first simulations and analysing baseline air-sea fluxes.

---

## Comment from Nils:

What is maybe nice to highlight for the surface wave part is that you will work in the **coupled setup** (it might be clear from the air-sea coupling statement but better be save than sorry) and that **you plan to study intermediate (CMIP-type) and high-resolution (5km ocean and 5 or 10km coupled).**

What they always prefer to see at the conference is that the candidate also gives a bit a **research overview and highlights what the new aspect of the method is**. In your case: new energtically consistent coupling method for surface waves, applications in high-resolution models, combination with water mass formation and different vertical mixing schemes.

The ECMWF is applying a coupled configuration of a surface wave model and it looks like that the FESOM model also has such a configuration (but they lack the above mentioned innovations). However, it is probably good to introduce and cite their work to emphasize the "new" part that we are dealing with here. I am attaching three paper that are worth reading (not sure if I send them two you before).

# Abstract

## Draft 1

Projected weakening of the Atlantic Meridional Overturning Circulation (AMOC) exhibits a large and critical spread across climate models. Previous studies show that this inter-model spread is strongly correlated with model discrepancies in the simulated oceanic mean state, such as upper-ocean stratification and mixed-layer properties. These differences in the mean state are, in turn, clearly manifested in key atmosphere-ocean coupling process. For example, they drive the models’ discrepancies in both the representation of surface-forced water mass transformation (WMT) and the geological location of primary deep convection, as illustrated in my Master’s research. Since the oceanic mean state is acutely sensitive to the parameterisation of small-scale surface processes that modulate air-sea exchanges, their accurate representation is therefore essential for constraining the uncertainty in AMOC projections under climate change.

My PhD project focuses on one particularly influential yet often simplified surface process: Surface waves. Wind-induced surface waves carry energy that propagates into the ocean interior, modifying air-sea interactions through several key wave effects. For instance, surface waves alter surface roughness to modify momentum fluxes, generate Langmuir turbulence that enhances upper-ocean mixing, and influence heat and gas exchange on the interface between atmosphere and ocean. These processes have profound but poorly constrained feedbacks on both oceanic and atmospheric circulation, largely due to reduced representations in the climate models.

Using the novel, energetically-consistent fully coupled ICON-waves model across multiple resolutions, we are now able to systematically evaluate wave-induced impacts beyond conventional parameterizations. Through comparative and sensitivity experiments, dominant mechanisms linking wave effects to mean-state biases and circulation responses can be investigated. The ultimate goal is to determine how wave-driven feedbacks influence the model mean state and large-scale circulation variability, thereby identifying a pathway to reduce critical uncertainties in future climate projections.

## Draft 2

Projected weakening of the Atlantic Meridional Overturning Circulation (AMOC) exhibits a significant spread across climate models, strongly correlated with model discrepancies in the simulated oceanic mean state, particularly the upper-ocean stratification and mixed-layer properties. These differences in the mean state are clearly manifested in key atmosphere-ocean coupling process. For instance, they drive the discrepancies in the representation of surface-forced water mass transformation (WMT) and the location of primary deep convection, as illustrated in my Master’s research. Since the oceanic mean state is acutely sensitive to the parameterisation of small-scale surface processes that modulate air-sea interactions, their accurate representation is therefore essential for constraining the uncertainty in AMOC projections. My PhD project focuses on one particularly influential yet often simplified surface process: Surface waves. Wind-induced surface waves carry energy that propagates into the ocean interior, modifying air-sea interactions through several key wave effects, including surface roughness and Langmuir turbulence. These effects have profound, yet poorly constrained feedbacks on large-scale circulation, largely due to reduced representation in the climate models. Using the novel, energetically-consistent fully coupled ICON-waves model across multiple resolutions, we are now able to systematically evaluate wave-induced impacts beyond conventional parameterizations. Through comparative and sensitivity experiments, dominant mechanisms linking wave effects to mean-state biases and circulation responses can be investigated. The ultimate goal is to determine how wave-driven feedbacks influence the model mean state and large-scale circulation variability, thereby identifying a pathway to reduce critical uncertainties in future climate projections.

## Draft 3 → final draft

Projected weakening of the Atlantic Meridional Overturning Circulation (AMOC) exhibits a large and critical spread across climate models. Previous studies show that this inter-model disparity is strongly correlated with models’ discrepancies in the simulated oceanic mean state, particularly in upper-ocean stratification and mixed-layer properties. These differences in the mean state are subsequently manifested as robust uncertainties in key atmosphere-ocean coupling process. For example, they drive the models’ discrepancies in both the representation of surface-forced water mass transformation (WMT) and the geological location of primary deep convection, as illustrated in my Master’s research. Given that the oceanic mean state is acutely sensitive to the parameterisation of small-scale (e.g., sub-grid scale) surface processes that modulate air-sea exchanges, a more physically grounded representation of these processes is therefore essential for constraining the uncertainty in the projections of AMOC’s response under climate change.

My PhD research will focus on one such highly influential yet frequently oversimplified component of the air-sea interface: ocean surface waves. Wind-generated surface waves facilitate the propagation of energy and momentum into the ocean interior, thereby modifying air-sea interactions through several key wave effects. For instance, surface waves alter momentum transfer by modifying surface roughness, enhance upper-ocean mixing via generation of Langmuir turbulence, and regulate the air-sea exchange of heat and climatically important gases like CO2. These effects exert profound influences on large-scale oceanic and atmospheric circulation patterns. However, their feedbacks remain poorly studied, largely due to reduced representations in contemporary climate models.

By implementing the novel, fully coupled, energetically-consistent ICON-waves model across multiple resolutions, this research will systematically evaluate wave-induced impacts beyond conventional simplified parameterizations. Through targeted comparative and sensitivity experiments, I will investigate the dominant feedback mechanisms by which wave-induced processes influence air-sea exchanges, upper-ocean mixing, and large scale circulation. The ultimate goal is to ****provide a process-based assessment of how surface waves modify the climate system and to use this understanding to guide the development of more physically grounded parameterizations in the ICON model.