---
tags:
  - project/WMT
  - AMOC
  - AMOC/weakening
Last Eddited: 2025-12-01
---
# General Information

For GRL, the length limit is **12 publication units**, where 1 publication unit = 1 figure, 1 table, or 500 words. I would recommend 4-5 figures and 3500-4000 words (see details in [[Figure_Plan]])

# Figure planned: 4-5 Figures

![[IMG_3785.jpeg |center]]

> **The logic link would be:** $AMOC_{z}$ → $AMOC_{sfc}(\sigma_{max})$ → $WMT(\sigma > \sigma_{AMOC_{max}})$ → Surface conditions

- **Figure 1**:
    - WMT plot (condensed to 4 panels);
        - 2 clusters average: climatology + response
    - Region boxes selected (LAB, ENA)
- **Figure 2**:
    - panel a: $AMOC_{z}$ vs $AMOC_{sfc}(\sigma_{max})$
    - panel b: $\Delta AMOC_{z}$ vs $\Delta AMOC_{sfc}(\sigma_{max})$
    - panel c: $\Delta AMOC_{sfc}(\sigma_{max})$ vs $\Delta WMT(\sigma > \sigma_{AMOC_{max}})$
    
    ***$\sigma_{max}$**: **The densest portion of the density space which still has active WMT**; defined as the final 1 kg m⁻³ bin with significant water mass transformation (WMT > threshold), identifying the densest actively-forming waters.
    
    - One potential issue of $\sigma_{max}$ is that: The sigma max is defined as by finding **the densest valid σ** for each region separately and then take the bigger one; However, when we focus on the $AMOC_{sfc}$, we choose the the value contributed from both regions (i.e., LAB and ENA); Thus, a more naturally thinking is that the $\sigma$ cut-off should be selected based on the combined WMT distribution
    
    *$\sigma_{AMOC_{max}}$: The density (or $\sigma$) level of the $AMOC_{sfc}$ (maximum surface forced AMOC)
    
- **Figure 3**:
    Bar chart of climatological Total WMT north of 50 N $WMT_{clima}(\sigma > \sigma_{AMOC_{max}})$ for each model; Divided into:
    - $WMT_{4xCO2}(\sigma > \sigma_{AMOC_{max}})$
    - $\Delta WMT_{lab}(\sigma > \sigma_{AMOC_{max}})$: reductions in WMT from LAB
    - $\Delta WMT_{ena}(\sigma > \sigma_{AMOC_{max}})$: reductions in WMT from ENA
    
    For each model, separated into 10x LAB models and 4x ENA models
    
- **Figure 4:**
    panel figure (1) : $\Delta WMT_{total}(\sigma > \sigma_{AMOC_{max}})$ vs $WMT_{clima,total}(\sigma > \sigma_{AMOC_{max}})$, something similar to the figure below. **Do i have to separate them into two regions?**
    
    ![[image.png]]
    
    panel figure (2): Partial derivative of surface density to temperature change, two regions, each cluster labeled in different colores
    
- **Figure 5:**
    Figures for explaining mechanisms
    surface plots, response….
    

# New Figure Planned

See details in [[Meeting_Note_WMT]]

## Density Flux, WMT, and WMT Integration:

### Density Flux and WMT calculation:
The surface-forced WMT is first computed in density space (σ) by binning the local density flux $D(x,y,t)$ over small $\sigma$ bins of width $\Delta \sigma$:

$$ F(\sigma) = \frac{1}{\Delta \sigma} \int_{\text{bin}} D(x,y,t) \, dA, $$

where $D(x,y,t)$ is the local surface-forced flux (units: kg m⁻² s⁻¹) and $dA$ is the area of each surface grid cell. **Each $F(\sigma)$ thus represents the WMT per density bin and has units of Sv (per $\sigma$ bin).**

### WMT Integration
To obtain the total WMT over a given $\sigma$ range, the flux is accumulated over selected bins:

$$ F_\mathrm{integrated} = \sum_{\sigma > \sigma_\mathrm{cutoff}} F(\sigma), $$

or equivalently, using numerical integration. **Since each $F(\sigma)$ is already normalised per bin, the accumulated flux $F_{\text{integrated}}$ has units of Sv.**