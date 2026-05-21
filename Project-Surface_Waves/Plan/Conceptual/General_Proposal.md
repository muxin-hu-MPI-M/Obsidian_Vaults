---
tags:
  - project/surfwaves
  - proposal/surfwaves
Last Eddited: 2025-12-01
---
# **Topic**The implication of surface waves and upper-ocean mixing on air-sea coupling (WP3)


> [!Attention]
> Aiming at investigating how surface waves impact the air-sea heat and momentum exchange and how this affects the oceanic and atmospheric circulation; Focusing on **surface wave effects in coupled and high-resolution configurations and the evaluation of feedbacks** between these compartments.

## WP3a

- Carry out coupled ICON-a/o simulations, computing the Stokes drift (#Stokes_drift ) directly from the local wind field and assuming that the sea state is in equilibrium with the wind (a reduced configuration without the coupling of a surface wave model) --> served as a reference for estimating the relevance of propagating wave energy within the fully coupled configuration
    - **Key synthesis:**
        - **An assessment of how air-sea exchanges and upper-ocean tracer fluxes are modified** by including:
            1. a local parametric surface-wave model where the Stokes drift is derived from the wind field alone, assuming that the wave field is in equilibrium with the local wind **→ ‼️’reference’**
            2. a fully coupled prognostic wave model → **’fully coupled ICON-Waves setup’**
        - **A systematic overview of which surface wave effects dominate the modification of the air-sea exchanges?**
            - **Specific, desired-experiment**: consider single surface wave effects (e.g., Langmuir turbulence, surface roughness, modified tracer and momentum transports);
                
                - **Isolate** each of them by activating/deactivating parameterisations in simulations to determine which effects dominate the air-sea exchanges
                
                → The experience will be fed back to ICON-wave coupling
                

## WP3b

- Use the fully-coupled ICON-waves setup once it is available for further studies of surface-wave driven feedbacks in high resolution (5km hori. and 0.5 m vertical in upper ocean)
    - the atmospheric convection can be studied
    - The high vertical resolution allows to resolve the Langmuir turbulence (instead of prescribing it), and allows the development of diurnal warm layers (DWLs) and rain layers (RLs) → enabling us to study the impact of surface-wave-driven Langmuir turbulence on DWL and RL properties
    - **Consider the impacts in both the ocean and atmospheric circulation.**
        - Ocean: focusing on **upper-ocean mixing of T and S from subtropical to polar regions and the effect on WMT**. For tropical regions, focusing on how surface waves alter momentum mixing and the effect on **equatorial current system and the atmospheric Walker Circulation and Tropical Cyclones**.

# Summary

## Research questions:

1. What is the quantitative impact of transitioning from a simplified, local parametric wave model to a fully coupled, prognostic wave model on the simulated air-sea exchanges of momentum, heat, and tracer, and on the structure of the upper ocean?
2. Which specific wave effects, including Langmuir turbulence, surface roughness and modified tracer and momentum transports, dominate the modification of air-sea exchange?
3. What are the downstream impacts of a fully coupled wave model on large-scale oceanic and atmospheric circulation patterns, and what are the key feedback mechanisms that drive these changes?