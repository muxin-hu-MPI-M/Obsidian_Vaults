---
tags:
  - project/surfwaves
  - wave/surface_wave
  - ICON-O
  - ICON/experiment
Last Eddited: 2026-08-31
---
# Thoughts: [[2026-08-31]]
For the simulations, I would think in three layers:
1. **Immediate / transient adjustment**  
    After Stokes forcing is introduced, the ocean will first adjust through fast processes: velocity changes, pressure-gradient response, vertical motion, and near-surface compensation. This is worth analysing because my forcing is hourly and seasonally varying, so the transient response is part of the physical problem, not just noise.
2. **Seasonal adjusted response**  
    This is probably my main target. After removing the initial spin-up, compare monthly/seasonal means between CTRL and STOKES. The question becomes: does the Stokes-forced experiment show a coherent residual response organised by wave regimes, swell seasonality, latitude bands, or basins?
3. **Longer-term mean imprint**  
    I would avoid calling this “quasi-equilibrium” unless the simulation is very long. The ocean, especially below the mixed layer, may not equilibrate on my integration length. A safer phrase is **“adjusted climatological response”** or **“multi-year mean residual imprint.”**
    
For “non-negligible,” I would define it later using three criteria rather than one number:
- **Detectable**: STOKES minus CTRL exceeds internal variability or remains robust across months/seasons/years.
- **Dynamically organised**: the response is spatially coherent and linked to Stokes transport convergence/divergence, swell regimes, rotation, stratification, or basin geometry.
- **Tracer-relevant**: the circulation response changes heat/salt/tracer transport, especially where compensation occurs at depths with different tracer properties.